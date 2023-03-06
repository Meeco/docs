# Information Security

Meeco’s Information Security Management System (ISMS) is a framework of policies and controls that systematically manage security and risks across Meeco’s operations and technology. Meeco follows the [ISO27001](https://www.iso.org/isoiec-27001-information-security.html) standard to manage Information Security. ISO27001 is a set of specifications detailing how to create, manage, and implement ISMS policies and procedures.

Meeco’s ISMS is designed to establish holistic information security management capabilities and to embed them in our day-to-day activities. This includes our goal to digitise non-digital assets and services, such as Verifiable Credentials (VCs) and to never store unencrypted customer data.

Our digital ambitions related to SSI and Web3, require us to continuously implement and improve our security policies and controls. An artefact of these continuous efforts is a yearly ISO27001 audit and [accreditation (2022 Certificate)](https://media.meeco.me/public-assets/certification/MEEQ01-CCER02\_Certificate\_of\_Confidence.pdf).

## Meeco’s ISMS Security Controls <a href="#meecos-isms-security-controls" id="meecos-isms-security-controls"></a>

ISMS Security Controls span multiple domains of Information Security as specified in the ISO27001 standards.

| **Control**                                        | **Description**                                                                                                                                                                                                                                                                                                                                        |
| -------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| _**Information Security Policies**_                | Overall direction and support to help establish appropriate security policies.                                                                                                                                                                                                                                                                         |
| _**Asset Management**_                             | This component covers Meeco’s assets such as software codebase, intellectual property and data within and beyond the corporate IT network which may involve the exchange of sensitive business information.                                                                                                                                            |
| _**Communications and Operations Management**_     | Daily IT operations such as service provisioning and problem management follow IT security policies and ISMS controls.                                                                                                                                                                                                                                 |
| _**Access Control**_                               | This policy mainly deals with limiting access to authorised personnel and monitoring network traffic for abnormal behaviour. Access permissions relate to both digital and physical mediums of technology. The roles and responsibilities of individuals are well defined, with access to business information available only when necessary.          |
| _**Information Security and Incident Management**_ | Meeco identifies and resolves IT issues in ways that minimise the impact to end-users.                                                                                                                                                                                                                                                                 |
| _**Business Continuity Management**_               | Meeco avoids interruptions to business processes whenever possible. Ideally, any disaster situation is followed by recovery procedures to minimise the damage.                                                                                                                                                                                         |
| _**Compliance**_                                   | Meeco’s security requirements are enforced as per regulatory bodies, including ISO27001 and GDPR.                                                                                                                                                                                                                                                      |
| _**Cryptography**_                                 | Meeco has developed its CKMS (Cryptography Key Management System) which includes a high-level set of rules that were established by Meeco. These rules describe the goals, responsibilities, and overall requirements for the management of cryptographic key-related material used to protect Meeco and its customer’s data and critical information. |
| _**Supplier Relationships**_                       | Our third party vendors and business partners may require access to our network and sensitive customer data. Meeco has adopted controls to mitigate potential risks through IT Security Policies and contractual obligations.                                                                                                                          |

## Meeco’s Privacy Policy. <a href="#meecos-privacy-policy." id="meecos-privacy-policy."></a>

For a comprehensive description of our Privacy Policy, please view the policy [here.](https://www.meeco.me/privacy-policy)

## Vault-specific data security <a href="#vault-specific-data-security" id="vault-specific-data-security"></a>

All personal data at rest is encrypted with symmetric key encryption based upon AES-256-GCM. Additionally, all data is encrypted on a per user basis. If data is exchanged Vault-to-Vault a unique and encrypted shared space is generated. For each encryption space, each user has different symmetric keys.

Symmetric keys are stored into a Keystore and are encrypted with asymmetric key encryption based upon RSA-4096. The private key for this is derived from a passphrase created by the user and entered during Vault creation. In addition to the encryption, the Vault and Keystore are secured through the user’s unique personal log-in and/or device PIN Code.

Meeco support Anti-Correlation of Customer Information by ensuring that every connection between two Vaults has a distinct identity which can prevent correlation even if two Issuer Vaults were to collaborate. Our platform ensures that every interaction with a VC (issuing, accessing, verifying) can't be related by maintaining pairwise pseudonymity. For example, when the Vault is used to share VC data between connections, we use connection IDs; resulting in Issuers and Verifiers knowing each user by a different ID.Additionally, every attribute has a UUID.

For full transparency, Meeco makes this important documentation publicly available and has published additional information about the key encryption library, which is open sourced and available on GitHub at [Meeco](https://github.com/Meeco) for review and auditing. This is further supported by documentation at [docs.meeco.me](https://docs.meeco.me/).

## Additional Security Methods​ <a href="#additional-security-methods" id="additional-security-methods"></a>

The following operational security measures further enhance our data security:

#### 1. Security Awareness and Employee Education  <a href="#1.-security-awareness-and-employee-education" id="1.-security-awareness-and-employee-education"></a>

All Meeco employees undertake and maintain Security Awareness Training.

#### 2. Encryption <a href="#2.-encryption" id="2.-encryption"></a>

To protect our data, Meeco use data encryption at rest, in transit, and in cloud​. We use two types of cryptographic primitives:, which are generated on the device controlled by the end-user.

* **Data Encryption Keys** (DEKs) ​
  * These are symmetric encryption keys used to encrypt data ​
  * AES-256-GCM​
* **Public-key cryptography** (asymmetric encryption​)
  * Allows users to securely share DEKs (sender encrypts with recipient’s public key)​
  * Identifies users in the Vault (used for login)​
  * RSA-4096​

For more information about the Key ceremonies at Meeco, please read the [SVX Key section](https://meecosystem.atlassian.net/wiki/spaces/MPD/pages/2454224911/Overview).

#### 3. Microsoft Azure Security Tools <a href="#3.-microsoft-azure-security-tools" id="3.-microsoft-azure-security-tools"></a>

Our data security is managed in Azure via:

* _**Encryption**_ - Azure Disk Encryption, Azure storage service encryption, transparent data encryption.
* _**Rights Management**_ - Azure Right Management is a cloud-based service that uses encryption, identity, and authorisation policies to secure files.
* _**Access Control**_ - Azure role-based access control to restrict access to Azure resources based on user roles.
* _**Network**_ - To protect data in transit, we use SSL/TLS when exchanging data across different locations.
* _**Monitoring**_ - Microsoft Defender for Cloud automatically collects, analyses, and integrates log data from Azure resources, the network, and connected partner solutions, such as firewall solutions, to detect real threats and reduce false positives. Log Analytics provides centralised access to logs and helps in the analysis of data to create custom alerts.
