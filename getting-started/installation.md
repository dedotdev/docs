# Installation

Let's install Dedot to your project and start connecting to blockchain networks

### Install `dedot` package

Dedot provides an umbrella package named `dedot`, it's the only package you need to install to access to most of the primitives & APIs when you're working with Dedot.

{% tabs %}
{% tab title="npm" %}
```sh
npm i dedot
```
{% endtab %}

{% tab title="yarn" %}
```sh
yarn add dedot
```
{% endtab %}

{% tab title="pnpm" %}
```sh
pnpm add dedot
```
{% endtab %}
{% endtabs %}

### Enable auto-completion/IntelliSense

Each Substrate-based blockchain has their own set of Data Types & APIs to interact with, so being aware of those Types & APIs when working with a blockchain will greatly improve the overall development experience. Dedot exposes TypeScript's Types & APIs for each individual Substrate-based blockchain via [`ChainApi`](https://github.com/dedotdev/chaintypes/blob/main/packages/chaintypes/src/index.ts) interfaces, we recommend using TypeScript for your project to have the best experience.

Types & APIs for each known Substrate-based blockchains are defined in package [`@dedot/chaintypes`](https://github.com/dedotdev/chaintypes)

{% tabs %}
{% tab title="npm" %}
```sh
npm i -D @dedot/chaintypes
```
{% endtab %}

{% tab title="yarn" %}
```sh
yarn add -D @dedot/chaintypes
```
{% endtab %}

{% tab title="pnpm" %}
```sh
pnpm add -D @dedot/chaintypes
```
{% endtab %}
{% endtabs %}

Make sure to install `@dedot/chaintypes` as a `dev dependency`.

{% hint style="info" %}
If you don't find the `ChainApi` for the network that you're working with in [the list](https://github.com/dedotdev/chaintypes/blob/main/packages/chaintypes/src/index.ts), you generate the `ChainApi` (Types & APIs) for it using [`dedot` cli](../cli.md).

```sh
# Generate ChainApi interface for Polkadot network via rpc endpoint: wss://rpc.polkadot.io
npx dedot chaintypes -w wss://rpc.polkadot.io
```



Or open a [pull request](https://github.com/dedotdev/chaintypes/pulls) to add your favorite Substrate-based network to the `@dedot/chaintypes` repo.
{% endhint %}

## Next

After setting up your project & installing all the necessary packages, let's start connecting the blockchain network and have some fun!
