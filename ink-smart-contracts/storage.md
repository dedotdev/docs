# Storage

{% hint style="info" %}
Contract Storage API's currently only available for ink! v5 and v6! [Let us know](https://t.me/JoinDedot) if you want to use storage API for ink! v4.
{% endhint %}

### Root Storage

Flipper Contract

```typescript
import { FlipperContractApi, Flipper } from './flipper/index.ts'; // Generated types for flipper contract

const contract = new Contract<FlipperContractApi>(api, metadata, contractAddress);

/**
 * interface Flipper {
 *   value: boolean
 * }
 */
const root: Flipper = await contract.storage.root();

const value: boolean = root.value;
```

PSP22 Contract

```typescript
import { Psp22ContractApi, Psp22Token } from './psp22/index.ts'; // Generated types for psp22 contract 

const contract = new Contract<Psp22ContractApi>(api, metadata, contractAddress);

/**
 * Generated types RootStorage type for PSP22 contract
 * 
 * export type Psp22Token = {
 *   data: Psp22DataPsp22Data;
 *   name?: string | undefined;
 *   symbol?: string | undefined;
 *   decimals: number;
 * };
 *
 * export type Psp22DataPsp22Data = {
 *   totalSupply: bigint;
 *   balances: { get(arg: AccountId32Like): Promise<bigint | undefined> };
 *   allowances: { get(arg: [AccountId32Like, AccountId32Like]): Promise<bigint | undefined> };
 * };
 */
const root: Psp22Token = await contract.storage.root();

const { name, symbol, decimals } = root;
const totalSupply = root.data.totalSupply;

const ALICE = '5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY';
const aliceBalance: bigint | undefined = await root.data.balances.get(ALICE);

```

### Lazy Storage

PSP22 Contract

```typescript
import { Psp22ContractApi, Psp22Token } from './psp22/index.ts'; // Generated types for psp22 contract 

const contract = new Contract<Psp22ContractApi>(api, metadata, contractAddress);

/**
 * Generated types RootStorage type for PSP22 contract
 * 
 * export type LazyPsp22Token = {
 *   data: Psp22DataPsp22Data;
 * };
 *
 * export type LazyPsp22DataPsp22Data = {
 *   balances: { get(arg: AccountId32Like): Promise<bigint | undefined> };
 *   allowances: { get(arg: [AccountId32Like, AccountId32Like]): Promise<bigint | undefined> };
 * };
 */
const lazyStorage: Psp22Token = await contract.storage.lazy();

const ALICE = '5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY';
const aliceBalance: bigint | undefined = await root.data.balances.get(ALICE);

```

