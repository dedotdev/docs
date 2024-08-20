# Constants

Runtime constants (parameter types) are defined in metadata, and can be inspected via `client.consts` entry point with format: `client.consts.<pallet>.<constantName>`. All available constants are also exposed in the `ChainApi` interface. E.g: Available constants for Polkadot network is defined [here](https://github.com/dedotdev/chaintypes/blob/main/packages/chaintypes/src/polkadot/consts.d.ts), similarly for other networks.

```typescript
// Get runtime version
const runtimeVersion = client.consts.system.version;

// Get existential deposit in pallet balances
const existentialDeposit = client.consts.balances.existentialDeposit;
```
