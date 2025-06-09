# Utilities

### toEvmAddress

Convert a Substrate address (32 bytes / H256) to an evm address (20 bytes / H160)

```typescript
import { toEvmAddress } from 'dedot/contracts'

const ALICE = "5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY";
const ALICE_EVM = toEvmAddress(ALICE); // 0x9621dde636de098b43efb0fa9b61facfe328f99d
```

### generateRandomHex

Generate a random hex as deployment salt, by default a 32-byte hex will be generated

```typescript
import { generateRandomHex } from 'dedot/utils'

const salt = generateRandomHex(); // 32 bytes
const salt64 = generateRandomHex(64); // 64 bytes
```

More utilities can be found in [utilities page](https://docs.dedot.dev/utilities) e.g: [formatBalance](https://docs.dedot.dev/utilities/balances#formatbalance), [isEvmAddress](https://docs.dedot.dev/utilities/address#evm-address), [hash functions](https://docs.dedot.dev/utilities/hash-functions), ...

