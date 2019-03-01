Web-Library Errors Documentation
============

## Content

- [Validation](#1-validation)
- [Token Verification](#2-token-verification)
- [Transactions](#3-transactions)
- [Wallets](#4-wallets)

### 1. Validation

| Error Name            | Code  | Message    | Description | Оccurrence |
| --------------------- | ----- |--------------- | -------- | -------- |
| `VALIDATION_ERROR` | `4014` | {propertyName} is not an object | General validation of objects that are passed to the library  | - |
| `VALIDATION_ERROR` | `4015` | {propertyName} is not a string | General validation of strings that are passed to the library | - |
| `VALIDATION_ERROR` | `4016` | {propertyName} is not a boolean | General validation of booleans that are passed to the library | - |
| `VALIDATION_ERROR` | `4017` | {propertyName}is not defined | General validation of parameter that is required | - |
| `VALIDATION_ERROR` | `4019` | Provided wallet is not instance of ethers.Wallet | When you are signing transactions with decrypted wallet and the wallet is not an instance of `ethers.Wallet` | Signing transactions with `.signWithDecryptedWallet` |

### 2. Token Verification

| Error Name            | Code  | Message    | Description | Оccurrence |
| --------------------- | ----- |--------------- | -------- | -------- |
| `TOKEN_VERIFICATION_ERROR` | `1140` | Invalid LimePay token | When the provided lime token for loading payments is not valid  | Loading payments |
| `TOKEN_VERIFICATION_ERROR` | `1141` | No token provided | When no token is provided but it is required  | Creating, Changing and Getting Wallet for a Shopper |

### 3. Transactions

| Error Name            | Code  | Message    | Description | Оccurrence |
| --------------------- | ----- |--------------- | -------- | -------- |
| `TRANSACTION_VALIDATION_ERROR` | `3020` | "No transactions provided!" | When you are trying to sign transactions, but haven't provided them  | Signing transactions with `.Transactions` |
| `TRANSACTION_VALIDATION_ERROR` | `3120` | The provided signed transactions are not an array | When you are trying to sign transactions, but haven't array of transactions | Signing transactions with `.Transactions` |
| `TRANSACTION_VALIDATION_ERROR` | `3220` | Invalid function name. {functionName} cannot be found in the provided ABI | When you are trying to sign transactions, but the `functionName` of a generic transactions is invalid (not present in the ABI) | Signing transactions with `.Transactions` |
| `TRANSACTION_VALIDATION_ERROR` | `3021` | It should have at least one signed transaction. | When you are trying to sign transactions, but you have provided an empty array | Signing transactions with `.Transactions` |
| `TRANSACTION_VALIDATION_ERROR` | `3110` | Transaction has been provided with invalid format. TX: {transaction} | When you are trying to sign transactions, but you have provided transaction with invalid properties | Signing transactions with `.Transactions` |

### 4. Wallets

| Error Name            | Code  | Message    | Description | Оccurrence |
| --------------------- | ----- |--------------- | -------- | -------- |
| `WALLET_ERROR` | `8003` | No mnemonic provided | When you don't provide a mnemonic when you are changing the password of a encrypted wallet   | Changing password of the Wallet for a Shopper |
| `WALLET_ERROR` | `8004` | No password provided | When you don't provide new password when you are changing the password of a encrypted wallet   | Changing password of the Wallet for a Shopper |