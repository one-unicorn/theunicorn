# Next Generation dApps

## 2 Runtime Enviroments

One is the Mobile Wallet. The dApp is supplied with a web server, a web browser, a secure database persisstence layer, web3, access to wallet's services as further discribed in (Wallet_Proposals.Tentative_EIPs_and_dTypes.md)

The second one is a Desktop web browser with a wallet plugin. In this case the dApp is supplied only with web3 and other wallet services.

We envision a transition from the second setup to the first even for Desktop, this being the basis for the wallets to guarantee a secure DB connection and a homogenous behaviour of all dApps across devices. Therefore: we recommend creators of wallet web browser plugins to migrate to wallet as a stand-alone application that fully controls its internet requests, embeds a browser, a web server, and a wallet.


### Security for Each Environment

For the Mobile Wallet: the first layer of security of the runtime code of the app is handled by the AppStore or GooglePlay. The private keys are held into a secure enclave. That is why we further recommend that the keys never leave the enclave: desktop transactions are approved from the mobile (like Opera Touch pioneered).

For Desktop Wallet: Public signatures for the integrity of the wallet should be produced at the beginning of a wallet session and approved by the mobile wallet by a comparison to a private key, wallet and version specific. The desktop wallet application should be kept as secure as the mobile version and provide further the security cover to the dApp data transmission.

Therefore the security authority is concentrated into the Mobile Secure Enclave.



## Characteristics of the Next Gen dApps

- target a single purpose, even though they might use multiple other dApps within that purpose
- interoperable with other dApps through a permissioned communication system
- content is stored on a decentralized storage system
- dApp can be registered in a globally available registry, at creation time
- persistence is of two types:
  - public data - stored on-chain or in a decentralized storage system
  - private data - user maintains ownership of that data, encrypted storage, saved on the user's device, in the wallet

## Components of the Next Gen dApps

- data inputs/outputs
- functions: Javascript and on-chain EVM functions
- dtypes (with UI)
- processes/metafunctions will have UI components (e.g. Pipeline graphs)
- permissions
- dependency dApps


For homogeneous & compatible wallet implementations, standards need to exist for:
- developers registering dApps (common registry)
- wallet-specific information requested by dApps
- dApp-specific information cached by wallets
- dApp-specific information broadcasted to other dApps

A wallet should provide a local server, so the dApps install locally, eliminating the dependency on web2. A wallet should not be a browser on web2 - they should create web2 internally (where possible).

dApps will have a dependency tree on other dApps. When installing a dApp, it will install all other dApps that it depends on. The Open dApp Registry should contain the dependency tree.

## Creation

The next gen dApps should be created in IDEs that have collaborating plugins as described in more detail in [dApp Assembly](dApp_Assembly.md).

## Services

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


### Data Transfer Across dApps, Across Users

- by QR
- by URL

From one user to another. You give a QRcode to another user, that contains the dApp, the permission, and a data identifier or arrays of data identifiers.

The intended dApp will know how to consume the data and will delete the permission after consumption (through the wallet).

### Shadow Web2 Services

Every dApp that is also intended to be consumed by automatic actors shouls have symmetric Web2 services that provide at least the functionality provided by its GUI counterpart (the normal dApp). This way the functionality of such dApp could eventually be automated (where and when possible and profitable).


## Conclusions

Same conclusions as for [Wallet Proposals](Wallet_Proposals.Tentative_EIPs_and_dTypes.md)
We propose a creation of a dApp/Wallet Improvement Proposals (dWIP) repo that will function much like the EIP repo for this purpose.
