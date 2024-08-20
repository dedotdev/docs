# Providers

Providers are means to provide connection to the network, Dedot comes by default with providers for connection via [WebSocket](https://developer.mozilla.org/en-US/docs/Web/API/WebSockets\_API) (`wss://`) and [smoldot](https://www.npmjs.com/package/smoldot) light client. But you can implement your own provider for your own needs.

### WsProvider

```typescript
import { WsProvider } from 'dedot';

// Initialize the provider & connect to the network
const provider = new WsProvider('wss://rpc.polkadot.io');
await provider.connect();  

// Fetch the genesis hash 
const genesisHash = await provider.send('chain_getBlockHash', [0]); 
console.log(genesisHash);  

// Subscribe to runtimeVersion changes 
await provider.subscribe({
  subname: 'chain_newHead', // subscription name for notification
  subscribe: 'chain_subscribeNewHeads', // subscribe method
  params: [], // params for subscribe method
  unsubscribe: 'chain_unsubscribeNewHeads', // unsubscribe method
}, (error, newHead, subscription) => { 
  console.log('newHead', newHead);   
});  

// Disconnect from the network
await provider.disconnect();
```

### SmoldotProvider

`SmoldotProvider` take in a parameter of type [`Chain`](https://github.com/smol-dot/smoldot/blob/cde274e628e3f34cf05e1a73a46cf323b6702a94/wasm-node/javascript/src/public-types.ts#L127) from `smoldot`, so before initialize a `SmoldotProvider`, one should install `smoldot` package and following the [instruction](https://github.com/smol-dot/smoldot/tree/main/wasm-node/javascript#example) to instanciate a `Chain` connection to the network.

If you're building a browser dapp, it's highly recommended to setup a [worker](https://github.com/smol-dot/smoldot/tree/main/wasm-node/javascript#usage-with-a-worker) for `smoldot`

1. Install `smoldot`

```sh
npm i smoldot
```

2. Initialize `SmoldotProvider`

```typescript
import { SmoldotProvider } from 'dedot';
import * as smoldot from 'smoldot';
import chainSpec from './polkadot-chainspec.json';

// Start smoldot instance & initialize a chain
const client = smoldot.start();
const chain = await client.addChain({ chainSpec });

// Initialize providers & connect to the network
const provider = new SmoldotProvider(chain);

await provider.connect();  

// Fetch the genesis hash 
const genesisHash = await provider.send('chain_getBlockHash', [0]); 
console.log(genesisHash);  

// Subscribe to runtimeVersion changes 
await provider.subscribe({
  subname: 'chain_newHead', // subscription name for notification
  subscribe: 'chain_subscribeNewHeads', // subscribe method
  params: [], // params for subscribe method
  unsubscribe: 'chain_unsubscribeNewHeads', // unsubscribe method
}, (error, newHead, subscription) => { 
  console.log('newHead', newHead);   
});  

// Disconnect from the network
await provider.disconnect();
```

### Add your own custom provider?

`TODO`
