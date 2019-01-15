
## **Getting Started with LimePay**

### Contents

- [Organizations](#1-organization)
- [Users](#2-user)
- [API Credentials](#3-api-credentials)
- [Vendor](#4-vendor)
- [Shoppers](#5-shopper)
- [Payments](#6-payments)
 
In order to use LimePay service, one would need to contact LimePay's on-boarding team at TODO mail here.
This document aims to provide information on how you can use LimePay to process Ethereum transactions through fiat card payments.
___

### 1. Organization

Every LimePay customer has its own organisation. It is used as a domain in which all of your settings, entities and data belong to. Once you have been on-boarded by LimePay's team, your organisation will be created.
####  Escrow Smart Contract 
The escrow smart contract holding the funds for the future payments is being deployed as part of your on-boarding process. Information on what should be configured in the contract and the source code of it you can find [here](https://github.com/LimePay/smart-contracts). 
___
### 2. User
Every organisation has users, through which the owner or the other members of the organisation can manage and configure it.
___
### 3. API Credentials
The users of the organisation can create API credentials. The credentials are required in order to consume the LimePay REST API.
___
### 4. Vendor
Every organisation has one or more vendors. They contain the legal information of the Vendor (such as vendor principal, payout information, address etc) . The complete list of the properties can be found [here](https://github.com/LimePay/docs/blob/master/API-Documentation.md#vendor). 
You will be requested to provide the required and preferred information regarding your vendor/vendors and it will be created by LimePay's on-boarding team. You can start using LimePay's service once your organisation has at-least one vendor. 
___
### 5. Shoppers
Whenever new user want to execute Ethereum transaction that requires ethers and/or tokens and you want to use LimePay so that he pays in fiat, you must create him as a shopper in LimePay. Once created you do not need to create him again for every payment after that. 

**NOTE:** Important to point out is that whenever you are creating a shopper you are providing the `walletAddress`. This address will be then funded whenever you are creating payments for that specific shopper.
**The shopper's wallet address should be the same as the address which signs the Ethereum transactions. If this is not the case, the payment will `fail`.**

For more information on how to create a shopper or what the properties of the shopper are, see the [Shopper Resource](https://github.com/LimePay/docs/blob/master/API-Documentation.md#3-shoppers) and [Shopper properties](https://github.com/LimePay/docs/blob/master/API-Documentation.md#3-shoppers) in the `API Documentation`
___
### 6. Payments
Payments are created every time you want your users to execute Ethereum transactions using LimePay. There are two types of payments: `Fiat` and `Relayed`. Depending on the use-case you would want to process the one or the other. 

`Fiat` payments are used when your user has to be charged with credit/debit card, because he does not have ethers/tokens at all. In this case the user pays for the ethers and tokens that are required in order for him to execute the required Ethereum transactions, rendering the service that you are providing.

`Relayed` payments are used when your user has tokens for the services he wants to use, but does not have ether for the gas costs, therefore he cannot execute the transactions even though he has the tokens. In this scenario, you as a service provider decide to pay the gas costs of the user in order for him to be able to consume your service. Therefore the user is not charged with fiat funds.

For more information on how to create and process Payments, you can view the [Process Payments]()TODO documentation.