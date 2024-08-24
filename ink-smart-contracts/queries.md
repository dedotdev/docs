# Queries

The `Contract` interface will be using to interact with a contract using syntax: `contract.query.<message>`.

```typescript
import { Contract } from 'dedot/contract';
import { FlipperContractApi } from './flipper';
import flipperMetadata from './flipper.json' assert { type: 'json' };

// ... initializing DedotClient

const ALICE = '5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY'; // Alice
const contractAddress = '...';

// create a contract instace from its metadata & address
const contract = new Contract<FlipperContractApi>(client, flipperMetadata, contractAddress);

// Making call to get the current value of the flipper contract
const result = await contract.query.get({ caller: ALICE });

// Typescipt can inspect the type of value as `boolean` with the support of FlipperContractApi interface
const value: boolean = result.data;

// You can also have access to the detailed/raw result of the call
const rawResult = result.raw;
```
