# Transactions

Transaction apis are designed to be compatible with [`IKeyringPair`](https://github.com/polkadot-js/api/blob/3bdf49b0428a62f16b3222b9a31bfefa43c1ca55/packages/types/src/types/interfaces.ts#L15-L21) and [`Signer`](https://github.com/polkadot-js/api/blob/3bdf49b0428a62f16b3222b9a31bfefa43c1ca55/packages/types/src/types/extrinsic.ts#L135-L150) interfaces, so you can sign the transactions with accounts created by a [`Keyring`](https://github.com/polkadot-js/common/blob/22aab4a4e62944a2cf8c885f50be2c1b842813ec/packages/keyring/src/keyring.ts#L41-L40) or from any [Polkadot{.js}-based](https://github.com/polkadot-js/extension?tab=readme-ov-file#api-interface) wallet extensions.

All transaction apis are exposed in `ChainApi` interface and can be access with syntax: `api.tx.<pallet>.<transactionName>`. E.g: Available transaction apis for Polkadot network are defined [here](https://github.com/dedotdev/chaintypes/blob/main/packages/chaintypes/src/polkadot/tx.d.ts), similarly for other networks as well.

### Sign & send a transaction

Example 1: Sign transaction with a `Keying` account

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

Example 2: Sign transaction using `Signer` from Polkadot{.js} wallet extension

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

Example 3: Submit a batch transaction

```typescript
import type { PolkadotRuntimeRuntimeCallLike } from '@dedot/chaintypes/polkadot';

// Omit the detail for simplicity
const account = ...;
const signer = ...;

const transferTx = client.tx.balances.transferKeepAlive(<destAddress>, 2_000_000_000_000n);
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
```

### SubmittableExtrinsic

`TODO`

#### sign

#### send

#### signAndSend

#### paymentInfo

### SubmittableResult

`TODO`

### TxStatus

`TODO`
