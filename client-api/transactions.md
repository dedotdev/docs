# Transactions

Transaction apis are designed to be compatible with [`IKeyringPair`](https://github.com/polkadot-js/api/blob/3bdf49b0428a62f16b3222b9a31bfefa43c1ca55/packages/types/src/types/interfaces.ts#L15-L21) and [`Signer`](https://github.com/polkadot-js/api/blob/3bdf49b0428a62f16b3222b9a31bfefa43c1ca55/packages/types/src/types/extrinsic.ts#L135-L150) interfaces, so you can sign the transactions with accounts created by a [`Keyring`](https://github.com/polkadot-js/common/blob/22aab4a4e62944a2cf8c885f50be2c1b842813ec/packages/keyring/src/keyring.ts#L41-L40) or from any [Polkadot{.js}-based](https://github.com/polkadot-js/extension?tab=readme-ov-file#api-interface) wallet extensions.

All transaction apis are exposed in `ChainApi` interface and can be access with syntax: `api.tx.<pallet>.<transactionName>`. E.g: Available transaction apis for Polkadot network are defined [here](https://github.com/dedotdev/chaintypes/blob/main/packages/chaintypes/src/polkadot/tx.d.ts), similarly for other networks as well.

### Sign & send a transaction

#### Example 1: Sign transaction with a `Keying` account

```typescript
import { cryptoWaitReady } from '@polkadot/util-crypto';
import { Keyring } from '@polkadot/keyring';

// ...

await cryptoWaitReady();
const keyring = new Keyring({ type: 'sr25519' });
const alice = keyring.addFromUri('//Alice');

const unsub = await client.tx.balances
    .transferKeepAlive(<destAddress>, 2_000_000_000_000n)
    .signAndSend(alice, async ({ status }) => {
      console.log('Transaction status', status.type);
      if (status.type === 'BestChainBlockIncluded') { // or status.type === 'Finalized'
        console.log(`Transaction completed at block hash ${status.value.blockHash}`);
        await unsub();
      }
    });
```

#### Example 2: Sign transaction using `Signer` from Polkadot{.js} wallet extension

```typescript
const injected = await window.injectedWeb3['polkadot-js'].enable('A cool dapp');
const account = (await injected.accounts.get())[0];
const signer = injected.signer;

const unsub = await client.tx.balances
    .transferKeepAlive(<destAddress>, 2_000_000_000_000n)
    .signAndSend(account.address, { signer }, async ({ status }) => {
      console.log('Transaction status', status.type);
      if (status.type === 'BestChainBlockIncluded') { // or status.type === 'Finalized'
        console.log(`Transaction completed at block hash ${status.value.blockHash}`);
        await unsub();
      }
    });
```

<details>

<summary>Example 3: Submit a batch transaction</summary>

<pre class="language-typescript"><code class="lang-typescript"><strong>import type { PolkadotRuntimeRuntimeCallLike } from '@dedot/chaintypes/polkadot';
</strong>
// Omit the detail for simplicity
const account = ...;
const signer = ...;

const transferTx = client.tx.balances.transferKeepAlive(&#x3C;destAddress>, 2_000_000_000_000n);
const remarkCall: PolkadotRuntimeRuntimeCallLike = {
  pallet: 'System',
  palletCall: {
    name: 'RemarkWithEvent',
    params: {
      remark: 'Hello Dedot!',
    },
  },
};

const unsub = client.tx.utility.batch([transferTx.call, remarkCall])
    .signAndSend(account.address, { signer }, async ({ status }) => {
      console.log('Transaction status', status.type);
      if (status.type === 'BestChainBlockIncluded') { // or status.type === 'Finalized'
        console.log(`Transaction completed at block hash ${status.value.blockHash}`);
        await unsub();
      }
    });
</code></pre>

</details>

<details>

<summary>Example 4: Teleport WND from Westend Asset Hub to Westend via XCM</summary>

```typescript
import { WestendAssetHubApi, XcmVersionedLocation, XcmVersionedAssets, XcmV3WeightLimit } from '@dedot/chaintypes/westendAssetHub';
import { AccountId32 } from 'dedot/codecs';

const TWO_TOKENS = 2_000_000_000_000n;
const destAddress = <bobAddress>;

const client = await DedotClient.new<WestendAssetHubApi>('...westend-assethub-rpc...');

const dest: XcmVersionedLocation = {
  type: 'V3',
  value: { parents: 1, interior: { type: 'Here' } },
};

const beneficiary: XcmVersionedLocation = {
  type: 'V3',
  value: {
    parents: 0,
    interior: {
      type: 'X1',
      value: {
        type: 'AccountId32',
        value: { id: new AccountId32(destAddress).raw },
      },
    },
  },
};

const assets: XcmVersionedAssets = {
  type: 'V3',
  value: [
    {
      id: {
        type: 'Concrete',
        value: {
          parents: 1,
          interior: { type: 'Here' },
        },
      },
      fun: {
        type: 'Fungible',
        value: TWO_TOKENS,
      },
    },
  ],
};

const weight: XcmV3WeightLimit = { type: 'Unlimited' };

client.tx.polkadotXcm
  .limitedTeleportAssets(dest, beneficiary, assets, 0, weight)
  .signAndSend(alice, { signer, tip: 1_000_000n }, (result) => {
    console.dir(result, { depth: null });
  });
```

</details>

### SubmittableExtrinsic

`SubmittableExtrinsic` is an extrinsic instance that's ready to sign and send to the network for inclusion. A `SubmittableExtrinsic` will be created after you execute a tx method with required arguments.

```typescript
const extrinsic: SubmittableExtrinsic = client.tx.balances.transferKeepAlive(<destAddress>, 2_000_000_000_000n)
```

#### `sign`

Sign the extrinsic using the [`IKeyringPair` or `Signer`](../keyring-and-signer.md)

```typescript
client.tx.balances
    .transferKeepAlive(<destAddress>, 2_000_000_000_000n)
    .sign(<signer_address>);
```

You can also customize the transaction by using signer options as the second parameter:

```typescript

import { SignerOptions } from 'dedot/types';
const options: SignerOptions = {
    nonce: 10,
    tip: 1_000n,
    metadataHash: '0x...',
    assetId: { ... },
    signer: CustomSignerInstance,
}

client.tx.balances
    .transferKeepAlive(<destAddress>, 2_000_000_000_000n)
    .sign(<signer_address>, options);
```

#### `send`

Send the extrinsic to the network

```typescript
const txHash = await client.tx.balances
                  .transferKeepAlive(<destAddress>, 2_000_000_000_000n)
                  .sign(<signer_address>) // we need to sign the tx before sending it
                  .send();
```

And watch its status as needed

```typescript
const unsubFn = await client.tx.balances
                  .transferKeepAlive(<destAddress>, 2_000_000_000_000n)
                  .sign(<signer_address>) // we need to sign the tx before sending it
                  .send(({ status }) => { console.log(status) }); // ref: TxStatus
```

#### `signAndSend`

Sign and send the extrinsic to the network. This method's simply a combination of `sign` & `send` methods.

```typescript
const txHash = await client.tx.balances
                  .transferKeepAlive(<destAddress>, 2_000_000_000_000n)
                  .signAndSend(<signer_address>);

// Or keep watching the tx status
const unsubFn = await client.tx.balances
                  .transferKeepAlive(<destAddress>, 2_000_000_000_000n)
                  .signAndSend(<signer_address>, (result) => {
                     const { status } = result;
                     console.log(status);
                  });
```

Or pass in an options object to customize the transaction parameters

```typescript
import { SignerOptions } from 'dedot/types';
const options: SignerOptions = {
    nonce: 10,
    tip: 1_000n,
    metadataHash: '0x...',
    assetId: { ... },
    signer: CustomSignerInstance,
}

const unsubFn = await client.tx.balances
                  .transferKeepAlive(<destAddress>, 2_000_000_000_000n)
                  .signAndSend(<signer_address>, options, (result) => {
                     const { status } = result;
                     console.log(status);
                  });
```

#### `paymentInfo`

Estimate gas fee for a transaction

```typescript
const { partialFee } = client.tx.balances
    .transferKeepAlive(<destAddress>, 2_000_000_000_000n)
    .paymentInfo(<signer_address>);
    
console.log('Estimated gas fee', partialFee);
```

### SubmittableResult

`TODO`

### TxStatus

Available statuses of a transaction:

```typescript
export type TxStatus =
  // emits after we validate the transaction via `call.taggedTransactionQueue.validateTransaction`
  | { type: 'Validated' } 
  // emits after we submit the transaction via TxBroadcaster
  | { type: 'Broadcasting' } 
  // emits when the tx is included in the best block of the chain
  | { type: 'BestChainBlockIncluded'; value: { blockHash: HexString; blockNumber: number; txIndex: number } }
  // emits when the tx was retracted from the chain for various reasons
  | { type: 'NoLongerInBestChain' } 
  // emits when the tx is finalized
  | { type: 'Finalized'; value: { blockHash: HexString; blockNumber: number; txIndex: number } }
  // emits when the tx is invalid for some reasons: invalid nonce ...
  | { type: 'Invalid'; value: { error: string } }
  // emits when the client cannot keep track of the tx
  | { type: 'Drop'; value: { error: string } };
```

### SignerOptions

```typescript
export interface PayloadOptions {
  nonce?: number; // customize the nonce
  tip?: bigint; // add a tip for the block producer
  assetId?: number | object; // customize asset to pay for fee
  metadataHash?: HexString; // submit metadata hash for validation
}

export interface SignerOptions extends PayloadOptions {
  signer?: Signer; // customize the signer instance to sign the transaction
}
```
