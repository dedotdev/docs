# Runtime upgrades

With [forkless upgrade](https://wiki.polkadot.network/docs/learn-runtime-upgrades#forkless-upgrades) as a feature, it's important for dapps to keep up with the changes from runtime and have a better preparation when there're breaking upgrades coming up on-chain.

### Listen to runtime upgrades

Clients will emit a `runtimeUpgraded` event whenever there is a new runtime upgrade happens. Dapps can listen to this event and react accordingly (e.g: show/hide ui components, notifications ...)

```typescript
client.on('runtimeUpgraded', (runtimeVersion: SubstrateRuntimeVersion) => {
  console.log('New runtime spec version:', runtimeVersion.specVersion);
});
```

### Prepare for breaking runtime upgrades

`TODO`
