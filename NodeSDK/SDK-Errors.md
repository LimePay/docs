SDK Errors Documentation
============

## Content

- [Signing](#1-signing)
- [Wallets](#2-Wallets)
- [Validation](#3-validation)
- [Vendors](#4-vendors)

### 1. Signing
| Error Name            | Code  | Message    | Description | Оccurrence |
| --------------------- | ----- |--------------- | -------- | -------- |
| `SIGNING_ERROR` | `100011` | Could not sign authorization signature. Invalid parameters provided.  | When you are creating a payment and you provide invalid parameters | Creating Payments |

### 2. Wallets

| Error Name            | Code  | Message    | Description | Оccurrence |
| --------------------- | ----- |--------------- | -------- | -------- |
| `WALLET_ERROR` | `100012` | Wallet configuration object is required but not provided | When you are creating a payment and you do not provide wallet configuration | Creating Payments |
| `WALLET_ERROR` | `100013` | Cannot decrypt wallet. Invalid JSON Wallet provided | When you are creating a payment and you provide invalid JSON Wallet in the wallet configuration | Creating Payments |
| `WALLET_ERROR` | `100014` | Cannot initialise wallet. Invalid Private key provided | When you are creating a payment and you provide invalid private key in the wallet configuration | Creating Payments |
| `WALLET_ERROR` | `100015` | Cannot initialise wallet. Invalid Mnemonic provided | When you are creating a payment and you provide mnemonic in the wallet configuration | Creating Payments |
| `WALLET_ERROR` | `100016` | Invalid wallet configuration provided | When you are creating a payment and your provide invalid wallet configuration | Creating Payments |

### 3. Validation

| Error Name            | Code  | Message    | Description | Оccurrence |
| --------------------- | ----- |--------------- | -------- | -------- |
| `VALIDATION_ERROR` | `100018` | Invalid fundTxData object provided. weiAmount cannot be undefined | When you are creating a payment and you do not provide `weiAmount` in `fundTxData` | Creating Payments |
| `VALIDATION_ERROR` | `100111` | fundTxData object cannot be undefined | When you are creating a payment and you do not provide `fundTxData` | Creating Payments |

### 4. Vendors

| Error Name            | Code  | Message    | Description | Оccurrence |
| --------------------- | ----- |--------------- | -------- | -------- |
| `NO_VENDOR_ERROR` | `100019` | You are required to have vendor in order to perform this operation | When you are creating a payment but you do not have a vendor yet | Creating Payments |