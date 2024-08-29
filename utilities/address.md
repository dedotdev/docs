# Address

### SS58 Address

#### `decodeAddress`

```typescript
import { decodeAddress } from 'dedot/utils';

decodeAddress('5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY') // return alice raw public key
```

#### `encodeAddress`

```typescript
import { encodeAddress } from 'dedot/utils';

encodeAddress('5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY', 2) // convert to Kusama address (prefix: 2)

const ALICE_PUBLIC_KEY = new Uint8Array([...]);
encodeAddress(ALICE_PUBLIC_KEY) // 5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY
```

### EVM Address

#### `isEvmAddress`

```typescript
import { isEvmAddress } from 'dedot/utils';

isEvmAddress('0x00a329c0648769A73afAc7F9381E08FB43dBEA72') // true
isEvmAddress('0xInvalidAddress') // false
```
