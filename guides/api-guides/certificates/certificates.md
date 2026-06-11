# Certificates

Certificates are used to establish trust between parties in [OID4VCI](https://openid.net/specs/openid-4-verifiable-credential-issuance-1_0.html) and [OID4VP](https://openid.net/specs/openid-4-verifiable-presentations-1_0.html) flows. They are included in the `x5c` header of issued credentials and presentation requests, allowing verifiers and wallets to validate the certificate chain back to a trusted root.

SVX Wallet supports multiple key types, but the two most relevant for certificate-backed trust are:

- **Credential key** – used to sign credentials (for example `mso_mdoc` Document Signing Certificate, DSC).
- **Presentation request key** – used to sign presentation requests by the verifier, establishing verifier identity.

The typical setup flow is:

1. Create a root CA locally.
2. Create a Certificate Signing Request (CSR) via the SVX Wallet API.
3. Sign the CSR with your root CA using `openssl`.
4. Import the signed certificate back into the SVX Wallet API.
5. Optionally import trust anchor certificates for external chain validation.

## Who can undertake this operation?

Certificate management can only be performed by people who have access to the SVX Wallet Dashboard and API.

## Create a Root CA

Before creating CSRs, you need a root CA to sign leaf certificates. The following examples create a self-signed root CA for credential signing (IACA) and a separate root for presentation request signing.

```bash
# Create a root CA for credential signing (IACA)
openssl req -new -newkey ec -pkeyopt ec_paramgen_curve:prime256v1 -nodes \
  -x509 -sha256 -days 1825 \
  -keyout meeco_iaca.key \
  -out meeco_iaca.cer \
  -subj "/C=AU/O=Meeco/CN=Meeco IACA" \
  -addext "basicConstraints=critical,CA:true,pathlen:0" \
  -addext "keyUsage=critical,keyCertSign,cRLSign" \
  -addext "subjectKeyIdentifier=hash" \
  -addext "subjectAltName=URI:<issuer_uri>"

# Create a root CA for presentation request signing
openssl req -new -newkey ec -pkeyopt ec_paramgen_curve:prime256v1 -nodes \
  -x509 -sha256 -days 1825 \
  -keyout meeco_reader_root.key \
  -out meeco_reader_root.cer \
  -subj "/C=AU/O=Meeco/CN=Meeco" \
  -addext "basicConstraints=critical,CA:true,pathlen:0" \
  -addext "keyUsage=critical,keyCertSign,cRLSign" \
  -addext "subjectKeyIdentifier=hash" \
  -addext "subjectAltName=URI:<issuer_uri>"
```

## Create a CSR

CSRs are created via the SVX Wallet API against a named key. The API returns the CSR PEM for external signing.

Create a CSR for the credential signing key:

```bash
curl -sS -X POST "$SVX_WALLET_BASE_URL/system/certificates/csrs" \
  -H "Authorization: Bearer $ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "key_name": "CredentialKey",
    "subject": {
      "common_name": "wallet.meeco.cloud",
      "country": "AU",
      "state": "New South Wales",
      "locality": "Sydney",
      "organization": "Meeco",
      "organizational_unit": "Identity"
    },
    "issuer_alternative_name_url": [
      "DNS:wallet.meeco.cloud"
    ]
  }' | tee credential-csr.json

jq -r '.csr.csr_pem' credential-csr.json > credential.csr.pem
```

Create a CSR for the presentation request signing key:

```bash
curl -sS -X POST "$SVX_WALLET_BASE_URL/system/certificates/csrs" \
  -H "Authorization: Bearer $ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "key_name": "PresentationRequestKey",
    "subject": {
      "common_name": "wallet.meeco.cloud",
      "country": "AU",
      "state": "New South Wales",
      "locality": "Sydney",
      "organization": "Meeco",
      "organizational_unit": "Identity"
    },
    "issuer_alternative_name_url": [
      "DNS:wallet.meeco.cloud"
    ]
  }' | tee request-csr.json

jq -r '.csr.csr_pem' request-csr.json > request.csr.pem
```

## Sign the CSRs

Use `openssl` to sign each CSR with the appropriate root CA. Certificate extensions differ depending on the intended key usage.

Save the following extension configuration to a file (e.g. `openssl-extensions.ext`):

> **Note:** `openssl x509 -req` does not automatically copy all attributes from the CSR into the signed certificate. Properties such as `subjectAltName`, `keyUsage`, and `extendedKeyUsage` must be explicitly defined in the extension file to ensure they appear correctly in the issued certificate.

```ini
[ v3_mdoc_dsc ]
basicConstraints = critical,CA:false
keyUsage = critical,digitalSignature
extendedKeyUsage = 1.0.18013.5.1.2,1.3.6.1.4.1.61546.0
subjectKeyIdentifier = hash
authorityKeyIdentifier = keyid,issuer
subjectAltName = DNS:wallet.meeco.cloud

[ v3_request ]
basicConstraints = critical,CA:false
keyUsage = critical,digitalSignature
subjectKeyIdentifier = hash
authorityKeyIdentifier = keyid,issuer
subjectAltName = DNS:wallet.meeco.cloud
```

Sign the credential CSR (DSC):

```bash
openssl x509 -req -sha256 \
  -in credential.csr.pem \
  -CA meeco_iaca.cer \
  -CAkey meeco_iaca.key \
  -CAcreateserial \
  -out credential-leaf.cert.pem \
  -days 365 \
  -extfile openssl-extensions.ext \
  -extensions v3_mdoc_dsc
```

Sign the presentation request CSR:

```bash
openssl x509 -req -sha256 \
  -in request.csr.pem \
  -CA meeco_reader_root.cer \
  -CAkey meeco_reader_root.key \
  -CAcreateserial \
  -out request-leaf.cert.pem \
  -days 365 \
  -extfile openssl-extensions.ext \
  -extensions v3_request
```

## Import Signed Certificates

Once signed, import each leaf certificate the SVX Wallet API. The certificate must be DER-encoded and base64-encoded in the `x5c` field.

```bash
# Import credential leaf certificate
curl -sS -X POST "$SVX_WALLET_BASE_URL/system/certificates/import" \
  -H "Authorization: Bearer $ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d "{\"key_name\":\"CredentialKey\",\"x5c\":[\"$(openssl x509 -in credential-leaf.cert.pem -outform DER | openssl base64 -A)\"]}" | jq

# Import presentation request leaf certificate
curl -sS -X POST "$SVX_WALLET_BASE_URL/system/certificates/import" \
  -H "Authorization: Bearer $ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d "{\"key_name\":\"PresentationRequestKey\",\"x5c\":[\"$(openssl x509 -in request-leaf.cert.pem -outform DER | openssl base64 -A)\"]}" | jq
```

## Import Trust Anchor Certificates

Trust anchor certificates allow the wallet to validate incoming certificate chains from external issuers or verifiers.

Import a trust anchor certificate chain:

```bash
curl -X POST "$SVX_WALLET_BASE_URL/system/certificates/import" \
  -H "Authorization: Bearer $ACCESS_TOKEN" \
  -H "Content-Type: application/json" \
  -d "{\"x5c\":[\"$(openssl x509 -in root-ca.cert.pem -outform DER | openssl base64 -A)\"]}"
```

## Remaining endpoints

The following endpoints are used to maintain and read certificate information:

- `GET /system/certificates` - List all imported trust anchor certificates.
- `DELETE /system/certificates/{certificate_id}` - Delete a trust anchor certificate.
