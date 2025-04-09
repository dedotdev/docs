# Storage Queries

### Query on-chain storage

On-chain storage can be queried via `client.query` entry point. All the available storage entries for a chain are exposed in the `ChainApi` interface for that chain and can be executed with format: `client.query.<pallet>.<storgeEntry>`. E.g: You can find all the available storage queries of Polkadot network [here](https://github.com/dedotdev/chaintypes/blob/main/packages/chaintypes/src/polkadot/query.d.ts), similarly for other networks as well.

```typescript
// Query account balance
const balance = await client.query.system.account(<address>);

// Get all events of current block
const events = await client.query.system.events();
```

### Subscribe to on-chain storage

You can also subscribe to on-chain storage changes by passing callback as the last argument to the storage query.

<pre class="language-typescript"><code class="lang-typescript">// Subscribe to account balance changes
const unsub = await client.query.system.account(&#x3C;address>, (balance) => {
<strong>  console.log('New free balance', balance.data.free);
</strong>
  unsub(); // unsubsribe from the subscription
});

// Subscribe to events of current best block
const unsub = await client.query.system.events((records) => {
  const transferEvent = client.events.balances.Transfer.find(records);
  
  if (transferEvent) {
    console.log('New transfer event: ', transferEvent);
    
    unsub(); // unsubsribe from the subscription
  }
});
</code></pre>

### Multi queries

#### Same storage types

Each [StorageMap](https://paritytech.github.io/polkadot-sdk/master/frame_support/storage/types/struct.StorageMap.html) query expose a `.multi` method provides a way to query multiple keys of the same type in a single RPC call. You can either make a direct query or pass a callback to subscribe to changes as they occur.

Direct one-time query

```typescript
const balances = await client.query.system.account.multi([ALICE, BOB]);

const [aliceBalance, bobBalance] = balances;
```

Subscription

```typescript
const unsub = await client.query.system.account.multi([ALICE, BOB], (balances) => {
  // Trigger when there're changes in alice or bob balances
  const [aliceBalance, bobBalance] = balances;
});
```

#### Different storage types

When you need to retrieve multiple storage values of different types/storage keys in a single call, you can use the `queryMulti` method. This is more efficient than making multiple individual queries as it batches the requests into a single RPC call.

Direct one-time query

```typescript
const [accountBalance, blockNumber] = await client.queryMulti([
  { fn: client.query.system.account, args: [ALICE] },
  { fn: client.query.system.number }
]),

// The return results are fully-typed
//  - accountBalance: FrameSystemAccountInfo
//  - blockNumber: number
```

Subscription

```typescript
const unsub = await client.queryMulti([
  { fn: client.query.system.account, args: [ALICE] },
  { fn: client.query.system.number }
], ([accountBalance, blockNumber]) => {
  // Trigger every time there're changes in accountBalance or block number
})
    
await unsub() // unsub later when done
```



