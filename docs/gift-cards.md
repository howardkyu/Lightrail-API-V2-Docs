# Gift Cards
Allow your customers to purchase gift cards from your store and redeem them directly as part of the checkout process.   

Like all other Lightrail value, gift cards are backed by `ValueStores`. You’ll need to set up a Gift Card program to establish the default parameters for your gift cards.

### Getting Started with Gift Cards
To get started with gift cards, you first need to create a `Program` which defines the default parameters for your gift cards.

#### Creating a Program for your Gift Cards
The Program will define the basic properties like currency and allowed value ranges for your gift cards (`ValueStores`).    

Create your Account Program through the Lightrail web-app [here](https://www.lightrail.com) (web-app not yet complete).
Once the Program has been created you'll supply the `programId` into requests to create a Gift Card.  

#### Issuing a Gift Card
Creating Gift Cards is easy. Below is the call to do this. 

`POST https://api.lightrail.com/v2/valueStores`
```json
{
    "valueStoreId": "gc-n6se54",
    "programId": "gift-cards-usd",
    "value": 2500
}
``` 

### Common Requests  
Below are the most common requests made when interacting with Gift Cards.

#### Retrieve Gift Code
The unique code that's generated for a Gift Card must be retrieved via the following endpoint. It is not returned with the Gift Card (`ValueStore`) for security purposes.

`GET https://api.lightrail.com/v2/valueStores/<valueStoreId>/code`

Example response:

```json 
{
    "code": "ABCDEFGHIJKLM"
}
``` 

#### Using a Gift Card as a Payment Source in Checkout
Checkout is done using the `/transactions/orders` endpoint. To use a Gift Card as a payment source simply provide the following in the sources property of the request. 

```json
{
    "rail": "lightrail",
    "code": "ABCDEFGHIJKLM"
}
```

See [here](https://lightrailapi.docs.apiary.io/#reference/0/transactions/process-an-order) for full documentation of `/transactions/orders` endpoint.

### Support
Want more information on your gift card use-case? [Contact us](mailto:hello@lightrail.com) any time if you have any questions, we're here to help. 