### Get a Currency [GET /currencies/{code}]

+ Parameter
    + code (string) - the code of the Currency to get.

+ Request
    + Headers
    
            {{header.authorization}}

+ Response 200 (application/json)
    + Attributes (Currency)

    + Body

            {
                "code": "USD",
                "name": "US Dollars",
                "symbol": "$",
                "decimalPlaces": 2
            }

### List Currencies [GET /currencies]

+ Request
    + Headers
    
            {{header.authorization}}

+ Response 200 (application/json)
    + Attributes (array[Currency])

    + Body

            [
                {
                    "code": "USD",
                    "name": "US Dollars",
                    "symbol": "$",
                    "decimalPlaces": 2
                }
            ]
