# @polkadot/api -> dedot

`dedot` is inspired by `@polkadot/api`, so both are sharing some common patterns and api styling (eg: api syntax `api.<type>.<module>.<section>`). Although we have experimented some other different api stylings but to our findings and development experience, we find that the api style of `@polkadot/api` is very intuiative and easy to use. We decide the use a similar api styling with `@polkadot/api`, this also helps the migration from `@polkadot/api` to `dedot` easier & faster.

While the api style are similar, but there're also some differences you might need to be aware of when switching to use `dedot`.

### Initialize clients

`@polkadot/api`

```typescript
import { ApiPromise, WsProvider } from '@polkadot/api';

const provider = new WsProvider('wss://rpc.polkadot.io');
const client = await ApiPromise.create({ provider });
```

`dedot`

```typescript
import { DedotClient, WsProvider } from 'dedot';
import type { PolkadotApi } from '@dedot/chaintypes';

const provider = new WsProvider('wss://rpc.polkadot.io');
const client = await DedotClient.new<PolkadotApi>(provider); // or DedotClient.create(...) if you prefer

// OR if you want to put additional api options when initialize the client
const client = await DedotClient.new<PolkadotApi>({ provider, cacheMetadata: true });
```

{% hint style="info" %}
1. `dedot` only supports provider can make subscription request (e.g: via WebSocket or light client).&#x20;
2. We recommend specifying the `ChainApi` interface (e.g: [`PolkadotApi`](https://github.com/dedotdev/chaintypes/blob/main/packages/chaintypes/src/polkadot/index.d.ts) in the example above) of the chain that you want to interact with. This enable apis & types suggestion/autocompletion for that particular chain (via IntelliSense). If you don't specify a`ChainApi` interface, the default [`SubstrateApi`](https://github.com/dedotdev/dedot/blob/a762faf8f6af40d3e4ef163bd538b270a5ca31e8/packages/chaintypes/src/substrate/index.d.ts) interface will be used.
3. `WsProvider` from `dedot` and `@polkadot/api` are different, they cannot be used interchangeable.
{% endhint %}

### Type system

Unlike `@polkadot/api` where data are wrapped inside a [codec types](https://polkadot.js.org/docs/api/start/types.basics), so we always need to unwrap the data before using it (e.g: via `.unwrap()`, `.toNumber()`, `.toString()`, `.toJSON()` ...). `dedot` leverages the native TypeScript type system to represent scale-codec types, so you can use the data directly without extra handling/unwrapping.

Example 1:

```typescript
const runtimeVersion = client.consts.system.version;

// @polkadot/api
const specName: string = runtimeVersion.toJSON().specName; // OR runtimeVersion.specName.toString()

// dedot
const specName: string = runtimeVersion.specName;
```

Example 2:

```typescript
const balance = await client.query.system.account(<address>);

// @polkadot/api
const freeBalance: bigint = balance.data.free.toBigInt();

// dedot
const freeBalance: bigint = balance.data.free;
```

Example 3:

```typescript
// @polkadot/api
const proposalBondMaximum: bigint | undefined = client.consts.treasury.proposalBondMaximum.unwrapOr(undefined)?.toBigInt();

// dedot
const proposalBondMaximum: bigint | undefined = client.consts.treasury.proposalBondMaximum;
```
