# Plugems: Next-Gen dApps for the World Computer


This is a proposal, following the principles laid out in [The Unicorn article](https://github.com/one-unicorn/theunicorn/blob/master/The_Ethereum_Unicorn.md).
It also further explores solutions to problems presented in [I Have Gambled and Lost Devcon5](https://medium.com/@loredana.cirstea/i-have-gambled-and-lost-devcon5-meditations-on-why-i-am-here-f174ae946aa2).

## Definitions

We define the **plugem** (the "m" signifies "multi") as the type of plugin that "plugs" into multiple other components rather than into the plugin manager alone. **Particularly, a plugem will discover other plugems in the execution environment, create the proper interfaces for them, and announce its presence to the other plugems.**

## Claim

We claim that the plugem software pattern will become the dominant form of product delivery in the not so distant future. At least for the Turing-complete blockchain tech.

We further claim that:
- dApps will become plugems for wallets.
- dApps will be developed in IDEs that will accept plugems themselves
- dApps will depend upon other dApps in a symbiotic or embedded manner

## Rationale

### Blockchain as OS

A Turing-complete blockchain exposes data and behavior to any new element/contract that is created within. In a way, it provides the proto-environment for a plugem interaction between elements/contracts. We can expect that layer 2 and the dApp layer (both use layer 1), will also further express this pattern of co-operation.

#### For Ethereum

Contracts can cooperate either through **embedding** or **symbiosis**. Embedding is done through contract inheritance and library linking through bytecode inclusion. Symbiosis is done through communicating with other contracts and libraries that live at separate addresses but in the same global blockchain scope.

If we use **embedding**, the main contract can have access to the inherited functions or library functions from its own scope.

If we use **symbiosis**, the main contract can use the external API of other contracts and libraries through the blockchain's matrix. In Solidity, symbiosis is achieved through `call`, `delegatecall`, `staticcall`.

Given a pure or view `ContractXConstantFunc` function from a `ContractX` contract , a `ContractY` contract can use `ContractXConstantFunc` in his `ContractYFunc` through `address(ContractX).staticcall(bytes memory <encodedData>)` or through `ContractX.ContractXConstantFunc(<decodedData>)` (if `ContractY` knows `ContractX`'s interface).

Given a payable or non-payable `ContractXFunc`, the same pattern applies, with the exception that `call` is used in place of `staticcall`.

`staticcall` and `call` will use `ContractX`'s data context for processing `ContractXConstantFunc` and `ContractXFunc`.

By using `delegatecall`, the logic from `ContractXConstantFunc` or from a non-payable `ContractXFunc` is used, but in the data context of `ContractY`.

At the EVM level, this communication is achieved by having a standardized way of producing and consuming bytecode. Bytecode is stored on-chain and consumed by clients (Ethereum node software, dApps, etc.).

### For dApps

The same can be achieved in layers built on top of Turing-complete blockchains. **A component/dApp can embed or live in symbiosis with other components/dApps, as long as they can produce a standardized description of their interface.** Layer 2 & dApps actual implementations live outside the chain, but the root of the software is stored on-chain. Therefore, these component interfaces will have an on-chain root. dApps should truly be plugems.

**Plugems will have a manifest describing their interface** (functions, types, events, etc.), **along with their dependencies and plugems that they can communicate with.** Such a manifest will be registered in a Plugem Registry contract - either entirely, or in an encoded format, or by referencing another decentralized storage system, that can provide content checksums.

The purpose is to provide an environment with the following properties:
- interoperability
- data availability
- deterministic data retrievability
- non-breaking functionality

Non-breaking functionality can be achieved by deploying new plugem versions without removing old ones, which can still be in use.

A global removal process can be done by counting the number of plugems that depend on a certain plugem. Wallets can also provide anonymous information about how many times the plugem is accessed and notify users if a plugem they use is going to be deprecated from the global scope. Users can do the effort to maintain the plugem themselves in this case (storage & network requirements).

### The Next-Gen IDE

**In a more detailed document, we proposed these [IDE architecture ideas](https://github.com/one-unicorn/theunicorn/blob/master/proposals/dApp_Assembly.md), along with a description of[ next-gen dApps](https://github.com/one-unicorn/theunicorn/blob/master/proposals/Next_Gen_dApps.md).**

We envision an IDE that accepts plugems much similar to the present behavior of [Remix](https://remix.ethereum.org/). In Remix the plugins can collaborate with each other and, some even check for the instantiation of helper plugins in the Development Environment. Such plugins fully deserve the name of plugems.

**Imagine this process of dApp development:**
1. Choose the necessary IDE plugems, that help you create components
1. Create the components inside each plugem
1. Assemble the components into a deliverable/dApp. Maybe use a [dApp Assembly](https://github.com/one-unicorn/theunicorn/blob/master/proposals/dApp_Assembly.md) plugem for this purpose
1. Save the work in a decentralized delivery system
1. Users will be able to discover, install, and use the dApp as a plugem in their wallets

### Wallet as Plugem Manager

**In a more detailed document, we proposed these [wallet architecture ideas](https://github.com/one-unicorn/theunicorn/blob/master/proposals/Wallet_Proposals.Tentative_EIPs_and_dTypes.md).**

Wallets will provide the user with a dApp ecosystem. dApps can be **active** in the background or **be activated** only when the user runs them.

When a new plugem is installed, it will check what other plugems can collaborate with it, from the ones that are already installed, and instantiate an interface with each one. The wallet will also announce other active plugems that know how to interface with this plugem, so they can instantiate their interface.


### The Human Being as a Plugem

Considering the Whole Humanity as a Meta-Entity, we can envision it also as a software application. Human beings are free to interface inside the community, collaborate among themselves. They are expected to respect each other's rights and liberties.

**In this sense: Humanity as a whole is a Plugin Manager and each human being is a plugem.**

A **software plugin** interfaces only with the mother application. That's not how humans work.

A **software application** interfaces with nothing/little else - like a hermit, but we can't all be hermits.

A **software library** does not have output unless used in something else. It is similar to the subset of DNA genes which encode molecules needed for a specific function in the cells of an organ. Those molecules are created/instantiated when needed by the parent process.

**Databases** are impotent without verbs - they are not acting, they are just repositories of memory. The blockchain by itself is a database at core and it is not a compatible metaphor for a whole human being. Rather only for the memorization function of the brain.

**What we see in humans is that they all share the same data structures, the same types and the interfaces are the same.** This is why humans can fix themselves and can exchange parts of themselves, to extend their life. Their organs work under the same principles. Cells contain the same type of molecules and their membrane channels work with the same type of ions. Their DNA is composed of the same building blocks and follow the same composition principles. There are variations in the ordering and combination of these building blocks, but the same principles apply everywhere.

**In society, the function of a human being can tightly depend on functions of other human beings** (a surgeon needs an anesthesiologist), in the same way, one plugem could be dependent on other plugems for its function.

**There are cases where a human being does not depend so much on others.** Subsistence agriculture produces enough to live and maybe share with the local community, **but such a human does not contribute much to Humanity itself**.

**A very well oiled interaction between humans will lead to the evolution and efficiency of the whole of humanity. We want to create the premises of such a mechanism in software.**

### Reasons for Not Converging to Plugems

There are reasons why this convergence on a plugem-like system may not happen:

- decentralization by reflex rather than by reason
- fragmentation of intent
- inability to cooperate (lack of a good medium, disagreement, bikeshedding)
- lack of respect for other projects, developers
- profit reasons that lead to shortcuts in development in the detriment of common good

But we think developers are getting wiser with time and we think the reasons for converging on plugems are becoming apparent day by day and will gain dominance.

## Consequences

### Creation-Consumption Symmetry

We envision that every IDE plugem developer will also create a wallet plugem able to consume any component created by the said IDE plugem.

### Wallets as Meta-applications

Wallets will change a lot from the present incarnation. We are not even sure if the word "wallet" still fits. But it very well may, in the sense that it will contain a lot of private information and information derived from private information.

It will be paramount for wallets to keep this private information safe and fully under the control of the user.

### Rich and Strong Interactions Between Developers

Interactions based on the common good. Clear definitions of common values, visions, interfaces.

**We want to see proofs of good intent:**

- Data that you have the option to keep for yourself, we want to see it shared, to be used by other dApps (than yours).
When you have new data to add to the data that was shared with you, share it with the same license and notify the originator.
- When you have an idea for a better standard or dApp than an existing one, first go to the developers of the original one and propose improvements, instead of creating a new dApp, just slightly different than the old one.
- When you develop new standards, do it transparently and invite other interested parties to contribute.
- Prefer to use and improve common good, open-source projects instead of for-profit or closed source/data projects.
- If you are a developer, contribute with your own proposal of best practices for collaboration and an upgraded moral standard. **You can start by opening issues [here](https://github.com/one-unicorn/theunicorn/issues) and we can collaborate on [a best practices document for next-gen devs](../principles/Next_Gen_Devs.md).**

**Let us learn how to program plugems by behaving like plugems among ourselves.**

## Conclusion

We created a repository for exploring the theory, implementation, and improvements of plugems, dApps, and Wallets on GitHub [UIPs](https://github.com/one-unicorn/UIPs). We expect Ethereum and other Turing-complete blockchain developers and visionaries to contribute.

Ideas from this will be forwarded to the corresponding Improvement Proposals for each chain - e.g. [EIPs](https://github.com/ethereum/EIPs), [ICS](https://github.com/cosmos/ics), [w3f](https://github.com/w3f/Web3-collaboration), [HIPs](https://github.com/hashgraph/hip), etc. repos.
