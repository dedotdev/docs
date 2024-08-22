---
description: Dedot's Command line interface
---

# CLI

Dedot comes by default with a cli when you install [`dedot`](https://www.npmjs.com/package/dedot) package, you can access the cli using the name `dedot` (or `djs`). `dedot` cli helps you generate Types & APIs to any Substrate-based chains or ink! smart contracts that you're working with. This enable Types & APIs suggetions/auto-completion via IntelliSense for any on-chain interactions.

### `dedot chaintypes`

Generate Types & APIs for a Substrated-based blockchain given its metadata. The cli can fetch metadata from a WebSocket endpoint, a wasm runtime file or a raw metadata (.scale) file.

Usage:

```bash
npx dedot chaintypes -w wss://rpc.polkadot.io
```

Options:

* `-w, --wsUrl`: Fetch metadata from a WebSocket endpoint
* `-r, --wasm`: Fetch metadata from a runtime wasm file (.wasm)
* `-m, --medadata`: Fetch metadata from [a raw metadata file](https://github.com/paritytech/subxt?tab=readme-ov-file#downloading-metadata-from-a-substrate-node) (.scale)
* `-o, --output`: Folder to put generated files
* `-c, --chain`: Customize the chain name to generate, default to [`runtimeVersion.specName`](https://github.com/paritytech/polkadot-sdk/blob/002d9260f9a0f844f87eefd0abce8bd95aae351b/substrate/primitives/version/src/lib.rs#L165)
* `-d, --dts`: Generate `.d.ts` files instead of `.ts`, default: `true`&#x20;
* `-s, --subpath`: Using subpath for shared packages (e.g: `dedot/types` instead of `@dedot/types`), default: `true`

### `dedot typink`

Generate Types & APIs for an [ink!](https://use.ink/) smart contract given its metadata.

Usage:

```bash
npx dedot typink -m ./path/to/metadata.json # or metadata.contract
```

Options:

* `-m, --medadata`: Path to contract metadata file (`.json`, `.contract`)
* `-o, --output`: Folder to put generated files
* `-c, --contract`: Custom contract name, default is contract name from metadata
* `-d, --dts`: Generate `.d.ts` files instead of `.ts`, default: `true`&#x20;
* `-s, --subpath`: Using subpath for shared packages (e.g: `dedot/types` instead of `@dedot/types`), default: `true`

