
# React Native SDK Documation

Welcome to the documentation for the Kashier React Native SDK, your gateway to seamless and secure payment processing within your mobile applications.

Kashier is a robust payment gateway solution designed to simplify and enhance the way businesses handle online transactions. By integrating the Kashier SDK into your React Native applications, you can provide your users with a reliable and efficient payment experience.

## What is Kashier?
[Kashier](https://kashier.io/) is a payments platform built to empower
and simplify your business by providing you with
simple and efficient tools to make it easier to
run your business.

## Key Features

- [Pay without OTP](#payWithoutOtp)
- [Pay with OTP](#payWithOtp)
- [Authorization with OTP](#payAuthorizeWithOtp)
- [Authorization without OTP](#payAuthorizeWithoutOtp)
- [Pay using wallet](#payUsingWallet)
- [Save user Card as Token](#saveUserCard)
- [Pay With Token](#payWithToken)
- [List user Cards](#retriveSavedCardsTokens)
- [Delete Card tokens](#deleteToken)
- [Reconcile Card Payments and wallet Payments](#reconcilePayment)
- [Void and refund Transactions](#refundPayment)
- [Installments](#getAvailableInstallmentsMethods)


## Prerequisites
Before you begin integrating the Kashier React Native SDK, ensure that you have the following:
- **React Native Environment**: A working React Native development environment set up on your machine. This includes Node.js, npm (or yarn), and the React Native CLI.
- **Kashier Account**: A registered account with Kashier. You will need your API keys and other credentials provided by Kashier to configure the SDK.
- **Required Packages**: You must install the following packages in your React Native project:
    - `axios`
    - `react-native-webview`
    
## Installation
Extract `KashierPaymentSdk.zip` into your project root directory to start using the SDK available methods.
## Initialization

You’ll need to initialize Kashier SDK once in your application, preferred early before any possible calling for any of it's other functions


```js
//Make sure to use your test apiKey and secretKey for Development mode and your live apiKey and secretKey for Production mode
const ApiKey = '********-****-****-****-************'; 
const SecretKey = '**************************************************************';
const merchantId = 'MID-XXXX-XXXX';
const webHookUrl = "https://your-domain.com/webhook";
const merchantRedirectUrl = "https://your-call-back-url.com";
const sdkMode = "DEVELOPMENT" | "PRODUCTION";



      KashierInit.initialize({
            apiKey:ApiKey
            secretKey: SecretKey,
            merchantId: merchantId
            webHookUrl: webHookUrl,
            merchantRedirectUrl:merchantRedirectUrl
            currency: "EGP",
            sdkMode: "DEVELOPMENT"
        })

  ```      
### payWithoutOtp
Use this API to pay Internet Payment (no OTP required)

this API takes 2 arguments {PaymentModel,kashierHashKey}

`PaymentModel` => typeof PaymentModel Interface
`kashierHashKey` => hash key String 

**Required Interfaces**
```js
export interface PaymentModel {
    cardData: CardData,
    amount: number,
    customerReference: string, //a unique Id for the user who is making the payment
    orderID: string //This is refrenced to as merchantOrderId (the id that you provide for this payment order)

}
export interface CardData {
    expiryMonth: string,
    expiryYear: string,
    nameOnCard: string,
    number: string,
    save: boolean, // use true if you want to save user card or false if you don't want to save user card
    securityCode: string
}
```
**Example**
```js
KashierServices.payWithoutOtp(paymentModel,kashierHashKey)
```


### payWithOtp
Use this API to pay after the user use OTP

this API takes 2 arguments {PaymentModel,kashierHashKey}

`PaymentModel` => typeof PaymentModel Interface
`kashierHashKey` => hash key String (you can find how to generate `kashierHashKey` from [here](https://developers.kashier.io/payment/paymentui/#hashing))



**Required Interfaces**
```js
export interface PaymentModel {
    cardData: CardData,
    amount: number,
    customerReference: string,
    orderID: string //This is refrenced to as merchantOrderId (the id that you provide for this payment order)

}
export interface CardData {
    expiryMonth: string,
    expiryYear: string,
    nameOnCard: string,
    number: string,
    save: boolean,  // use true if you want to save user card or false if you don't want to save user card
    securityCode: string
}
```
**Example**
```js
KashierServices.payWithOtp(paymentModel,kashierHashKey)
```
then we need to call Otp Component that handle Otp redirect 

1- we need to `import { OtpComponent } from 'KashierPaymentSdk/src/OtpComponent';`

2- passing `redirectUrl` and `merchantOrderId` (which you can obtain from payWithOtp success response) to the OtpComponent

3- OtpComponent returns two callBack Functions
```js
onSuccess() //callback function which is returned when Otp success
onFailure() // callback function which is returned when Otp fails
```
    
**Example**
```js
<OtpComponent redirectUrl={redirectUrl} merchantOrderId={merchantOrderId}
        onSuccess={()=>{}} // handle success response 
        onFailure={() =>{}} // handle Failure response
         />
```
### payAuthorizeWithOtp

Use this API to make authorization payments

this API takes 2 arguments {PaymentModel,kashierHashKey}

`PaymentModel` => typeof PaymentModel Interface
`kashierHashKey` => hash key String 

**Required Interfaces**
```js
export interface PaymentModel {
    cardData: CardData,
    amount: number,
    customerReference: string,
    orderID: string //This is refrenced to as merchantOrderId (the id that you provide for this payment order)

}
export interface CardData {
    expiryMonth: string,
    expiryYear: string
    nameOnCard: string,
    number: string,
    save: boolean,  // use true if you want to save user card or false if you don't want to save user card
    securityCode: string
}
```

**Example**
```js
KashierServices.payAuthorizeWithOtp(paymentModel,kashierHashKey)
```
then we need to call OTP Component that handle Otp redirect 

1- we need to import { OtpComponent } from 'KashierPaymentSdk/src/OtpComponent';
2- passing redirectUrl and merchantOrderId which returned from payWithOtp success response to the OtpComponent
3- OtpComponent return tow callBackFunction
        1-onSuccess() // callback function which return when Otp success 
        2-onFailure()  // callback function which return when Otp OnFailure 

**Example**
<OtpComponent redirectUrl={redirectUrl} merchantOrderId={merchantOrderId}
        onSuccess={()=>{}} // handle success response 
        onFailure={() =>{}} // handle Failure response
        />



### payAuthorizeWithoutOtp

Use this API to pay with Authorize Payment with Otp

this Api take 2 arguments {PaymentModel,kashierHashKey}

`PaymentModel` => typeof PaymentModel Interface
`kashierHashKey` => hash key String 

**Required Interfaces**
```js
export interface PaymentModel {
    cardData: CardData,
    amount: number,
    customerReference: string,
    orderID: string //This is refrenced to as merchantOrderId (the id that you provide for this payment order)

}
export interface CardData {
    expiryMonth: string,
    expiryYear: string,
    nameOnCard: string,
    number: string,
    save: boolean,  // use true if you want to save user card or false if you don't want to save user card
    securityCode: string
}
```

**Example**
```js
KashierServices.payAuthorizeWithoutOtp(paymentModel,kashierHashKey)
```

### payUsingWallet

Use this API to pay with Wallet
this Api take 2 arguments {WalletPaymentModel,kashierHashKey}

`WalletPaymentModel` => typeof WalletPaymentModel Interface
`kashierHashKey` => hash key String 

**Required Interfaces**
```js
interface WalletPaymentModel {
    amount: number, 
    mobileNumber: string, 
    orderID:string //This is refrenced to as merchantOrderId (the id that you provide for this payment order)

}
```
**Example**
```js
KashierServices.payWithWallet(walletPaymentModel,kashierHashKey)
```
### saveUserCard
You can enable saving user card on regular payment and authorization by setting save parameter in  CardData Interface with `true`

### payWithToken

Use this API to pay with saved Card token
this API takes 2 arguments {TokenPaymentModel,kashierHashKey}

`TokenPaymentModel` => typeof TokenPaymentModel Interface
`kashierHashKey` => hash key String 

**Required Interfaces**
```js
export interface TokenPaymentModel {
    cardToken: string,
    amount: number, 
    customerName: string,
    customerRefrence: string,
    orderID: string

}
```
**Example**
```js
KashierServices.payWithToken(tokenPaymentModel, kashierHashKey: string)
```
### retriveSavedCardsTokens

Use this API to retrive All cards tokens you saved for a specific user
this API takes 2 arguments {customerReference,kashierHashKey}

`TransactionModel` => typeof TransactionModel Interface

**Example**
```js
KashierServices.retriveSavedCardsTokens(customerReference,kashierHashKey)
```
### deleteToken

Use this API to delete card token for a selected user
this API takes 2 arguments {customerReference,kashierHashKey}

`DeleteTokenModelModel` => typeof DeleteTokenModelModel Interface
```js
export interface DeleteTokenModelModel{
    cardToken: string,
    customerReference: string
}
```
**Example**
```js
KashierServices.deleteToken(customerReference,kashierHashKey)
```

### reconcilePayment
Use this API to check payment Status (success or fail)
this API takes 1 argument {merchantOrderId}

`merchantOrderId` => string 

**Example**
```js
KashierServices.reconcilePayment(merchantOrderId)
```

### reconcileWallet
Use this API to reconcile wallet payment Status
this API takes 2 arguments {orderID,kashierHashKey}

orderID => typeof system orderID string which returned from wallet payment
kashierHashKey => hash key String 

**Example**
```js
KashierServices.reconcileWallet(orderID,kashierHashKey)
```

### refundPayment

Use this API to Refund Payment to the user
this API takes 1 argument {TransactionModel}

`TransactionModel` =>typeof TransactionModel Interface

**Required Interfaces**
```js
export interface TransactionModel{
    merchantOrderId: string,
    amount: number,
    transactionID: string,
    reason: string

}
```
**Example**
```js
KashierServices.refundPayment(transactionModel)
```
### voidPayment

Use this API to cancel the  Payment operation as if it didn't happen and instantly return the money to the user

this API takes 1 argument {TransactionModel}

`TransactionModel` => typeof TransactionModel Interface

**Required Interfaces**
```js
export interface TransactionModel{
    merchantOrderId: string,
    amount: number,
    transactionID: string,
    reason: string

}
```
**Example**
```js
KashierServices.voidPayment(transactionModel)
```

### getAvailableInstallmentsMethods

Use this API to retrive the available installment methods the merchant can offer based on the merchant credentials at Kashier

this API takes 1 argument {amount}

`amount` => Integer number

**Example**
```js
KashierServices.getAvailableInstallmentsMethods(amount)
```

## Capture full and Partial authorized amount
You can find how to fully or partially capture the authorized amount from [here](https://developers.kashier.io/payment/auth&cap) 