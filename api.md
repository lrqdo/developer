FORMAT: 1A
HOST: https://api.thefoodassembly.com/

# The Food Assembly API documentation

# Group Authentication

# Oauth token [/oauth/v2/token]

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

## Memberships [/me]

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
                { ... }
            ]
        }
