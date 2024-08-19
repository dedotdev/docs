---
description: Install Dedot to your project and start connecting to blockchain networks
---

# Installation

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

Each Substrate-based blockchain has their own set of Data Types & APIs to interact with, so being aware of those Types & APIs when working with a blockchain will greatly improve the overall development experience. Dedot exposes TypeScript's Types & APIs for each individual Substrate-based blockchain, we recommend using TypeScript for your project to have the best experience.

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

Make sure to install `@dedot/chaintypes` as a `dev dependency.`

## Next

After setting up your project & installing all the necessary packages, let's start connecting the blockchain network and have some fun!
