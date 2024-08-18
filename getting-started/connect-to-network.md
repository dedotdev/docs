# Connect to network

After setting your project, let's connect to the network to make some on-chain interactions.

### Initializing `DedotClient` and interact with Polkadot network

{% tabs %}
{% tab title="WebSocket" %}
```typescript
import { DedotClient, WsProvider } from 'dedot';
import type { PolkadotApi } from '@dedot/chaintypes';


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

```typescript
import { DedotClient, SmoldotProvider } from 'dedot';
import type { PolkadotApi } from '@dedot/chaintypes';
import * as smoldot from 'smoldot';
import chainSpec from './chainspec.json';

const client = smoldot.start();
const chain = await client.addChain({ chainSpec });

const provider = new SmoldotProvider(chain);
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

// await unsub();
// await client.disconnect();
```
{% endtab %}
{% endtabs %}

### Supports CommonJS (`require`)

Dedot supports `CommonJS`, so you can use `require` to import primitives & APIs.

```typescript
// main.js
const { DedotClient, WsProvider } = require('dedot');

// ...
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

