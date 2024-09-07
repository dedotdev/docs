# Deploy contracts

Whether it's to deploy a contract from a wasm code or using an existing wasm code hash. You can do it using the `ContractDeployer`.

### Initialize `ContractDeployer`

```typescript
import { DedotClient, WsProvider } from 'dedot';
import { ContractDeployer } from 'dedot/contract';
import { stringToHex } from 'dedot/utils'
import { FlipperContractApi } from './flipper';
import flipperMetadata from './flipper.json' assert { type: 'json' };

// instanciate an api client
const client = await DedotClient.new(new WsProvider('...'));

// load contract wasm or prepare a wasm codeHash
const wasm = '0x...';
const existingCodeHash = '0x...' // uploaded wasm

// create a ContractDeployer instance
const deployer = new ContractDeployer<FlipperContractApi>(client, flipperMetadata, wasm);

// OR from existingCodeHash
// const deployer = new ContractDeployer<FlipperContractApi>(client, flipperMetadata, existingCodeHash);
```

### Dry-run the contract instantiation

Dry run the constructor call to help validate the contract instantiation and estimate gas-fee for the transaction.

```typescript
const CALLER = '5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY'; // Alice

// Some random salt to prevent duplication issue
// Salt is optional, you can skip this to use an empty salt 
const salt = stringToHex('random-salt'); 

// Dry run the constructor call for validation and gas estimation
// An Error will be thrown out if there's a DispatchError or LangError (contract level error)
// More on this in the handling error section below
const dryRun = await deployer.query.new(true, { caller: CALLER, salt })
const { raw: { gasRequired, storageDeposit } } = dryRun;
```

#### Error handling if contract constructor returning a `Result<Self, Error>`

In case the contract constructor returning a `Result<Self, Error>`, you can also check the see if the instantiation get any errors before submitting the transaction.

```typescript
const { data, raw } = await deployer.query.new(true, { caller: ALICE, salt })
if (data.isErr) {
  console.log('Contract instantiation returning an error:', data.err);
} else {
  // submitting the transaction
}
```

### Submit contract instantiation transaction

After dry-run the transaction to make sure there will be no errors. Now let's submit the transaction to instantiate the contract and listen for events to extract contract address.

```typescript
// Submitting the transaction to instanciate the contract
await deployer.tx.new(true, { gasLimit: gasRequired, salt })
  .signAndSend(CALLER, ({ status, events}) => {
    if (status.type === 'BestChainBlockIncluded' || status.type === 'Finalized') {
      // fully-typed event
      const instantiatedEvent = client.events.contracts.Instantiated.find(events);
      const contractAddress = instantiatedEvent.palletEvent.data.contract.address();
      console.log(contractAddress);
    }    
  });
```

### Listen for `contract.Instantiated` event from system events

```typescript
await client.query.system.events(async (records) => {
  const instantiatedEvent = client.events.contracts.Instantiated.filter(events)
                  .find((e) => e.palletEvent.data.deployer.address() === CALLER);

  if (instantiatedEvent) {
    const contractAddress = instantiatedEvent.palletEvent.data.contract.address();
    console.log(contractAddress);
  }
});
```
