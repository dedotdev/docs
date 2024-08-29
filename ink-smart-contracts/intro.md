# Introduction

Dedot offers type-safe APIs to interact with ink! smart contracts. Primitives to work with contracts are exposed in `dedot/contract` package.

<figure><img src="../.gitbook/assets/inkcontractapi.gif" alt=""><figcaption><p>Interact with a PSP22 contracts using Dedot's type-safe APIs</p></figcaption></figure>

### Supported ink! versions

{% hint style="info" %}
Current Dedot only supports ink! versions `v4` & `v5`. We do not have plans to support older versions, but let us know your thoughts if we should reconsider this.
{% endhint %}

### Getting started

1. [Generate Types & APIs](generate-types-and-apis.md) for your contracts
2. [Deploy contracts](images-and-media.md) using `ContractDeployer` interface
3. Interact with contracts using `Contract` interface ([queries](queries.md), [submit transactions](transactions.md), ...)
4. Working with [fully-typed contract events](events.md)

{% hint style="warning" %}
If you're connecting to a local [`substrate-contracts-node`](https://github.com/paritytech/substrate-contracts-node/releases) for development, you might want to connect to the network using `LegacyClient` since the latest version of `substrate-contracts-node` ([`v0.41.0`](https://github.com/paritytech/substrate-contracts-node/releases/tag/v0.41.0)) does not working fine/comply with the latest updates for [new JSON-RPC specs](https://paritytech.github.io/json-rpc-interface-spec/introduction.html) for `DedotClient` to work properly.

Following [this instruction](https://docs.dedot.dev/getting-started/connect-to-network#using-legacyclient-to-connect-via-legacy-json-rpc-apis) to connect to the network via `LegacyClient`.
{% endhint %}
