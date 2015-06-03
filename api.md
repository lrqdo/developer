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

