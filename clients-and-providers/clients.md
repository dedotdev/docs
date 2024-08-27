# Clients

Dedot exposes 3 high-level clients offering powerful API that abstracting over the complexities of on-making chain interactions (queries, transactions...) or sending raw JSON-RPC request to a blockchain nodes.

* `DedotClient`: High-level Substrate client built on top of the [new JSON-RPC API](https://paritytech.github.io/json-rpc-interface-spec/introduction.html)
* `LegacyClient`: High-level Substrate client built on top of the [legacy JSON-RPC API](https://github.com/w3f/PSPs/blob/master/PSPs/drafts/psp-6.md)
* `JsonRpcClient`: High-level JSON-RPC client to help making raw JSON-RPC requests or subscriptions seamlessly. Both `DedotClient` and `LegacyClient` are also a `JsonRpcClient`.

Both `DedotClient` and `LegacyClient` are both implementing [`ISubstrateClient<ChainApi, ApiEvent>`](https://github.com/dedotdev/dedot/blob/f7910058d5e379f3f51476e10696a6f157f08591/packages/api/src/types.ts#L120) interface, so they would shared a list of [common APIs](https://github.com/dedotdev/dedot/blob/f7910058d5e379f3f51476e10696a6f157f08591/packages/api/src/types.ts#L99-L144)  of a client to interact with Subtrated-based blockchains.

### DedotClient&#x20;

`TODO`

### LegacyClient

`TODO`

### JsonRpcClient

`TODO`

***

### Connection Status

`TODO`

### Client Events

`TODO`

### ApiOptions

`TODO`

### JsonRpcClientOptions

`TODO`



