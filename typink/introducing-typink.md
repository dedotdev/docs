---
description: Typesafe react hooks to interact with ink! smart contracts powered by Dedot!
---

# Introducing Typink

> WIP  🚧

## Overview

The **ink! smart contract language** is an essential component of the **Polkadot ecosystem**, enabling developers to build decentralized applications (dApps) using **Rust-based smart contracts**. As the ecosystem evolves, **ink!** is now maintained by a dedicated community and continues to see promising advancements.

However, while **ink!** is powerful, integrating with its smart contracts remains a challenge for developers. Many struggle with the **lack of a fully type-safe API for contract interactions**, making development **prone to errors** and **less efficient**. Furthermore, **contract event handling** is often cumbersome, and existing tooling does not provide optimal solutions for **wallet authentication** and **multi-chain compatibility**.

To address these challenges, we introduce **Typink**.

## Limitations of current toolings

While the ink! ecosystem has evolved significantly, developing decentralized applications (dApps) with ink! smart contracts still presents several challenges. Existing tooling has made great strides in simplifying contract interactions, but there are areas where improvements can further enhance the developer experience.

**Limited Type Safety**

Many solutions provide utilities for interacting with smart contracts, but **fully type-safe interactions at the contract message and event level are still lacking**. Without strict type enforcement, developers often encounter runtime errors and unexpected behaviors, making debugging and maintenance more complex.

**Rigid Wallet Connector Integration**

Current approaches often **couple wallet authentication tightly with client connections**, making it difficult to swap or integrate different wallet providers seamlessly. Developers looking to support multiple authentication methods or custom wallet solutions may find this restrictive, requiring workarounds to achieve the desired flexibility.

**Inefficient Contract Event Handling**

Listening to contract events efficiently is crucial for real-time dApp interactions. However, **existing implementations may not provide a structured, type-safe approach to event subscriptions**, leading to inconsistent handling patterns and increased development effort.

**Overhead from Code Generation**

Some solutions rely on **code generation techniques to enforce type safety**, which, while beneficial, can lead to **increased bundle sizes and additional complexity**. This approach may introduce unnecessary overhead, particularly for lightweight applications that prioritize performance and minimal dependencies.

Addressing these challenges requires a modernized approach—one that **prioritizes type safety, flexibility, and lightweight integration** while ensuring that contract interactions are efficient, scalable, and easy to maintain. This is where **Typink** comes in.

## **Introducing Typink!**

**Typink** is a **fully type-safe React hooks library** designed to **accelerate ink! dApp development** and **seamlessly integrate smart contracts**. Built on top of **Dedot’s powerful type system**, Typink ensures **reliable, efficient, and scalable contract interactions**.

**Key Features**

* **Fully Type-Safe React Hooks**: Provides complete **type safety** for **contract messages and event handling**.
* **Decoupled Wallet Integration**: Supports **external wallet solutions** e.g: **SubConnect** or **Talisman Connect**.
* **Provided a built-in CLI & Boilerplate Project**: Simplifies onboarding, setting up projects and speeds up development.
* **Multi-chain supports with lazy initialization**: Connects to multiple chains **only when needed**, reducing resource consumption.

Let's dive into the details for each feature

## Features

### **Fully Type-Safe React Hooks**

### Choose your favorite wallet connector

### Built-in CLI to start new boilerplate project

### Multi-chain supports with lazy initialization

## Conclusion

Typink is a game-changer for ink! developers, providing a fully type-safe, efficient, and developer-friendly solution for integrating smart contracts into React dApps. Unlike existing tools, it prioritizes type safety, flexibility, and performance, enabling faster development and fewer errors.

**Why Choose Typink?**

🚀 Fully Type-Safe API

🔌 Flexible Wallet Integration

🛠️ Built-in CLI & Boilerplate for Quick Setup

⚡ Optimized for Multi-Chain Interactions

With Typink, developing ink! dApps has never been easier. Get started today and build the next-generation Polkadot applications with confidence!

