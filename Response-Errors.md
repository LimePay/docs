

// LimePay error

// LimePay Token Errors

    Each of these errors could be happen when API route is accessed with limePay token
    {
        errorName: TOKEN_VERIFICATION, 
        code: 1100,
        message: "Token verification failed. Token is not provided"
    }
    {
        errorName: TOKEN_VERIFICATION, 
        code: 1110,
        message: "Token verification failed. Some part is missing"
    }
    {
        errorName: TOKEN_VERIFICATION, 
        code: 1120,
        message: "Token verification failed. Invalid algorithm"
    }
    {
        errorName: TOKEN_VERIFICATION, 
        code: 1140,
        message: "Failed to verify token"
    }

// Transaction errors

    
    {
        errorName: TRANSACTION_VALIDATION_ERROR_NAME,
        code: 3070,
        message: "Invalid amount"
    } -> if a payment amount is lower than 1 USD

    {
        errorName: TRANSACTION_VALIDATION_ERROR_NAME,
        code: 3080,
        message: "Transactions with the given currency are not allowed"
    } -> If the given payment currency code does not exists

    {
        errorName: TRANSACTION_VALIDATION_ERROR_NAME,
        code: 3100,
        message: "The payment token is invalid"
    } -> You are required to provide payment token in order to process fiat payment

    { 
        errorName: TRANSACTION_VALIDATION_ERROR_NAME, 
        code: 3020, 
        message: "Array of signed transactions must be provided" 
    } -> You are required to provide signed transactions in order to process fiat/relayed payment 

    { 
        errorName: TRANSACTION_VALIDATION_ERROR_NAME, 
        code: 3030, 
        message: "Invalid number of signed transactions" 
    } -> On payment creation, you pass generic transactions. When the shopper signs his intent, the number of transactions should be equal to the count of generic transactions

// Payments
    {
        errorName: PAYMENT_ERROR,
        code: 1070,
        message: "Payment has already been processed"
    } -> You are allowed to create a payment and process it, but not re-process it

    {
        errorName: NOT_FOUND_ERROR_NAME,
        code: 6010,
        message: "Payment was not found"
    } -> Searching payment does not exists

    {
        errorName: TRANSACTION_VALIDATION_ERROR_NAME,
        code: 3010,
        message: "Invalid signer. The transactions were not signed by the Shopper's private key."
    } -> You pass signed transactions in order to process payment. Their signer( public address) should be equal to shopper's `walletAddress`

    {
        errorName: NOT_FOUND_ERROR_NAME,
        code: 6020,
        message: "The searched payment document is not found"
    } -> You will get this error if vendor's invoice functionality is disabled and you try to get an invoice

    {
        errorName: PAYMENT_ERROR,
        code: 6110,
        message: "Payment can't be proceed, because shopper is malicious."
    } -> You will get this error if a shopper is flaged as malicious one. This prevents each attempt a shopper do to interact with LimePay API

    {
        errorName: PAYMENT_ERROR,
        code: 6100,
        message: "Payment can't be created, while another one is in processing state"
    } -> You will get this error if a shopper tries to create a new payment when already has one in processing

//Ethereum Errors
    INSUFFICIENT_TOKEN_AMOUNT: {
        errorName: ETHEREUM_ERROR,
        code: 4010,
        message: "Insufficient token amount in escrow contract"
    } -> You will get this error if vendor's escrow contract does not have enough tokens to fund a shopper

    INSUFFICIENT_WEI_AMOUNT: {
        errorName: ETHEREUM_ERROR,
        code: 4000,
        message: "Insufficient wei amount in escrow contract"
    } -> You will get this error if vendor's escrow contract does not have enough wei to fund a shopper

    INVALID_GAS_LIMIT: {
        errorName: TRANSACTION_VALIDATION_ERROR_NAME,
        code: 3040,
        message: "Invalid gasLimit."
    } -> You will get this error if gasLimit provided in generic transactions object is different than one in shopper's signed transactions

    INVALID_RECEIVER: {
        errorName: TRANSACTION_VALIDATION_ERROR_NAME,
        code: 3040,
        message: "Invalid receiver."
    } -> You will get this error if signed transactions `to` addresses is different from the `to` addresses provided with generic transactions

    INVALID_GAS_PRICE: {
        errorName: TRANSACTION_VALIDATION_ERROR_NAME,
        code: 3040,
        message: "Invalid Gas Price."
    } -> You will get this error if signed transactions gasPrice is different from the gasLimit provided with generic transactions

    FUNCTION_NAME_MISMATCH: {
        errorName: TRANSACTION_VALIDATION_ERROR_NAME,
        code: 3040,
        message: "Method ID/function signature mismatch!"
    } -> You will get this error if a shopper signs different function than these provided with generic transactions

    INVALID_FUNCTION_PARAMS: {
        errorName: TRANSACTION_VALIDATION_ERROR_NAME,
        code: 3060,
        message: "Function params does not match!"
    } -> You will get this error if a shopper signs a function with different parameter/s than equivalent one provided with generic transactions

    CANNOT_DECODE_GENERIC_DATA: {
        errorName: TRANSACTION_VALIDATION_ERROR_NAME,
        code: 3050,
        message: "Cannot decode generic transaction data!"
    } -> You will get this error if a shopper somehow replace signed transactions with fake data

// General Errors
    // NOT_FOUND

        USER_NOT_FOUND: {
            errorName: AUTHORIZATION_ERROR,
            code: 1010,
            message: "User was not found"
        } -> Searching user does not exists

        API_USER_NOT_FOUND: {
            errorName: AUTHORIZATION_ERROR,
            code: 1020,
            message: "API User was not found!"
        } -> Searching api user does not exists

        VENDOR_NOT_FOUND: {
            errorName: NOT_FOUND_ERROR_NAME,
            code: 5010,
            message: "Vendor was not found"
        } -> Searching vendor does not exists

        VENDOR_NOT_FOUND_FOR_USER: {
            errorName: NOT_FOUND_ERROR_NAME,
            code: 1060,
            message: "User does not have such vendor"
        } -> You will get this error if you try to create a payment for a shopper whose vendor is different than yours

        SHOPPER_NOT_FOUND: {
            errorName: NOT_FOUND_ERROR_NAME,
            code: 7010,
            message: `Shopper was not found`
        } -> Searching shopper does not exists

        ORGANIZATION_NOT_FOUND: {
            errorName: NOT_FOUND_ERROR_NAME,
            code: 1080,
            message: "Organization was not found!"
        } -> Searching organization does not exists

        WALLET_NOT_FOUND: {
            errorName: NOT_FOUND_ERROR_NAME,
            code: 1060,
            message: "Wallet was not found"
        } -> You will get this error if you try to activate a wallet which your organization does not hold

    // VALIDATION
    VALIDATION: {

        NON_EXISTING_COUNTRY_CODE: {
            errorName: VALIDATION_ERROR,
            code: 150,
            message: "Nonexistent country code"
        } -> You will get this error if you pass non-existing country code to Vendor, CardHolderInformation

        SHOULD_BE_BOOLEAN: (propertyName) => {
            return {
                errorName: VALIDATION_ERROR,
                code: 230,
                message: `[isCompany] should be true/false`
            }
        } -> You will get this error if you pass non-boolean value to CardHolderInformation.isCompany

        WEI_AMOUNT_SHOULD_BE_NON_ZERO: {
            errorName: VALIDATION_ERROR,
            code: 210,
            message: `weiAmount must have non-zero value`
        } -> You will get this error if your `fundTx.weiAmount` is zero

        INVALID_VAT_NUMBER:  {
            errorName: VALIDATION_ERROR,
            code: 160,
            message: `Vat number **passed vat number** is invalid`
        } -> You will get this error if you pass invalid VAT number in CardHolderInformation

        EMPTY_VAT_NUMBER: {
            errorName: errorNames.VALIDATION_ERROR,
            code: 170,
            message: `Vat number is empty`
        } -> You will get this error if you have vat number as property in CardHolderInformation, but it does not have a value
    },

    // Vendor

    INACTIVE_VENDOR: {
        errorName: VENDOR_ERROR,
        code: 5000,
        message: "Vendor processing status is INACTIVE!"
    } -> You will get this error if you try to create a payment for inactive vendor

    SEARCHING_RECORD_NOT_EXISTS: {
        errorName: NOT_FOUND_ERROR_NAME,
        code: 9050,
        message: "The record you have searched for does not exists"
    } -> You will get this error if you pass invalid input, which we can not decode

    NOT_WHITELISTED_WALLET: {
        errorName: GLOBAL_ERROR, // maybe there is better name
        code: 1090,
        message: "Wallet is not white-listed"
    } -> You will get this error if you try to activate a wallet, but it is not whitelisted in the vendor's escrow contract

    CONTACT_SUPPORT: {
        errorName: INTERNAL_SERVER_ERROR,
        code: 9000,
        message: "Something went wrong. Contact your support for more information"
    } -> You will get this error if something internally breaks down

    BAD_INPUT: {
        errorName: NOT_FOUND_ERROR_NAME,
        code: 9040,
        message: "Wrong input parameters"
    } -> You will get this error if you pass wrong input parameter. For example in Get Shopper request instead of `id` you pass `randomThing`

    
    UNAUTHORIZED_REQUEST: {
        errorName: AUTHORIZATION_ERROR,
        code: 1010,
        message: "Unauthorized request!"
    } ->  You will get this error if your api user authentication fails

    UNAUTHORIZED_REQUEST_INVALID_USER: {
        errorName: REQUEST_ERROR,
        code: 1010,
        message: 'Unauthorized request! Invalid User!'
    } ->  You will get this error if your user authentication fails

    RATE_LIMIT: {
        errorName: REQUEST_ERROR,
        code: 9010,
        message: 'Too many requests, please try again later.'
    } -> You will get this error if you exceed LimePay API rate limit

    CANNOT_GET_PAYMENT_TOKEN: {
        errorName: errorNames.GLOBAL_ERROR,
        code: 1025,
        message: 'Cannot get payment token!'
    } -> You will get this error if for some reason you can not get `Payment Token`. In this case contact support!

    PAYMENT_TOKEN_NOT_PROVIDED: {
        errorName: REQUEST_ERROR,
        code: 1000,
        message: 'Payment token not provided!'
    } -> You will get this error if you try to process fiat payment without providing `Payment Token`

   
    INVALID_EMAIL_PASSWORD: {
        errorName: "AUTHORIZATION_ERROR",
        code: 1030,
        message: "Invalid email or password!"
    } -> You will get this error if you try to create a user and provided password contains forbidden symbols
        Allowed symbols are 
        1. letters
        2. digits
        3. .
        4. -
        5. _
        6. [
        7. ]
