# Events

The `Contract` interface also have APIs to help you work with fully-typed contract events easily and smoothly.

```typescript
import { ContractEvent } from 'dedot/contract';

// Initialize Contract instance
const contract = new Contract<FlipperContractApi>(client, flipperMetadata, contractAddress);

// Extracting contract events from transaction events
await contract.tx.flip({ gasLimit: raw.gasRequired })
  .signAndSend(ALICE, ({ status, events }) => {
    if (status.type === 'BestChainBlockIncluded' || status.type === 'Finalized') {
      // fully-typed event
      const flippedEvent = contract.events.Flipped.find(events);
      console.log('Old value', flippedEvent.data.old);
      console.log('New value', flippedEvent.data.new);
      
      // an array of Flipped event
      const flippedEvents = contract.events.Flipped.filter(events);
      
      // Get all contract events from current transactions
      const contractEvents: ContractEvent[] = contract.decodeEvents(events);
      
      // Another way to get the Flipper event
      const flippedEvent2 = contractEvents.find(contract.events.Flipped.is);
    }
  });

// Extracting contract events from system events
await client.query.system.events((events) => {
  // fully-typed event
  const flippedEvent = contract.events.Flipped.find(events);
  
  // get all events of this contract from current block
  const contractEvents: ContractEvent[] = contract.decodeEvents(events);
})
```
