# Connect to network

After setting up your project & installing `dedot` packages, let's connect to the network to make some on-chain interactions. You can connect to the network using a WebSocket (`wss://`) connection or via light client ([smoldot](https://www.npmjs.com/package/smoldot)).

### Initializing `DedotClient` and interact with `Polkadot` network

{% tabs %}
{% tab title="WebSocket" %}
```typescript
import { DedotClient, WsProvider } from 'dedot';
import type { PolkadotApi } from '@dedot/chaintypes';

// Initialize providers & clients
const provider = new WsProvider('wss://rpc.polkadot.io');
const client = await DedotClient.new<PolkadotApi>(provider);

// Call rpc `state_getMetadata` to fetch raw scale-encoded metadata and decode it.
const metadata = await client.rpc.state_getMetadata();
console.log('Metadata:', metadata);

// Query on-chain storage
const balance = await client.query.system.account(<address>);
console.log('Balance:', balance);

// Subscribe to on-chain storage changes
const unsub = await client.query.system.number((blockNumber) => {
  console.log(`Current block number: ${blockNumber}`);
});

// Get pallet constants
const ss58Prefix = client.consts.system.ss58Prefix;
console.log('Polkadot ss58Prefix:', ss58Prefix);

// Call runtime api
const pendingRewards = await client.call.nominationPoolsApi.pendingRewards(<address>)
console.log('Pending rewards:', pendingRewards);

// Unsubcribe to storage changes & disconnect from the network
// await unsub();
// await client.disconnect();
```
{% endtab %}

{% tab title="Smoldot (Light Client)" %}
1. Install `smoldot` package

```sh
npm i smoldot # or via yarn, pnpm
```

2. Preparing a `chainSpec.json` file for the network that you want to connect to, you can find the chain spec for well-known chains from [substrate-connect](https://github.com/paritytech/substrate-connect/tree/main/packages/connect-known-chains/specs).
3. Initialize `SmoldotProvider` and `DedotClient` to connect to network

<pre class="language-typescript"><code class="lang-typescript"><strong>import { DedotClient, SmoldotProvider } from 'dedot';
</strong>import type { PolkadotApi } from '@dedot/chaintypes';
import * as smoldot from 'smoldot';
import chainSpec from './polkadot-chainspec.json';

// Start smoldot instance &#x26; initialize a chain
const client = smoldot.start();
const chain = await client.addChain({ chainSpec });

// Initialize providers &#x26; clients
const provider = new SmoldotProvider(chain);
const client = await DedotClient.new&#x3C;PolkadotApi>(provider);

// Call rpc `state_getMetadata` to fetch raw scale-encoded metadata and decode it.
const metadata = await client.rpc.state_getMetadata();
console.log('Metadata:', metadata);

// Query on-chain storage
const balance = await client.query.system.account(&#x3C;address>);
console.log('Balance:', balance);


// Subscribe to on-chain storage changes
const unsub = await client.query.system.number((blockNumber) => {
  console.log(`Current block number: ${blockNumber}`);
});

// Get pallet constants
const ss58Prefix = client.consts.system.ss58Prefix;
console.log('Polkadot ss58Prefix:', ss58Prefix);

// Call runtime api
const pendingRewards = await client.call.nominationPoolsApi.pendingRewards(&#x3C;address>)
console.log('Pending rewards:', pendingRewards);

// await unsub();
// await client.disconnect();
</code></pre>
{% endtab %}
{% endtabs %}

### Pick `ChainApi` interface for the network you're working with

We recommend specifying the [`ChainApi`](https://github.com/dedotdev/chaintypes/blob/main/packages/chaintypes/src/index.ts) interface (e.g: [`PolkadotApi`](https://github.com/dedotdev/chaintypes/blob/main/packages/chaintypes/src/polkadot/index.d.ts) in the example above) of the chain that you want to interact with. This enable Types & APIs suggestion/autocompletion for that particular chain (via IntelliSense). If you don't specify a `ChainApi` interface, the default [`SubstrateApi`](https://github.com/dedotdev/dedot/blob/main/packages/api/src/chaintypes/substrate/index.ts) interface will be used.

```typescript
import { DedotClient, WsProvider } from 'dedot';
import type { PolkadotApi, KusamaApi, MoonbeamApi, AstarApi } from '@dedot/chaintypes';

const polkadotClient = await DedotClient.new<PolkadotApi>(new WsProvider('wss://rpc.polkadot.io'));
const kusamaClient = await DedotClient.new<KusamaApi>(new WsProvider('wss://kusama-rpc.polkadot.io'));
const moonbeamClient = await DedotClient.new<MoonbeamApi>(new WsProvider('wss://wss.api.moonbeam.network'));
const astarClient = await DedotClient.new<AstarApi>(new WsProvider('wss://rpc.astar.network'));
const genericClient = await DedotClient.new(new WsProvider('ws://localhost:9944'));
```

{% hint style="info" %}
If you don't find the `ChainApi` for the network that you're working with in [the list](https://github.com/dedotdev/chaintypes/blob/main/packages/chaintypes/src/index.ts), you generate the `ChainApi` (Types & APIs) for it using [`dedot` cli](../cli.md).

```sh
# Generate ChainApi interface for Polkadot network via rpc endpoint: wss://rpc.polkadot.io
npx dedot chaintypes -w wss://rpc.polkadot.io
```



Or open a [pull request](https://github.com/dedotdev/chaintypes/pulls) to add your favorite Substrate-based network to the `@dedot/chaintypes` repo.
{% endhint %}

### Caching metadata

In the bootstrapping/intialization process, clients have to download metadata from the network to prepare for on-chain interactions. Downloading a big metadata blob can take a large amount of time, depending on the JSON-RPC server that dapps are connecting to, it might potentially take longer if the connection is via a light client. For example, downloading Polkadot metadata (\~500 kB) can take up to 500ms or \~1s or even longer depends on the network conditions.

Clients have an option to cache the downloaded metadata and using it again next time the dapp is loaded without having to download again. By default, Dedot save the cached metadata to `localStorage` in browser environment.

Enable caching metadata when initialize Dedot's clients:

<pre class="language-typescript"><code class="lang-typescript"><strong>import { DedotClient, WsProvider } from 'dedot';
</strong>import type { PolkadotApi } from '@dedot/chaintypes';

// Initialize providers &#x26; clients
const provider = new WsProvider('wss://rpc.polkadot.io');
const client = await DedotClient.new&#x3C;PolkadotApi>({ provider, cacheMetadata: true });
</code></pre>

<details>

<summary>Add a custom caching storage?</summary>

You can also add a custom cache storage for different environments:

```typescript
import { DedotClient, WsProvider } from 'dedot';
import type { PolkadotApi } from '@dedot/chaintypes';
import type { IStorage } from '@dedot/storage';

// Implement CustomStorage
class CustomStorage implements IStorage {
   // implementation details
}

// Initialize providers & clients
const provider = new WsProvider('wss://rpc.polkadot.io');
const client = await DedotClient.new<PolkadotApi>({ 
   provider, 
   cacheMetadata: true,
   cacheStorage: new CustomStorage()
});
```

</details>

### Dedot supports CommonJS

Dedot supports `CommonJS`, so you can use `require` to import primitives & APIs.

```typescript
const { DedotClient, WsProvider } = require('dedot');

const provider = new WsProvider('wss://rpc.polkadot.io');
const client = await DedotClient.new(provider);
```

### **Using `LegacyClient` to connect via legacy JSON-RPC APIs**

If the JSON-RPC server doesn't support [new JSON-RPC APIs](https://paritytech.github.io/json-rpc-interface-spec/introduction.html) yet, you can connect to the network using the `LegacyClient` which build on top of the [legacy JSON-RPC APIs](https://github.com/w3f/PSPs/blob/master/PSPs/drafts/psp-6.md).

```typescript
import { LegacyClient, WsProvider } from 'dedot';

const provider = new WsProvider('wss://rpc.polkadot.io');
const client = await LegacyClient.new(provider);
```

### When to use `DedotClient` or `LegacyClient`?

{% hint style="info" %}
The [new JSON-RPC APIs](https://paritytech.github.io/json-rpc-interface-spec/introduction.html) are not well implemented/unstable for RPC Nodes using Polkadot-SDK version < `1.11.0`, so one should connect to the network using `LegacyClient` in such cases. For nodes using Polkadot-SDK version >= `1.11.0`, it's recommended to use `DedotClient` to connect to the network.

You can easily check the current node's implementation version by calling RPC `system_version`

```typescript
const version = await client.rpc.system_version();
```
{% endhint %}

{% hint style="info" %}
It's recommended to use `DedotClient` for better performance when you connect to the network using [smoldot](https://www.npmjs.com/package/smoldot) light client via [`SmoldotProvider`](https://github.com/dedotdev/dedot/blob/main/packages/providers/src/smoldot/SmoldotProvider.ts).
{% endhint %}

