## Cross-chain deterministic deployment using CREATEX

### Introduction

While `CREATE2` was a welcome addition to the EVM that allowed you to deploy smart contracts to deterministic addresses, it was never a silver bullet for all use cases. Enter `CREATE3`.

With `CREATE2` you can deploy two smart contracts to two different chains on the same address only if they have the exact same bytecode, i.e. they are the same contract. This is because it uses creation bytecode as one of the inputs to the hash function that determines the address.
This is a problem if you want to deploy two different smart contracts to two different chains, but with the same address. Or even if you want to deploy the same contract to two different chains, but with different constructor params.

> ℹ️ **Note**:
> If you are not familiar with `CREATE2`, you can check out [this tutorial](./create2-tutorial.md).

While not an official part of the EVM, `CREATE3` is a powerful pattern built on top of `CREATE2` that makes it possible to deploy different contracts across different chains on the same address. This is made possible by cleverly making the main hash function independent of the creation bytecode.

In this tutorial, we will:

1. Learn about how the `CREATE3` pattern works.
2. Deploy our own `CREATE3` factory on two different chains from scratch.
3. Use the factory to deploy two different contracts on the same address on both chains.
4. Look at how to use one the latest implementations of `CREATE3` with novel front-running protections built in.

### Prerequisites

1. Some familiarity with Solidity, Foundry and `CREATE2` is required, and some familiarity with the inline assembly is recommended.
Refer to the [official Solidity docs](https://docs.soliditylang.org/en/latest/assembly.html) for a primer on inline assembly.
2. Make sure you have Foundry [installed](../getting-started/installation.md) on your system.
3. [Initialize](../projects/creating-a-new-project.md) a new Foundry project somewhere in your system.
