API Documentation
============

##### This repository contains the documentation of LimePay API.

## Contents

- [Resources](#resources)
  - [API Users](#api-users)
  - [Vendors](#vendors)
  - [Shoppers](#shoppers)
  - [Payments](#payments)
- [Entities](#entities)
  - [API User](#api-user)
  - [Vendor](#vendor)
  - [Payout Info](#payout-info)
  - [Vendor Principal](#vendor-principal)
  - [Email Setup](#email-setup)
  - [Shopper](#shopper)
  - [Payment](#payment)

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

In order to create Vendor one should execute the following request:

    POST /v1/vendors

Body data:

| Field                    | Type     | Description                                                                        | Required |
| ------------------------ | -------- | ---------------------------------------------------------------------------------- | -------- |
| `name`                   | `string` | Vendor name                                                                        | yes      |
| `address`                | `string` | Vendor street address                                                              | yes      |
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
| `vendorStatus`           | `string` | Vendor status. Can be [X,Y,Z] TODO                                                 | yes      |
| `state`                  | `string` | Vendor state                                                                       | no, if country is not `US` or `CA` |

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

In order to update Vendor's properties one should execute the following request:

    PATCH /v1/vendors/:id

Body data:

| Field                    | Type     | Description                                                                        | Required |
| ------------------------ | -------- | ---------------------------------------------------------------------------------- | -------- |
| `name`                   | `string` | Vendor name                                                                        | yes      |
| `address`                | `string` | Vendor street address                                                              | yes      |
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
| `vendorStatus`           | `string` | Vendor status. Can be [X,Y,Z] TODO                                                 | yes      |
| `state`                  | `string` | Vendor state                                                                       | no, if country is not `US` or `CA` |

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
| `zip`                    | `string` | Shopper's zip code                                                                 | yes      |
| `walletAddress`          | `string` | Shopper's wallet address                                                           | yes      |
| `vatId`                  | `string` | The company’s VAT number                                                           | no, can be undefined if `taxId` is set |
| `taxId`                  | `string` | The company’s business ID number                                                   | no, can be undefined if `vatId` is set |

Returns object of type [Shopper](#shopper) 

#### 3.2 Getting Shopper

In order to get Shopper one should execute the following request:

    GET /v1/shoppers/:id

Returns object of type [Shopper](#shopper) 

#### 3.3 Getting all Shoppers

In order to get all Shoppers one should execute the following request:

    GET /v1/shoppers

Returns array of [Shopper](#shopper) objects 

#### 3.4 Reseting Shopper's malicious attempts

In order to reset the malicious attempts of a Shopper one should execute the following request:

    POST /v1/shoppers/reset

Body data:

| Field                | Type     |Description                                                                      | Required   |
| -------------------- | -------- | ------------------------------------------------------------------------------- | ---------- |
| `shopperId`          | `string` | The ID of the shopper for which we will reset the malicious attempts            | yes        |

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
| `shopper`            | `string` | The shopperID of the shopper that will be charged                               | yes        |
| `vendor`             | `string` | The vendorID of the vendor that will charge the payment                         | yes        |
| `items`              | `array`  | Objects containing information about the items that are being purchased         | yes        |
| `fundTxData`         | `object` | Object containing information about the funding of an shopper                   | yes        |
| `genericTransactions`| `array`  | Objects containing information about every transaction that should be executed  | yes        |

Returns object of type  [Payment](#payment)

#### 4.2 Getting Payment

In order to get Payment one should execute the following request:

    GET /v1/payments/:id

Returns object of type  [Payment](#payment)

#### 4.2 Getting All Payments for an Organization

In order to get all Payments one should execute:

    GET /v1/payments/all

Returns array of [Payment](#payment) objects
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
| `name`                   | `string` | Vendor name                                                                        | no       |
| `address`                | `string` | Vendor's street address                                                            | no       |
| `city`                   | `string` | Vendor's city                                                                      | no       |
| `country`                | `string` | Country code                                                                       | no       |
| `phone`                  | `string` | Vendor's phone                                                                     | no       |
| `email`                  | `string` | Vendor email                                                                       | no       |
| `vatId`                  | `string` | The company’s VAT number                                                           | yes, can be null if `taxId` is set |
| `taxId`                  | `string` | The company’s business ID number                                                   | yes, can be null if `vatId` is set |
| `payoutInfo`             | `array`  | Array of [Payout Info](#payout-info) objects                                       | no       |
| `vendorPrincipal`        | `object` | [Vendor Principal](#vendor-principal) object                                       | no       |
| `emailSetup`             | `object` | [Email Setup](#email-setup) object                                                 | yes      |
| `defaultPayoutCurrency`  | `string` | Currency code (ISO 4217) for the default payout currency                           | no       |
| `vendorStatus`           | `string` | Vendor status. Can be [X,Y,Z] TODO                                                 | no       |
| `state`                  | `string` | Vendor state                                                                       | yes, if country is not `US` or `CA` |

### Payout Info

| Attribute                | Type     | Description                                                                        | Nullable |
| ------------------------ | -------- | ---------------------------------------------------------------------------------- | -------- |
| `_id`                    | `string` | The ID Payout info                                                                 | no       |
| `bankId`                 | `integer`| Bank ID number (ie. Routing number)                                                | no       |
| `bankName`               | `string` | Name of vendor bank                                                                | no       |
| `country`                | `string` | Country code of vendor bank                                                       | no       |
| `city`                   | `string` | City of vendor bank                                                                | no       |
| `address`                | `string` | Street address of vendor bank                                                      | no       |
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
| `subject`                | `string` | The subject that will be shown when sending receipt to shoppers.                   | yes      |
| `bodyHeaderText`         | `string` | The subject that will be shown when sending receipt to shoppers.                   | yes      |

#### Invoice
| Attribute                | Type     | Description                                                                        | Nullable |
| ------------------------ | -------- | ---------------------------------------------------------------------------------- | -------- |
| `subject`                | `string` | The subject that will be shown when sending invoice to shoppers.                   | yes      |
| `bodyHeaderText`         | `string` | The subject that will be shown when sending invoice to shoppers.                   | yes      |

### Shopper
| Attribute                | Type     | Description                                                                        | Nullable |
| ------------------------ | -------- | ---------------------------------------------------------------------------------- | -------- |
| `_id`                    | `string` | The ID of the Shopper                                                              | no       |
| `vendor`                 | `string` | The vendor's name for which the shopper is created.                                | no       |
| `firstName`              | `string` | Shopper's first name                                                               | no       |
| `lastName`               | `string` | Shopper's last name                                                                | no       |
| `zip`                    | `string` | Shopper's zip                                                                      | no       |
| `email`                  | `string` | Shopper email                                                                      | no       |
| `walletAddress`          | `string` | Shopper wallet address                                                             | no       |
| `vatId`                  | `string` | The shopper’s VAT number                                                           | no       |
| `maliciousAttempts`      | `integer`| The number of malicious attemps for a shopper. It is incremented when a given signed transaction reverts. | no |

### Payment

| Attribute            | Type     | Description                                                                     |
| -------------------- | -------- | ------------------------------------------------------------------------------- |
| `_id`                | `string` | The id of the payment                                                           |
| `status`             | `string` | The status of the Payment                                                       |
| `date`               | `date`   | The date time when the payment was created                                      |
| `currency`           | `string` | Currency code (ISO 4217) of the amount to be charged                            |
| `shopper`            | `string` | The shopperID of the shopper that will be charged                               |
| `vendor`             | `string` | The vendorID of the vendor that will charge the payment                         |
| `items`              | `array`  | Objects containing information about the items that are being purchased         |
| `fundTxData`         | `object` | Object containing information about the funding of an shopper                   |
| `genericTransactions`| `array`  | Objects containing information about every transaction that should be executed  |
| `totalAmount`        | `decimal` | The total amount of the payment, represented in the selected currency          |
