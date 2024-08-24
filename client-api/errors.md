# Errors

Pallet errors are thrown out when things go wrong in the runtime, those are defined in the metadata. Available errors for each pallet are also exposed in `ChainApi` interface, so we can get information an error through this syntax: `client.errors.<pallet>.<errorName>`. E.g: Available errors for Polkadot network can be found [here](https://github.com/dedotdev/chaintypes/blob/main/packages/chaintypes/src/polkadot/errors.d.ts).

Similar to Event API, this API is helpful when we want to check if an error maches with an error that we're expecting.

Example if an error is `AlreadyExists` from `Assets` pallet:

```typescript
// ...
await client.query.system.events(async (records) => {
  for (const tx of records) {
    if (client.events.system.ExtrinsicFailed.is(tx.event)) {
      const { dispatchError } = tx.event.palletEvent.data;
      if (client.errors.assets.AlreadyExists.is(dispatchError)) {
        console.log('Assets.AlreadyExists error occurred!');
      } else {
        console.log('Other error occurred', dispatchError);
      }
    }
  }
});
// ...
```

### Error API

`TODO`

#### is

#### meta

### How to find error meta from `DispatchError`?

`TODO`
