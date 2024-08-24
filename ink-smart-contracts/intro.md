# Introduction

Dedot offers type-safe APIs to interact with ink! smart contracts. Primitives to work with contracts are exposed in `dedot/contract` package.

### Supported ink! versions

{% hint style="info" %}
Current Dedot only supports ink! versions `v4` & `v5`. We do not have plans to support older versions, but let us know your thoughts if we should reconsider this.
{% endhint %}

### Getting started

1. [Generate Types & APIs](generate-types-and-apis.md) for your contracts
2. [Deploy contracts](images-and-media.md) using `ContractDeployer` interface
3. Interact with contracts using `Contract` interface ([queries](queries.md), [submit transactions](transactions.md), ...)
4. Working with [fully-typed contract events](events.md)
