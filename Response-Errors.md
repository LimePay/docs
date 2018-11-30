Response Errors
============


## Contents

- [Authorization Errors](#1-authorization-errors)
- [Transaction Errors](#2-transaction-errors)
- [Ethereum Errors](#3-ethereum-errros)
- [Not Found Errors](#4-not-found-errors)
- [Validation Errors](#5-validations-errors)
- [Payment Errors](#6-payment-errors)
- [Vendor Errors](#7-vendor-errors)
- [Organization Errors](#8-organization-errors)
- [User Errors](#9-user-errors)
- [Global Errors](#10-global-errors)

### 1. Authorization Errors
Each of these errors could happen when API route is accessed with `Lime Pay` token

TODO: 1000 - 1100 token errors
TODO: 1100 - 1200 user/api-user errors

| Error Name            | Code  | Message    | Description | Оccurrence |
| --------------------- | ----- |--------------- | -------- | -------- |
| `AUTHORIZATION_ERROR` | `1010` | Token verification failed. Token is not provided  | You are required to provide Lime Pay token for authorization where it is applicable |
| `AUTHORIZATION_ERROR` | `1020` | Token verification failed. Some part is missing | You will get this error if you provide us wrong token structure |
| `AUTHORIZATION_ERROR` | `1030` | Token verification failed. Invalid algorithm | You will get this error if provided token is calculated with wrong algorithm |
| `AUTHORIZATION_ERROR` | `1040` | Failed to verify token | You will get this error if the structure of the token is valid, but it is not actually Lime Pay token |
| `AUTHORIZATION_ERROR` | `1110` | Unauthorized request. Invalid API user | You will get this error if your api user authentication fails |
| `AUTHORIZATION_ERROR` | `1120` | Unauthorized request. Invalid User | You will get this error if your user authentication fails |


### 2. Transaction errors
Each of these errors could happen in case of invalid payment data
    
| Error Name            | Code  | Message    | Description | Оccurrence |
| --------------------- | ----- |----------- | ----------- |----------- |
| `TRANSACTION_VALIDATION_ERROR` | `2010` | Invalid amount | Your payment's amount should be atleast bigger than `1` USD |
| `TRANSACTION_VALIDATION_ERROR` | `2020` | Transactions with the given currency are not allowed | You will get this error if the given payment currency code does not exists |
| `TRANSACTION_VALIDATION_ERROR` | `2030` | The payment token is invalid | You are required to provide payment token in order to process fiat payment |
| `TRANSACTION_VALIDATION_ERROR` | `2040` | Array of signed transactions must be provided | You are required to provide signed transactions in order to process fiat/relayed payment |
| `TRANSACTION_VALIDATION_ERROR` | `2050` | Invalid number of signed transactions | On payment creation, you pass generic transactions. When the shopper signs his intent, the number of transactions should be equal to the count of generic transactions |
| `TRANSACTION_VALIDATION_ERROR` | `2060` | Invalid signer. The transactions were not signed by the Shopper's private key | You pass signed transactions in order to process payment. Their signer( public address) should be equal to shopper's `walletAddress` |

### 3. Ethereum Errors
Each of these errors could happen in case of invalid signed transactions or escrow contract insufficient wei/token amount

| Error Name            | Code  | Message    | Description | Оccurrence |
| --------------------- | ----- |----------- | ----------- | ---------- |
| `ETHEREUM_ERROR` | `3010` | Insufficient token amount in escrow contract |  You will get this error if vendor's escrow contract does not have enough tokens to fund a shopper |
| `ETHEREUM_ERROR` | `3020` | Insufficient wei amount in escrow contract | You will get this error if vendor's escrow contract does not have enough wei to fund a shopper |
| `ETHEREUM_ERROR` | `3030` | Invalid gasLimit. | You are required to provide payment token in order to process fiat payment |
| `ETHEREUM_ERROR` | `3040` | Invalid receiver. | You will get this error if signed transactions `to` addresses is different from the `to` addresses provided with generic transactions |
| `ETHEREUM_ERROR` | `3050` | Invalid Gas Price. | You will get this error if signed transactions gasPrice is different from the gasLimit provided with generic transactions |
| `ETHEREUM_ERROR` | `3060` | Method ID/function signature mismatch! | You will get this error if a shopper signs different function than these provided with generic transactions |
| `ETHEREUM_ERROR` | `3070` | Function params does not match! | You will get this error if a shopper signs a function with different parameter/s than equivalent one provided with generic transactions |
| `ETHEREUM_ERROR` | `3080` | Cannot decode generic transaction data! | You will get this error if a shopper somehow replace signed transactions with fake data |
    
### 4. Not Found Errors
    
| Error Name            | Code  | Message    | Description | Оccurrence |
| --------------------- | ----- |--------------- | -------- | -------- |
| `NOT_FOUND_ERROR` | `4010` | Payment was not found | Searching payment does not existsg |
| `NOT_FOUND_ERROR` | `4020` | The searched payment document is not found | You will get this error if vendor's invoice functionality is disabled and you try to get an invoice |
| `NOT_FOUND_ERROR` | `4030` |Vendor was not found  | Searching vendor does not exists |
| `NOT_FOUND_ERROR` | `4040` | User does not have such vendor |   You will get this error if you try to create a payment for a shopper whose vendor is different than yours |
| `NOT_FOUND_ERROR` | `4050` | Shopper was not founde | Searching shopper does not exists |
| `NOT_FOUND_ERROR` | `4060` | API User was not found! | Searching api user does not exists |
| `NOT_FOUND_ERROR` | `4070` | User was not found | Searching user does not exists |
| `NOT_FOUND_ERROR` | `4080` | Organization was not found! | Searching organization does not exists |
| `NOT_FOUND_ERROR` | `4090` | Wallet was not found | You will get this error if you try to activate a wallet which your organization does not hold |
| `NOT_FOUND_ERROR` | `4500` | The record you have searched for does not exists | ou will get this error if you pass invalid input, which we can not decode to determine what you are searching for |


### 5. Validation Errors

| Error Name            | Code  | Message    | Description | Оccurrence |
| --------------------- | ----- |--------------- | -------- | -------- |
| `VALIDATION_ERROR` | `9010` | Nonexistent country code | You will get this error if you pass non-existing country code to Vendor, CardHolderInformation |
| `VALIDATION_ERROR` | `9020` | isCompany should be true/false | You will get this error if you pass non-boolean value to CardHolderInformation.isCompany
| `VALIDATION_ERROR` | `9030` | weiAmount must have non-zero value  | You will get this error if your `fundTx.weiAmount` is zero |
| `VALIDATION_ERROR` | `9040` | Vat number **passed vat number** is invalid | You will get this error if you pass invalid VAT number in CardHolderInformation |
| `VALIDATION_ERROR` | `9050` | Vat number is empty | You will get this error if you have vat number as property in CardHolderInformation, but it does not have a value


### 6. Payment errors

| Error Name            | Code  | Message    | Description | Оccurrence |
| --------------------- | ----- |--------------- | -------- | -------- |
| `PAYMENT_ERROR` | `1070` | Payment has already been processed  | You are allowed to create a payment and process it, but not re-process it |
| `PAYMENT_ERROR` | `6110` | Payment can't be proceed, because shopper is malicious. |  You will get this error if a shopper is flaged as malicious one. This prevents each attempt a shopper do to interact with LimePay API |
| `PAYMENT_ERROR` | `6100` | Payment can't be created, while another one is in processing state | You will get this error if a shopper tries to create a new payment when already has one in processing |
| `PAYMENT_ERROR` | `1025` | Cannot get payment token! | You will get this error if for some reason you can not get `Payment Token`. In this case contact support! |

### 7. Vendor Errors

| Error Name            | Code  | Message    | Description | Оccurrence |
| --------------------- | ----- |--------------- | -------- | -------- |
| `VENDOR_ERROR` | `5000` | Vendor processing status is INACTIVE!  | You will get this error if you try to create a payment for inactive vendor |

### 8. Organization Errors

| Error Name            | Code  | Message    | Description | Оccurrence |
| --------------------- | ----- |--------------- | -------- | -------- |
| `ORGANIZAION_ERROR` | `5000` | Wallet is not white-listed  | You will get this error if you try to activate a wallet, but it is not whitelisted in the vendor's escrow contract |

### 9. User Errors

| Error Name            | Code  | Message    | Description | Оccurrence |
| --------------------- | ----- |--------------- | -------- | -------- |
| `USER_ERROR` | `1030` | Password contains forbidden symbols  | You will get this error if you try to create a user and provided password contains forbidden symbols  Allowed symbols are `letters` / `digits` / `point` / `dash` / `under score` / `square brackets` |


### 10. Global Errors

| Error Name            | Code  | Message    | Description | Оccurrence |
| --------------------- | ----- |--------------- | -------- | -------- |
| `INTERNAL_SERVER_ERROR` | `9000` | Something went wrong. Contact your support for more information | You will get this error if something internally breaks down |
| `BAD_INPUT_ERROR` | `9040` | Wrong input parameters | You will get this error if you pass wrong input parameter |
| `REQUESTS_RATE_LIMIT_ERROR` | `9010` | Too many requests, please try again later. | You will get this error if you exceed LimePay API rate limit |
   
