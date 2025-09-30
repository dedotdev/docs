# Generate Types & APIs

Before interacting with a contract, you need to generate Types & APIs from the contract metadata to interact with. You can do that using `dedot` cli, this works with both [ink! ABI](https://use.ink/docs/v6/basics/abi/ink) or [Solidity ABI](https://use.ink/docs/v6/basics/abi/solidity).

```sh
dedot typink -m ./path/to/metadata.json # or metadata.contract, storage.abi

# use option -o to customize folder to put generated types
dedot typink -m ./path/to/metadata.json -o ./where/to-put/generated-types
```

After running the command, Types & APIs of the contract will be generated. E.g: if the contract's name is `flipper`, the Types & APIs will be put in a folder named `flipper`, the entry-point interface for the contract will be `FlipperContractApi` in `flipper/index.d.ts` file. An example of Types & APIs for flipper contract can be found [here](https://github.com/dedotdev/dedot/blob/main/examples/scripts/inkv5/flipper/index.d.ts).

### `ContractApi` interface

The generated `ContractApi` interface has the following structure, illustrated with an example of `FlipperContractApi` using ink! contracts using ink! ABI.

```typescript
/**
 * @name: FlipperContractApi
 * @contractName: flipper
 * @contractVersion: 6.0.0
 * @authors: Parity Technologies <admin@parity.io>
 * @language: ink! 6.0.0-alpha.3
 **/
export interface FlipperContractApi<
  Rv extends RpcVersion = RpcVersion,
  ChainApi extends VersionedGenericSubstrateApi = SubstrateApi,
> extends InkGenericContractApi<Rv, ChainApi> {
  metadataType: 'ink';
  query: ContractQuery<ChainApi[Rv], 'ink'>;
  tx: ContractTx<ChainApi[Rv], 'ink'>;
  constructorQuery: ConstructorQuery<ChainApi[Rv], 'ink'>;
  constructorTx: ConstructorTx<ChainApi[Rv], FlipperContractApi, 'ink'>;
  events: ContractEvents<ChainApi[Rv], 'ink'>;
  storage: {
    root(): Promise<Flipper>;
    lazy(): WithLazyStorage<Flipper>;
  };

  types: {
    ChainApi: ChainApi[Rv];
    RootStorage: Flipper;
    LazyStorage: WithLazyStorage<Flipper>;
    LangError: InkPrimitivesLangError;
  };
}

```

For ink! or Solidity contracts using Sol ABI, the structure will be a bit different with some APIs are disabled. Below is an example of `ContractApi` for flipper contract using Sol ABI.

```typescript
export interface FlipperContractApi<
  Rv extends RpcVersion = RpcVersion,
  ChainApi extends VersionedGenericSubstrateApi = SubstrateApi,
> extends SolGenericContractApi<Rv, ChainApi> {
  metadataType: 'sol';
  query: ContractQuery<ChainApi[Rv], 'sol'>;
  tx: ContractTx<ChainApi[Rv], 'sol'>;
  constructorQuery: ConstructorQuery<ChainApi[Rv], 'sol'>;
  events: ContractEvents<ChainApi[Rv], 'sol'>;
  constructorTx: ConstructorTx<ChainApi[Rv], FlipperContractApi, 'sol'>;

  types: {
    ChainApi: ChainApi[Rv];
  };
}

```
