# Runtime APIs

### Metadata `>= v15`

The latest stable Metadata v15 now includes all the runtime apis type information. For chains that are supported Metadata V15, we can now execute all available runtime apis with syntax `client.call.<runtimeApi>.<methodName>`, those apis are exposed in `ChainApi` interface. E.g: Runtime Apis for Polkadot network is defined [here](https://github.com/dedotdev/chaintypes/blob/main/packages/chaintypes/src/polkadot/runtime.d.ts), similarly for other networks as well.

```typescript
// Get account nonce
const nonce = await client.call.accountNonceApi.accountNonce(<address>);

// Query transaction payment info
const tx = client.tx.balances.transferKeepAlive(<address>, 2_000_000_000_000n);
const queryInfo = await client.call.transactionPaymentApi.queryInfo(tx.toU8a(), tx.length);

// Get runtime version
const runtimeVersion = await client.call.core.version();
```

### Metadata `v14`

For chains that only support Metadata v14, we need to bring in the Runtime Api definitions when initializing the DedotClient instance to encode & decode the calls. You can find all supported Runtime Api definitions in [`dedot/runtime-specs`](https://github.com/dedotdev/dedot/blob/fefe71cf4a04d1433841f5cfc8400a1e2a8db112/packages/runtime-specs/src/all.ts#L21-L39) package.

```typescript
import { RuntimeApis } from 'dedot/runtime-specs';

const client = await DedotClient.new({ 
  provider: new WsProvider('wss://rpc.mynetwork.com'), 
  runtimeApis: RuntimeApis 
});

// Or bring in only the Runtime Api definition that you want to interact with
import { AccountNonceApi } from 'dedot/runtime-specs';
const client = await DedotClient.new({ 
  provider: new WsProvider('wss://rpc.mynetwork.com'), 
  runtimeApis: { AccountNonceApi } 
});

// Get account nonce
const nonce = await client.call.accountNonceApi.accountNonce(<address>);
```

You absolutely can define your own Runtime Api definition if you don't find it in the [supported list](https://github.com/dedotdev/dedot/blob/fefe71cf4a04d1433841f5cfc8400a1e2a8db112/packages/runtime-specs/src/all.ts#L21-L39).
