# Promotions
Lightrail enables a wide variety of promotion use-cases. A few common examples are:
 1. Personalized promotion associated with a customer (`access: customerId`).  
 2. Unique promotion code delivered to a potential customer (`access: secureCode`).  
 3. A site wide promotion code that can be entered during checkout (`access: publicCode`).
 
These types of promotions are differentiated primarily based on the `access` property of ValueStores as you can see above. 
In addition to how the promotion is accessed, they type of value they hold can also differ. 
Promotions can be valid for a number of dollars or points off, but they can also represent a percent discount.
These variations are all determined by properties on the ValueStore which represents the promotion.

See below for concrete examples for each type of promotion. 

### Getting Started with Promotions
To get started with promotions, you first need to create a Program which defines the type of promotion you wish to create.
The Program is created within your Lightrail Account. 

While creating the Promotion Program, you'll specify parameters such as value, currency and additional properties regarding the Promotion.

#### 1. Personalized Promotion Attached to a Customer
In the [Promotion Program](www.lightrail.com)(not yet live) you'll select the option "Personalized Customer Promotion". 

Also, if no Customer exists, you first need to create one to associate the `Account` with. Below is the request to create a Customer. 
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
Details coming soon.

#### 3. Public Promotion Code
Details coming soon.

#### 2. 

### Common Requests 

#### Using the Promotion as a Payment Source in Checkout
Checkout is done using the `/transactions/orders` endpoint. Since the account is associated with the customer, you can directly use the `customerId` as a payment source. 
This will automatically use the promotion along with any other ValueStores associated with the customer.  

```json
{
    "rail": "lightrail",
    "customerId": "cus_123"
}
```

See [here](https://lightrailapi.docs.apiary.io/#reference/0/transactions/process-an-order) for full documentation of `/transactions/orders` endpoint.

### Support
Want more information on promotions? [Contact us](mailto:hello@lightrail.com) any time if you have any questions, we're here to help. 
