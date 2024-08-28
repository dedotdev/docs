# ChainApi

`ChainApi` interface is a concept that allows Dedot to enable Types & APIs suggestions for any Substrated-based blockchains. Each Substrate-based blockchain has its own `ChainApi` interface that exposes all of its Types & APIs to interact with the runtime.

A `ChainApi` interface ussually has the following structure:

```typescript
export interface VersionedSubstrateApi<Rv extends RpcVersion>
  rpc: ChainJsonRpcApis<Rv>; // json-rpc methods
  consts: ChainConsts<Rv>; // runtime constants
  query: ChainStorage<Rv>; // on-chain storage queries
  errors: ChainErrors<Rv>; // on-chain errors
  events: ChainEvents<Rv>; // on-chain events
  call: RuntimeApis<Rv>; // runtime apis
  tx: ChainTx<Rv>; // transactions
}

export interface ChainApi {
  legacy: VersionedSubstrateApi<RpcLegacy>; // interface for legacy JSON-RPC
  v2: VersionedSubstrateApi<RpcV2>; // interface for new JSON-RPC
}
```

### Access `ChainApi` interfaces

Dedot offers 2 different ways to access `ChainApi` interfaces for different Substrate-based blockchains.

#### Known `ChainApi` interfaces defined in `@dedot/chaintypes` package

Install package `@dedot/chaintypes` to get access to a [list of known `ChainApi` interfaces](https://github.com/dedotdev/chaintypes/blob/main/packages/chaintypes/src/index.ts). For example:

* [`PolkadotApi`](https://github.com/dedotdev/chaintypes/blob/main/packages/chaintypes/src/polkadot/index.d.ts)
* [`KusamaApi`](https://github.com/dedotdev/chaintypes/blob/main/packages/chaintypes/src/kusama/index.d.ts)
* [`PolkadotAssetHubApi`](https://github.com/dedotdev/chaintypes/blob/main/packages/chaintypes/src/polkadot-asset-hub/index.d.ts)
* [`KusamaAssetHubApi`](https://github.com/dedotdev/chaintypes/blob/main/packages/chaintypes/src/kusama-asset-hub/index.d.ts)
* ...

{% hint style="info" %}
We're welcome everyone to open a [pull request](https://github.com/dedotdev/chaintypes/pulls) to add your favorite Substrate-based network to the `@dedot/chaintypes` repo.
{% endhint %}

#### Generate `ChainApi` interface using Dedot's CLI

You can also generate `ChainApi` interface for any Substrate-based blockchains using `dedot`' CLI.

Generate `ChainApi` interface for a local substrate-based blockchains running on `ws://127.0.0.1:9944`:

```sh
npx dedot chaintypes -w ws://127.0.0.1:9944
```

You can also generate `ChainApi` interface using a raw metadata file (.scale) or runtime wasm file (.wasm). More information about this can be found in the [CLI page](https://docs.dedot.dev/cli#dedot-chaintypes).

### Using `ChainApi` interface

`ChainApi` interface is the generic parameter for Dedot's clients, so when intialize a Dedot client, make sure to pick the right `ChainApi` interface of the chain you're working with to enable Types & APIs suggestions for that particular chains.

<figure><img src="../.gitbook/assets/typesafe-apis.gif" alt=""><figcaption></figcaption></figure>

Example working with Polkadot Asset Hub:

<pre class="language-typescript"><code class="lang-typescript"><strong>import { DedotClient, WsProvider } from 'dedot';
</strong>import type { PolkadotAssetHubApi } from '@dedot/chaintypes';

// Initialize providers &#x26; clients
const provider = new WsProvider('wss://polkadot-asset-hub-rpc.polkadot.io');
const client = await DedotClient.new&#x3C;PolkadotAssetHubApi>(provider);
</code></pre>
