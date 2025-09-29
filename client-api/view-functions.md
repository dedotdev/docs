# View Functions

View functions are new features available for chains using Runtime Metaada V16. It's available as part of the `ChainApi` interface through syntax: `client.view.<pallet>.<methodName>` similar to [runtime apis](runtime-apis.md).&#x20;

```typescript
const provider = new WsProvider('wss://westend-rpc.polkadot.io');
const dedotClient = await DedotClient.create<WestendApi>(provider);

const shouldBeTrue: boolean = await client.view.proxy.isSuperset('Any', 'Governance');
```
