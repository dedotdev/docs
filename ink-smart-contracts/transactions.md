# Transactions

Similarly to query contracts, the `Contract` interface will also be using to submitting transactions with syntax: `contract.tx.<message>`.

```typescript
import { Contract } from 'dedot/contract';
import { FlipperContractApi } from './flipper';
import flipperMetadata from './flipper.json' assert { type: 'json' };

// ... initializing DedotClient

const ALICE = '5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY'; // Alice
const contractAddress = '...';

// create a contract instace from its metadata & address
const contract = new Contract<FlipperContractApi>(client, flipperMetadata, contractAddress);

// Dry-run the call for validation and gas estimation
const { data, raw } = await contract.query.flip({ caller: ALICE });

// Check if the message return a `Result<Data, Error>`
// Skip this check if the message returning raw Data
if (data.isErr) {
  console.log('Cannot make transaction due to error:', data.err);
}

// Submitting the transaction after passing validation
const { events } = await contract.tx.flip({ gasLimit: raw.gasRequired })
  .signAndSend(ALICE, ({ status }) => {
    console.log(status.type)
  })
  .untilFinalized(); // or .untilBestChainBlockIncluded();
  
  // fully-typed event
const flippedEvent = contract.events.Flipped.find(events);
console.log('Old value', flippedEvent.data.old);
console.log('New value', flippedEvent.data.new);
```
