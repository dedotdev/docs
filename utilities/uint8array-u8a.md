# Uint8Array (U8a)

### `u8aToHex`

Convert a Uint8Array to a hex string

```typescript
import { u8aToHex } from 'dedot/utils';

u8aToHex(new Uint8Array([128, 0, 10])) // '0x80000a'
```

### `u8aToString`

Convert a Uint8Array to a string

```typescript
import { u8aToString } from 'dedot/utils';

const sampleU8a = new Uint8Array([0x68, 0x65, 0x6c, 0x6c, 0x6f, 0x20, 0x77, 0x6f, 0x72, 0x6c, 0x64]);
u8aToString(sampleU8a) // 'hello world'
```

### `u8aEq`

Compare two Uint8Arrays for equality

```typescript
import { u8aEq } from 'dedot/utils';

u8aEq(new Uint8Array([1, 2, 3]), new Uint8Array([1, 2, 3])) // true
u8aEq(new Uint8Array([1, 2, 3]), new Uint8Array([4, 5, 6])) // false
```

### `toU8a`

Converts the input to a hex string, input can be in different types: `string | number | Uint8Array | HexString`

```typescript
import { toU8a } from 'dedot/utils';

toU8a(new Uint8Array([0x12, 0x34])) // new Uint8Array([0x12, 0x34]);
toU8a(4660)) // new Uint8Array([0x12, 0x34])
toU8a('0x1234')) // new Uint8Array([0x12, 0x34])
toU8a('abc')) // new Uint8Array([0x61, 0x62, 0x63])
```

### `concatU8a`

Concat multiple Uint8Array instances into a single Uint8Array.

```typescript
import { concatU8a } from 'dedot/utils';

concatU8a(new Uint8Array([1, 2]), new Uint8Array([3, 4])) // new Uint8Array([1, 2, 3, 4])
```

