# About MellonPass

A password manager built using Python and Svelte (Typescript). This project is open source, so everybody can do security reviews, propose improvements, and fork MellonPass.

# About MellonPass Repos

## MellonPass Server

A Django-powered API server for serving and storing information securely. All stored data are encrypted before reaching the server from the client application, It also provides database-level encryption at rest to protect data.

## MellonPass Web

The front-end application is built using Svelte (TypeScript), having a security feature for safely storing decrypted data and users' encrpytion/decryption keys.

## Key features

- **End-to-end encryption**: Passwords and data are encrypted and authenticated using a 512-bit symmetric key: AES CTR 256-bit for confidentiality and HMAC 256-bit for integrity.
- **Secure master password generation**: The master password is salted and hashed via the Password-Based Key Derivation Function 2 (SHA-256) and is stretched using the HMAC-based Extract-and-Expand Key Derivation Function (SHA-512). The master password and stretched master passwords are not sent to the server.
- **Zero-knowledge encryption**: Users' vault items are encrypted locally before they are sent to the server. There's no way for MellonPass (basically, me) to see the data, and only you can decrypt them using your master password.
- **DB Column-level encryption**: Each database column that stores cipher texts is encrypted using Fernet (AES-CBC 128-bit, HMAC 256-bit, IV generated from a cryptographic secure random number generator).
- **Supported Vault Items**: Logins and Secure notes only for now. I will add more types in the future.
- **Organization Vaults**: These will be supported in the future!

## Usage

### Docker compose

.. WIP

## Get in touch

If you have questions or improvements in mind, don't hesitate to reach out to me at eikz.dev@protonmail.com.

Reported bugs are highly appreciated, please [create an issue]([url](https://github.com/mellonpass/.github/issues)) 🙏
