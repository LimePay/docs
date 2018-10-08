Getting Started
============

## Contents

- [Create Organization](#create-organization)
- [Create User](#create-user)
- [Create API User](#create-api-user)
- [Create Vendor](#create-vendor)
- [Create Payment](#create-payment)

In order to process payments through credit card, one would need to have organization, user, vendor and api-user created in LimePay.
The steps that are required for the creation and configuration are described below. 
___
### 1. Create Organization
The creation of organization is performed by LimePay's support and it is part of the onboarding process.
___

### 2. Create User

The creation of organization is performed by LimePay's support and it is part of the onboarding process.

___

Once you have a registered Organization and user, you could proceed with the integration.

### 3. Create API User

The API user is a pair of `API Key` and `secret` that is used for authentication. In order for you to create `API User` you will need your `user` credentials.

For more information on how to get API credentials, see the `API Users` resource in the `API Documentation`
___

### 4. Create Vendor

The next step is to create a vendor which is used to register you in our Payment provider for processing the card payments. 

For more information on how to create vendor, see the `Vendors` resource in the `API Documentation`

___

### 5. Create Shopper

Whenever you are charging a new user, you must register him as a shopper in LimePay. Once registered you do not need to register him again for every charge after that. If the user is a already registered and you have already charged him you do not need to register him as a shopper in LimePay.
**Important** to notice is that whenever you are creating a shopper you provide the `walletAddress`.
This address will be then funded whenever you are creating Payments for that specific shopper.
**The shopper's wallet address should be the same as the one who signs transactions. If this is not the case, the payment will `fail`.**  

For more information on how to create a shopper, see the `Shoppers` resource in the `API Documentation`

### 6. Create Payment

Once you have completed the steps above (have Organization, User, API cretendtials, Vendor and a Shopper), you could proceed with creating a payment.
Whenever you create a Payment, you must provide the following data: 
 - `vendor` - The Vendor who will charge the shopper's credit card and will receive the fiat funds.
 - `shopper` - The Shopper who will be charged
 - `fundTXData` - This object represents the `token` and `ethers` amount that will be needed for the Payment to be executed.

If your payment requires for example `100 tokens X` and no `ethers` (meaning that you charge only with `tokens`), the value for `tokenAmount` must be set to `100` and the value for `ethersAmount` should be set to `Y` where `Y = gasPrice * required gas`. Simply put: the `token` and `ethers` are the amounts that we are going to fund the `shopper's` wallet with. If the values are not correct, after we fund the shopper and broadcast his _signed transactions_[[1](#1)] , they will fail. If the `ethers` amount is not enought for the execution of the shopper's _signed transactions_[[1](#1)]  - the payment will `fail`. 

**As a result of the request you will receive `x-lime-token` as a header parameter. This token must be used to initialize the check-out form!**

**Summary:** The values that you provide for `tokenAmount` and `etherAmount` are crucial. In the `etherAmount` value you not only set the `ethers` that your signed transactions will charge the shopper, but the `required gas` for the execution of the _signed transactions_[[1](#1)] as-well. 

For more information on how to create a payment, see the `Payments` Resource in the `API Documentation`

[[1](#1)] - The set of transactions that must be executed by the shopper in order for him to buy the service/product. If your dApp charges `tokens`, the first transaction must be `approve` transaction executed at the `Token` contract  and the second one - executing transaction for buying your product/service. 
**Important:** The transactions are provided as an array and are executed sequentially in their order in the array!

### 7. Initialize LimePay's Checkout form

1) You will need to install the npm library - `limepay-web`

2) Define your `config` object: 
The config object must have the following structure:
```javascript
let limePayConfig = {
    signingTxCallback: function () {
        // Function that returns array of signed transactions
        returns [];
    },
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

The property `signingTxCallback` must be a function that is performing the signing of the transactions. It must return array of the **signed** transactions.
**Important**: The transactions are executed sequentially, meaning that the execution starts with the first transaction in the array and finishes with the last transaction in the array!

3) Initialize the checkout:
```javascript
let LimePayWeb = require('limepay-web');

LimePayWeb.init(limeToken, limePayConfig).catch((err) => {
    alert('Form initialization failed');
    // Implement some logic
});
```

The `.init()` has 2 parameters. The first parameter is the `x-lime-token` that is received when you create your payment (described in [6. Create Payment](#create-payment)) and the second one is the `config` object that we described in step `2)`
