## Errors

Lightrail uses the following HTTP status codes to indicate an error:

| code | meaning |
|------|---------|
| 400  | The request could not be understood.  eg: JSON body could not be parsed. |
| 401  | Authentication missing. |
| 403  | The operation is not allowed for the given authentication. |
| 404  | The resource was not found.  eg: There is no Contact for the given ID. |
| 409  | The operation could not be performed because of the state of the system.  eg: There is not enough balance for a Transaction to complete. |
| 422  | The request was understood but has a logical problem.  eg: Attempting to credit a negative amount. If using the `stripe` payment rail, most Stripe errors will result in a `422`. |
| 424  | The request failed due to an error which leave the system in an inconsistent state between Lightrail and a third party API. eg: While attempting to reverse a checkout Transaction with two Stripe charges, the first charge is refunded but the second refund fails. Refunds in Stripe cannot be undone which leaves the system in a state that it cannot automatically recover from. Note, this status code is extremely rare and represents a worst-case scenario for which some intervening action will be necessary. |  
| 429  | Too many requests in a given amount of time. |
| 500  | Internal server error.  Please [contact us](mailto:hello@lightrail.com) with details of your request and we'll look into it. |

Lightrail errors contain a JSON body with the following properties:

| property    | always present | purpose |
|-------------|----------------|---------|
| message     | yes            | English explanation of the error.  This is for display purposes only as the explanation may be formatted or change between system updates. |
| statusCode  | yes            | The HTTP status code. |
| messageCode | no             | A constant corresponding to the message.  This can be used to take action in response to the error. |
| stripeError | no             | When using the `stripe` rail: the full error response from Stripe in case of an error charging a credit card. |

An example:

```json
{
    "message": "Insufficient balance for the transaction.",
    "statusCode": 409,
    "messageCode": "InsufficientBalance"
}
```
