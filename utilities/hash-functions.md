# Hash functions

### `blake2AsHex`

```typescript
import { blake2AsHex } from 'dedot/utils';

blake2AsHex('abc') // 256 bits, 0xbddd813c634239723171ef3fee98579b94964e3bb1cb3e427262c8c068d52319
blake2AsHex('abc', 64) // 0xd8bb14d833d59559
blake2AsHex('abc', 128) // 0xcf4ab791c62b8d2b2109c90275287816
blake2AsHex('abc', 512) // 0xba80a53f981c4d0d6a2797b69f12f6e94c212f14685ac4b74b12bb6fdbffa2d17d87c5392aab792dc252d5de4533cc9518d38aa8dbf1925ab92386edd4009923
```

### `blake2AsU8a`

```typescript
import { blake2AsU8a } from 'dedot/utils';

blake2AsU8a('abc') // 256 bits, Uint8Array(32) [ 189, 221, 129,  60,  99,  66, 57, 114, 49, 113, 239,  63, 238, 152, 87, 155, 148, 150,  78,  59, 177, 203, 62,  66, 114,  98, 200, 192, 104, 213, 35,  25 ]
blake2AsU8a('abc', 64) // Uint8Array(8) [ 216, 187,  20, 216, 51, 213, 149,  89 ]
blake2AsU8a('abc', 128) // Uint8Array(16) [ 207, 74, 183, 145, 198, 43, 141, 43,  33,   9, 201,  2, 117, 40, 120,  22 ]
blake2AsU8a('abc', 512) // Uint8Array(64) [ 186, 128, 165,  63, 152,  28,  77,  13, 106,  39, 151, 182, 159,  18, 246, 233,  76,  33,  47,  20, 104,  90, 196, 183,  75,  18, 187, 111, 219, 255, 162, 209, 125, 135, 197,  57,  42, 171, 121,  45, 194,  82, 213, 222, 69,  51, 204, 149,  24, 211, 138, 168, 219, 241, 146, 90, 185,  35, 134, 237, 212,   0, 153,  35 ]
```

### `keccakAsHex`

```typescript
import { keccakAsHex } from 'dedot/utils';

keccakAsHex('test') // 256 bits, 0x9c22ff5f21f0b81b113e63f7db6da94fedef11b2119b4088b89664fb9a3cb658
keccakAsHex('test', 512) // 0x1e2e9fc2002b002d75198b7503210c05a1baac4560916a3c6d93bcce3a50d7f00fd395bf1647b9abb8d1afcc9c76c289b0c9383ba386a956da4b38934417789e
```

### `keccakAsU8a`

```typescript
import { keccakAsU8a } from 'dedot/utils';

keccakAsU8a('test') // 256 bits, Uint8Array(32) [ 156,  34, 255,  95,  33, 240, 184,  27, 17,  62,  99, 247, 219, 109, 169,  79, 237, 239,  17, 178,  17, 155,  64, 136, 184, 150, 100, 251, 154,  60, 182,  88 ]
keccakAsU8a('test', 512) // Uint8Array(64) [ 30,  46, 159, 194,   0,  43,   0,  45, 117,  25, 139, 117,   3,  33,  12,   5, 161, 186, 172,  69,  96, 145, 106,  60, 109, 147, 188, 206,  58,  80, 215, 240,  15, 211, 149, 191,  22,  71, 185, 171, 184, 209, 175, 204, 156, 118, 194, 137, 176, 201,  56,  59, 163, 134, 169, 86, 218,  75,  56, 147,  68,  23, 120, 158 ]
```

### `xxhashAsHex`

```typescript
import { xxhashAsHex } from 'dedot/utils';

xxhashAsHex('abc') // 64 bits, 0x990977adf52cbc44
xxhashAsHex('abc', 128) // 0x990977adf52cbc440889329981caa9be
xxhashAsHex('abc', 256) // 0x990977adf52cbc440889329981caa9bef7da5770b2b8a05303b75d95360dd62b
```

### `xxhashAsU8a`

```typescript
import { xxhashAsU8a } from 'dedot/utils';

xxhashAsHex('abc') // 64 bits, Uint8Array(8) [ 153,  9, 119, 173, 245, 44, 188,  68 ]
xxhashAsHex('abc', 128) // Uint8Array(16) [ 153,   9, 119, 173, 245, 44, 188,  68,   8, 137, 50, 153, 129, 202, 169, 190 ]
xxhashAsHex('abc', 256) // Uint8Array(32) [ 153,   9, 119, 173, 245,  44, 188,  68, 8, 137,  50, 153, 129, 202, 169, 190, 247, 218,  87, 112, 178, 184, 160,  83, 3, 183,  93, 149,  54,  13, 214,  43 ]
```
