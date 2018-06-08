# Value Rules
Value rules enable the value of a Promotion to be dependant on the line items in a Transaction. 
Value rules typically enable a simple value calculation like %20 off a certain product in checkout or 10% off orders over $50.

## Examples
Here are some common examples.

### 10% Everything
```
valueRule = currentLineItem.lineTotal.subtotal * 0.20
```

### 10% Off a particular product

## Advanced
If you want to get creative this is what you should know.

### Context  
The context is what's passed into the rule and what the rule evaluates.

```json
{
  "id": "transaction-id",
  "type": "order",
  "totals": {
    "subTotal": 600
  },
  "lineItems": [
    {
      "type": "product", // required. must = "product", "shipping", or "fee"
      "productId": "1", // optioanl
      "shippingId": "s1yk234", // optional 
      "feeId": "atse3a", // optional
      "variantId": "atse3a", // optional
      "unitPrice": 300, // optional, in cents
      "quantity": 2, // optional, defaults to 1
      "tags": ["summer", "clearance"], // optional 
      "taxRate": 0.05, // optional
      
      "metadata": { // optional additional data
        "additonalData": "xyz"
      }
    }
  ],
  "currentLineItem":{
    "type": "product", // required. must = "product", "shipping", or "fee"
    "productId": "1", // optioanl
    "shippingId": "s1yk234", // optional 
    "feeId": "atse3a", // optional
    "variantId": "atse3a", // optional
    "unitPrice": 300, // optional, in cents
    "quantity": 2, // optional, defaults to 1
    "tags": ["summer", "clearance"], // optional 
    "taxRate": 0.05, // optional
    "lineTotal": {
      "subtotal": 600  
    },
    "metadata": { // optional additional data
      "additonalData": "xyz"
    }
  }
}

```