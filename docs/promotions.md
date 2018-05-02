# Promotions
Lightrail enables a wide variety of promotion use-cases. A few common examples are:
 1. Personalized promotion associated with a customer.
 2. A site wide promotion code that can be entered during checkout.
 3. Unique promotion code delivered to a potential customer.  
 
These types of promotions are differentiated primarily based on the `accessType` property.
In addition to how the promotion is accessed, they type of value they hold can also differ. 
Promotions can be valid for a number of dollars or points off, but they can also represent a percent discount.
These variations are all determine by properties on the `ValueStore` which represents the promotion.

### Getting Started with Promotions
To get started with promotions, you first need to create a `Program` which defines the default parameters for the promotion you want to create.

Let's look at an example of creating a sign-up promotion which will be attached to new sign-ups.  

#### Creating a Program for a Customer Promotion
The `Program` will define the basic properties like `currency`, `accessType` and `value` for the (`ValueStores`) that will be created from it. 
Below are the required and optional attributes needed for creating a `Program` for a promotion.

Example of creating a `Program` for a promotion that will be directly attached to a customer. 

`POST https://api.lightrail.com/v2/programs`
```json
{
    "programId": "sign-up-promotion",
    "name": "Spring Promotion USD",
    "currency": "USD",
    "access": "customerId",
    "preTax": false,
    "discount": true,
    "fixedInitialValues": [
        500
    ],
    "tags": ["promotion", "new-sign-up"]
}
``` 

#### Attributes
Below is the list of attributes used when creating an Account Program.
 - **programId** (_required_): Unique idempotent ID for the Program.
 - **currency** (_required_): Currency code. Can be a standard ISO form such as USD or CAD but can also be any branded currency, eg: `megabucks`.
 - **access** (_required_): In this case use `customerId` for attaching to a customer.
 - **discount** (_required_): Set to `true`. 
 - **minInitialValue** (_optional_): The minimum value the Value Store can be created with.
 - **maxInitialValue** (_optional_): The maximum value the Value Store can be created with.  
 - **fixedInitialValues** (_optional_): A list of values the Value Store can be created with.    
 - **name** (_optional_): A human-readable name for the Program.
 - **tags** (_optional_): Segmentation tags.
 - **metadata** (_optional_): Arbitrary data associated with the Program.

### Request to add the Promotion to a Customer 
Request to add a promotion to a customer.  

`POST https://api.lightrail.com/v2/valueStores`
```json
{
    "valueStoreId": "cus_123-sign-up-promotion",
    "programId": "sign-up-promotion",
    "customerId": "cus_123",
    "value": 500
}
``` 

#### Attributes
Below is the list of attributes used when creating an account from a Program.
- **valueStoreId** (_required_): Unique idempotent id for the ValueStore.
- **programId** (_required_): The programId of the Program this ValueStore is in.
- **customerId** (_required_): Unique ID for the Customer.
- **value** (_required_): In this case, must match the value defined in `fixedInitialValues`. 

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