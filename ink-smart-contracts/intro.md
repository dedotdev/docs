# Introduction

Dedot offers type-safe APIs to interact with **ink!** **& solidity contracts** on PolkaVM (pallet-revive) using unified interfaces. Primitives to work with contracts are exposed in `dedot/contract` package.

<figure><img src="../.gitbook/assets/inkcontractapi.gif" alt=""><figcaption><p>Interact with a PSP22 contracts using Dedot's type-safe APIs</p></figcaption></figure>

### Supported versions

{% hint style="info" %}
Current Dedot only supports ink! versions `v4` , `v5` and `v6` (experimental). We do not have plans to support older versions, but let us know your thoughts if we should reconsider this.
{% endhint %}

{% hint style="warning" %}
Support for [ink! v6](https://use.ink/docs/v6/) and [solidity contracts](https://docs.soliditylang.org/en/latest/contracts.html) are currently **experimental**. Please [let us know](https://t.me/JoinDedot) if you run into any issues or have any feedback while trying it out!
{% endhint %}

### Getting started

1. [Generate Types & APIs](generate-types-and-apis.md) for your contracts
2. [Deploy contracts](deploy.md) using `ContractDeployer` interface
3. Interact with contracts using `Contract` interface ([queries](queries.md), [submit transactions](transactions.md), ...)
4. Working with [fully-typed contract events](events.md)
5. Retrieve contract storage with [Storage API](https://docs.dedot.dev/ink-smart-contracts/storage) (only for ink! contracts using ink! ABI)
6. [Handling errors](https://docs.dedot.dev/ink-smart-contracts/handle-errors)

{% hint style="warning" %}
If you're connecting to a local [`substrate-contracts-node`](https://github.com/paritytech/substrate-contracts-node/releases) for development, you might want to connect to the network using `LegacyClient` since the latest version of `substrate-contracts-node` ([`v0.41.0`](https://github.com/paritytech/substrate-contracts-node/releases/tag/v0.41.0)) does not working fine/comply with the latest updates for [new JSON-RPC specs](https://paritytech.github.io/json-rpc-interface-spec/introduction.html) for `DedotClient` to work properly.

Following [this instruction](https://docs.dedot.dev/getting-started/connect-to-network#using-legacyclient-to-connect-via-legacy-json-rpc-apis) to connect to the network via `LegacyClient`.
{% endhint %}

{% hint style="info" %}
To deploy and interact with ink! and solidity contracts on PolkaVM (pallet-revive) in development mode, we recommend using local [ink\_node](https://github.com/use-ink/ink-node), you can download and run the binary directly or launch it easily via [POP CLI](https://learn.onpop.io/contracts/guides/deploy#local-deployment-default).
{% endhint %}
