# Balances

## formatBalance

Format a balance value to a human-readable string

```typescript
import { formatBalance } from 'dedot/utils';

const options = { decimals: 12, symbol: 'UNIT' };
formatBalance('12023172837123', options) // 12.0231 UNIT;
formatBalance(12_023_172_837_123n, options) // 12.0231 UNIT;
formatBalance(BigInt(12_023_172_837_123), options // 12.0231 UNIT;
```
