Response Errors
============


## Contents

- [Error Code Legend](#0-error-code-legend)
- [Authorization Errors](#1-authorization-errors)
- [Ethereum Errors](#2-blockchain-errros)
- [Not Found Errors](#3-not-found-errors)
- [Validation Errors](#4-validations-errors)
- [Global Errors](#5-global-errors)
- [Payment Errors](#6-payment-errors)
- [Vendor Errors](#7-vendor-errors)
- [Organization Errors](#8-organization-errors)
- [User Errors](#9-user-errors)

### 0. Error Code Legend

Each error code follows a specific format

- First Digit: Group of error
- Second Digit: Sub group of error
- Third Digit: If the error is due to **request parameters** or due to configuration 
- Fourth Digit: Sequent error number


### 1. Authorization Errors

| Error Name            | Code  | Message    | Description | Оccurrence |
| --------------------- | ----- |--------------- | -------- | -------- |
| `AUTHORIZATION_ERROR` | `1011` | Token verification failed. Token is not provided  | You are required to provide Lime Pay token for authorization where it is applicable | Accessing resources which requires token authorization |
| `AUTHORIZATION_ERROR` | `1012` | Token verification failed. Some part is missing | You will get this error if you provide us wrong token structure | Accessing resources which requires token authorization |
| `AUTHORIZATION_ERROR` | `1013` | Token verification failed. Invalid algorithm | You will get this error if provided token is calculated with wrong algorithm | Accessing resources which requires token authorization |
| `AUTHORIZATION_ERROR` | `1014` | Failed to verify token | You will get this error if the structure of the token is valid, but it is not actually Lime Pay token | Accessing resources which requires token authorization |
| `AUTHORIZATION_ERROR` | `1111` | Unauthorized request. Invalid API user | You will get this error if your api user authentication fails | Accessing resources which requires api user authorization |
| `AUTHORIZATION_ERROR` | `1112` | Unauthorized request. Invalid User | You will get this error if your user authentication fails | Accessing resources which requires user authorization |

### 2. Blockchain Errors

| Error Name            | Code  | Message    | Description | Оccurrence |
| --------------------- | ----- |----------- | ----------- | ---------- |
| `BLOCKCHAIN_ERROR` | `2001` | Insufficient token amount in escrow contract |  You will get this error if vendor's escrow contract does not have enough tokens to fund a shopper | Processing payment |
| `BLOCKCHAIN_ERROR` | `2002` | Insufficient wei amount in escrow contract | You will get this error if vendor's escrow contract does not have enough wei to fund a shopper | Processing payment |
| `BLOCKCHAIN_ERROR` | `2111` | Invalid gas limit | You will get this error if signed transactions gas limit is different that the relevant one provided with generic transactions | Processing payment |
| `BLOCKCHAIN_ERROR` | `2112` | Invalid receiver | You will get this error if signed transactions `to` addresses is different from the `to` addresses provided with generic transactions | Processing payment |
| `BLOCKCHAIN_ERROR` | `2113` | Invalid Gas Price | You will get this error if signed transactions gasPrice is different from the gasLimit provided with generic transactions | Processing payment |
| `BLOCKCHAIN_ERROR` | `2114` | Method ID/function signature mismatch | You will get this error if a shopper signs different function than these provided with generic transactions | Processing payment |
| `BLOCKCHAIN_ERROR` | `2115` | Function params does not match! | You will get this error if a shopper signs a function with different parameter/s than equivalent one provided with generic transactions | Processing payment |
| `BLOCKCHAIN_ERROR` | `2116` | Array of signed transactions must be provided | You are required to provide signed transactions in order to process fiat/relayed payment | Processing payment |
| `BLOCKCHAIN_ERROR` | `2117` | Invalid number of signed transactions | On payment creation, you pass generic transactions. When the shopper signs his intent, the number of transactions should be equal to the count of generic transactions | Processing payment |
| `BLOCKCHAIN_ERROR` | `2118` | Invalid signer. The transactions were not signed by the Shopper's private key | You pass signed transactions in order to process payment. Their signer( public address) should be equal to shopper's `walletAddress` | Processing payment |
| `BLOCKCHAIN_ERROR` | `2119` | Cannot decode generic transaction data! | You will get this error if a shopper somehow replace signed transactions with fake data | Processing payment |
    
### 3. Not Found Errors
    
| Error Name            | Code  | Message    | Description | Оccurrence |
| --------------------- | ----- |--------------- | -------- | -------- |
| `NOT_FOUND_ERROR` | `3010` | Payment was not found | Searching payment does not exists | Getting payment |
| `NOT_FOUND_ERROR` | `3011` | The searched payment document is not found | You will get this error if vendor's invoice functionality is disabled and you try to get an invoice | Preview invoice/receipt / send invoice |
| `NOT_FOUND_ERROR` | `3012` | Vendor was not found  | Searching vendor does not exists | Getting vendor |
| `NOT_FOUND_ERROR` | `3013` | User does not have such vendor | You will get this error if you try to create a payment for a shopper whose vendor is different than yours | Creating payment |
| `NOT_FOUND_ERROR` | `3014` | Shopper was not found | Searching shopper does not exists | Getting shopper |
| `NOT_FOUND_ERROR` | `3015` | API User was not found! | Searching api user does not exists | Getting api user |
| `NOT_FOUND_ERROR` | `3016` | User was not found | Searching user does not exists | Getting user |
| `NOT_FOUND_ERROR` | `3017` | Organization was not found! | Searching organization does not exists | Getting organization |
| `NOT_FOUND_ERROR` | `3018` | Wallet was not found | You will get this error if you try to activate a wallet which your organization does not hold | Activating vendor wallet |
| `NOT_FOUND_ERROR` | `3019` | The record you have searched for does not exists | ou will get this error if you pass invalid input, which we can not decode to determine what you are searching for | Every time we can not decode searching filter |


### 4. Validation Errors

| Error Name            | Code  | Message    | Description | Оccurrence |
| --------------------- | ----- |--------------- | -------- | -------- |
| `VALIDATION_ERROR` | `4011` | Nonexistent country code | You will get this error if you pass non-existing country code to Vendor, CardHolderInformation | Processing Fiat payment / calculating VAT  |
| `VALIDATION_ERROR` | `4111` | isCompany should be true/false | You will get this error if you pass non-boolean value to CardHolderInformation.isCompany | Processing payment |
| `VALIDATION_ERROR` | `4211` | weiAmount must have non-zero value  | You will get this error if your `fundTx.weiAmount` is zero | Creating payment |
| `VALIDATION_ERROR` | `4311` | Vat number (**your vat number**) is invalid | You will get this error if you pass invalid VAT number in CardHolderInformation | Creating vendor / Processing Fiat payment | 
| `VALIDATION_ERROR` | `4312` | Vat number is empty | You will get this error if you have vat number as property in CardHolderInformation, but it does not have a value | Creating vendor / Processing Fiat payment |

### 5. Global Errors

| Error Name            | Code  | Message    | Description | Оccurrence |
| --------------------- | ----- |--------------- | -------- | -------- |
| `INTERNAL_SERVER_ERROR` | `5001` | Something went wrong. Contact your support for more information | You will get this error if something internally breaks down | -
| `BAD_INPUT_ERROR` | `5111` | Wrong input parameters | You will get this error if you pass wrong input parameter | Every time bad input is passed |
| `REQUESTS_RATE_LIMIT_ERROR` | `5211` | Too many requests, please try again later. | You will get this error if you exceed LimePay API rate limit | Every time rate limit is exceeded |


### 6. Payment errors

| Error Name            | Code  | Message    | Description | Оccurrence |
| --------------------- | ----- |--------------- | -------- | -------- |
| `PAYMENT_ERROR` | `6011` | Payment has already been processed  | You are allowed to create a payment and process it, but not re-process it | Processing payment |
| `PAYMENT_ERROR` | `6012` | Payment can't be proceed, because shopper is malicious. |  You will get this error if a shopper is flagged as malicious one. This prevents each attempt a shopper do to interact with LimePay API | Processing payment |
| `PAYMENT_ERROR` | `6111` | Payment can't be created, while another one is in processing state | You will get this error if a shopper tries to create a new payment when already has one in processing | Creating payment |
| `PAYMENT_ERROR` | `6112` | Invalid amount | Your payment's amount should be at least bigger than `1` USD | Creating payment |
| `PAYMENT_ERROR` | `6113` | Transactions with the given currency are not allowed | You will get this error if the given payment currency code does not exists | Creating payment |
| `PAYMENT_ERROR` | `6114` | The payment token is invalid | You are required to provide payment token in order to process fiat payment | Creating payment |
| `PAYMENT_ERROR` | `6201` | Can not get payment token | You will get this error if for some reason you can not get `Payment Token`. In this case contact support! | Retrieving payment token |


### 7. Vendor Errors

| Error Name            | Code  | Message    | Description | Оccurrence |
| --------------------- | ----- |--------------- | -------- | -------- |
| `VENDOR_ERROR` | `7011` | Vendor processing status is INACTIVE  | You will get this error if you try to create a payment for inactive vendor | Creating Fiat payment | 

### 8. Organization Errors

| Error Name            | Code  | Message    | Description | Оccurrence |
| --------------------- | ----- |--------------- | -------- | -------- |
| `ORGANIZAION_ERROR` | `8001` | Wallet is not white-listed | You will get this error if you try to activate a wallet, but it is not whitelisted in the vendor's escrow contract | Activating vendor wallets |

### 9. User Errors

| Error Name            | Code  | Message    | Description | Оccurrence |
| --------------------- | ----- |--------------- | -------- | -------- |
| `USER_ERROR` | `9010` | Password contains forbidden symbols | You will get this error if you try to create a user and provided password contains forbidden symbols  Allowed symbols are `letters` / `digits` / `point` / `dash` / `under score` / `square brackets` | Creating user |



   
