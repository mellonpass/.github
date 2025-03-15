# About MellonPass

This project is open source, so everybody can do security reviews, propose improvements, and fork MellonPass.

## Features

- **Personal Vault** - Logins and Secure notes. (Will support more vault types).
- **Organization Vaults**: These will be supported in the future!

## Security

- **End-to-end encryption**: Vault data are encrypted and authenticated using a 512-bit symmetric key: AES CTR 256-bit for confidentiality and HMAC 256-bit for integrity, generated from the cryptographically secure pseudorandom number generator (CSPRNG).
- **Secure master password generation**: The master password is salted and hashed via the Password-Based Key Derivation Function 2 (PBKDF2) and is stretched using the HMAC-based Extract-and-Expand Key Derivation Function (HKDF). The master password and stretched master passwords are not sent to the server.
- **Zero-knowledge encryption**: Vault data are encrypted locally before they are sent to the server. There's no way for MellonPass to see the data, and only you can decrypt them using your master password.
- **DB Column-level encryption**: Each database column that stores cipher data is encrypted using Fernet (AES-CBC 128-bit, HMAC 256-bit, IV generated from CSPRNG).

## MellonPass Repos

### MellonPass Server

A Django-powered API server for serving and storing information securely. It also provides database-level encryption at rest to protect data.

### MellonPass Web

The front-end application is built on top of SvelteKit using TypeScript and has a security feature for safely storing decrypted data and users' encryption/decryption keys.

## Get in touch

If you have questions or improvements in mind, don't hesitate to reach out to me at eikz.dev@protonmail.com.

Reported bugs are highly appreciated. Please [create an issue]([url](https://github.com/mellonpass/.github/issues)) 🙏
