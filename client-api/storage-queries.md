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
