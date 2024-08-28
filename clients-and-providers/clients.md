# Clients

Dedot exposes 3 high-level clients offering powerful API that abstracting over the complexities of on-making chain interactions (queries, transactions...) or sending raw JSON-RPC request to a blockchain nodes.

* `DedotClient`: High-level Substrate client built on top of the [new JSON-RPC API](https://paritytech.github.io/json-rpc-interface-spec/introduction.html)
* `LegacyClient`: High-level Substrate client built on top of the [legacy JSON-RPC API](https://github.com/w3f/PSPs/blob/master/PSPs/drafts/psp-6.md)
* `JsonRpcClient`: High-level JSON-RPC client to help making raw JSON-RPC requests or subscriptions seamlessly. Both `DedotClient` and `LegacyClient` are also a `JsonRpcClient`.

Both `DedotClient` and `LegacyClient` are both implementing the[`ISubstrateClient<ChainApi, ApiEvent>`](https://github.com/dedotdev/dedot/blob/f7910058d5e379f3f51476e10696a6f157f08591/packages/api/src/types.ts#L120) interface, so they are sharing a list of [common APIs](https://github.com/dedotdev/dedot/blob/f7910058d5e379f3f51476e10696a6f157f08591/packages/api/src/types.ts#L99-L144)  of a client to interact with Subtrated-based blockchains.

### DedotClient&#x20;

`DedotClient`'s high-level APIs are built on top of the new JSON-RPC API, beside implementing the common client APIs, `DedotClient` are also exposing access to `JsonRpcGroup` instances like `ChainHead` or `ChainSpec`.&#x20;

```typescript
import { DedotClient, WsProvider, PinnedBlock } from 'dedot';
import type { PolkadotApi } from '@dedot/chaintypes';

// Initialize providers & clients
const provider = new WsProvider('wss://rpc.polkadot.io');
const client = await DedotClient.new<PolkadotApi>(provider);

// Get current best runtime version
const currentBestRuntimeVersion = await client.getRuntimeVersion();

// Execute JSON-RPC methods
const metadata = await client.rpc.state_getMetadata();

// On-chain interactions
const balance = client.query.system.balance('<address>');
const transferTx = client.tx.balances.transferKeepAlive(<address>, 2_000_000_000_000n);

// Fetch ChainSpec information
const genesisHash = await client.chainSpec.genesisHash();
const chainProps = await client.chainSpec.properties();

// Access ChainHead data
const currentBestBlock = await client.chainHead.bestBlock();
const currentFinalizedBlock = await client.chainHead.finalizedBlock();
const finalizedRuntimeVersion = await client.chainHead.runtimeVersion();
const bestRuntimeVersion = await client.chainHead.bestRuntimeVersion();
// ...

// Listen to ChainHead events (best blocks, finalized block, ...)
const unsub = client.chainHead.on('bestBlock', (block: PinnedBlock, bestChainChanged: boolean) => {
    console.log(`Current best block: ${block.number}, hash: ${block.hash}, bestChainChanged: ${bestChainChanged}`);
});

// Other ChainHead events available like: 
//    'finalizedBlock': new best finalized block
//    'bestChainChanged': new best chain changed, a fork happened

// Or broadcast a raw transaction
const rawTxHex = '0x...';
const unsub = await client.chainHead.txBroadcaster.broadcastTx(rawTxHex);
```

### LegacyClient

Similarly to `DedotClient`, `LegacyClient` is also another client that Dedot offers, but its high-level APIs are built on of the legacy JSON-RPC API.

```typescript
import { LegacyClient, WsProvider } from 'dedot';
import type { PolkadotApi } from '@dedot/chaintypes';

// Initialize providers & clients
const provider = new WsProvider('wss://rpc.polkadot.io');
const client = await LegacyClient.new<PolkadotApi>(provider);

// Get current best runtime version
const currentBestRuntimeVersion = await client.getRuntimeVersion();

// Execute JSON-RPC methods
const metadata = await client.rpc.state_getMetadata();

// On-chain interactions
const balance = client.query.system.balance('<address>');
const transferTx = client.tx.balances.transferKeepAlive(<address>, 2_000_000_000_000n);

// Fetch ChainSpec information
const genesisHash = await client.rpc.chain_getBlockHash(0);
const chainProps = await client.rpc.system_properties();

// Access ChainHead data
const currentBestBlock = await client.rpc.chain_getBlock();
const currentFinalizedHash = await client.rpc.chain_getFinalizedHead();
const finalizedBlock = await client.rpc.chain_getBlock(currentFinalizedHash);
// ...

// Or broadcast a raw transaction
const rawTxHex = '0x...';
const txHash = await client.rpc.author_submitExtrinsic(rawTxHex);
// or
const unsub = await client.rpc.author_submitAndWatchExtrinsic(rawTxHex, (txStatus) => {});
```

### JsonRpcClient

`JsonRpcClient` is a light-weight JSON-RPC client. Both `DedotClient` and `LegacyClient` are also extending `JsonRpcClient`, so one can access all the methods and props of a `JsonRpcClient` on `DedotClient` or `LegacyClient`. So unless for advanced interaction intensively via raw JSON-RPC, we recommend using high-level abstraction APIs on `DedotClient` or `LegacyClient`.

```typescript
import { JsonRpcClient, WsProvider } from 'dedot';
import type { PolkadotApi } from '@dedot/chaintypes';

// Initialize providers & clients
const provider = new WsProvider('wss://rpc.polkadot.io');
const client = await JsonRpcClient.new<PolkadotApi>(provider);

// Get current best runtime version
console.log('Connection status:', client.status);

// Execute JSON-RPC methods
const metadata = await client.rpc.state_getMetadata();
const chain = await client.rpc.system_chain();
const nodeVersion = await client.rpc.system_version();

// Submit raw tx
const txHash = await client.rpc.author_submitExtrinsic('0x...');
```

***

### Connection Status

Clients keep track of connection status, and it can be access via  `client.status` with type `ConnectionStatus`

```typescript
import { ConnectionStatus } from 'dedot';

// client: DedotClient, LegacyClient or JsonRpcClient
const status: ConnectionStatus = client.status;
```

There are 3 available statues:

* `connected`: The client is connected to the network
* `disconnected`: The client is disconnected from the network
* `reconnecting`: The client is trying to reconnect to the network

### Client Events

Clients will emit the following events in specific situations:

```typescript
type ApiEvent = 'connected' | 'disconnected' | 'reconnecting' | 
                'error' | 'ready' | 'runtimeUpgraded';
```

* `connected`: The client is connected to the network
* `disconnected`: The client is disconnected from the network
* `reconnecting`: The client is trying to reconnect to the network
* `error`: There is a connection error occurred (e.g: WebSocket error)
* `ready`: Client's initialization process (download metadata, current runtime version...) is completed and ready to perform on-chain interactions
* `runtimeUpgraded`: A runtime upgrade occurred

```typescript
import { SubstrateRuntimeVersion } from 'dedot';

// Listen to client/api events
client.on('connected', () => {});
client.on('disconnected', () => {});
client.on('reconnecting', () => {});
client.on('ready', () => {});
client.on('error', (e: Error) => {});
client.on('runtimeUpgraded', (runtimeVersion: SubstrateRuntimeVersion) => {});
```

### ApiOptions & JsonRpcClientOptions

`TODO`



