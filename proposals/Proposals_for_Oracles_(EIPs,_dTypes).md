# Proposals for Oracles (EIPs, dTypes)

As per our umbrella proposal for a tighter integration of the Ethereum projects ([The Unicorn](https://github.com/pipeos-one/theunicorn/blob/master/The_Ethereum_Unicorn.md)), we will make in the following some proposals for the "oracle" pattern.

If Ethereum is the World Computer, the oracles are the mechanism for data transport from web2 memory to web3 memory. Such mechanism has to have some ways of increasing trust in the resulted data since the chain data is intended to be less mutable and is used by smart contracts that process real value.

## Cache for Oracles

We have started with a Eth 2.0 proposal for a [Cache/Master Shard](https://ethresear.ch/t/a-master-shard-to-account-for-ethereum-2-0-global-scope/5730/7) that will store the most inter-shard communicated data in a form compatible with [dType](http://eips.ethereum.org/EIPS/eip-1900) and [Alias](http://eips.ethereum.org/EIPS/eip-2193). Such a cache should also bring significant gas savings to Eth 1.x. The oracles are the location where data from external sources could be determined to be worth storing on-chain (or not).



## Oracle Cooperation

dType stores should be a common digital good for the oracles. We hope that each oracle will benefit from the cache of other oracles and doing such avoid duplication of data and gas.


This would also allow Oracles to focus on complementary, trustworthy data sources. In other words, choose different dTypes to provide data for. In this case, the range information covered by all Oracles will be bigger, benefiting developers and users.

## Cache Mechanism

### Overview

A simplified view of the Oracle on-chain mechanism is:
- user/contract sends a transaction to a contract that requires Oracle data - `ClientSmartContract` **(1)**
- the `ClientSmartContract` calls the `OracleContract` and requests data **(1)**
- the `OracleContract` emits an on-chain event, notifying the `OracleClient` that data is needed **(1)**
- the `OracleClient` sends the data (after acquiring it off-chain) to the `OracleContract` **(2)**
- the `OracleContract` passes the information to the `ClientSmartContract` **(2)**
- the `ClientSmartContract` is prepared to receive the Oracle data and finalize the user's request. **(2)**

There are two on-chain transactions are needed to process the user's request: (1) and (2)

dType can provide caching for Oracles, that would reduce the process to only one transaction, when the needed data has been frequently queried and its value does not change frequently, in time.

### Specification

With dType, data stored on-chain is structured based on type. Each type has a unique identifier and storage contracts for data items. Alias provides the link between the data item and an Oracle identifier for that data. Human readable identifiers for the data item can also be additionally added.

The Oracle identifier will be computed through the same standard across Oracle implementations and will have the following form: `keccak256(<Inputs for Data Harvesting>)`. This will be the `aliasIdentifier` used to identify the data.

The `OracleContract` will first check if the `aliasIdentifier` leads to a valid (not `NULL`) data item. If data is already on-chain, then it is returned to the `ClientSmartContract` in the same transaction.

Data is harvested off-chain only if it does not exist on-chain.

A standard will be needed to uniquely determine Oracle data, across Oracle implementation - specifically, `Inputs for Data Harvesting`. This is an attempt to create such a standard.

## List of Hopeful Collaborators

- https://chain.link/
- http://provable.xyz/
- https://www.town-crier.org/
- https://github.com/pipeos-one/dType

## Conclusion

From the above proposals multiple EIPs could be derived and authored together with the interested parties.
