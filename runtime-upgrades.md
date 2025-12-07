# Runtime upgrades

With [forkless upgrade](https://wiki.polkadot.network/docs/learn-runtime-upgrades#forkless-upgrades) as a feature, it's important for dapps to keep up with the changes from runtime and have a better preparation when there're breaking upgrades coming up on-chain.

### Listen to runtime upgrades

Clients will emit a `runtimeUpgraded` event whenever there is a new runtime upgrade happens. Dapps can listen to this event and react accordingly (e.g: show/hide ui components, notifications ...)

```typescript
client.on('runtimeUpgraded', (runtimeVersion: SubstrateRuntimeVersion, at: BlockInfo) => {
  console.log('New runtime spec version:', runtimeVersion.specVersion, 'at block: ', at.number);
});
```

### Prepare for breaking runtime upgrades

Breaking changes from runtime upgrades happen all the time, and it's really crutial to prepare your dapps to run smoothly and not breaking before and after runtime upgrades.

If you have access to metadata or wasm runtime of the next runtime upgrades, you can generate the `ChainApi` interface (Types & APIs) for the next runtime upgrade using [`dedot`'s CLI](cli.md#dedot-chaintypes) and adapt accordingly.

```typescript
import { DedotClient, WsProvider } from 'dedot';
import type { PolkadotApi } from '@dedot/chaintypes';
import type { PolkadotNextApi } from './polkadot-next';

// Initialize providers & clients
const provider = new WsProvider('wss://rpc.polkadot.io');
const client = await DedotClient.new<PolkadotApi>(provider);

// Check the spec version of the next runtime upgrade 
// in the comment on top of the ChainApi interface definition
// E.g: https://github.com/dedotdev/chaintypes/blob/1d728e5cd5027c3768c79f6c6f230682337c997e/packages/chaintypes/src/polkadot/index.d.ts#L26
const NEXT_UPGRADE_SPEC_VERSION = 1003000; // SpecVersion of PolkadotNextApi

// Get the current runtime version
const runtimeVersion = await client.getRuntimeVersion();

// Check if we're on the next runtime version, 
// then use the new transferKeepAliveWithRemark tx
if (runtimeVersion.specVersion >= NEXT_UPGRADE_SPEC_VERSION) {
  // Casting the client to using the new ChainApi interface: PolkadotNextApi
  const nextClient = client as unknown as DedotClient<PolkadotNextApi>;
  await nextClient.tx.balances
    .transferKeepAliveWithRemark(<destAddress>, 2_000_000_000_000n, 'Remark!')
    .signAndSend(<address>);    
} else {
  // else using the old transferKeepAlive tx
  await client.tx.balances
    .transferKeepAlive(<destAddress>, 2_000_000_000_000n)
    .signAndSend(<address>);
}
```

