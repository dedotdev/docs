# Generate Types & APIs

Before interacting with a contract, you need to generate Types & APIs from the contract metadata to interact with. You can do that using `dedot` cli:

```sh
dedot typink -m ./path/to/metadata.json # or metadata.contract

# use option -o to customize folder to put generated types
dedot typink -m ./path/to/metadata.json -o ./where/to-put/generated-types
```

After running the command, Types & APIs of the contract will be generated. E.g: if the contract's name is `flipper`, the Types & APIs will be put in a folder named `flipper`, the entry-point interface for the contract will be `FlipperContractApi` in `flipper/index.d.ts` file. An example of Types & APIs for flipper contract can be found [here](https://github.com/dedotdev/dedot/tree/main/zombienet-tests/src/contracts/flipper).

{% hint style="info" %}
If you're connecting to a local [`substrate-contracts-node`](https://github.com/paritytech/substrate-contracts-node/releases) for development, you might want to connect to the network using `LegacyClient` since the latest version of `substrate-contracts-node` ([`v0.41.0`](https://github.com/paritytech/substrate-contracts-node/releases/tag/v0.41.0)) does not working fine/comply with the latest updates for [new JSON-RPC specs](https://paritytech.github.io/json-rpc-interface-spec/introduction.html) for `DedotClient` to work properly.

Following [this instruction](https://github.com/dedotdev/dedot?tab=readme-ov-file#using-legacyclient-to-connect-via-legacy-json-rpc-apis) to connect to the network via `LegacyClient`.
{% endhint %}
