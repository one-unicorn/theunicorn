# Wallet Proposals. Tentative EIPs and dTypes

If Ethereum chain is the Singleton Computer, then the wallet is its device/driver manager, app manager, and a repository of temporary data. A careful consideration has to be taken for a clear, usable, flexible architecture, as early as possible.

Projects that lead to the compatibility of wallets and their function should be encouraged. Eg. https://walletconnect.org/.

As per [The Unicorn](../The_Ethereum_Unicorn.md), in the following, we are proposing additional functionality and data types.

## Wallet as dApp Manager

A dApp manager is not simply a start menu for dApps. It manages inter-dapp messaging, global profiles and history as well as the private keys.


For homogeneous & compatible wallet implementations, standards need to exist for:
- developers registering dApps (common registry)
- wallet-specific information requested by dApps
- dApp-specific information cached by wallets
- dApp-specific information broadcasted to other dApps

A wallet should provide a local server, so the dApps install locally, eliminating the dependency on web2. A wallet should not be a browser on web2 - they should create web2 internally (where possible).

dApps will have a dependency tree on other dApps. When installing a dApp, it will install all other dApps that it depends on. The Open dApp Registry should contain the dependency tree.

### RPC for Embedded dApps

dApp1 wants to use the result of dApp2, in order to process the user's request.
An RPC call with a payload will be sent dApp1 -> dApp2. dApp2 will open with a payload-based state. After the user finishes with dApp2, dApp2 will close and send a response to dApp1 with the outcome payload.


### Open dApp Registry (on chain)

Centralization and censorship need to be avoided. A solution that fits The Unicorn principles is an on-chain dApp Registry, where anyone can register their dApp.

Any wallet will be able to show all the registered dApps to their users. dApp ratings can initially be done by each wallet, as they please.

We propose dType as the base for creating the dApp Registry, which will be a dType type (e.g. `dAppRegistry`), with an attached storage contract.

Each `dAppRegistry` data instance needs to provide access to:
- dApp name
- dependency tree (see dApp Manager)
- reference to an on-chain or off-chain (Swarm, IPFS, etc.) resource with:
  - dApp icon
  - short description
  - URI

`dAppRegistry` structure TBD.


### Wallet-Specific Information

Wallets should be clients for user data. They should be able to provide dApps with data that would otherwise be hard to collect.

These methods need the user's permission.

#### Address Book Access

Returning address book items from a wallet.

JSON-RPC method: `wallet_getAddressBook(type)`
`type` can be:
- `0`: entire address book
- `1`: single user-made selection
- `2`: multiple user-made selection

Returned data type is a dType - e.g. `WalletAddressItem[]`

Proper structure to be determined:
```
struct WalletAddressItem {
    address itemAddress;
    string itemName;
}
```
`itemName` or other metadata can be optional.


## Owned Token Selection

Returning a list of tokens that the user owns.

JSON-RPC method: `wallet_getTokensList(type)`
`type` can be:
- `0`: entire token list
- `1`: single user-made selection
- `2`: multiple user-made selection

Returned data type is a dType - e.g. `WalletTokenItem[]`

TODO: (see token interface identifier EIPs)

```
struct WalletTokenItem {
    address tokenAddress;
    ? tokenInterface;
}
```

## Frequently Used Contract Selection

There are smart contracts that the user frequently interacts with. There should be an easy way to access their Ethereum address and send it as input to dApps, if needed.

JSON-RPC method: `wallet_getContractList(type)`
`type` can be:
- `0`: entire contract list
- `1`: single user-made selection
- `2`: multiple user-made selection

Returned data type is a dType - e.g. `WalletContractItem[]`

```
struct WalletContractItem {
    address contractAddress;
    bytes32 contractName;
    bytes32 dtypeIdentifier;
}
```

If the contract is a dType-registered storage contract, the `dtypeIdentifier` is needed and it will provide means to determine the contract's ABI.


### Wallet Notifications

- wallet -> user
- wallet -> dApp
- dApp -> wallet

Wallets should be clients for user data across dApps. This requires each dApp to have a unique identifier (see dApp Registry) and be able to listen for messages from other dApps on certain topics.

Source of notifications can be: on-chain events, dApps, wallet.


### Homogeneous History Across Dapps

Wallets should be able to cache meaningful dApp interaction history (user input and dApp output). The user has total control over this data and can choose to sync it across wallets.

Standard for interaction items to be determined.


### Wallet as Accumulator of Data and Profiles


### Data Sync Between Wallets

### Data Transfer Across Daps

- by QR
- by URL


## dTypes for Sensor Data

- GPS: GeoPopint = uint32 latitude, uint32 longitude
- NFC
- compass
- QR entry
- motion (activity detection)
- internal clock
