FORMAT: 1A
HOST: https://api.thefoodassembly.com/

# The Food Assembly API documentation

# Group Authentication

# Oauth token [/oauth/v2/token/]

## Create Token [POST]

Create a new access token by submitting a username/password pair or a refresh token.

+ Request username and password (application/json)
    {
        "grant_type": "password",
        "client_id": "example-client-id",
        "client_secret": "example-client-secret",
        "username": "test@example.org",
        "password": "passw123"
    }

+ Request refresh token (application/json)
    {
        "grant_type": "refresh_token",
        "refresh_token": "some-refresh-token",
        "client_id": "example-client-id",
        "client_secret": "example-client-secret",
    }

+ Response 200 (application/json)
    + Body
        {
            "access_token": "some_access_token",
            "expires_in": 3600,
            "token_type": "bearer",
            "scope": null,
            "refresh_token": "some_refresh_token"
        }

+ Response 400 (application/json)
    + Body
        {
            "error": "invalid_grant"
        }

# Group Assemblies

## Memberships [/me/]

Request user information to find out if user is member of one or several assemblies.

## GET

+ Request (application/json)

+ Response 200 (application/json)
    + Body
        {
            "isMember": true,
            "hivesAsMember": [
                {
                    "id": 123,
                    "name": "Le Comptoir Général",
                    "status": "open",
                    "joinedAt": "2015-04-26T17:25:47+02:00"
                },
                (...)
            ]
        }

# Group Sale

## Products for sale [/distribution/:id/products/]

This route is public.

## GET

+ Request (application/json)

+ Response 200 (application/json)
    + Body
        [
            {
                "id": 71175,
                "name": "cerises bigarreaux",
                "description": "Cerises bigarreaux de l'Yonne",
                "freshness": 1,
                "composition": null,
                "dfsp_priority": 28,
                "type": {
                    "id": 225,
                    "quantityUnit": "mg",
                    "quantityStrategy": "weight"
                },
                "farmId": 2893,
                "photoId": "5478488b5f4271bf76edfd3e",
                "storageLife": {
                    "amount":"2",
                    "unit":"days"
                },
                "offers": [
                    {
                        "id": 284201,
                        "count": null,
                        "quantity": {
                            "amount": 2000000,
                            "unit": "mg"
                        },
                        "price": {
                            "amount": 780,
                            "currency": "EUR"
                        },
                        "availableStock": "unlimited"
                    },
                    (...)
                ]
            },
            (...)
        ]

# Group Orders

## Current basket and orders [/orders/]

## GET

Attributes:

    + state (number)

Number | Name        | Description
-------|-------------|------------
1      | Cart        | Ongoing order. User is shopping.
2      | Cart locked | User has clicked button "Pay". A payment form has been generated on PSP side.
3      | Pending     | Payment is in progress. Pending PSP feedback.
4      | Confirmed   | Payment is successful.
5      | Shipped     | Distribution is over. Member got the order.
6      | *n/a*       | *n/a*
7      | Canceled    | Payment has been denied or canceled.
8      | *n/a*       | *n/a*

+ Request (application/json)

+ Response 200 (application/json)
    + Body
        {
            "count": 2,
            "orders: [
                {
                    "id": 123,
                    "createdAt": "2015-06-24T11:15:26+02:00",
                    "status": "basket",
                    "state": 1,
                    "totalPrice": {
                        "amount": 1120,
                        "currency": "EUR"
                    },
                    "countItems": 2,
                    "items": [...],
                    "distributionId": 456,
                    "distribution": {}
                },
                {
                    "id": 123,
                    "status": "order",
                    "state": 2,
                    "distributionId": 456,
                    "paymentUrl": "",
                    "paymentForm": "<form>...</form>",
                    "paymentRequest": {
                        "url": "https://secure.ogone.com/ncol/test/orderstandard_utf8.asp",
                        "data": "PSPID=TunzLRQDO&ORDERID=390878&AMOUNT=420&CURRENCY=EUR..."
                    },
                    "totalPrice": {
                        "amount": 1120,
                        "currency": "EUR"
                    },
                    "countItems": 2,
                    "items": [...],
                    "distributionId": 456,
                    "distribution": {}
                }
            ]
        }

## Pay a basket [/distributions/:id/basket/confirm/]

## POST

+ Request (application/json)

    {
        "urlAccept": "https://ppr.thefoodassembly.com/fr/basket/success/456",
        "urlDecline": "https://ppr.thefoodassembly.com/fr/basket",
        "urlCancel": "https://ppr.thefoodassembly.com/fr/basket"
    }

+ Response 200 (application/json)

    {
        "paymentForm": "<form>...</form>",
        "paymentRequest": {
            "url": "https://secure.ogone.com/ncol/test/orderstandard_utf8.asp",
            "data": "PSPID=TunzLRQDO&ORDERID=390878&AMOUNT=420&CURRENCY=EUR..."
        }
    }

+ Response 400 (application/json)

    {
        "error": "MissingUserInformation",
        "details": "Order cannot be placed because some user information are missing.",
        "orderUuid": "cce4b7d2-3e1b-45ef-b8a2-467b4db72f96"
    }

## Repay a failed order [/orders/:id/payments/]

## POST

+ Request (application/json)

    {
        "urlAccept": "https://ppr.thefoodassembly.com/fr/basket/success/456",
        "urlDecline": "https://ppr.thefoodassembly.com/fr/basket",
        "urlCancel": "https://ppr.thefoodassembly.com/fr/basket"
    }

+ Response 200 (application/json)

    {
        "id": 123,
        "status": "order",
        "state": 2,
        "distributionId": 456,
        "paymentUrl": "",
        "paymentForm": "<form>...</form>",
        "paymentRequest": {
            "url": "https://secure.ogone.com/ncol/test/orderstandard_utf8.asp",
            "data": "PSPID=TunzLRQDO&ORDERID=390878&AMOUNT=420&CURRENCY=EUR..."
        },
        "totalPrice": {
            "amount": 1120,
            "currency": "EUR"
        },
        "countItems": 2,
        "items": [...],
        "distributionId": 456,
        "distribution": {}
    }

+ Response 400 (application/json)

    {
        "error": "MissingUserInformation",
        "details": "Order cannot be placed because some user information are missing.",
        "orderUuid": "cce4b7d2-3e1b-45ef-b8a2-467b4db72f96"
    }
