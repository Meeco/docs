Organisations are able to build applications that enable credential issuance via the use of the SVX API. After issuing a credential Organisation Administrators can view the credential and associated details via the Portal.

## View a credential

To view an issued credential navigate to _Credentials_ on the left-side menu of the SVX Portal, and select _View Credentials_. Find the credential in the list either by using the sort or search function. Once located, select the horizontal ellipsis icon ⋯ to reveal menu options. Select _View credential_.

You will be presented with a summary of the credential, including:
* VC ID
* Wallet DID
* Issuer ID
* Format
* Credential template
* Date issued
* Expiry date
* Status
* Credential details

### Administrators with viewing rights

Organisation Administrators with the necessary security permissions can view Holder information within a credential. This information is displayed in the *Credential details* section, which expands to reveal a box containing the attributes and corresponding Holder details. To emphasise SVX's robust security measures and promote awareness around the safe handling of personally identifiable information (PII), the Portal presents PII within identifiable *security components*. For more information, see the [Security](./dashboard-and-navigation.md#security) section under [Dashboard and Navigation](./dashboard-and-navigation.md).

### Administrators without viewing rights

Organisation Administrators without the necessary security permissions will not be able to view Holder information within a credential. The *Credential details* section will be left blank with only a message in the security component stating that "*You do not have permission to view this information. To view this information enter the Organisation’s passphrase.*". For more information, see the [Security](./dashboard-and-navigation.md#security) section under [Dashboard and Navigation](./dashboard-and-navigation.md).

> **Note**
> To gain the necessary security rights to view PII, an Organisation Administrator must enter the Organisation's Passphrase as part of their Account Settings. For more information, see [Set Up an Organisation's Passphrase](./onboarding-and-organisation-setup.md#setup-an-organisations-passphrase).

## Revoke a credential

To revoke a credential, navigate to _Credentials_ on the left-side menu of the SVX Portal, and select _View Credentials_. Find the credential in the list either by using the sort or search function. Once located, select the horizontal ellipsis icon ⋯ to reveal menu options. Select _Revoke credential_.

You will be presented with a confirmation message where you can choose to _Cancel_ or _Revoke_. If you decide to revoke the credential, you will return to the list of credentials where a confirmation toast message will appear. The revoked credential will no longer be visible in the list of credentials.

> **Note**
> Only some credentials can be revoked depending on their credential format. Currently, the credential format(s) that support revocation are W3C (jwt-vc-json).
