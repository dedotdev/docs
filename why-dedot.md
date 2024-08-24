# Why Dedot?

The idea of building Dedot was conceived & motivated from our frustration of building Dapps in the Polkadot ecosystem using `@polkadot/api` (pjs). `@polkadot/api` is a good library and was built by the great minds behind it. But over the years, its limitations haven’t been addressed yet, and this hinders developers from creating optimal Dapps for the Polkadot ecosystem and makes it really hard to onboard new developers to the ecosystem.

{% hint style="info" %}
Please refer to the [original post](https://forum.polkadot.network/t/introducing-dedot-a-delightful-javascript-client-for-polkadot-substrate-based-blockchains/8956) to introduce Dedot on Polkadot forum for more detailed information on why we build Dedot.
{% endhint %}

### Limitations of [Polkadot.js API](https://github.com/polkadot-js/api) (@polkadot/api)

* Large bundle-size (`wasm` & `bn.js` & unused type defs)
* High memory consumption
* Limitations in Types & APIs suggestions for individual chains

### Dedot comes to address those issues

* Small bundle-size and tree-shakable
* Consume Less memory
* Types & APIs suggestion/auto-complete for individual Substrate-based chains

### Dedot also comes with more & more features

* Native TypeScript type system for scale-codec
* Adopted latest changes in metadata V14 & V15
* Build on top of new JSON-RPC specs (and the legacy as well but deprecated soon)
* Support light clients (smoldot)
* Typed APIs ink! smart contracts
* Fully typed low level JSON-RPC client
* A builtin mechanism to cache metadata
* Compact Metadata 🔜
* XCM utilities 🔜
* React Native supports 🔜

... and a lot more toolings around Dedot to come.

