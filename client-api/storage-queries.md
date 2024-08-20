# Storage Queries

On-chain storage can be queried via `client.query` entry point. All the available storage entries for a chain are exposed in the `ChainApi` interface for that chain and can be executed with format: `client.query.<pallet>.<storgeEntry>`. E.g: You can find all the available storage queries of Polkadot network [here](https://github.com/dedotdev/chaintypes/blob/main/packages/chaintypes/src/polkadot/query.d.ts), similarly for other networks as well.

```typescript
// Query account balance
const balance = await client.query.system.account(<address>);

// Get all events of current block
const events = await client.query.system.events();
```
