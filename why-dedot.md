# Why Dedot?

The idea of building Dedot was conceived & motivated from our frustration of building Dapps in the Polkadot ecosystem using @polkadot/api (pjs). @polkadot/api is a good library and was built by the great minds behind it. But over the years, its limitations haven’t been addressed yet, and this hinders developers from creating optimal Dapps for the Polkadot ecosystem and makes it really hard to onboard new developers to the ecosystem.

### Limitations of Polkadot.js API (@polkadot/api)

1. Large bundle-size (`wasm` & `bn.js` & unused type defs)
2. High memory consumption
3. Limitations in Types & APIs suggestions for individual chains

### Dedot comes to address those issues

1. Small bundle-size and tree-shakable
2. Less memory consumption
3. Types & APIs suggestion/auto-complete for individual Substrate-based chains

### Dedot also comes with more & more features

1. Native TypeScript type system for scale-codec
2. Adopted latest changes in metadata V14 & V15
3. Build on top of new JSON-RPC specs (and the legacy as well but deprecated soon)
4. Support light clients (smoldot)
5. Typed Contract APIs
6. Fully typed low level JSON-RPC client
7. A builtin mechanism to cache metadata
8. Compact Metadata 🔜
9. XCM utilities 🔜
10. React Native supports 🔜

... and a lot more toolings around Dedot to come

