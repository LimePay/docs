API Documentation
============

##### This repository contains the documentation of LimePay API.

## Contents

- [Resources](#resources)
  - [API Users](#1-api-users)
  - [Vendors](#2-vendors)
  - [Shoppers](#3-shoppers)
  - [Payments](#4-payments)
  - [Invoices](#5-invoices)
  - [Receipts](#6-receipts)
- [Entities](#entities)
  - [API User](#api-user)
  - [Vendor](#vendor)
  - [Payout Info](#payout-info)
  - [Vendor Principal](#vendor-principal)
  - [Email Setup](#email-setup)
  - [Shopper](#shopper)
  - [Payment](#payment)
  - [Item](#item)
  - [Payment Details](#payment-details)
  - [Card Holder](#card-holder)
  - [Fund TX Data](#fund-transaction-data)
  - [Generic Transaction](#generic-transaction)
  
## Resources
___
### 1. API Users

The source is located at `/v1/apiusers` and it is restricted resource. One could execute requests only with his email/password credentials.

The authorization is made by adding `Basic Authorization` header and setting `email` and `password` as username and password. 

#### 1.1 Creating API User

In order to create API User one should execute the following request:

    POST /v1/apiusers

No body data should be passed.

Returns object of type [API User](#api-user) 

#### 1.2 Getting API User

In order to get API User one should execute the following request:

    GET /v1/apiusers/:id

Returns object of type  [API User](#api-user)

#### 1.3 Getting all API Users

In order to get all API Users one should execute the following request:

    GET /v1/apiusers

Returns array of [API User](#api-user) objects.

#### 1.4 Delete API User

In order to delete API User one should execute the following request:

    DELETE /v1/apiusers/:id

If the deletion was successful, HTTP `status` code `200` will be returned 

___

### 2. Vendors

The source is located at `/v1/vendors` and it is restricted resource. One could execute requests only with his `API credentials`.

The authorization is made by adding `Basic Authorization` header and setting `apiKey` and `secret` as username and password. 

#### 2.1 Creating Vendor
##### ***Notice**: The Creation of Vendors is **NOT** accessible for now. Once we make it accessible, we will update the documentation.* 
In order to create Vendor one should execute the following request:

    POST /v1/vendors

Body data:

| Field                    | Type     | Description                                                                        | Required |
| ------------------------ | -------- | ---------------------------------------------------------------------------------- | -------- |
| `name`                   | `string` | Vendor name                                                                        | yes      |
| `address`                | `string` | Vendor street address                                                              | yes      |
| `zip`                    | `string` | Zip/Postal code of vendor bank                                                     | yes      |
| `city`                   | `string` | Vendor city                                                                        | yes      |
| `country`                | `string` | Country code                                                                       | yes      |
| `phone`                  | `string` | Vendor's phone                                                                     | yes      |
| `email`                  | `string` | Vendor email                                                                       | yes      |
| `vatId`                  | `string` | The company’s VAT number                                                           | no, can be undefined if `taxId` is set |
| `taxId`                  | `string` | The company’s business ID number                                                   | no, can be undefined if `vatId` is set |
| `payoutInfo`             | `array`  | Array of [Payout Info](#payout-info) objects                                       | yes      |
| `vendorPrincipal`        | `object` | [Vendor Principal](#vendor-principal) object                                       | yes      |
| `emailSetup`             | `object` | [Email Setup](#email-setup) object                                                 | yes      |
| `defaultPayoutCurrency`  | `string` | Currency code (ISO 4217) for the default payout currency                           | yes      |
| `state`                  | `string` | Vendor state                                                                       | no, if country is not `US` or `CA` |
| `receiptEmail`           | `string` | The email from which the receipts and invoices will be sent                        | yes      |
| `autoReceipt`            | `boolean`| If set to true, receipts will be sent automatically to the shoppers                | no       |
| `autoInvoice`            | `boolean`| If set to true, invoices will be sent automatically to the shoppers                | no       |
| `frequency`              | `string` | The payout frequency of the vendor. Possible values are: `DAILY`, `WEEKLY`, `SEMIMONTHLY` and `MONTHLY` | no   |
| `rawLogo`            	   | `string` | Logo used in invoices. Pure base64 format               						   | no       |
| `firstName`              | `string` | First name of Vendor (if individual vendor, not business)                          | no       |
| `lastName`               | `string` | First name of Vendor (if individual vendor, not business)                          | no       |

Returns object of type [Vendor](#vendor) 

#### 2.2 Getting Vendor

In order to get Vendor one should execute the following request:

    GET /v1/vendors/:id
    
Returns object of type [Vendor](#vendor) 

#### 2.3 Getting All Vendors

In order to get all Vendors one should execute the following request:

    GET /v1/vendors

Returns array of [Vendor](#vendor) objects

#### 2.4 Update Vendor
##### ***Notice**: The Update of Vendor is **NOT** accessible for now. Once we make it accessible, we will update the documentation.* 
In order to update Vendor's properties one should execute the following request:

    PATCH /v1/vendors/:id

Body data:

| Field                    | Type     | Description                                                                        | Required |
| ------------------------ | -------- | ---------------------------------------------------------------------------------- | -------- |
| `name`                   | `string` | Vendor name                                                                        | no       |
| `address`                | `string` | Vendor street address                                                              | no       |
| `zip`                    | `string` | Zip/Postal code of vendor bank                                                     | no       |
| `city`                   | `string` | Vendor city                                                                        | no       |
| `country`                | `string` | Country code                                                                       | no       |
| `phone`                  | `string` | Vendor's phone                                                                     | no       |
| `email`                  | `string` | Vendor email                                                                       | no       |
| `payoutInfo`             | `array`  | Array of [Payout Info](#payout-info) objects                                       | no       |
| `vendorPrincipal`        | `object` | [Vendor Principal](#vendor-principal) object                                       | no       |
| `emailSetup`             | `object` | [Email Setup](#email-setup) object                                                 | no       |
| `defaultPayoutCurrency`  | `string` | Currency code (ISO 4217) for the default payout currency                           | no       |
| `state`                  | `string` | Vendor state                                                                       | no   	  |
| `receiptEmail`           | `string` | The email from which the receipts and invoices will be sent                        | no       |
| `autoReceipt`            | `boolean`| If set to true, receipts will be sent automatically to the shoppers                | no       |
| `autoInvoice`            | `boolean`| If set to true, invoices will be sent automatically to the shoppers                | no       |
| `frequency`              | `string` | The payout frequency of the vendor. Possible values are: `DAILY`, `WEEKLY`, `SEMIMONTHLY` and `MONTHLY` | no   |
| `rawLogo`            	   | `string` | Logo used in invoices. Pure base64 format               						   | no       |
| `firstName`              | `string` | First name of Vendor (if individual vendor, not business)                          | no       |
| `lastName`               | `string` | First name of Vendor (if individual vendor, not business)                          | no       |

Returns object of type [Vendor](#vendor) 
___

### 3. Shoppers

The source is located at `/v1/shoppers` and it is restricted resource. One could execute requests only with his `API credentials`.

The authorization is made by adding `Basic Authorization` header and setting `apiKey` and `secret` as username and password. 

#### 3.1 Creating Shopper

In order to create Shopper one should execute the following request:

    POST /v1/shoppers

Body data:

| Field                    | Type     | Description                                                                        | Required |
| ------------------------ | -------- | ---------------------------------------------------------------------------------- | -------- |
| `firstName`              | `string` | First name of shopper                                                              | yes      |
| `lastName`               | `string` | Last name of shopper                                                               | yes      |
| `email`                  | `string` | Shopper email                                                                      | yes      |
| `vendor`                 | `string` | The ID of the Vendor.                                                              | yes      |
| `walletAddress`          | `string` | Shopper's wallet address                                                           | yes      |

Returns object of type [Shopper](#shopper) 

#### 3.2 Getting Shopper

In order to get Shopper one should execute the following request:

    GET /v1/shoppers/:id

Returns object of type [Shopper](#shopper) 

#### 3.3 Getting all Shoppers

In order to get all Shoppers one should execute the following request:

    GET /v1/shoppers

Returns array of [Shopper](#shopper) objects 


#### 3.4 Update Shopper
In order to update Shopper's properties one should execute the following request:

    PATCH /v1/shoppers/:id

Body data:

| Field                    | Type     | Description                                                                        | Required |
| ------------------------ | -------- | ---------------------------------------------------------------------------------- | -------- |
| `firstName`              | `string` | Shopper's first name                                                               | no       |
| `lastName`               | `string` | Shopper's last name                                                                | no       |
| `email`                  | `string` | Shopper email                                                                      | no       |
| `walletAddress`          | `string` | Shopper wallet address                                                             | no       |

Returns object of type [Shopper](#shopper) 

#### 3.5 Delete Shopper 
In order to delete Shopper one should execute the following request:

    DELETE /v1/shoppers/:id

If the deletion was successful, HTTP `status` code `200` will be returned 

#### 3.6 Reseting Shopper's malicious attempts

In order to reset the malicious attempts of a Shopper one should execute the following request:

    POST /v1/shoppers/reset

Body data:

| Field                | Type     |Description                                                                      | Required   |
| -------------------- | -------- | ------------------------------------------------------------------------------- | ---------- |
| `shopperId`          | `string` | The ID of the shopper for which it will reset the malicious attempts            | yes        |

Returns object of type [Shopper](#shopper) 
___

### 4. Payments
The source is located at `/v1/payments` and it is restricted resource. One could execute requests only with his `API credentials`.

The authorization is made by adding `Basic Authorization` header and setting `apiKey` and `secret` as username and password. 

#### 4.1 Creating Payment

In order to create Payment one should execute the following request:

    POST /v1/payments

Body data:

| Field                | Type     |Description                                                                      | Required   |
| -------------------- | -------- | ------------------------------------------------------------------------------- | ---------- |
| `currency`           | `string` | Currency code (ISO 4217) of the amount to be charged                            | yes        |
| `shopper`            | `string` | The ID of the shopper that will be charged                                      | yes        |
| `items`              | `array`  | Array of [Item](#item) objects                                                  | yes        |
| `fundTxData`         | `object` | [Fund TX Data](#fund-transaction-data) object                                   | yes        |
| `genericTransactions`| `array`  | Array of [Generic Transaction](#generic-transaction) objects                  	| yes        |

Returns object of type  [Payment](#payment).

The returned result contains `x-lime-token` as a header. The token is used for the initialization of the LimePay checkout form.

#### 4.2 Getting Payment

In order to get Payment one should execute the following request:

    GET /v1/payments/:id

Returns object of type  [Payment](#payment)

#### 4.2 Getting all Payments

In order to get all Payments one should execute:

    GET /v1/payments

Returns array of [Payment](#payment) objects

### 5. Invoices
The source is located at `/v1/payments/:id/invoice/` and it is restricted resource. One could execute requests only with his `API credentials`.

The authorization is made by adding `Basic Authorization` header and setting `apiKey` and `secret` as username and password. 

#### 5.1 Preview Invoice

In order to preview Invoice one should execute the following request:

    GET /v1/payments/:id/invoice/preview

Returns HTML formated invoice

#### 5.2 Send Invoice

In order to send Invoice one should execute the following request:

    GET /v1/payments/:id/invoice

Returns HTTP status code 200 every time.  
**Notice!** One can preview the invoice design which will be send by making API call to [`/v1/payments/:id/invoice/preview`](#preview-invoice)


### 6. Receipts
The source is located at `/v1/payments/:id/receipt/` and it is restricted resource. One could execute requests only with his `API credentials`.

The authorization is made by adding `Basic Authorization` header and setting `apiKey` and `secret` as username and password. 

#### 6.1 Preview Receipt

In order to preview Receipt one should execute the following request:

    GET /v1/payments/:id/receipt/

Returns HTML formated receipt


___
## Entities

### API User 

| Attribute                | Type     | Description                                                                        | Nullable |
| ------------------------ | -------- | ---------------------------------------------------------------------------------- | -------- |
| `_id`                    | `string` | The ID of the API User                                                             | no       |
| `userId`                 | `string` | The username which created the API User                                            | no       |
| `apiKey`                 | `string` | The apiKey of the API User                                                         | no       |
| `secret`                 | `string` | The secret of the API User                                                         | no       |

### Vendor 

| Attribute                | Type     | Description                                                                        | Nullable |
| ------------------------ | -------- | ---------------------------------------------------------------------------------- | -------- |
| `_id`                    | `string` | The ID of Vendor                                                                   | no       |
| `name`                   | `string` | Vendor name. (Company's name)                                                      | no       |
| `email`                  | `string` | Vendor email                                                                       | no       |
| `country`                | `string` | Country code                                                                       | no       |
| `city`                   | `string` | Vendor's city                                                                      | no       |
| `firstName`              | `string` | First name of Vendor (if individual vendor, not business)                          | yes      |
| `lastName`               | `string` | First name of Vendor (if individual vendor, not business)                          | yes      |
| `address`                | `string` | Vendor's street address                                                            | no       |
| `phone`                  | `string` | Vendor's phone                                                                     | yes       |
| `zip`                    | `string` | The zip of the vendor                                                              | no      |
| `state`                  | `string` | Vendor state                                                                       | yes, if country is not `US` or `CA` |
| `vatId`                  | `string` | The company’s VAT number                                                           | yes, if `taxId` is set      |
| `taxId`                  | `string` | The company’s business ID number                                                   | yes, if `vatId` is set      |
| `payoutInfo`             | `array`  | Array of [Payout Info](#payout-info) objects                                       | no       |
| `vendorPrincipal`        | `object` | [Vendor Principal](#vendor-principal) object                                       | no       |
| `receiptEmail`           | `string` | The email from which the receipts and invoices will be sent                        | no       |
| `emailSetup`             | `object` | [Email Setup](#email-setup) object                                                 | yes      |
| `defaultPayoutCurrency`  | `string` | Currency code (ISO 4217) for the default payout currency                           | no       |
| `autoReceipt`            | `boolean`| If set to true, receipts will be sent automatically to the shoppers                | yes      |
| `autoInvoice`            | `boolean`| If set to true, invoices will be sent automatically to the shoppers                | yes      |
| `frequency`              | `string` | The payout frequency of the vendor. Possible values are: `DAILY`, `WEEKLY`, `SEMIMONTHLY` and `MONTHLY` | no   |
| `rawLogo`            	   | `string` | Logo used in invoices.                									   		   | yes      |



### Payout Info

| Attribute                | Type     | Description                                                                        | Nullable |
| ------------------------ | -------- | ---------------------------------------------------------------------------------- | -------- |
| `_id`                    | `string` | The ID of a Payout info                                                            | no       |
| `bankId`                 | `string` | Bank ID number (ie. Routing number)                                                | no       |
| `bankName`               | `string` | Name of vendor bank                                                                | no       |
| `country`                | `string` | Country code of vendor bank                                                        | no       |
| `city`                   | `string` | City of vendor bank                                                                | yes       |
| `address`                | `string` | Street address of vendor bank                                                      | yes       |
| `zip`                    | `string` | Zip/Postal code of vendor bank                                                     | no       |
| `bankAccountId`          | `string` | Vendor bank account number                                                         | no       |
| `iban`                   | `string` | Vendor International Bank Account Number                                           | yes, if `payoutType` is `CHAPS` or `SEPA` |
| `bankAccountType`        | `string` | Account type. Possible values: `CHECKING`, `SAVINGS`                               | no       |
| `bankAccountClass`       | `string` | Type of bank account. Possible values: `PERSONAL`, `CORPORATE`, `INTERNATIONAL`    | no       |
| `nameOnAccount`          | `string` | Name on bank account for vendor                                                    | no       |
| `payoutType`             | `string` | Method of payout to vendor. Possible values: `ACH` (USD, CAD), `CHAPS` (GBP), `SEPA` (EUR), `WIRE` | no       |
| `baseCurrency`           | `string` | Payout currency for vendor's bank account.                                         | no       |
| `minimalPayoutAmount`    | `integer`| Minimum amount for which a vendor may be paid out. If the vendor's account balance is below this amount, their balance will be carried forward to the next pay cycle. The currency for `minimalPayoutAmount` is the same as `baseCurrency`.               | no       |
| `state`                  | `string` | Vendor state                                                                       | yes, if country is not `US` or `CA` |
| `swiftBic `              | `string` | International identification bank code                                             | yes       |


Possible values for `payoutType`:

| Value | Full name                |
| ----- | ------------------------ |
| `AUD` | Australia Dollar         |
| `CAD` | Canada Dollar            |
| `CHF` | Switzerland Franc        |
| `DKK` | Denmark Kroner           |
|`EUR` | Euro                      |
|`GBP` | United Kingdom Pound      |
|`HKD` | Hong Kong Dollar          |
|`JPY` | Japan Yen                 |
|`NOK` | Norway Kroner             |
|`NZD` | New Zealand Dollar        |
|`SEK` | Sweden Kroner             |
|`USD` | US Dollar                 |

### Vendor Principal

| Attribute                      | Type     | Description                                                                        | Nullable |
| ------------------------------ | -------- | ---------------------------------------------------------------------------------- | -------- |
| `firstName`                    | `string` | First name of vendor principal owner                                               | no       |
| `lastName`                     | `string` | Last name of vendor principal owner                                                | no       |
| `address`                      | `string` | Address of vendor principal owner                                                  | no       |
| `city`                         | `string` | City of vendor principal owner                                                     | no       |
| `country`                      | `string` | Country code of vendor principal owner                                             | no       |
| `zip`                          | `string` | Zip of vendor principal owner                                                      | no       |
| `dob`                          | `string` | Date of Birth of vendor principal owner. Format: `dd-mm-yyyy`                      | no       |
| `personalIdentificationNumber` | `string` | In the US, this value is the last four digits of the principal owner's Social Security Number. In countries outside of the US, this value is the full National Identification Number of the principal owner, or TaxID if the country doesn’t support a National Identification Number. | no |
| `driverLicenseNumber`          | `string` | Driver license number of vendor principal owner                                    | yes, if vendor principal country is not `US` |
| `passportNumber`               | `string`  | Passport number of vendor principal owner.                                        | yes, if vendro principal country is `US` |
| `email`                        | `string`  | Email address of vendor principal owner                                           | no       |


### Email setup

| Attribute                | Type     | Description                                                                        | Nullable |
| ------------------------ | -------- | ---------------------------------------------------------------------------------- | -------- |
| `receipt`                | `object` | Object of type [Receipt](#receipt). Configuration of the receipt email.            | no       |
| `invoice`                | `object` | Object of type [Invoice](#invoice). Configuration of the invoice email.            | no       |

#### Receipt
| Attribute                | Type     | Description                                                                        | Nullable |
| ------------------------ | -------- | ---------------------------------------------------------------------------------- | -------- |
| `subject`                | `string` | The email subject that will be shown when sending receipt to shoppers.                   | yes      |
| `bodyHeaderText`         | `string` | The body header text that will be shown on top of the receipt document when sending receipt to shoppers.                   | yes      |

#### Invoice
| Attribute                | Type     | Description                                                                        | Nullable |
| ------------------------ | -------- | ---------------------------------------------------------------------------------- | -------- |
| `subject`                | `string` | The email subject that will be shown when sending invoice to shoppers.                   | yes      |
| `bodyHeaderText`         | `string` | The body header text that will be shown on top of the invoice document when sending invoice to shoppers.                   | yes      |

### Shopper
| Attribute                | Type     | Description                                                                        | Nullable |
| ------------------------ | -------- | ---------------------------------------------------------------------------------- | -------- |
| `_id`                    | `string` | The ID of the Shopper                                                              | no       |
| `vendor`                 | `string` | The vendor's name for which the shopper is created.                                | no       |
| `firstName`              | `string` | Shopper's first name                                                               | no       |
| `lastName`               | `string` | Shopper's last name                                                                | no       |
| `email`                  | `string` | Shopper email                                                                      | no       |
| `walletAddress`          | `string` | Shopper wallet address                                                             | no       |
| `maliciousAttempts`      | `integer`| The number of malicious attemps for a shopper. It is incremented when a given signed transaction reverts. | no |

### Payment

| Attribute            | Type     | Description                                                                     | Nullable |
| -------------------- | -------- | ------------------------------------------------------------------------------- | -------- |
| `_id`                | `string` | The id of the payment                                                           | no       |
| `status`             | `string` | The [status](#payment-statuses) of the Payment                                   | no       |
| `date`               | `date`   | The date time when the payment was created                                      | no       |
| `currency`           | `string` | Currency code (ISO 4217) of the amount to be charged                            | no       |
| `shopper`            | `string` | The shopperID of the shopper that will be charged                               | no       |
| `vendor`             | `string` | The vendorID of the vendor that will charge the payment                         | no       |
| `items`              | `array`  | Array of [Items](#item) objects that are being purchased         				| no       |
| `fundTxData`         | `object` | Object containing information about the funding of an shopper                   | no       |
| `genericTransactions`| `array`  | Objects containing information about every transaction that should be executed  | no       |
| `paymentDetails`     | `object` | [Payment Details](#payemnt-details) object  									| no       |

### Payment statuses

| Attribute   | Description                                    					 |                                      
| ----------- | ---------------------------------------------------------------- |
| NEW		  | When a payment is created										 |															
| PROCESSING  | When a payment is sent for processing and all validation passes	 |
| SUCCESSFUL  |	When a payment is processed without errors						 |
| FAILED	  |	When a payment is sent for processing but something went wrong	 |


### Item

| Attribute            | Type     | Description                                                                     | Nullable |
| -------------------- | -------- | ------------------------------------------------------------------------------- | -------- |
| `description`        | `string` | Description of the item that is being bought. This information is displayed in the invoice. | no       |
| `lineAmount`         | `decimal`| Fiat amount of the item excluding VAT amount.                                   | no       |
| `quantity`           | `integer`| Quantity of the items                                                           | no       |


### Payment Details

| Attribute       | Type      | Description                                                                     | Nullable |
| --------------- | --------  | ------------------------------------------------------------------------------- | -------- |
| `taxRate`       | `decimal` | Tax rate for the payment. Depends on the country of the Vendor 					| yes      |
| `taxAmount`     | `decimal` | Tax amount of the payment. Calculated with taxRate and the baseAmount             | yes      |
| `baseAmount`    | `decimal` | Payment amount without tax                                                      | no       |
| `totalAmount`   | `decimal` | Payment amount with tax (if any)      											| no       |
| `cardHolder`    | `object`  | [Card Holder](#card-holder) object                                              | no       |



### Card Holder

| Attribute       | Type      | Description                                                                     | Nullable |
| --------------- | --------  | ------------------------------------------------------------------------------- | -------- |
| `vatNumber`     | `string`  | `vatNumber` of the shopper 														| yes      |
| `name`     	  | `string`  | Personal/Company name of the shopper                                    		| no       |
| `isCompany`     | `boolean` | If the shopper is company                                                       | no       |
| `country`       | `string`  | Shopper's country 																| no       |
| `zip`           | `string`  | Shopper's zip/postal code                                                       | no       |
| `street`        | `string`  | Shopper's street address                                                        | no       |



### Fund Transaction data

| Attribute            | Type     | Description                                                                                         | Nullable |
| -------------------- | -------- | --------------------------------------------------------------------------------------------------- | -------- |
| `weiAmount`          | `string` | wei amount of the payment. This will be the amount of ethers that the shopper will be funded with   | no       |
| `tokenAmount`        | `string` | token amount of the payment. This will be the amount of tokens that the shopper will be funded with | no       |
| `transactionHash`    | `string` | The hash of the transaction. It will be set once the transaction is executed                        | yes      |
| `status`             | `string `| The status of the transaction. Possible values are: `PENDING`, `PROCESSING`, `SUCCESSFUL`, `FAILED` | no       |

### Generic transaction

| Attribute            | Type     | Description                                                                                         | Nullable |
| -------------------- | -------- | --------------------------------------------------------------------------------------------------- | -------- |
| `to`                 | `string `| Address that the transaction will be sent to                                                        | no       |
| `functionName`       | `string` | Name of the smart contract function                                                                 | no       |
| `gasPrice`           | `string` | Gas price of the transaction                                                                        | no       |
| `gasLimit`           | `string` | Gas limit of the transactions                                                                       | no       |
| `signedTransaction`  | `string` | Signed transaction by the shoppers private key, that will be broadcasted                            | no       |
| `status`             | `string `| Status of the transaction. Possible values are: `PENDING`, `PROCESSING`, `SUCCESSFUL`, `FAILED`     | no       |
| `transactionHash`    | `string` | The hash of the transaction. It will be set once the transaction is broadcasted                     | yes      |
| `functionParams`     | `array`  | Array of [Function parameter](#function-parameter]) objects. Each object contain information(type,value) of parameter that should be passed to a function   | no |

### Function parameter

| Attribute            | Type     | Description                                                                                         | Nullable |
| -------------------- | -------- | --------------------------------------------------------------------------------------------------- | -------- |
| `value`              | `any`    | Value that should be passed to the function                                                         | no       |
| `type`               | `string` | The solidity type of the value. For example: bool, bool[].....                                      | no       |