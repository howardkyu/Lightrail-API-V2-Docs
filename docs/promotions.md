# Promotions
Lightrail enables a wide variety of promotion use-cases. A few common examples are:

 1. Personalized promotion associated with a customer (`access: customerId`).  
 
 2. Unique promotion code delivered to a potential customer (`access: secureCode`).  
 
 3. A site wide promotion code that can be entered during checkout (`access: publicCode`).
 
These types of promotions are differentiated primarily based on the `access` property of ValueStores. 

In addition to how the promotion is accessed, they type of value they hold can also differ. 
Promotions can be valid for a number of dollars or points off, but they can also represent a percent discount (details to come).
These variations are all determined by properties on the ValueStore which represents the promotion.

See below for concrete examples for each type of promotion. 

### Getting Started with Promotions
To get started with Promotions, you first need to create a Promotion Program which defines the type of promotion you wish to create.
Promotion Programs are created within your Lightrail Account and specify details such as value and currency.

#### 1. Personalized Promotion Attached to a Customer
In the [Promotion Program Creation Flow](www.lightrail.com) (not yet live) you'll select the option "Personalized Customer Promotion". 

Also, if no Customer exists, you first need to create one to associate with the `Account`. Below is the request to create a Customer. 
See [here](https://lightrailapi.docs.apiary.io/#reference/0/customers/create-customer) for full details on the `/customers` endpoint. 

`POST https://api.lightrail.com/v2/customers`
```json
{
   "customerId": "cus_123",
   "email": "alice@examle.com"
}
```

Now that you have a `customerId` and a `programId` you can attach the promotion to a customer. Below is the request to do this.

`POST https://api.lightrail.com/v2/valueStores`
```json
{
    "valueStoreId": "cus_123-sign-up-promotion",
    "programId": "sign-up-promotion",
    "customerId": "cus_123",
    "value": 500
}
``` 

#### 2. A Unique Code Promotion 
In the [Promotion Program Creation Flow](www.lightrail.com) (not yet live) you'll select the option "Unique Promotion Code". Once you've created the Promotion Program, all you need is the `programId`. 

Below is the request to issue a unique code promotion.

`POST https://api.lightrail.com/v2/valueStores`
```json
{
    "valueStoreId": "unique-code-promo-xyz",
    "programId": "spring-savings-promo",
    "value": 500
}
```  

#### 3. Public Promotion Code
In the [Promotion Program Creation Flow](www.lightrail.com) (not yet live) you'll select the option "Public Promotion Code". 
Typically for these types of Promotions you'll create an instance of the Promotion as part of the Promotion Program Flow. 
This is where you'll specify the `publicCode` which can be used in your checkout. 

### Common Requests 

#### Using a Promotion as a Payment Source in Checkout
To use a Promotion in checkout you simply need to include it in the `sources` property. Again, this is based on `access` and possible values are `[customerId, secureCode, publicCode]`.   

Example:
```json
{
    "rail": "lightrail",
    "customerId": "cus_123"
}
```

#### Other Use-cases
See [here](https://lightrailapi.docs.apiary.io/#reference) for full documentation of what else you can do with Promotions.

### Support
Want more information on promotions? [Contact us](mailto:hello@lightrail.com) any time if you have any questions, we're here to help. 
