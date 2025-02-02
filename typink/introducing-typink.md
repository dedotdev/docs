---
description: Typesafe react hooks to interact with ink! smart contracts powered by Dedot!
---

# Introducing Typink

## Overview

The **ink! smart contract language** is an essential component of the **Polkadot ecosystem**, enabling developers to build decentralized applications (dApps) using **Rust-based smart contracts**. As the ecosystem evolves, **ink!** is now maintained by a dedicated community and continues to see promising advancements.

However, while **ink!** is powerful, integrating with its smart contracts remains a challenge for developers. Many struggle with the **lack of a fully type-safe API for contract interactions**, making development **prone to errors** and **less efficient**. Furthermore, **contract event handling** is often cumbersome, and existing tooling does not provide optimal solutions for **wallet authentication** and **multi-chain compatibility**.

To address these challenges, we introduce **Typink**.

## Limitations of current toolings

While the ink! ecosystem has evolved significantly, developing decentralized applications (dApps) with ink! smart contracts still presents several challenges.&#x20;

Existing tooling has made great strides in simplifying contract interactions, but there are areas where improvements can further enhance the developer experience:

1. **Lack of TypeSafe API for contract interactions** – Many utilities support contract interactions, but strict type enforcement at the message and event level is missing, leading to runtime errors and complex debugging.
2. **Inflexible Wallet Connectors Integration** – Current solutions tightly couple wallet authentication with client connections, limiting flexibility for multiple authentication methods or custom wallets.
3. **Inefficient Event Handling** – Existing implementations lack a structured, type-safe approach to contract event subscriptions, causing inconsistent handling and added development effort.
4. **Code Generation Overhead** – While useful for type safety, code generation increases bundle size and complexity, which may be excessive for lightweight applications.

Addressing these challenges requires a modernized approach—one that **prioritizes type safety, flexibility, and lightweight integration** while ensuring that contract interactions are efficient, scalable, and easy to maintain. This is where **Typink** comes in.

## **Introducing Typink!**

**Typink** is a **fully type-safe React hooks library** designed to **accelerate ink! dApp development** and **seamlessly integrate smart contracts**. Built on top of **Dedot’s powerful type system**, Typink ensures **reliable, efficient, and scalable contract interactions**.

**Key Features**

* **Fully Type-Safe React Hooks**: Provides complete **type safety** for **contract messages and event handling**.
* **Decoupled Wallet Connector Integration**: Supports **external wallet solutions** like [**SubConnect**](https://github.com/Koniverse/SubConnect-v2)**,** [**Talisman Connect**](https://github.com/TalismanSociety/talisman-connect) or built your own wallet connector with provided utilities.
* **Built-in CLI to start new project**: Simplifies onboarding, setting up projects and speeds up development.
* **Multi-chain supports with lazy initialization**: Connects to multiple chains **only when needed**, reducing resource consumption.

### Getting started now!

We've put together a quick tutorial that guide you through the process of building a simple ink! dApp using Typink, dive in right now to see the differences!

* Tutorial: [Develop ink! dApp using Typink](https://docs.dedot.dev/help-and-faq/tutorials/develop-ink-dapp-using-typink)
* Github Repo: [psp22-transfer](https://github.com/sinzii/psp22-transfer)

## Conclusion

Typink is a game-changer for ink! developers, providing a fully type-safe, efficient, and developer-friendly solution for integrating smart contracts into your ink! dApps. Unlike existing tools, it prioritizes type safety, flexibility, and performance, enabling faster development and fewer errors.

With Typink, developing ink! dApps has never been easier. [Get started today](https://github.com/dedotdev/typink?tab=readme-ov-file#start-a-new-project-from-scratch) and build the next-generation Polkadot applications with confidence!

