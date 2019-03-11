
  

JavaScript SDK Documentation
============
 This file contains the documentation of LimePay's JavaScript SDK.
## Contents

 - [Connecting to the API](#1-connecting-to-the-api)
 - [Shoppers](#2-shoppers)
 	- [Getting Shopper](#21-getting-shopper)
	- [Getting all Shoppers](#22-getting-all-shoppers)
	- [Creating Shopper](#23-creating-shopper)
	- [Updating Shopper](#24-updating-shopper)
	- [Deleting Shopper](#25-deleting-shopper)
	- [Getting Wallet Token](#26-getting-wallet-token-for-shopper)
- [Payments](#3-payments)
	- [Getting Payment](#31-getting-payment)
	- [Getting All Payments](#32-getting-all-payments)
- [Fiat Payments](#4-fiat-payments)
	- [Creating Fiat Payment](#41-creating-fiat-payment)
	- [Sending Invoice for Payment](#42-sending-invoice-for-payment)
	- [Getting Invoice Template for Payment](#43-getting-invoice-template-for-payment)
	- [Getting Receipt Template for Payment](#44-getting-receipt-template-for-payment)
- [Relayed Payments](#5-relayed-payments)
	- [Creating Relayed Payment](#51-creating-relayed-payment)
- [Objects](#6-objects)
	- [Wallet Configurations](#wallet-configurations)
	- [Item](#item)
	- [Fund Transaction Data For Fiat Payment](#fund-transaction-data-for-fiat-payment)
	- [Fund Transaction Data For Relayed Payment](#fund-transaction-data-for-relayed-payment)
	- [Generic Transaction](#generic-transaction)
	- [Function Parameter](#function-parameter)

## 1. Connecting to the API

Installing the JavaScript SDK:

    npm install limepay

Once you have installed the sdk you need to include it in your project and connect to LimePay's API.

```javascript
const LimePaySDK = require('limepay');
const LimePay = await LimePaySDK.connect({
	environment: LimePay.Environment.Production, // or LimePay.Environment.Sandbox
	apiKey: 'YOUR_API_KEY_HERE',
	secret: 'YOUR_API_SECRET_HERE'	
});
```

Once the `.connect` promise is being resolved you can start consuming the SDK.

## 2. Shoppers
One functionality that the SDK provides is that you can consume the Shoppers resource.

### 2.1 Getting Shopper

```javascript
LimePay.shoppers.get('shopperId') // returns promise
	.then(shopper => {})
	.catch(error => {});
```

Returns object of type [Shopper](https://github.com/LimePay/docs/blob/latest/4.%20API-Documentation.md#shopper).

### 2.2 Getting All Shoppers

```javascript
LimePay.shoppers.getAll() // returns promise
	.then(shoppers => {})
	.catch(error => {});
```
Returns array of [Shopper](https://github.com/LimePay/docs/blob/latest/4.%20API-Documentation.md#shopper) objects.

### 2.3 Creating Shopper

```javascript
LimePay.shoppers.create(shopperData) // returns promise
	.then(shopper => {})
	.catch(error => {});
```

Where `shopperData` is object with the following properties:

| Field 		  | Type	 | Description 				  | Required |
| --------------- | -------- | -------------------------- | -------- |
| `firstName` 	  | `string` | Shopper's first name 	  | yes	   |
| `lastName`      | `string` | Shopper's last name  	  | no	   |
| `vendor`		  | `string` | Vendor ID for which the shopper will be created. If not provided, the default organisation's vendor will be used| no|
| `email`		  | `string` | Shopper's email 			  | yes    |
| `useLimePayWallet` | `boolean` | Whether the shopper will use LimePay Wallets or not. Default value is `false`  | no |
| `walletAddress` | `string` | Shopper's wallet address   | yes, if `useLimePayWallet` is `false` |

Returns object of type [Shopper](https://github.com/LimePay/docs/blob/latest/API/API-Documentation.md#shopper).

### 2.4 Updating Shopper

```javascript
LimePay.shoppers.update('shopperId', shopperData) // returns promise
	.then(shopper => {})
	.catch(error => {});
```
Where `shopperData` is object with the following properties:

| Field 		  | Type	 | Description 				  | Required |
| --------------- | -------- | -------------------------- | -------- |
| `firstName` 	  | `string` | Shopper's first name 	  | no 	   |
| `lastName`      | `string` | Shopper's last name  	  | no 	   |
| `email`		  | `string` | Shopper's email 			  | no	   |
| `walletAddress` | `string` | Shopper's wallet address   | no 	   |

Returns object of type [Shopper](https://github.com/LimePay/docs/blob/latest/API/API-Documentation.md#shopper).

### 2.5 Deleting Shopper

```javascript
LimePay.shoppers.delete('shopperId') // returns new Promise<>
	.catch(error => {});
```

### 2.6 Getting Wallet Token for Shopper

```javascript
LimePay.shoppers.getWalletToken('shopperId') // returns new Promise<>
	.then(result => {})
	.catch(error => {});
```

Returns object of type [Wallet Token](https://github.com/LimePay/docs/blob/latest/API/API-Documentation.md#wallet-token)

## 3. Payments
Through the SDK you can consume the Payments resource.

### 3.1 Getting Payment
```javascript
LimePay.payments.get('paymentId') // returns promise
	.then(payment => {})
	.catch(error => {});
```

Returns object of type [Payment](https://github.com/LimePay/docs/blob/latest/API/API-Documentation.md#payment).

### 3.2 Getting All Payments
```javascript
LimePay.payments.getAll() // returns promise
	.then(payments => {})
	.catch(error => {});
```
Returns array of [Payment](https://github.com/LimePay/docs/blob/latest/API/API-Documentation.md#payment). objects.

## 4. Fiat Payments

You can create Fiat Payments through the JavaScript SDK.

### 4.1 Creating Fiat Payment

```javascript
LimePay.fiatPayment.create(fiatPaymentData, signerWalletConfig) // returns promise
	.then(payment => {})
	.catch(error => {});
```

Once executed successfully returns object of type [Payment](https://github.com/LimePay/docs/blob/latest/API/API-Documentation.md#payment).

---
`signerWalletConfig` is the wallet configuration of a wallet that is marked as `signer` in the Escrow contract.
You can find more information of the different kinds of wallet configurations [here](#wallet-configurations).

`fiatPaymentData` is object with the following properties:

| Field | Type |Description | Required |
| -------------------- | -------- | ---------------------------------- | ---------- |
| `currency` | `string` | Currency code (ISO 4217) of the amount to be charged | yes |
| `shopper` | `string` | The ID of the shopper that will be charged | yes |
| `items` | `array` | Array of [Item](#item) objects | yes |
| `fundTxData` | `object` | [Fund Transaction Data](#fund-transaction-data-for-fiat-payment) object | yes |
| `genericTransactions`| `array` | Array of [Generic Transaction](#generic-transaction) objects | yes |

---

### 4.2 Sending Invoice for Payment

You can trigger the sending of an invoice for a specific payment to the shopper by executing the following function:
```javascript
LimePay.fiatPayment.sendInvoice('paymentId') // returns promise
	.then(() => {})
	.catch(error => {});
```
On successful request for sending the invoice you will receive status `code 200`

### 4.3 Getting Invoice Template for Payment

You can preview the invoice template that is being send to your users by executing the following function:
```javascript
LimePay.fiatPayment.getInvoice('paymentId') // returns promise
	.then((invoice) => {})
	.catch(error => {});
```
### 4.4 Getting Receipt Template for Payment

You can preview the receipt template that is being send to your users by executing the following function:
```javascript
LimePay.fiatPayment.getReceipt('paymentId') // returns promise
	.then((receipt) => {})
	.catch(error => {});
```

## 5. Relayed Payments

You can create Relayed Payments through the JavaScript SDK.

### 5.1 Creating Relayed Payment

```javascript
LimePay.relayedPayment.create(relayedPaymentData, signerWalletConfig) // returns promise
	.then(shopper => {})
	.catch(error => {});
```

Once executed successfully returns object of type [Payment](https://github.com/LimePay/docs/blob/latest/API/API-Documentation.md#payment).

---
`signerWalletConfig` is the wallet configuration of a wallet that is marked as `signer` in the Escrow contract.
You can find more information of the different kinds of wallet configurations [here](#wallet-configurations).

`relayedPaymentData` is object with the following properties:

| Field | Type |Description | Required |
| -------------------- | -------- | ---------------------------------- | ---------- |
| `shopper` | `string` | The ID of the shopper that will be charged | yes |
| `fundTxData` | `object` | [Fund TX Data](#fund-transaction-data-for-relayed-payment) object | yes |
| `genericTransactions`| `array` | Array of [Generic Transaction](#generic-transaction) objects | yes |

---

### 6. Objects

#### Wallet Configurations

Below are some examples of the different wallet configurations.

```javascript
const encryptedWalletConfig = {
	encryptedWallet: {
		jsonWallet: wallet, // JSON encrypted Wallet
		password: password
	}
}  
const decryptedWalletConfig = {
	decryptedWallet: wallet  // Ethers decrypted wallet 
} 
const privateKeyWalletConfig = {
	privateKey: 'private key here'
}

const mnemonicWalletConfig = {
	mnemonic: {
		mnemonic:  'saddle must leg organ divide fiction cupboard nothing useless flower polar arrive',
		nonEnglishLocaleWorldList:  null
	}
}
```

#### Item

| Attribute | Type | Description | Required |
| -------------------- | -------- | --------------------------------------- | -------- |
| `description` | `string` | Description of the item that is being bought. This information is displayed in the invoice. | yes |
| `lineAmount` | `decimal`| Fiat amount of the item excluding VAT amount. | yes |
| `quantity` | `integer`| Quantity of the items | yes |
---
#### Fund Transaction Data for Fiat Payment

| Attribute | Type | Description | Required |
| -------------------- | -------- | -------------------------------------------------- | -------- |
| `weiAmount` | `string` | Amount of ethers that the shopper will be funded with | yes |
| `tokenAmount` | `string` | Amount of tokens that the shopper will be funded with| no |
---
#### Fund Transaction Data for Relayed Payment

| Attribute | Type | Description | Required |
| -------------------- | -------- | -------------------------------------------------- | -------- |
| `weiAmount` | `string` | Amount of ethers that the shopper will be funded with | Yes |
---
#### Generic Transaction

| Attribute | Type | Description | Required |
| -------------------- | -------- | ----------------------------------------------------- | -------- |
| `to` | `string `| Address that the transaction will be send to | yes |
| `gasPrice` | `string` | Gas price of the transaction | yes |
| `gasLimit` | `integer` | Gas limit of the transactions | yes |
| `functionName` | `string` | Name of the contract's function | yes |
| `functionParams` | `array` | Array of [Function parameter](#function-parameter) objects. | no |
---
### Function parameter

| Attribute | Type | Description | Required |
| -------------------- | -------- | --------------------------- | -------- |
| `value` | `any` | Value that should be passed to the function | yes |
| `type` | `string` | The solidity type of the value. For example: `bool`, `bool[]`, `address` ..... | yes |
