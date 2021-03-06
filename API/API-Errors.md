API Errors Documentation
============

## Content

- [Authorization Errors](#1-authorization-errors)
- [Blockchain Errors](#2-blockchain-errors)
- [Not Found Errors](#3-not-found-errors)
- [Validation Errors](#4-validation-errors)
- [Global Errors](#5-global-errors)
- [Payment Errors](#6-payment-errors)
- [Shopper Errors](#7-shopper-errors)


### 1. Authorization Errors

| Error Name            | Code  | Message    | Description | Оccurrence |
| --------------------- | ----- |--------------- | -------- | -------- |
| `AUTHORIZATION_ERROR` | `1011` | Token is not provided  | You are required to provide Lime Pay token for authorization where it is applicable | Accessing resources which require token authorization |
| `AUTHORIZATION_ERROR` | `1012` | Invalid token provided | You will get this error if the token provided is invalid | Accessing resources which require token authorization |
| `AUTHORIZATION_ERROR` | `1111` | Unauthorized request | You will get this error if your non-token authorization fails | Accessing resources which require non-token authorization |

### 2. Blockchain Errors

| Error Name            | Code  | Message    | Description | Оccurrence |
| --------------------- | ----- |----------- | ----------- | ---------- |
| `BLOCKCHAIN_ERROR` | `2001` | Insufficient token amount in escrow contract | You will get this error if your organization's escrow contract does not have enough tokens to fund a shopper | Processing payment |
| `BLOCKCHAIN_ERROR` | `2002` | Insufficient wei amount in escrow contract | You will get this error if your organisation's escrow contract does not have enough wei to fund a shopper | Processing payment |
| `BLOCKCHAIN_ERROR` | `2111` | Invalid gas limit | You will get this error if the gas limit of a signed transaction does not match the provided gas limit on payment creation | Processing payment |
| `BLOCKCHAIN_ERROR` | `2112` | Invalid receiver | You will get this error if the `to` address of a signed transaction does not match the provided `to` address on payment creation | Processing payment |
| `BLOCKCHAIN_ERROR` | `2113` | Invalid gas price | You will get this error if the `gas price` of a signed transaction does not match the provided `gas price` on payment creation | Processing payment |
| `BLOCKCHAIN_ERROR` | `2114` | Method ID/function signature mismatch | You will get this error if a shopper signs different function from the one provided on payment creation | Processing payment |
| `BLOCKCHAIN_ERROR` | `2115` | Function params does not match | You will get this error if a shopper signs a function with different parameter/s from the corresponding one that was provided on payment creation | Processing payment |
| `BLOCKCHAIN_ERROR` | `2116` | Array of signed transactions must be provided | You are required to provide signed transactions in order to process fiat/relayed payment | Processing payment |
| `BLOCKCHAIN_ERROR` | `2117` | Invalid number of signed transactions | The number of signed transactions must be the same as the number of generic transactions provided on payment creation | Processing payment |
| `BLOCKCHAIN_ERROR` | `2118` | Invalid signer. The transactions were not signed by the Shopper's private key | The signer of the transactions (his public address) should be equal to the shopper's `walletAddress` | Processing payment |
| `BLOCKCHAIN_ERROR` | `2119` | Cannot decode generic transaction data! | You will get this error if a provided signed transaction is not valid or the parameter types provided on payment creation are not valid | Processing payment |
| `BLOCKCHAIN_ERROR` | `2211` | Invalid signer. The Authorization signature is not signed by signer's wallet | You will get this error if the provided authorization signature for the funding of the shopper is not signed by a `signer` of the Escrow contract that is deployed for you organization | Creating payment |
| `BLOCKCHAIN_ERROR` | `2212` | {authorizationSignature} is not a valid authorization signature | You will get this error if the provided authorization signature for the funding of the shopper does not have the correct format | Creating payment |
    
### 3. Not Found Errors
    
| Error Name            | Code  | Message    | Description | Оccurrence |
| --------------------- | ----- |--------------- | -------- | -------- |
| `NOT_FOUND_ERROR` | `3010` | Payment was not found | The requested payment does not exist | Getting payment |
| `NOT_FOUND_ERROR` | `3011` | Payment document was not found | You will get this error if you try to preview invoice/receipt or send invoice for a payment which is not successful | Preview invoice/receipt / send invoice |
| `NOT_FOUND_ERROR` | `3012` | Vendor was not found  | The requested vendor does not exist | Getting vendor |
| `NOT_FOUND_ERROR` | `3013` | Shopper was not found | The requested shopper does not exist | Getting shopper |
| `NOT_FOUND_ERROR` | `3014` | API User was not found! | The requested API user does not exist | Getting api user |
| `NOT_FOUND_ERROR` | `3015` | User was not found |The requested user does not exist | Getting user |
| `NOT_FOUND_ERROR` | `3018` | The record you have searched for does not exists | You will get this error if your searching criteria is invalid | Every time we cannot parse searching criteria |


### 4. Validation Errors

| Error Name            | Code  | Message    | Description | Оccurrence |
| --------------------- | ----- |--------------- | -------- | -------- |
| `VALIDATION_ERROR` | `4011` | Invalid property **{property}**! **{property}** cannot be null, empty or whitespace | You will get this error when you do not provide a required property in the request | Everywhere where there is request body with required properties |
| `VALIDATION_ERROR` | `4012` | Missing **{request body object}** data | You will get this error when you do not provide a required object in the request | Everywhere where there is request body with required objects |
| `VALIDATION_ERROR` | `4013` | Property **{property}** should be an array of objects  | You will get this error when a property is array of objects, but you provide something different | Everywhere where there is request body with array of objects type properties |
| `VALIDATION_ERROR` | `4111` | Nonexistent country code | You will get this error if you pass non-existing country code to Vendor, CardHolderInformation | Processing Fiat payment / calculating VAT  |
| `VALIDATION_ERROR` | `4211` | (**Input property**) should be true/false | You will get this error if you pass non-boolean value to a property which is required to be boolean | Processing payment |
| `VALIDATION_ERROR` | `4311` | WeiAmount must have non-zero value | You will get this error if your `fundTx.weiAmount` is zero | Creating payment |
| `VALIDATION_ERROR` | `4411` | Vat number **{vat number here}** is invalid | You will get this error if you pass invalid VAT number in CardHolderInformation | Creating vendor / Processing Fiat payment | 
| `VALIDATION_ERROR` | `4412` | Vat number is empty | You will get this error if you have vat number as property in CardHolderInformation, but it does not have a value | Creating vendor / Processing Fiat payment |
| `VALIDATION_ERROR` | `4413` | {vat rate} is not a valid VAR rate for your country | You will get this error if you have provided an invalid REDUCED vat rate | Calculating the VAT for a fiat payment/processing payment with a reduced vat rate |
| `VALIDATION_ERROR` | `4212` | [{some property}] should be Number | You will get this error if you have provided reduced vat rate that was not number | Calculating the VAT for a fiat payment/processing payment with a reduced vat rate |
| `VALIDATION_ERROR` | `4501` | API user credentials limit has been exceeded, limit is 3 | You will get this error if you try to create more than 3 API Credentials | Creating API Credentials |

### 5. Global Errors

| Error Name            | Code  | Message    | Description | Оccurrence |
| --------------------- | ----- |--------------- | -------- | -------- |
| `INTERNAL_SERVER_ERROR` | `5001` | Something went wrong. Contact your support for more information | General error | -
| `BAD_REQUEST_ERROR` | `5111` | Bad request | You will get this error if you pass wrong input parameter | Every time bad input is passed |
| `REQUESTS_RATE_LIMIT_ERROR` | `5211` | Too many requests, please try again later. | You will get this error if you exceed LimePay API rate limit | Every time rate limit is exceeded |


### 6. Payment Errors

| Error Name            | Code  | Message    | Description | Оccurrence |
| --------------------- | ----- |--------------- | -------- | -------- |
| `PAYMENT_ERROR` | `6001` | Signature metadata for Payment cannot be retrieved | If your escrow contract is version 1 (by default all of them are version 2), you will get this error when you execute `get signature metadata` | Getting signature metadata for payment |
| `PAYMENT_ERROR` | `6011` | Payment has already been processed  | You are allowed to create a payment and process it, but not re-process it | Processing payment |
| `PAYMENT_ERROR` | `6012` | Payment can't be processed, because shopper is malicious |  You will get this error if a shopper is flagged as malicious one. You need to contact support once this error occurs | Processing payment |
| `PAYMENT_ERROR` | `6111` | Payment can't be created, while another one is in processing state | You will get this error if you try to create new payment for a shopper, while another one is in processing state | Creating payment |
| `PAYMENT_ERROR` | `6112` | Invalid payment amount | The Payment's amount must be bigger than 1 USD| Creating payment |
| `PAYMENT_ERROR` | `6113` | Transactions with the given currency are not allowed | The provided currency is not supported | Creating payment |
| `PAYMENT_ERROR` | `6201` | Cannot get payment token | You will get this error if for some reason you cannot get `Payment Token`. In this case contact support! | Retrieving payment token |
| May Vary | `6401` | **{payment transaction error}** | You will get this error if LimePay's Payment Provider cannot process the payment for some reason. Possible Values for Error name are: `INVALID_AMOUNT`, `PAYMENT_GENERAL_FAILURE`, `VALIDATION_GENERAL_FAILURE`, `XSS_EXCEPTION`, `THREE_D_SECURITY_AUTHENTICATION_REQUIRED`, `AUTHORIZATION_AMOUNT_NOT_VALID`, `CVV_ERROR`, `EXPIRED_CARD`, `HIGH_RISK_ERROR`, `INCORRECT_INFORMATION`, `INSUFFICIENT_FUNDS`, `INVALID_CARD_NUMBER`, `INVALID_CARD_TYPE`, `LIMIT_EXCEEDED`, `RESTRICTED_CARD`, `THREE_D_SECURE_FAILURE` | Processing Fiat payment |


### 7. Shopper Errors

| Error Name            | Code  | Message    | Description | Оccurrence |
| --------------------- | ----- |--------------- | -------- | -------- |
| `SHOPPER_ERROR` | `10011` | Shopper is invalid | You will get this error if you try to create a payment for invalid shopper | Creating payment |
| `SHOPPER_ERROR` | `10021` | Cannot update the wallet of a Shopper that is using LimePay powered Wallet | You will get this error if you try to update a Shopper's `walletAddress` but the shopper uses LimePay powered wallet | Updating Shopper |
| `SHOPPER_ERROR` | `10031` | Cannot get Encrypted wallet for Shopper that is not using LimePay powered wallet | You will get this error if you try to get/save/change the Encrypted Wallet of a Shopper that do not use LimePay powered wallets | GET/CREATE/Change the encrypted Wallet of a shopper |
| `SHOPPER_ERROR` | `10041` | No Wallet is saved for the requested shopper. Create the wallet first | You will get this error if you try to create a payment, get or change the encrypted wallet of a Shopper that does not have LimePay powered wallet yet | Create Payment, GET Encrypted Wallet, Change the password of a Encrypted wallet |
| `SHOPPER_ERROR` | `10051` | Shopper already has LimePay powered wallet | When you try to create Encrypted Wallet for a Shopper that already has one | Create Encrypted Wallet for a Shopper |
| `SHOPPER_ERROR` | `10061` | Invalid mnemonic was provided | When you want to change the password of a Encrypted Wallet for a Shopper and you provide wrong mnemonic  | Change the password of a encrypted wallet |
