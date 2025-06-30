# Transactions

Similarly to query contracts, the `Contract` interface will also be using to submitting transactions with syntax: `contract.tx.<message>`.

{% hint style="info" %}
Starting from `dedot@0.14.0`, dry-runs will be automatically performed internally to validate the transaction and estimate gas fees and storage deposit limits. You no longer need to run a dry-run manually unless you have advanced or custom use cases.&#x20;
{% endhint %}

```typescript
import { Contract } from 'dedot/contract';
import { FlipperContractApi } from './flipper';
import flipperMetadata from './flipper.json' assert { type: 'json' };

// ... initializing DedotClient

const ALICE = '5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY'; // Alice
const contractAddress = '...';

// create a contract instace from its metadata & address
const contract = new Contract<FlipperContractApi>(client, flipperMetadata, contractAddress, { defaultCaller: ALICE });

// Submitting the transaction
const { events } = await contract.tx.flip()
  .signAndSend(ALICE, ({ status }) => {
    console.log(status.type)
  })
  .untilFinalized(); // or .untilBestChainBlockIncluded();
  
  // fully-typed event
const flippedEvent = contract.events.Flipped.find(events);
console.log('Old value', flippedEvent.data.old);
console.log('New value', flippedEvent.data.new);
```

### Dry-run the transaction explicitly

If the contract message (transaction) returns a Result\<Data, Error>, you can explicitly perform a dry-run to verify the result before submitting the transaction.

```typescript
// Dry-run the call for validation and gas estimation
const result = await contract.query.flip();

// Check if the message return a `Result<Data, Error>`
// Skip this check if the message returning raw Data
if (result.data.isErr) {
  console.log('Cannot make transaction due to error:', data.err);
} else {
  // Submit the transaction via contract.tx.flip()
}
```
