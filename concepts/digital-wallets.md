# Digital Wallets

Digital wallets enable users to manage their digital assets easily and securely from the device(s) of their choosing. Wallets are commonly used for the management of data and transactions, ranging from identity documentation (driver licences and certificates), financial (payments and transfers), tokens (fungible and non-fungible), and loyalty programs (vouchers and point accumulation). Wallets can be divided into two categories: single purpose or multi-purpose. Single purpose wallets focus on the management of one service, while multipurpose deliver a range of services that can be inter-linked or standalone.

## What is an Identity Wallet?

An identity wallet focuses on the safeguarding and management of a wallet holder's (Holder) identity data. The wallet acts as a secure container that generates, stores and processes the Holder’s:

1. [Cryptographic keys](../platform/keys.md)
2. [Decentralised identifiers (DIDs)](../platform/did.md)
3. [Credentials](../concepts/verifiable-credentials.md) (e.g. W3C Verifiable Credentials, mDL, PDFs)

A wallet also maintains the many connections its controller makes with other wallets. It is coupled with a software agent that utilises different protocols which enables these interactions. It is for this reason that the adoption of open standards by software providers is important to ensure interoperability.

### UX Functions of an Identity Wallet

An identity wallet must provide the user with a UI that will enable them to:

1. Bind the identity of the Holder to the wallet
2. Request and receive credentials
3. Present (and possibly selectively disclose) credentials for verification
4. Manage credentials including auditing capabilities & consent management
5. Manage keys related to DIDs
6. Manage user settings
7. Provide help functionality

## Operating Model of Wallets

There are three different ways a wallet can be operated, and any of them may be fully decentralised, centralised, or managed:

* **Non-custodial**: all the associated keys are under the control of the Holder (most likely decentralised).
* **Custodial**: all the associated keys are under the control of the custodian (most likely centralised).
* **Hybrid**: a combination of Holder and custodian controls (most likely managed).

Non-custodial wallets operate as per the Holder’s discretion and cannot be accessed by third parties unless the Holder provides consent. Generally, a non-custodial wallet is not linked to any external data storage, therefore, if the Holder loses their device or is locked out of their wallet application, they are unable to retrieve the contents of the wallet.

Custodial wallets enable third parties to manage a Holder’s data on their behalf. Generally, third parties acting on a wallet Holder’s behalf work as intermediaries between the Holder and other wallets / services.

A hybrid wallet can support Privacy- & Security-by-Design by enabling the Holder to maximise their privacy and exercise their data rights. However, as it is partly custodial, it can provide an option to help the Holder if they lose their wallet or obtain a new device. This may include key escrow services, together with wallet backup capabilities.

## The Meeco Wallet

Find out more about Meeco’s Wallet by viewing our [Wallet product page](https://www.meeco.me/wallet).
