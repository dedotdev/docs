# Deploy contracts

Whether it's to deploy a contract from a wasm code or using an existing wasm code hash. You can do it using the `ContractDeployer`.

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

const ALICE = '5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY'; // Alice

// Some random salt to prevent duplication issue
// Salt is optional, you can skip this to use an empty salt 
const salt = stringToHex('random-salt'); 

// Dry run the constructor call for validation and gas estimation
// An Error will be thrown out if there's a DispatchError or LangError (contract level error)
// More on this in the handling error section below
const dryRun = await deployer.query.new(true, { caller: ALICE, salt })
const { raw: { gasRequired } } = dryRun;

// Submitting the transaction to instanciate the contract
await deployer.tx.new(true, { gasLimit: gasRequired, salt })
  .signAndSend(ALICE, ({ status, events}) => {
    if (status.type === 'BestChainBlockIncluded' || status.type === 'Finalized') {
      // fully-typed event
      const instantiatedEvent = client.events.contracts.Instantiated.find(events);
      const contractAddress = instantiatedEvent.palletEvent.data.contract.address();
    }    
  });
```

In case the contract constructor returning a `Result<Self, Error>`, you can also check the see if the instantiation get any errors before submitting the transaction.

```typescript
const { data } = await deployer.query.new(true, { caller: ALICE, salt })
if (data.isErr) {
  console.log('Contract instantiation returning an error:', data.err);
} else {
  // submitting the transaction
}
```
