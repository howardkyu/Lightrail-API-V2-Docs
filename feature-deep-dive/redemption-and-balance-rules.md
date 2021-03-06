# Redemption Rules and Balance Rules
Redemption Rules and Balance Rules are extra conditions placed on Values that are evaluated during checkout. Redemption rules determine if a Value can be used and evaluate to true or false. Balance Rules enable more advanced balance behaviour, such as percent off, and evaluate to a number. Rules are typically used for promotions and represent a discount to the customer. Let's look at a few common examples.  

**Example 1: $5 off transactions over $100** 

In this case, the Value would simply have a balance of $5 and the Redemption Rule would require that the transaction subtotal is over $100.

Create Value request:
```json
{
    "id": "example",
    "currency": "USD",
    "balance": 500,
    "redemptionRule": {
        "rule": "totals.subtotal >= 10000",
        "explanation": "Applies to orders over $100."
    },
    "discount": true
}
```

**Example 2: 50% off red hats**

This example requires the use of a Balance Rule in combination with a Redemption Rule. The Redemption Rule restricts the Value to apply strictly to an item with productId `red-hat`. The Balance Rule causes the Value to be worth 50% of the item's subtotal.

Create Value request:
```json
{
    "id": "example",
    "currency": "USD",
    "redemptionRule": {
        "rule": "currentLineItem.productId == 'red-hat'",
        "explanation": "Promotion can be used towards purchase of red hats."
    },
    "balanceRule": {
        "rule": "currentLineItem.lineTotal.subtotal * 0.5",
        "explanation": "50% off item's subtotal."
    },
    "discount": true
}
```

## How Rules Work
Balance and Redemption Rules are evaluated for each line item during checkout. Rules operate on a Rule Context which contains the current line item (`currentLineItem`), the transaction totals (`totals`), a list of all of the line items in the transaction (`lineItems`), and the transaction metadata (`metadata`).

### Rule Context 
```json
{
    "currentLineItem": {
        "type": "product | shipping | fee",
        "productId": "string",
        "variantId": "string",
        "unitPrice": "number",
        "quantity": "number",
        "tags": "string[]",
        "taxRate": "number",
        "metadata": "object",
        "lineTotal": {
            "subtotal": "number",
            "discount": "number",
            "remainder": "number"
        }
    }, 
    "totals": {
        "subtotal": "number"
    }, 
    "lineItems": [
        {
            "type": "product | shipping | fee",
            "productId": "string",
            "variantId": "string",
            "unitPrice": "number",
            "quantity": "number",
            "tags": "string[]",
            "taxRate": "number",
            "metadata": "object",
            "lineTotal": {
                "subtotal": "number",
                "discount": "number",
                "remainder": "number"
            }
        }
    ],
    "metadata": "object"
}
```

You can think of the Rule Context being created as a simple map which the Rules evaluate on. 

## Examples Continued
**50% off everything**

Create Value request:
```json
{
    "id": "example",
    "currency": "USD",
    "balanceRule": {
         "rule": "currentLineItem.lineTotal.subtotal * 0.5",
         "explanation": "50% off line item's subtotal."
     },
    "discount": true
}
```

**Up to $5 off order, limiting to one discount per line item**

Create Value request:
```json
{
    "id": "example",
    "currency": "USD",
    "balance": 500,
    "redemptionRule": {
        "rule": "currentLineItem.lineTotal.discount == 0",
        "explanation": "Limited to 1 discount per item."
    },
    "discount": true
}
```

**25% off transactions over $100 and limited to 1 promotion per item**
```json
{
    "id": "example",
    "currency": "USD",
    "balance": 500,
    "redemptionRule": {
        "rule": "totals.subtotal >= 10000 && currentLineItem.lineTotal.discount == 0",
        "explanation": "Applies to orders over $100. Limited to 1 discount per item."
    },
    "balanceRule": {
        "rule": "currentLineItem.lineTotal.subtotal * 0.25",
        "explanation": "25% off item's subtotal."
    },
    "discount": true
}
```

## Support
For more information on rule syntax please see [Rule Sytnax](https://github.com/Giftbit/Lightrail-API-V2-Docs/blob/master/feature-deep-dive/rule-syntax.md). 

[Contact us](mailto:hello@lightrail.com) any time if you have any questions, we're here to help. 
