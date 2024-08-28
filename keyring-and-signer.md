# Keyring & Signer

### Keyring

Currently Dedot do not have any official package for creating & managing keys, we recommend using `@polkadot/keyring` package for this purpose.

Please refer to the [official documentation](https://polkadot.js.org/docs/keyring) of `@polkadot/keyring` for more details on how to install, create & manage keys.

{% hint style="warning" %}
Please note that `@polkadot/keyring` is currently using `bn.js` and `wasm` blob for some cryptographic implementations, this might increase the overall bundle-size of dapps.
{% endhint %}

### Signer

Dedot APIs are designed to be compatible with [`IKeyringPair`](https://github.com/polkadot-js/api/blob/3bdf49b0428a62f16b3222b9a31bfefa43c1ca55/packages/types/src/types/interfaces.ts#L15-L21) and [`Signer`](https://github.com/polkadot-js/api/blob/3bdf49b0428a62f16b3222b9a31bfefa43c1ca55/packages/types/src/types/extrinsic.ts#L135-L150) interfaces, so you can sign the transactions with accounts/keys created by a [`Keyring`](https://github.com/polkadot-js/common/blob/22aab4a4e62944a2cf8c885f50be2c1b842813ec/packages/keyring/src/keyring.ts#L41-L40) (`@polkadot/api`) or signers exposed from any [Polkadot{.js}-extensions-based](https://github.com/polkadot-js/extension?tab=readme-ov-file#api-interface) wallets (SubWallet, Talisman, ...).

#### Signing transactions using `IKeyringPair`

```typescript
import { cryptoWaitReady } from '@polkadot/util-crypto';
import { Keyring } from '@polkadot/keyring';

// Setup Keyring & create a KeyringPair
await cryptoWaitReady();
const keyring = new Keyring({ type: 'sr25519' });
const aliceKeyringPair = keyring.addFromUri('//Alice');

// Sign & send transaction
const unsub = await client.tx.balances
    .transferKeepAlive(<destAddress>, 2_000_000_000_000n)
    .signAndSend(aliceKeyringPair, async ({ status }) => {
      console.log('Transaction status', status.type);
      if (status.type === 'BestChainBlockIncluded') { // or status.type === 'Finalized'
        console.log(`Transaction completed at block hash ${status.value.blockHash}`);
        await unsub();
      }
    });
```

#### Signing transactions using `Signer`

```typescript
// Retrieve signer from SubWallet extension
const injected = await window.injectedWeb3['subwallet-js'].enable('A cool dapp');
const account = (await injected.accounts.get())[0]; // get the first account
const signer = injected.signer;

// Setup global signer
client.setSigner(signer);

// Sign & send transaction
const unsub = await client.tx.balances
    .transferKeepAlive(<destAddress>, 2_000_000_000_000n)
    .signAndSend(account.address, async ({ status }) => {
      console.log('Transaction status', status.type);
      if (status.type === 'BestChainBlockIncluded') { // or status.type === 'Finalized'
        console.log(`Transaction completed at block hash ${status.value.blockHash}`);
        await unsub();
      }
    });
```

