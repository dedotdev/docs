# Deploy contracts

Whether it's to deploy a contract from a wasm/polkavm code or using an existing code hash. You can do it using the `ContractDeployer`.

### Initialize `ContractDeployer`

```typescript
import { DedotClient, WsProvider } from 'dedot';
import { ContractDeployer } from 'dedot/contract';
import { stringToHex } from 'dedot/utils'
import { FlipperContractApi } from './flipper';
import flipperMetadata from './flipper.json' assert { type: 'json' };

// instanciate an api client
const client = await DedotClient.new(new WsProvider('...'));

// load contract wasm or prepare a codeHash
const code = '0x...'; // wasm or polkavm (from .wasm or .polkavm files)
const existingCodeHash = '0x...' // uploaded wasm/polkavm

// create a ContractDeployer instance
const deployer = new ContractDeployer<FlipperContractApi>(client, flipperMetadata, code);

// OR from existingCodeHash
// const deployer = new ContractDeployer<FlipperContractApi>(client, flipperMetadata, existingCodeHash);
```

### \[`ink! v6 & solidity`] Map the account before interacting with `pallet-revive`

Pallet revive is designed to work with evm address/account (20 bytes / H160) by default. So before interact with contracts deployed on pallet revive via a Substrate address (32 bytes / H256), one need to map their Substrate address to a corresponding EVM address first.&#x20;

Simply submit a transaction (`tx.revive.mapAccount`) to map the account:

```typescript
import { toEvmAddress } from 'dedot/contracts';

// Check if the account is mapped yet
const mappedAccount = await client.query.revive.originalAccount(toEvmAddress(CALLER));
if (mappedAccount) {
  console.log('Address has already been mapped!');
} else {
  console.log('Address not mapped yet, map it now!');
  
  await client.tx.revive
    .mapAccount()
    .signAndSend(CALLER, ({ status }) => console.log(status.type))
    .untilFinalized();
}

```

### Dry-run the contract instantiation

{% hint style="info" %}
Starting from `dedot@0.14.0`, dry-runs will be automatically performed internally to validate the transaction and estimate gas fees and storage deposit limits. You no longer need to run a dry-run manually unless you have advanced or custom use cases.&#x20;
{% endhint %}

Dry run the constructor call to help validate the contract instantiation and estimate gas-fee for the transaction.

```typescript
import { generateRandomHex } from 'dedot/utils';

const CALLER = '5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY'; // Alice

// Some random salt to prevent duplication issue
// Salt is optional, you can skip this to use an empty salt 
const salt = generateRandomHex(); // for inkv6, salt need to be a 32-byte hex

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
// Generate a random salt
const salt = generateRandomHex();

// Submitting the transaction to instanciate the contract
const deploymentResult = await deployer.tx
  .new(true, { salt }) // `new` is the constructor defined in the contract
  .signAndSend(alice, ({ status }) => {
    console.log(`📊 Transaction status: ${status.type}`);
  })
  .untilFinalized(); // or .untilBestChainBlockIncluded();
```

### Retrieve contract address & contract instance after deployment

Once the contract deployment transaction is included in the best chain block or finalized, you can easily retrieve the contract address or initialize the contract instance directly from the deployment result.

```typescript
// Calculate contract address
const contractAddress = await deploymentResult.contractAddress()

// Initialize fully-typed contract instance 
const contract = await deploymentResult.contract();

// You can now interact with the contract via contract instance
const { data: value } = await contract.query.get();
console.log('Flipper value:', value)
```

#### Calculate contract address manually

The instructions below show how to manually calculate the contract address after deployment. This is intended for advanced use cases only — we recommend using the unified API to retrieve the contract address from the deployment result as shown above.

<details>

<summary>[ink! v4 &#x26; v5] Retrieve contract address from deployment events</summary>

#### Extract from `Contract.Instantiated` event

```typescript
const { events } = deploymentResult;

const instantiatedEvent = client.events.contracts.Instantiated.find(events);
const contractAddress = instantiatedEvent.palletEvent.data.contract.address();
console.log(contractAddress);
```

#### Listen for `Contract.Instantiated` event from system events

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

</details>

<details>

<summary>[ink! v6 &#x26; solidity] Calculate contract address deterministically</summary>

#### Calculate contract address via deployment salt (`CREATE2`)

If one deploy the contract using a deployment salt (32 bytes), one can deterministically calculate the contract address even before deploying it using the `CREATE2` method.

```typescript
import { toEvmAddress, CREATE2 } from 'dedot/contracts';
import { generateRandomHex } from 'dedot/utils';

const client = ... // initialize DedotClient
const CALLER = '5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY'; // Alice
const polkavmCode = '0x...' // polkavm contract code

// Initialize ContractDeployer
const deployer = new ContractDeployer<FlipperContractApi>(client, flipperMetadata, polkavmCode);

// Initialize deployment salt. For inkv6, the salt need to be a 32-byte hex
const salt = generateRandomHex(); 

const dryRun = await deployer.query.new(true, { caller: CALLER, salt })

const contractAddress = CREATE2(
  toEvmAddress(CALLER),
  polkavmCode,
  dryRun.inputData, // encoded contract call data (selector + encoded arguments)
  salt,
);
```

#### Calculate contract address via deployer's `nonce`  (`CREATE1`)

If one deploy the contract without a deployment salt, one can calculate the contract address using the deployer's `nonce` before submitting the contract deployment transaction using the `CREATE1` method

```typescript
import { toEvmAddress, CREATE1 } from 'dedot/contracts';

const client = ... // initialize DedotClient
const CALLER = '5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY'; // Alice

const dryRun = await deployer.query.new(true, { caller: CALLER });

const nonce = await client.call.accountNonceApi.accountNonce(CALLER);
const contractAddress = CREATE1(toEvmAddress(CALLER), nonce);

// deploying the contract
```

</details>
