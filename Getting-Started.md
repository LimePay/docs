Getting Started
============

## Contents

- [Create Organization](#1-create-organization)
- [Create User](#2-create-user)
- [Create API User](#3-create-api-user)
- [Create Vendor](#4-create-vendor)
- [Create Shopper](#5-create-shopper)
- [Create Payment](#6-payments)
- [Utils](#7-utils)
- [Signing Transactions](#8-signing-transactions)
- [Invoice/Receipt Configuration](#9-invoicereceipt-configuration)

In order to process payments through credit card, one would need to have organization, user, vendor and api-user created in LimePay.
The steps that are required for the creation and configuration are described below. 
___
### 1. Create Organization
The creation of organization is performed by LimePay's support and it is part of the on-boarding process.
___

### 2. Create User

The creation of user is performed by LimePay's support and it is part of the on-boarding process.
___

### 3. Create API User

The creation of API Credentials is performed by LimePay's support and it is part of the on-boarding process.
___

### 4. Create Vendor

The creation of Vendor is performed by LimePay's support and it is part of the on-boarding process.  
There are some [invoice/receipt configuration](#10-invoicereceipt-configuration) that you might want to be included in the on-boarding process.
___

### 5. Create Shopper

Whenever you are charging a new user, you must register him as a shopper in LimePay. Once registered you do not need to register him again for every charge after that. If the user is already registered/created you do not need to register him again.
**Important** to notice is that whenever you are creating a shopper you provide the `walletAddress`.
This address will be then funded whenever you are creating Payments for that specific shopper.
**The shopper's wallet address should be the same as the one who signs transactions. If this is not the case, the payment will `fail`.**  

For more information on how to create a shopper, see the [Shoppers](https://github.com/LimePay/docs/blob/initial-documentation/API-Documentation.md#shopper) resource in the `API Documentation`
___

### 6. Payments

There are two types of payments - `Fiat` and `Relayed` payments.
- `Fiat` Payments are used when your user has to be charged with credit card, because he/she does not have ethers/tokens at all, therefore the users
pay for the ethers and tokens and their intents (blockchain transactions) are executed afterwards
- `Relayed` Payments are used when your user has tokens for the services he wants to use but does not have ethers for the gas costs, therefore he cannot execute the transactions even though he has the tokens. In this scenario, you as a service provider decide to pay the gas costs of the user in order for him to be able to consume your service. 

##### 6.1. Create Payments

Once you have been on-boarded (have Organization, User, API cretendtials and Vendor) and created a Shopper, you could proceed with creating payments.

Properties that you must provide when creating different payments 
 - `shopper` - The Shopper who will be charged
 - `currency` - The currency in which the amounts are calculated **(NOT required for `relayed` payment)**
 - `items` - Array of `item` objects, containing `description`, `lineAmount` and `quantity` **(NOT required for `relayed` payment)**
 - `fundTXData` - This object represents the `token` and `ethers` amount that will be needed for the Payment to be executed.
 - `genericTransactions` - This is an array of `genericTransaction` objects that represent a single generic transaction that the shopper will later on have to sign. The data in the `genericTransaction` consists of `gasPrice`, `gasLimit`, `to`, `functionName` and `functionParams`. These parameters are used for validation of the signed transactions that will be sent from the UI (signed by the shopper).

If your payment requires for example `100 tokens X` and no `ethers` (meaning that you charge only with `tokens`), the value for `tokenAmount` must be set to `100` and the value for `ethersAmount` should be set to `Y` where `Y = gasPrice * required gas`.
Simply put - the `token` and `ethers` are the amounts that we are going to fund the `shopper's` wallet with. If the values are not correct, after we fund the shopper and broadcast his _signed transactions_[[1](#1)] , they will fail. If the `ethers` amount is not enough for the execution of the shopper's _signed transactions_[[1](#1)]  - the payment will `fail`. 

###### Fiat Payments
Whenever you create a Fiat Payment, you must provide the following data - `shopper`, `currency`, `items`, `fundTXData`, `genericTransactions`

###### Relayed Payments
Whenever you create a Relayed Payment, you must provide the following data - `shopper`, `fundTXData`, `genericTransactions`

**As a result of the request you will receive `x-lime-token` as a header parameter. This token is used to initialize the limepay-web library and process the payments!**

**Summary:** The values that you provide for `tokenAmount` and `etherAmount` are crucial. In the `etherAmount` value you not only set the `ethers` that your signed transactions will charge the shopper, but the `required gas` for the execution of the _signed transactions_[[1](#1)] as-well. 

For more information on how to create a payments, see the `Payments` Resource in the `API Documentation`

[[1](#1)] - The set of transactions that must be executed by the shopper in order for him to buy the service/product. For example, if your dApp charges `tokens`, the first transaction must be `approve` transaction executed at the `Token` contract  and the second one - transaction for buying your product/service.<br/>
**Important:** The transactions are provided as an array and are executed sequentially in their order in the array!

___

##### 6.2. Initialize LimePay's Checkout form (only for Fiat Payments)

1) Add the HTML Form in your application.
You are required to add the following HTML form into your applicaiton:

```HTML
    <form id="checkout-form" onsubmit="processPayment()">
        <div class="row">
            <!--Field for CCN -->
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

**Note**: `processPayment()` is a function that you define and implement.

##### 6.3. Initialize LimePay web

1) You will need to `npm install limepay-web` and `require` it OR include the provided `lime-pay.min.js`

2) Define your `config` object: 
The config object must have the following structure:
```javascript
let limePayConfig = {
    URL: "base url of limepay"
    eventHandler: {
        onSuccessfulSubmit: function () {
            alert('Your payment was send for processing');
            // Implement some logic
        },
        onFailedSubmit: function (err) {
            alert('Your payment failed');
            // Implement some logic
        }
    }
}
```

The property `URL` sets the base url for all of the requests towards LimePay API and the properties defined in `eventHandler` - `onSuccessfulSubmit` and `onFailedSubmit` are event handlers that are called once a payment has been submitted or fails respectively.

3) Initialize the checkout:
The provided sample below is the `require` and not the `lime-pay.min.js` option

```javascript
let LimePayWeb = require('limepay-web');

// For Fiat Payments
LimePayWeb.init(limeToken, limePayConfig).catch((err) => {
    alert('Form initialization failed');
    // Implement some logic
});

// For Relayed Payments
LimePayWeb.initRelayedPayment(limeToken, limePayConfig).catch((err) => {
    alert('Form initialization failed');
    // Implement some logic
});
```

The `.init()` and `.initRelayedPayment()` have 2 parameters. The first parameter is the `x-lime-token` that is received when you create your payment (described in [6. Create Payment](#create-payment)) and the second one is the `config` object that we described in step `2)`

##### 6.4. Processing Fiat Payment

Once `processPayment()` is called (based on our example above), one must call `LimePayWeb.PaymentService.processPayment()` and provide the 2 required parameters - `cardHolderInformation` and `signedTransactions`. 

`cardHolderInformation` is an object with the following properties: 
```javascript
let cardHolderInformation = {
    vatNumber: "123123" // VAT Number provided by the customer. Not required
    name: "" // Customer or Business Name. Required field
    countryCode: "us" // Country code. Required field
    zip: "1414" // ZIP code of the shopper/business. Required field
    street: "some address"  // Street address of the shopper/business. Required field
    isCompany: true // true/false properties. Required field
}
```

`signedTransactions` is an array of **signed** transactions. More details of how one could sign the transactions can be found in [signing transactions](#signing-transactions) section.
**Important**: The transactions are executed sequentially, meaning that the execution starts with the first transaction in the array and finishes with the last transaction in the array!

___

##### 6.4. Processing Relayed Payment
In order for one to process a relayed payment one should execute `LimePayWeb.PaymentService.processRelayedPayment(signedTransactions)` and provide the signed transactions as parameter.

`signedTransactions` is an array of **signed** transactions. More details of how one could sign the transactions can be found in [signing transactions](#signing-transactions) section.
**Important**: The transactions are executed sequentially, meaning that the execution starts with the first transaction in the array and finishes with the last transaction in the array!

___


### 7 Utils

##### 7.1 Calculate VAT

Once you initialize LimePayWeb you can get the vat/tax data (if any) for a payment.  
**Note!** When you create a new payment, VAT/TAX is not calculated, so you can use this method to calculate and show to your users the **TOTAL amount** they will be charged. 

```javascript
let cardHoldeVATData = {
    countryCode: "us", //required
    isCompany: true/false, // required
    vatNumber: 123456789 // optional
}

let vatResult = await LimePayWeb.utils.calculateVAT(cardHoldeVATData);

/* 
	vatResult contains => 
      {
           rate (VAT rate for the payment. Depends on the country of the Vendor)
           totalVAT (the vat amount for a payment)
           totalAmount (payment amount + totalVAT)
      }   
*/
```

___

### 8. Signing Transactions

This section describes how we can define/implement the signing of the transactions.

The `limepay-web` library provides helper function to ease the development efforts for providing the signed transactions array.
The provided sample below shows how you can achieve the signing.

You can use the `TransactionsBuilder` tool in `limepay-web` to sign transactions.

```javascript
// examples of wallet configuration
let walletConfigurationEncryptedWallet = {
    encryptedWallet: {
        jsonWallet: wallet,
        password: password
    }
}

let walletConfigurationDecryptedWallet = {
    decryptedWallet: wallet // Decrypted wallet for example created from ethers.Wallet.createRandom()
}

let walletConfigurationPrivateKey = {
    privateKey: 'private key here'
}

let walletConfigurationMnemonic = {
    mnemonic: {
        mnemonic: 'saddle must leg organ divide fiction cupboard nothing useless flower polar arrive', // || 'alloggio calvario danzare entrare episodio maggio pinguino motivare rotta vero zoccolo svolta'
        nonEnglishLocaleWorldList: null // default value is 'null' for english, if mnemonic is in italian, these field should be 'it' 
    }
}

let txBuilder = LimePayWeb.TransactionsBuilder.buildSignedTransactions(walletConfiguration, transactions);
```

The `TransactionsBuilder` has 2 parameters. The first one is the `walletConfiguration` - this configuration should have one of those properties (encryptedWallet, decryptedWallet, privateKey, mnemonic). `encryptedWallet` is an object that has `jsonWallet` and `password` that unlocks the JSON wallet. `decryptedWallet` is unlocked wallet that can be used directly without any processing. 
If you use `privateKey` or `mnemonic` configuration, the library will load the wallet from the `privateKey` or the `mnemonic` and the loaded wallet will be used to sign the transactions. 
The `mnemonic` configuration is an object with `mnemonic`(word list phrase) and `nonEnglishLocaleWorldList`(default value is null or two letter country code of used mnemonic language) properties.
The second parameter is the array of the transactions that will be signed.

**Note:** Keep in mind that the provided `Wallet` will be the **signer** of the transactions, meaning that this must be the wallet of the shopper that is going to be charged, and is registered as `shopper` in LimePay. It is your responsibility to keep the wallet address up-to-date, meaning that if you change the `Wallet` of your shopper, you must change `walletAddress` of the shopper in LimePay.

`buildSignedTransactions` returns a Promise. If resolved the result will be array of signed transactions. 

The transactions array contains objects of type:
```javascript
let transaction = {
    to: 'ethereumAddress', // The to address of the transaction
    abi: tokenABI, // The ABI of the contract that is deployed at the to address
    gasLimit: 4700000, // The gasLimit of the transaction
    value: 0, // The ethers that are going to be sent along with the transaction
    functionName: "approve", // The name of the function that must be called
    functionParams: [...params], // The parameters that are going to be passed to the function
}
```

The whole signing process is shown in the sample below:

```javascript
let transactions = [
    {
        to: '0xc8b06aA70161810e00bFd283eDc68B1df1082301',
        abi: tokenABI,
        gasLimit: 4700000,
        value: 0,
        functionName: "approve",
        functionParams: ["0x07F3fB05d8b7aF49450ee675A26A01592F922734", 1]
    },
    {
        to: '0x07F3fB05d8b7aF49450ee675A26A01592F922734',
        abi: contractABI,
        gasLimit: 4700000,
        value: 0,
        functionName: "buySomeService",
        functionParams: ["0x1835f2716ba8f3ede4180c88286b27f070efe985"]
    }
];

let result = await LimePayWeb.TransactionsBuilder.buildSignedTransactions(walletConfiguration, transactions);
// The result will be ["rawSignedTransaction0", "rawSignedTransaction1"]
```

In this example, the first transaction will be `approve` transaction that is going to call the Token contract and `approve` the service contract to `charge` 1 token.

The second transaction will call the service contract and execute the `buySomeService` function.

___


### 9. Invoice/Receipt configuration

This section describes how one can activate and configure invoice/receipt feature so one's customers can receive emails.

* ##### Activation steps 

1. You have to provide the on-boarding team a valid email [(receiptEmail)](https://github.com/LimePay/docs/blob/master/API-Documentation.md#vendor) from which LimePay will send invoices/receipts to your shoppers.    

	In order for you to automatically send invoices or receipts on successful payment through LimePay you must request the features from the on-boarding team

	1) Tell on-boarding team to activate automatic invoice feature
	2) Tell on-boarding team to activate automatic receipt feature
  
**Important!**  You can manually send an invoice via [rest API call](https://github.com/LimePay/docs/blob/master/API-Documentation.md#52-send-invoice), but the receipts however can be send only if the **automatic receipt** feature is enabled.


* ##### Configuration steps

**Important!** If you do not provide the on-boarding team with the configuration that you want, the default one will be used.

There are templates for four different components: 
1. Invoice email subject. ( **Default** - Invoice )
2. Receipt email subject. ( **Default** - Receipt )
3. Invoice body header text. ( **Default** - Thank you for your purchase. Below is your invoice! )
4. Receipt body header text. ( **Default** - Thank you for your purchase. Below is your receipt! )

Body header text - This is the text in email body shown on top of the invoice/receipt.



#### For more information and examples of how to integrate with LimePay, you can check the [sample-projects](https://github.com/LimePay/sample-projects) repository
