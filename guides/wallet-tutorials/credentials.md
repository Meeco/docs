The Meeco Wallet enables users (Holders) to import, store, view, share and delete their Verifiable Credentials without any tracking or third-party viewing their data. The Credential section of the Wallet can be accessed via the Credential icon on the navigation menu.

## Import a credential

Once a connection with an Organisation has been established (see the [Organisation Configuration tutorial](guides/wallet-tutorials/organisation-configuration.md) for more information), the Organisation can directly issue credentials to your wallet. When a credential is issued to a wallet, a toast message will appear notifying you that a credential is waiting to be added to your wallet. You can either tap on the _Add now_ button or the cross icon to cancel. A pop-up screen will appear asking you to confirm that you wish to add the credential(s) to your wallet. Select the credential(s) you wish to add and tap on the _Add_ button. You can also choose to _Cancel_ or _Reject all credentials_.

> **Note**
> _Reject all credentials:_ Once a credential is rejected, the option to add it to your wallet will not appear again. The Issuer will need to re-issue the credential for you to add to your wallet.

## View a credential

Once a credential has been added to your wallet, you will see it appear in the _Credentials_ section. The _face_ of the credential will display the following information:
- Issuer’s logo (note that not all credentials will have a logo)
- The name of the credential
- The name of the Issuer
- Date issued
- Date expires
- An icon displaying the status of the credential

To view additional credential details, tap on the credential. An expanded view will appear. The expanded view includes the following information:
- Associated attributes (this may include the Holder’s personal details and information specific to the credential type)
- Advanced details
  - Credential ID
  - Issuer Name
  - Issuer DID
  - Option to copy raw data
  - Option to share raw data

## Credential status

Credentials can exist in different statuses. The status of a credential is indicated by the icon in the top-right of the credential itself and by the credential’s colour. See below the different possible statuses:

#### Valid / Verified
A valid credential is indicated by a tick in a circle icon and the colour of the credential will not change.

#### Not verified
A credential that is unable to be verified in real-time is indicated by an exclamation mark in a circle icon. The colour of the credential will not change.

#### Revoked / Expired
An expired credential is indicated by a an exclamation mark in a hexagon icon. The colour of the credential will change to a grey striped pattern.

## Delete a credential
To delete a credential, tap on a credential to view more details. Tap on the horizontal ellipsis icon ⋯ in the top-right corner of the screen. Tap on the _Delete_ button. The credential will be removed from your wallet.
