
## **Processing Payments**

### Contents

- [Requirements](#1-requirements)
- [Payments](#2-payments)
- [Creating Payments](#3-creating-payments)
	- [Payment Data](#3.1-payment-data)
- [Processing Payments](#4-payment)
	- [Fiat Payment](#4.1-fiat-payment)
	- [Relayed Payment](#4.2-relayed-payment)
- [Additional Info](#5-additional-info)
	- [Wallet Configurations](#5.1-wallet-configurations)
	- [Signing Transactions](#5.2-signing-transactions)

This document contains detailed information on what the prerequisites of processing LimePay Payments are, how to create and process them. 

If you are not familiar with types of payments, check the [Getting Started](https://github.com/LimePay/docs/blob/master/Getting-Started.md#6-payments) document. 

### 1. Requirements
The requirements for processing LimePay payments, no matter the type are described below with their corresponding:
 - Your Escrow contract must be deployed - *Performed by LimePay's on-boarding team, with information provide by the dApp owner.*
 - The Escrow contract must be funded with ethers and tokens (if applicable) - *Performed by the dApp owner.*
 - The Signers of the Escrow contract must be configured - *Performed by the dApp owner.*
 - The Vendor of the organisation must be created and verified- *Performed by LimePay's on-boarding team, with information provided by the dApp owner.*
 - Once provided with a user, the dApp owner must create his API Credentials in order to consume the LimePay REST API

### 2. Payments

Payments are created every time you want your users to execute Ethereum transactions using LimePay. There are two types of payments: `Fiat` and `Relayed`. Depending on the use-case you would want to process the one or the other.

`Fiat` payments are used when your user has to be charged with credit/debit card, because he does not have ethers/tokens at all. In this case the user pays for the ethers and tokens that are required in order for him to execute the required Ethereum transactions, rendering the service that you are providing.

`Relayed` payments are used when your user has tokens for the services he wants to use, but does not have ether for the gas costs, therefore he cannot execute the transactions even though he has the tokens. In this scenario, you as a service provider decide to pay the gas costs of the user in order for him to be able to consume your service. Therefore the user is not charged with fiat funds.

### 3. Creating Payments

The creation of payments is done via the JavaScript SDK or using plain REST-API calls.

More information on the JavaScript SDK can be found in the [JavaScript SDK]() documentation TODO. 

```javascript
const LimePay = require('limepay');
LimePay.connect({
	environment: LimePay.Environment.Production, // or LimePay.Environment.Sandbox
	apiKey: 'YOUR_API_KEY_HERE',
	secret: 'YOUR_API_SECRET_HERE'	
}); // - returns promise

...

// Creating Fiat Payment
LimePay.fiatPayment.create(fiatPaymentData, signerWalletConfig) // - returns promise
	.then(payment => { }) // the created payment is returned once resolved
	.catch(error => { });

// Creating Relayed Payment
LimePay.relayedPayment.create(relayedPaymentData, signerWalletConfig) // - returns promise
	.then(payment => { }) // the created payment is returned once resolved
	.catch(error => { });
```

`signerWalletConfig`  - object containing wallet information. TODO

#### 3.1 Payment Data
Properties that you must provide when creating the different kinds of payments
- `shopper` - The Shopper ID who will be charged
- `currency` - The currency in which the amounts are calculated **(NOT required for `relayed` payment)**
- `items` - Array of `item` objects, containing `description`, `lineAmount` and `quantity`  **(NOT required for `relayed` payment)**
- `fundTXData` - This object represents the `tokenAmount` and `weiAmount` that will be needed for the Payment to be executed.
- `genericTransactions` - This is an array of `genericTransaction` objects each representing single generic transactions that the shopper will later on have to sign. 
Each of the `genericTransaction` objects consist of `gasPrice`, `gasLimit`, `to`, `functionName` and `functionParams`. These parameters are used for validation of the signed transactions that will be sent from the UI (signed by the shopper)
 
*Example for Fiat Payment Data:*
```javascript
const fiatPaymentData = {
	currency: "USD", // Currency code
	shopper: "SHOPPER_ID", // the id of the shopper 
	items: [
		{
			description: "Description of the item that is being bough",
			lineAmount: 100, // fiat amount the user will be charged in the specified currency
			quantity: 1
		}
	],
	fundTxData: {
		tokenAmount:"10000000000000000000", // Shopper will be funded with this amount of tokens
		weiAmount: "10000000000000000" // Shopper will be funded with this amount of wei
	},
	genericTransactions: [
		{
			gasPrice: "1000000000",
			gasLimit: 4700000,
			to: "0xc8b06aA70161810e00bFd283eDc68B1df1082301", // Your ERC20 Contract address
			functionName: "approve",
			functionParams: [
				{type: 'address', value: "0x1835f2716ba8f3ede4180c88286b27f070efe985"}, // Your dApp contract address
				{type: 'uint', value:  '10000000000000000000'}
			]
		},
		{
			gasPrice: "1000000000",
			gasLimit: 4700000,
			to: "0x1835f2716ba8f3ede4180c88286b27f070efe985", // Your dApp contract address
			functionName: "buySomeService",
			functionParams: [] // Your specific parameters here
		}
	]
}

```

*Example for Relayed Payment Data:*
```javascript
const relayedPaymentData = {
	shopper: "SHOPPER_ID", // the id of the shopper 
	fundTxData: {
		weiAmount: "10000000000000000" // Shopper will be funded with this amount of wei
	},
	genericTransactions: [
		{
			gasPrice: "1000000000",
			gasLimit: 4700000,
			to: "0xc8b06aA70161810e00bFd283eDc68B1df1082301", // Your ERC20 Contract address
			functionName: "approve",
			functionParams: [
				{type: 'address', value: "0x1835f2716ba8f3ede4180c88286b27f070efe985"}, // Your dApp contract address
				{type: 'uint', value:  '10000000000000000000'}
			]
		},
		{
			gasPrice: "1000000000",
			gasLimit: 4700000,
			to: "0x1835f2716ba8f3ede4180c88286b27f070efe985", // Your dApp contract address
			functionName: "buySomeService",
			functionParams: [] // Your specific parameters here
		}
	]
}
```

The following example will create a payment that, once processed will charge the shopper with $100, fund his address with  `10` tokens and  `0.01` ethers and execute the following transactions on his behalf:
 1. Execute `approve` transaction for the address of the dApp contract to spend `10` tokens
 2. Execute `buySomeService` transaction to the dApp's contract

**NOTICE** The amount of wei specified in `weiAmount` must include the possible transaction fees for the `genericTransactions` as-well. If the value for `weiAmount` is not enough the `genericTransactions` will result in failure. 

### 4. Processing Payments

Once you have created the payment, you will receive `limeToken` as part of the response. The token is used to initialise the `limepay-web` library and process the payments.

Installing the UI library: 

    npm install limepay-web
	
Once installed you need to create your configuration object:
The config object must have the following structure:

```javascript
const LimePayWeb = require('limepay-web');
const limePayConfig = {
	environment: LimePayWeb.Environment.Production, // or LimePayWeb.Environment.Sandbox 
	eventHandler: {
		onSuccessfulSubmit: () => {
			// Implement some logic
		},
		onFailedSubmit: (error) => {
			// Implement some logic
		}
	}
}
```

The property `environment` sets the Environment of the LimePay API and the properties defined in `eventHandler` - `onSuccessfulSubmit` and `onFailedSubmit` are event handlers that are called once a payment has been submitted or fails.

#### 4.1 Fiat Payments
In order for your dApp to be able to process fiat payments, you need to integrate the following HTML form.
```HTML
<form id="checkout-form" onsubmit="processPayment()">
	<div class="row">
		<!--Field for Credit card number -->
		<div class="form-group col-xs-9">
		<label for="card-number">Card Number</label>
			<div class="input-group">
				<div class="form-control" id="card-number" data-bluesnap="ccn"></div>
				<div id="card-logo" class="input-group-addon">
					<img src="https://files.readme.io/d1a25b4-generic-card.png" height="20px">
				</div>
			</div>
			<span class="helper-text" id="ccn-help"></span>
		</div>
		
		<!--Field for CVV-->
		<div class="form-group col-xs-3">
			<label for="cvv">CVV</label>
			<div class="form-control" id="cvv" data-bluesnap="cvv"></div>
			<span class="helper-text" id="cvv-help"></span>
		</div>

		<!--Field for EXP-->
		<div class="form-group col-xs-6">
			<label for="exp-date">Exp. (MM/YY)</label>
			<div class="form-control" id="exp-date" data-bluesnap="exp"></div>
			<span class="helper-text" id="exp-help"></span>
		</div>
	</div>
	<button class="btn btn-raised btn-info btn-lg col-md-4" type="submit" id="submit-button">Pay Now</button>
</form>
```
  

Must have requirements:
- Have a form, with attributes `id="checkout-form"`.
- Have element with attributes `id="card-number"` and `data-bluesnap="ccn"`
- Have element with attributes `id="cvv"` and `data-bluesnap="cvv"`
- Have element with attributes `id="exp-date"` and `data-bluesnap="exp"`

**NOTE**: `processPayment()` is a function that you define and implement.

#### **Three step processing of fiat payments**
The following three steps must be perform so that you successfully process the fiat payment

 1. Initialise the fiat payment
 2. Sign the desired Ethereum transactions
 3. Process the payment

**Step 1. Initialise the fiat payment** 
Initialising the payment through the library: 
```javascript
const LimePayWeb = require('limepay-web');
LimePayWeb.initFiatPayment(limeToken, limePayConfig) // returns promise
	.then(fiatPayment => {}) // fiatPayment object is returned once resolved 
	.catch(error => {});
```
Where `limeToken` is the token you received on payment creation.

**Step 2. Sign the desired Ethereum transactions** 

```javascript
fiatPayment.buildSignedTransactions(walletConfiguration, transactions) // returns promise
		.then(() => {})
		.catch(error => {});		
```

Additional information on `walletConfiguration` and `transactions` can be found in the [Wallet Configuration](#5.1-wallet-configuration) and [Signing Transactions](#5.2-signing-transactions) sections. 

**Step 3. Processing the Fiat Payment** 
Once the user fills the required card data and submits the credit card data form you must process the payment:
```javascript
function processPayment () {
	...
	const cardHolderInfo = {
		name: "Customer or Business name", // required field
		countryCode: "us",  // Country code. required field
		vatNumber:  "VAT NUMBER",  // VAT Number provided by the customer. Not required
		zip: "ZIP CODE",  // required field
		street: "street address of the shopper/business"  // required field
		isCompany: true  // true/false. Required field
	} 
	fiatPayment.processPayment(cardHolderInfo); // triggers the processing of the payment 
}
```
Once the payment is successfully sent for processing, an `onSuccessfulSubmit` event will be triggered. Respectively, if an error occurs, `onFailedSubmit` event will be triggered. 

You can monitor the status of the payment through the `limepay` SDK. More information on how to `GET` payments in the [LimePay SDK]() TODO documentation.

#### 4.2 Relayed Payments

#### **Three step processing of relayed payments**
The following three steps must be perform so that you successfully process relayed payment
 1. Initialise the relayed payment
 2. Sign the desired Ethereum transactions
 3. Process the payment

**Step 1. Initialise the relayed payment** 
Initialising the payment through the library: 
```javascript
const LimePayWeb = require('limepay-web');
LimePayWeb.initRelayedPayment(limeToken, limePayConfig) // returns promise
	.then(relayedPayment => {}) // relayedPayment object is returned once resolved 
	.catch(error => {});
```
Where `limeToken` is the token you received on payment creation.

**Step 2. Sign the desired Ethereum transactions** 

```javascript
relayedPayment.buildSignedTransactions(walletConfiguration, transactions) // returns promise
		.then(() => {})
		.catch(error => {});		
```

Additional information on `walletConfiguration` and `transactions` can be found in the [Wallet Configuration](#5.1-wallet-configuration) and [Signing Transactions](#5.2-signing-transactions) sections. 

**Step 3. Processing the Relayed Payment** 
```javascript
relayedPayment.processRelayedPayment(); // triggers the processing of the payment 
```
Once the payment is successfully sent for processing, an  `onSuccessfulSubmit`  event will be triggered. Respectively, if an error occurs,  `onFailedSubmit`  event will be triggered.

You can monitor the status of the payment through the  `limepay`  SDK. More information on how to  `GET`  payments in the  [LimePay SDK](https://stackedit.io/app)  TODO documentation.

### 5. Additional Info
#### 5.1 Wallet Configuration

In order for you to be able to process payments you need to provide the transactions, signed by the user's private key.
The `.buildSignedTransactions` functionality for payments is flexible and supports different wallet configurations of your user's wallet. Below are some examples of the different wallet configurations.

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

#### 5.2 Signing Transactions

In order for you to process payments you need to provide the initialised payment with the signed (by the shopper's private key) Ethereum transactions. For the sake of convenience, `limepay-web` supports signing Ethereum transactions  and directly providing them to the payment.

*Example:*
```javascript
payment.buildSignedTransactions(walletConfig, transactions) // returns promise
```

**Note:** Keep in mind that the provided `wallet` will be the **signer** of the transactions, meaning that this must be the wallet of the shopper that is going to be charged and is registered as `shopper` in LimePay. It is your responsibility to keep the wallet address up-to-date, meaning that if you change the `wallet` of your shopper, you must change `walletAddress` of the shopper in LimePay.

The `transactions` object is an array of `transaction` objects, each containing the required data so that they can be transformed into signed Ethereum transactions.

```javascript
const transaction = {
	to: 'ethereumAddress', // The 'to' address of the transaction
	abi: ContractABI, // The ABI of the contract that is deployed at the `to` address
	gasLimit: 4700000, // The gasLimit that will be used
	value: 0, // amount of ethers that are going to be sent along with the transaction
	functionName: "approve", // name of the function that will be called
	functionParams:  [...params], // values of the parameters that are going to be passed to the function
}
```

The example below demonstrates how you can build the signed transactions and provide them to the payment. There are two transactions that are being build.

 1. ERC20 Approve transaction that will approve the dApp contract to spend ERC20 Tokens on the behalf of the dApp user
 2. Generic transaction that will "buy some service" from the dApp contract. Once called `buySomeService` executes `transferFrom` transaction and charges the dApp user with tokens for the service. 

```javascript
const transactions = [{
		to:'0xc8b06aA70161810e00bFd283eDc68B1df1082301', //ERC20 Contract address
		abi: ERC20ABI,
		gasLimit:  4700000,
		value: 0,
		functionName: "approve",
		functionParams:  ["0x07F3fB05d8b7aF49450ee675A26A01592F922734", 10000000000000000000] // tokens in 18 decimals format
	},
	{
		to: '0x07F3fB05d8b7aF49450ee675A26A01592F922734', // Address of the dApp contract
		abi: dAppContractABI,
		gasLimit: 4700000,
		value: 0,
		functionName: "buySomeService",
		functionParams:  ["0x1835f2716ba8f3ede4180c88286b27f070efe985"] // Parameter provided to the `buySomeService` function
	}
];
payment.buildSignedTransactions(walletConfiguration, transactions);
```

**Note:** The provided transactions must be in the right order as they will be executed sequentially. In the case above, the first will be the `approve` transaction and then it will be the `buySomeService` transaction. 
 
**Note:** For security reasons, the provided data for the transactions MUST match the provided transactions data when you have created the payment through the `limepay` SDK (described in [Creating Payments](#3-creating-payments) section. If the data does not match, the payment will result in failure (the user will not be charged nor funded through the Escrow)