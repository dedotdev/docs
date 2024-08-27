# Generate Types & APIs

Before interacting with a contract, you need to generate Types & APIs from the contract metadata to interact with. You can do that using `dedot` cli:

```sh
dedot typink -m ./path/to/metadata.json # or metadata.contract

# use option -o to customize folder to put generated types
dedot typink -m ./path/to/metadata.json -o ./where/to-put/generated-types
```

After running the command, Types & APIs of the contract will be generated. E.g: if the contract's name is `flipper`, the Types & APIs will be put in a folder named `flipper`, the entry-point interface for the contract will be `FlipperContractApi` in `flipper/index.d.ts` file. An example of Types & APIs for flipper contract can be found [here](https://github.com/dedotdev/dedot/tree/main/zombienet-tests/src/contracts/flipper).
