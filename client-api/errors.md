# Errors

Pallet errors are thrown out when things go wrong in the runtime, those are defined in the metadata. Available errors for each pallet are also exposed in `ChainApi` interface, so we can get information an error through this syntax: `client.errors.<pallet>.<errorName>`. E.g: Available errors for Polkadot network can be found [here](https://github.com/dedotdev/chaintypes/blob/main/packages/chaintypes/src/polkadot/errors.d.ts).

Similar to Event API, this API is helpful when we want to check if an error maches with an error that we're expecting.

Example to check if an error is `AlreadyExists` from `Assets` pallet:

```typescript
// From system events
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
```

Example checking for error details in transaction callback:

```typescript
// Or from events in transaction callback
client.tx.balances.transferKeepAlive('<BOB_ADDRESS>', 1_000n)
  .signAndSend(aliceAddressOrKeyPair, ({ dispatchError, events }) => {
    if (dispatchError) { // DispatchError extracted from ExtrinsicFailed event if any
      // Check for specific error
      if (client.errors.balances.InsufficientBalance.is(dispatchError)) {
        console.log('Balance too low to send value!');
      } else {
        console.log('Other error occurred', dispatchError);
      }
      
      // Check by finding error metadata
      const errorMeta = client.registry.findErrorMeta(dispatchError);
      if (errorMeta) {
        const { pallet, name, docs } = errorMeta;
        console.log(`Error: [${pallet}][${name}] - ${docs.join(',')}`);
      }
    }
  });
```

### Error API

`TODO`

#### is

#### meta

### How to find error meta from `DispatchError`?

`TODO`
