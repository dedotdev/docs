---
description: Storage API to retrieve contract storage directly.
---

# Storage

{% hint style="info" %}
Contract Storage API's currently only available for ink! v5 and v6 using ink! ABI, and not available on ink! v6 or solidity contracts using Sol ABI.
{% endhint %}

Contract Storage API provides efficient access to smart contract storage data. This API allows developers to read contract storage without executing contract methods, enabling efficient data retrieval.

Dedot provides two distinct approaches to accessing contract storage, each optimized for different use cases

* **RootStorage**: Fetch and decode the entire root storage of the contract including API to retrieve data from lazy storage/fields like [Lazy](https://docs.rs/ink_storage/5.1.1/ink_storage/struct.Lazy.html), [Mapping](https://docs.rs/ink_storage/5.1.1/ink_storage/struct.Mapping.html), [StorageVec](https://docs.rs/ink_storage/5.1.1/ink_storage/struct.StorageVec.html). For lazy storage, you will have to make a separate call to fetch the data since they're using a different storage layout.
* **LazyStorage** (or [Non-Packed Storage](https://use.ink/docs/v5/datastructures/storage-layout#packed-vs-non-packed-layout)): Provide access to only lazy/non-packed storage/fields like [Lazy](https://docs.rs/ink_storage/5.1.1/ink_storage/struct.Lazy.html), [Mapping](https://docs.rs/ink_storage/5.1.1/ink_storage/struct.Mapping.html), [StorageVec](https://docs.rs/ink_storage/5.1.1/ink_storage/struct.StorageVec.html) if you just want to fetch data from these fields separately without having to fetch the whole root storage first.

### Root Storage

Example Root Storage for Flipper contract

```typescript
import { FlipperContractApi, Flipper } from './flipper/index.ts'; // Generated types for flipper contract

const contract = new Contract<FlipperContractApi>(api, metadata, contractAddress);

/**
 * Generated RootStorage type for Flipper contract
 *
 * interface Flipper {
 *   value: boolean
 * }
 */
const root: Flipper = await contract.storage.root();

const value: boolean = root.value;
```

Example Root Storage for PSP22 contract

```typescript
import { Psp22ContractApi, Psp22Token } from './psp22/index.ts'; // Generated types for psp22 contract 

const contract = new Contract<Psp22ContractApi>(api, metadata, contractAddress);

/**
 * Generated RootStorage type for PSP22 contract
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

Example Lazy Storage for PSP22 contract

```typescript
import { Psp22ContractApi } from './psp22/index.ts'; // Generated types for psp22 contract 

const contract = new Contract<Psp22ContractApi>(api, metadata, contractAddress);

/**
 * Generated LazyStorage type for PSP22 contract
 * 
 * export type Psp22Token = {
 *   data: Psp22DataPsp22Data;
 * };
 *
 * export type Psp22DataPsp22Data = {
 *   balances: { get(arg: AccountId32Like): Promise<bigint | undefined> };
 *   allowances: { get(arg: [AccountId32Like, AccountId32Like]): Promise<bigint | undefined> };
 * };
 */
const lazyStorage = contract.storage.lazy();

const ALICE = '5GrwvaEF5zXb26Fz9rcQpDWS57CtERHpNehXCPcNoHGKutQY';
const aliceBalance: bigint | undefined = await root.data.balances.get(ALICE);

```

