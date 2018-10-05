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
  - [Shopper](#shopper)
  - [Payment](#payment)
___

## 1. API Users Resource

The source is located at `/v1/apiusers` and it is restricted resource. One could execute requests only with his email/password credentials.

The authorization is made by adding `Basic Authorization` header and setting `email` and `password` as username and password. 

### 1.1 Creating API User

In order to create API User one should execute the following request:

    POST /v1/apiusers

No body data should be passed.

The returned object is of type [API User](#api-user) 
___

## 2. Vendors Resource

The source is located at `/v1/vendors` and it is restricted resource. One could execute requests only with his `API credentials`.

The authorization is made by adding `Basic Authorization` header and setting `apiKey` and `secret` as username and password. 

### 2.1 Creating Vendor

In order to create Vendor one should execute the following request:

    POST /v1/vendors

Body data:
// TODO

The returned object is of type [Vendor](#vendor) 
___

## 3. Shoppers Resource

The source is located at `/v1/shoppers` and it is restricted resource. One could execute requests only with his `API credentials`.

The authorization is made by adding `Basic Authorization` header and setting `apiKey` and `secret` as username and password. 

### 3.1 Creating Shopper

In order to create Vendor one should execute the following request:

    POST /v1/shoppers

Body data:
// TODO

The returned object is of type [Shopper](#shopper) 

___

## 4. Payments Resource
The source is located at `/v1/payments` and it is restricted resource. One could execute requests only with his `API credentials`.

The authorization is made by adding `Basic Authorization` header and setting `apiKey` and `secret` as username and password. 

### 4.1 Creating Payment

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

### 4.2 Getting Payment

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
| `totalAmount`        | `decimal` | The total amount of the payment, represented in the selected currency          |


### 4.2 Getting All Payments for an Organization

In order to get all Payments for an Organization one should execute:

    GET /v1/payments/all

Returns array of Payment objects


