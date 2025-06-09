# Generate Types & APIs

Before interacting with a contract, you need to generate Types & APIs from the contract metadata to interact with. You can do that using `dedot` cli:

```sh
dedot typink -m ./path/to/metadata.json # or metadata.contract

# use option -o to customize folder to put generated types
dedot typink -m ./path/to/metadata.json -o ./where/to-put/generated-types
```

After running the command, Types & APIs of the contract will be generated. E.g: if the contract's name is `flipper`, the Types & APIs will be put in a folder named `flipper`, the entry-point interface for the contract will be `FlipperContractApi` in `flipper/index.d.ts` file. An example of Types & APIs for flipper contract can be found [here](https://github.com/dedotdev/dedot/blob/main/examples/scripts/inkv5/flipper/index.d.ts).

### `ContractApi` interface

The generated `ContractApi` interface has the following structure, illustrated with an example of `FlipperContractApi`:

```typescript
/**
 * @name: FlipperContractApi
 * @contractName: flipper
 * @contractVersion: 0.1.0
 * @authors: Parity Technologies <admin@parity.io>
 * @language: ink! 6.0.0-alpha
 **/
export interface FlipperContractApi<
  Rv extends RpcVersion = RpcVersion,
  ChainApi extends VersionedGenericSubstrateApi = SubstrateApi,
> extends GenericContractApi<Rv, ChainApi> {
  query: ContractQuery<ChainApi[Rv]>;
  tx: ContractTx<ChainApi[Rv]>;
  constructorQuery: ConstructorQuery<ChainApi[Rv]>;
  constructorTx: ConstructorTx<ChainApi[Rv]>;
  events: ContractEvents<ChainApi[Rv]>;
  storage: {
    root(): Promise<Flipper>;
    lazy(): WithLazyStorage<Flipper>;
  };

  types: {
    RootStorage: Flipper;
    LazyStorage: WithLazyStorage<Flipper>;
    LangError: InkPrimitivesLangError;
    ChainApi: ChainApi[Rv];
  };
}
```
