API Documentation
============

##### This repository contains the documentation of LimePay API.

## Contents

TODO
___

## 1. On boarding steps:

1. An organization should be created in LimePay
2. An User for the organization should be created
3. An APIUser must be created by the organization's user

Once you have completed the described steps above you can start processing transactions.
___

## 2. Payments Resource
The source is located at `/v1/payments` and it is restricted resource. One could execute requests only with his `API credentials`.

The authorization is made by adding `Basic Authorization` header and setting `apiKey` and `secret` as username and password. 

### 2.1 Creating Payment

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

### 2.2 Getting Payment

In order to get Payment one should execute the following request:

    GET /v1/payments/:id

Returns Payment's data containing the following properties:

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
| `totalAmount`        | `number` | The total amount of the payment, represented in the selected currency           |






Objects:








In order for a full payment to be processed one should 
