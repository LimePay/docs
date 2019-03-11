
  

Web-Library Documentation
============
 This file contains the documentation of LimePay's Web Library.
## Contents
 - [Connecting to the API](#1-connecting-to-the-api)
 - [Wallets](#2-wallets)
 	- [Getting LimePay powered Wallet for a Shopper](#21-getting-limepay-powered-wallet-for-a-shopper)
	- [Create LimePay powered Wallet for a Shopper](#22-create-limepay-powered-wallet-for-a-shopper)
	- [Change the password of a LimePay powered Wallet](#23-change-the-password-of-a-limepay-powered-wallet)
- [Fiat Payments](#3-fiat-payments)
	- [Loading a Fiat Payment](#31-loading-a-fiat-payment)
	- [Calculating the VAT/TAX](#32-calculating-the-vattax)
	- [Processing Fiat Payment](#33-processing-fiat-payment)
- [Relayed Payments](#4-relayed-payments)
	- [Loading a Relayed Payment](#41-loading-a-relayed-payment)
	- [Processing Relayed Payment](#42-processing-relayed-payment)
- [Signing Transactions](#5-signing-transactions)
	- [Signing transactions without using LimePay powered wallets](#51-signing-transactions-without-using-limePay-powered-wallets)
	- [Signing transactions for a Shopper with LimePay powered wallets](#52-signing-transactions-for-a-shopper-with-limePay-powered-wallets)
- [Objects](#6-objects)
	- [VAT Data](#vat-data)

## 1. Connecting to the API

Installing the JavaScript SDK:

    npm install limepay-web

Once you have installed the library you need to include it in your project and connect to LimePay's API.

```javascript
const LimePayWeb = require('limepay-web');
const LimePay = await LimePayWeb.connect(LimePayWeb.Environment.Production); // or LimePayWeb.Environment.Sandbox; returns Promise<>
```

Once the `.connect` promise is being resolved you can start consuming the Library through the new instance that was created.

## 2. Wallets
The library provides a functionality for the management of the LimePay powered wallets.

### 2.1 Getting LimePay powered Wallet for a Shopper

```javascript
LimePay.Wallet.get(walletToken) // returns Promise<>
	.then(encryptedWallet => {})
	.catch(error => {});
```

Where `walletToken` is the token returned after performing [Get Wallet Token](https://github.com/LimePay/docs/blob/latest/NodeSDK/SDK-documentation.md#26-getting-wallet-token-for-shopper) request through the SDK or a [direct call](https://github.com/LimePay/docs/blob/latest/API/API-Documentation.md#36-getting-wallet-token) to LimePay's API.

Returns JSON Encrypted wallet.

### 2.2 Create LimePay powered Wallet for a Shopper

```javascript
LimePay.Wallet.create(walletToken, password) // return Promise<>
	.then(mnemonic => {})
	.catch(error => {});
```
Where `walletToken` is the token returned after performing [Get Wallet Token](https://github.com/LimePay/docs/blob/latest/NodeSDK/SDK-documentation.md#26-getting-wallet-token-for-shopper) request through the SDK or a [direct call](https://github.com/LimePay/docs/blob/latest/API/API-Documentation.md#36-getting-wallet-token) to LimePay's API and `password` is the passphrase that is going to be used for the encryption of the JSON keystore..

Returns the mnemonic of the wallet, which MUST be stored by the end-user and later can be used for providing a `forgotten password` feature.

### 2.3 Change the password of a LimePay powered Wallet

```javascript
LimePay.Wallet.changePassword(walletToken, mnemonic, newPassword) // returns Promise<>
	.catch(error => {});
```
Where `walletToken` is the token returned after performing [Get Wallet Token](https://github.com/LimePay/docs/blob/latest/NodeSDK/SDK-documentation.md#26-getting-wallet-token-for-shopper) request through the SDK or a [direct call](https://github.com/LimePay/docs/blob/latest/API/API-Documentation.md#36-getting-wallet-token) to LimePay's API.
`mnemonic` is the mnemonic that was returned after creating the original wallet and `newPassword` is the new passphrase that will be used for encryption of the wallet.

## 3. Fiat Payments
Using the library, one can load and process payments.

### 3.1 Loading a Fiat Payment
In order for you to initialise the payment form successfully, you will need to load the payment. 

```javascript
LimePay.FiatPayments.load(limePayToken) // returns Promise<>
	.then(fiatPayment => {})
	.catch(error => {});
```

Where `limePayToken` is the token returned after creating a payment through the LimePay API or SDK.
Returns instance of type `Fiat Payment`. It has `calculateVAT` and `process` functions

### 3.2 Calculating the VAT/TAX
Fiat Payments have VAT/TAX that is applied to them. You can calculate the VAT/TAX for a fiat payment by:
```javascript
fiatPayment.calculateVAT(cardHolderData)
    .then(vatData => {})
    .catch(error => {})
```

Where `fiatPayment` is the instance of `Fiat Payment` returned from `LimePay.FiatPayments.load()` and `cardHolderData` is object with the following properties:

| Attribute       | Type      | Description                                                                     | Required |
| --------------- | --------  | ------------------------------------------------------------------------------- | -------- |
| `countryCode`   | `string`  | Shopper's ISO, Alpha-2 country code      										| yes      |
| `isCompany`     | `boolean` | Whether the shopper is a company or not                                         | yes      |
| `vatNumber`     | `string`  | VAT or TAX number of the shopper    											| no       |
| `reducedVatRate` | `number`  | The reduced VAT/TAX rate that the shopper can apply for                        | no       |

The returned object is obejct of type [VAT Data](#VAT-Data)

### 3.3 Processing Fiat Payment
In order to trigger the processing of the fiat payment one should execute:

```javascript
fiatPayment.process(cardHolderInformaton, signedTransactions)
    .catch(error => {})
```

Where `fiatPayment` is the instance of `Fiat Payment` returned from `LimePay.FiatPayments.load()`.
`signedTransactions` are the signed transactions that were signed beforehand and `cardHolderInformaton` is object with the following properties:

| Attribute       | Type      | Description                                                                     | Required |
| --------------- | --------  | ------------------------------------------------------------------------------- | -------- |
| `name`          | `string`  | Name of the shopper that will be billed  										| yes      |
| `isCompany`     | `boolean` | Whether the shopper is a company or not                                         | yes      |
| `countryCode`   | `string`  | Shopper's ISO, Alpha-2 country code      										| yes      |
| `zip`           | `string`  | Shopper's ZIP                             										| yes      |
| `street`        | `string`  | Shopper's street address                 										| yes      |
| `vatNumber`     | `string`  | VAT or TAX number of the shopper    											| no       |

## 4. Relayed Payments
### 4.1 Loading a Relayed Payment
```javascript
LimePay.RelayedPayments.load(limePayToken) // returns Promise<>
	.then(relayedPayment => {})
	.catch(error => {});
```
Where `limePayToken` is the token returned after creating a payment through the LimePay API or SDK.
Returns instance of type `Relayed Payment`. It has only one function - `process`

### 4.2 Processing Relayed Payment
In order to trigger the processing of the relayed payment one should execute:

```javascript
relayedPayment.process(signedTransactions)
    .catch(error => {})
```

Where `relayedPayment` is the instance of `Relayed Payment` returned from `LimePay.RelayedPayments.load()`.
`signedTransactions` are the signed transactions that were signed beforehand.

## 5. Signing Transactions
In order for you to be able to process payments you need to provide the transactions, signed by the user's private key.
The `limepay.Transactions` functionality for signing payments is flexible and supports different signing methods

### 5.1 Signing transactions without using LimePay powered wallets
```javascript
Limepay.Transactions.signWithDecryptedWallet(transactions, decryptedWallet) // returns Promise<>

Limepay.Transactions.signWithEncryptedWallet(transactions, jsonWallet, password) // returns Promise<>

// nonEnglishLocaleWorldList is optional
Limepay.Transactions.signWithMnemonic(transactions, mnemonic, nonEnglishLocaleWorldList?) // returns Promise<>

Limepay.Transactions.signWithPrivateKey(transactions, privateKey) // returns Promise<>
```

Once the promise is resolved, the signed transactions are returned.

### 5.2 Signing transactions for a Shopper with LimePay powered wallets
```javascript
Limepay.Transactions.signWithLimePayWallet(transactions, limePayToken, password) // returns Promise<>
    .then(signedTransactions => {})
    .catch(error => {})
```

`transactions` is an array of objects contating `transactions`. They are documented in more details [here](#https://github.com/LimePay/docs/blob/latest/2.%20Processing-Payments.md#52-signing-transactions)
`limePayToken` is the token returned after creating a payment through the LimePay API or SDK.

### 6. Objects

#### VAT Data

| Attribute                | Type     | Description                                                                        | Nullable |
| ------------------------ | -------- | ---------------------------------------------------------------------------------- | -------- |
| `rate`                   | `number` | VAT/TAX percentage that is being applied to the payment                            | no       |
| `totalVAT`               | `number` | VAT/TAX amount calculated by `rate`*`paymentAmount` + `paymentAmount`              | no       |
| `totalAmount`            | `number` | This is the total amount for the payment. `paymentAmount`+`totalVAT`               | no       |
