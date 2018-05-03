# Accounts and Points
Accounts or Loyalty Point solutions are used when tracking value associated with a customer.  
Typically this is used for integrations where a customer can earn value, such as dollars or credits and we think of this value as an "account" associated with the customer.  

Your customer's account may represent value that can be used during checkout or it may represent points or credits that can be redeemed for in app rewards or promotions. 

Like all other Lightrail value, accounts are backed by `ValueStores`. 

### Getting Started with Accounts
To get started with accounts, you first need to create a `Program` which defines the default parameters for you accounts.

#### Creating a Program for your Accounts
The `Program` will define the basic properties like currency for the accounts (`ValueStores`) that will be created from it. 

Create your `Account Program` through the Lightrail web-app [here](https://www.lightrail.com) (web-app not yet complete).
Once the `Program` has been created you'll supply the `programId` into requests to create `Accounts`.  

### Creating an Account
If no `Customer` exists, you first need to create one to associate the `Account` with.

Request to create `Customer`.

`POST https://api.lightrail.com/v2/customers`
```json
{
    "customerId": "cus_123",
    "email": "alice@examle.com"
}
```

Next, you'll create a `ValueStore` which will represent the `Account` from your `Account Program`.
Remember, `Accounts` are just a `ValueStore` with certain properties defined by the `Program`. 
As such, you'll create `Accounts` through the `/valueStores` endpoint. 

Request to create `Account`.  
`POST https://api.lightrail.com/v2/valueStores`
```json
{
    "valueStoreId": "account-h54sya3",
    "programId": "customer-accounts-usd",
    "customerId": "cus_123",
    "value": 2500
}
``` 

#### Attributes
Below is the list of attributes used when creating an account from a Program.
- **valueStoreId** (_required_): Unique idempotent id for the ValueStore.
- **programId** (_required_): The programId of the Program this ValueStore is in.
- **customerId** (_required_): Unique ID for the Customer.
- **value** (_optional_): An integer greater than or equal to 0 representing the initial value of the Account.

### Common Requests  
Below are the most common requests made when interacting with accounts.

#### Crediting
Crediting is used when adding value to an account.

`POST https://api.lightrail.com/v2/transactions/credit`
```json
{
    "transactionId": "unique-id-123",
    "destination": {
        "rail": "lightrail",
        "valueStoreId": "account-h54sya3"
    },
    "amount": 2500,
    "currency": "USD",
    "metadata": {
        "note": "Reloaded balance from credit card.",
        "chargeId": "ch_1234567"
    }
}  
```

See full credit endpoint [here](https://lightrailapi.docs.apiary.io/#reference/0/transactions/credit). 

#### Debiting
Debiting is used when removing value from an account.

`POST https://api.lightrail.com/v2/transactions/credit`
```json
{
    "transactionId": "unique-id-123",
    "source": {
        "rail": "lightrail",
        "valueStoreId": "account-h54sya3"
    },
    "amount": 2500,
    "currency": "XXX",
    "metadata": {
      "note": "Reduce loyalty points after 3mo customer inactivity"
    }
}
```

See full debit endpoint [here](https://lightrailapi.docs.apiary.io/#reference/0/transactions/debit).

#### Using Accounts as a Payment Source in Checkout
Checkout is done using the `/transactions/orders` endpoint. To use an account directly as a payment source simply provide the following in the sources property of the request. 

```json
{
    "rail": "lightrail",
    "valueStoreId": "account-h54sya3"
}
```

Alternatively, since the account is associated with the customer, you can directly use the `customerId` as a payment source. This will consider all `ValueStores` associated with the customer.
```json
{
    "rail": "lightrail",
    "customerId": "cus_123"
}
```

See [here](https://lightrailapi.docs.apiary.io/#reference/0/transactions/process-an-order) for full documentation of `/transactions/orders` endpoint.

### Support
Want more information on your accounts or loyalty points use-case? [Contact us](mailto:hello@lightrail.com) any time if you have any questions, we're here to help. 