---
cover: .gitbook/assets/Dedot_Baner_final.png
coverY: 0
layout:
  cover:
    visible: true
    size: full
  title:
    visible: true
  description:
    visible: false
  tableOfContents:
    visible: true
  outline:
    visible: true
  pagination:
    visible: true
---

# Welcome to Dedot

Dedot is the next-generation JavaScript client for [Polkadot](https://polkadot.com) and [Substrate-based](https://substrate.io/) blockchains.&#x20;

Designed to elevate the dapp development experience, Dedot is built & optimized to be lightweight and tree-shakable, offering precise Types & APIs suggestions for individual Substrate-based blockchains and ink! Smart Contracts. Dedot also helps dapps efficiently connect to multiple chains simultaneously as we head toward a seamless multi-chain future.

### Features

* Small bundle size, tree-shakable (no more `bn.js` or wasm blob)
* Fully-typed APIs for [on-chain interactions](client-api/) & [ink! smart contract](ink-smart-contracts/)&#x20;
* Build on top of both the [new](https://paritytech.github.io/json-rpc-interface-spec/introduction.html) & [legacy](https://github.com/w3f/PSPs/blob/master/PSPs/drafts/psp-6.md) (\*deprecated soon) JSON-RPC APIs
* Support light clients (e.g: [smoldot](https://github.com/smol-dot/smoldot))
* Using native TypeScript type system for [scale-codec](https://docs.substrate.io/reference/scale-codec/)
* Compatible with [`@polkadot/extension`](https://github.com/polkadot-js/extension)-based wallets (SubWallet, Talisman...)
* Fully-typed low-level JSON-RPC client for advanced usage
* Similar API-style with `@polkadot/api`, easy and fast migration
* Support Metadata V14, V15 (latest)
* Metadata optimization (caching, compact mode 🔜)
* ... and a lot more 🔜

### How to get started

<table data-view="cards"><thead><tr><th></th><th></th><th data-hidden data-card-cover data-type="files"></th><th data-hidden></th><th data-hidden data-card-target data-type="content-ref"></th></tr></thead><tbody><tr><td><strong>Installation</strong></td><td>Install <code>dedot</code> packages &#x26; setup your projects</td><td></td><td></td><td><a href="getting-started/installation.md">installation.md</a></td></tr><tr><td><strong>Connecting to network</strong></td><td>Connect &#x26; interact with networks</td><td></td><td></td><td><a href="getting-started/connect-to-network.md">connect-to-network.md</a></td></tr><tr><td><strong>Clients &#x26; Providers</strong></td><td>Learn more about Clients &#x26; Providers API</td><td></td><td></td><td><a href="clients-and-providers/">clients-and-providers</a></td></tr><tr><td><strong>ink! Smart Contracts</strong></td><td>Deploy &#x26; tnteract with ink! smart contracts</td><td></td><td></td><td><a href="ink-smart-contracts/">ink-smart-contracts</a></td></tr><tr><td><strong>Runtime upgrades</strong></td><td>Prepare your dapps for the next runtime upgrades</td><td></td><td></td><td><a href="runtime-upgrades.md">runtime-upgrades.md</a></td></tr><tr><td><strong>Utilities</strong></td><td>Utility functions to work with hex, hash, Uint8Array...</td><td></td><td></td><td><a href="utilities/">utilities</a></td></tr></tbody></table>

### Join the community

* Join [Dedot Telegram](https://t.me/JoinDedot) to get supports and project updates
* Follow the creator of Dedot - [@realsinzii](https://x.com/realsinzii) on X
* Github repository: [dedotdev/dedot](https://github.com/dedotdev/dedot)

### Acknowledment

Dedot takes a lot of inspirations from project [`@polkadot/api`](https://github.com/polkadot-js/api). A big thank to all the maintainers/contributors of this awesome library.

Funded by [Web3 Foundation Grants Program](https://grants.web3.foundation/).
