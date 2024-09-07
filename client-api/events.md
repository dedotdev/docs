# Events

Events for each pallet emit during runtime operations and are defined in the medata. Available events are also exposed in `ChainApi` interface so we can get information of an event through syntax `client.events.<pallet>.<eventName>`. E.g: Events for Polkadot network can be found [here](https://github.com/dedotdev/chaintypes/blob/main/packages/chaintypes/src/polkadot/events.d.ts), similarly for other network as well.

This `client.events` is helpful when we want quickly check if an event matches with an event that we're expecting in a list of events, the API also comes with type narrowing for the matched event, so event name & related data of the event are fully typed.

### Working with on-chain events

Example to list new accounts created in each block:

```typescript
// ...
const ss58Prefix = client.consts.system.ss58Prefix;
await client.query.system.events(async (records) => {
  const newAccountEvents = client.events.system.NewAccount.filter(records); // or find, is

  console.log(newAccountEvents.length, 'account(s) was created in block', await client.query.system.number());

  newAccountEvents.forEach((event, index) => {
    console.log(`New Account ${index + 1}:`, event.palletEvent.data.account.address(ss58Prefix));
  });
});
// ...
```

### Event API

#### is

Check for a specific on-chain event or event record.

```typescript
const records = await client.query.system.events();

const instantiatedEvent = records.map(({ event }) => event)
                                .find(api.events.contracts.Instantiated.is); // narrow down the type for type suggestions
                                
// OR
const instantiatedEventRecord = records.find(api.events.contracts.Instantiated.is);
```

#### find

Find an on-chain event from a list of system event records.

```typescript
const records = await client.query.system.events();

const instantiatedEvent = api.events.contracts.Instantiated.find(records);
```

#### filter

Return a list of a specific on-chain event from a list of system event records.

```typescript
const records = await client.query.system.events();

const instantiatedEvents = api.events.contracts.Instantiated.filter(records);
```
