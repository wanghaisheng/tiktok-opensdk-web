https://developers.tiktok.com/doc/oauth-user-access-token-management

Endpoints
1. Fetch an access token using an authorization code

Once the authorization code callback is handled, you can use the code to retrieve the user's access token.
Endpoint

POST https://open.tiktokapis.com/v2/oauth/token/
Headers

Key
	

Value

Content-Type
	

application/x-www-form-urlencoded
Request body parameters

Key
	

Type
	

Description

client_key
	

string
	

The unique identification key provisioned to the partner.

client_secret
	

string
	

The unique identification secret provisioned to the partner.

code

	

string

	

The authorization code from the web, iOS, or Android authorization callback. The value should be URL decoded.

grant_type
	

string
	

Its value should always be set as authorization_code.

redirect_uri

	

string
	

Its value must be the same as the redirect_uri used for requesting code.

code_verifier
	

string
	

Required for mobile app only. Code verifier is used to generate code challenge in PKCE authorization flow.
Response struct

Key
	

Type
	

Description

open_id
	

string
	

The TikTok user's unique identifier.

scope
	

string
	

A comma-separated list (,) of the scopes the user has agreed to authorize.

access_token
	

string
	

The access token for future calls on behalf of the user.

expires_in

	

int64
	

The expiration of access_token in seconds. It is valid for 24 hours after initial issuance.

refresh_token
	

string
	

The token to refresh access_token. It is valid for 365 days after the initial issuance.

refresh_expires_in

	

int64
	

The expiration of refresh_token in seconds.

token_type
	

string
	

The value should be Bearer.

Make sure to store these values on your back end as they are needed to persist access.
Example

curl --location --request POST 'https://open.tiktokapis.com/v2/oauth/token/' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--header 'Cache-Control: no-cache' \
--data-urlencode 'client_key=CLIENT_KEY' \
--data-urlencode 'client_secret=CLIENT_SECRET' \
--data-urlencode 'code=CODE' \
--data-urlencode 'grant_type=authorization_code' \
--data-urlencode 'redirect_uri=REDIRECT_URI'

If the request is successful, the response will look like the following.

{
    "access_token": "act.9db86c59fab331b4f8b4c516ae66a960Nn4RYBnDsdkVlzDowUoDG4Gwt7TW!5259",
    "expires_in": 86400,
    "open_id": "afd97af1-b87b-48b9-ac98-410aghda5344",
    "refresh_expires_in": 31536000,
    "refresh_token": "rft.e10a897yu15f821ab5f08stuart1fc9f245qgUitnFeEvbWq4ZBoemE36DKZABC!5294",
    "scope": "user.info.basic,video.list",
    "token_type": "Bearer"
}

If the request is not successful, an error response body will be returned in the response, like the following.

{
    "error": "invalid_request",
    "error_description": "Redirect_uri is not matched with the uri when requesting code.",
    "log_id": "202206221854370101130062072500FFA2"
}

2. Refresh an access token using a refresh token

Although the fetched access_token expires within 24 hours, it can be refreshed without user consent. The developer's back-end server can schedule background jobs to keep tokens up to date.
Endpoint

POST https://open.tiktokapis.com/v2/oauth/token/
Headers

Key
	

Value

Content-Type
	

application/x-www-form-urlencoded
Request body parameters

Key
	

Type
	

Description

client_key
	

string
	

The unique identification key provisioned to the partner.

client_secret
	

string
	

The unique identification secret provisioned to the partner.

grant_type
	

string
	

Its value should always be set as refresh_token.

refresh_token
	

string
	

The user's refresh token.
Response struct

Key
	

Type
	

Description

open_id
	

string
	

The partner-facing user ID.

scope

	

string
	

A comma-separated list (,) of the scopes the user has agreed to authorize.

access_token

	

string
	

The new token for future calls on behalf of the user.

expires_in
	

int64
	

The expiration of the access token in seconds.

refresh_token

	

string
	

The token to refresh a user's access_token.

Note: The returned refresh_token may be different than the one passed in the payload. You must use the newly-returned token if the value is different than the previous one.

refresh_expires_in

	

int64
	

The expiration for refresh_token in seconds.

token_type
	

string
	

The value should be Bearer.

Make sure to store these values on your back end as they are needed to persist access.
Example

curl --location --request POST 'https://open.tiktokapis.com/v2/oauth/token/' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--header 'Cache-Control: no-cache' \
--data-urlencode 'client_key=CLIENT_KEY' \
--data-urlencode 'client_secret=CLIENT_SECRET' \
--data-urlencode 'grant_type=refresh_token' \
--data-urlencode 'refresh_token=REFRESH_TOKEN'

If the request is successful, the response will look like the following.

{
    "access_token": "act.9db86c59fab331b4f8b4c516ae66a960Nn4RYBnDsdkVlzDowUoDG4Gwt7TW!5259",
    "expires_in": 86400,
    "open_id": "afd97af1-b87b-48b9-ac98-410aghda5344",
    "refresh_expires_in": 31536000,
    "refresh_token": "rft.e10a897yu15f821ab5f08stuart1fc9f245qgUitnFeEvbWq4ZBoemE36DKZABC!5294",
    "scope": "user.info.basic,video.list",
    "token_type": "Bearer"
}

If the request is not successful, an error response body will be returned in the response, like the following.

{
    "error": "invalid_request",
    "error_description": "The request parameters are malformed.",
    "log_id": "202206221854370101130062072500FFA2"
}

3. Revoke access

When a user wants to disconnect the connection between your application and TikTok, you can revoke their tokens so the user will no longer see your application on the Manage app permissions page of the TikTok for Developers website.
Endpoint

POST https://open.tiktokapis.com/v2/oauth/revoke/
Headers

Key
	

Value

Content-Type
	

application/x-www-form-urlencoded
Request body parameters

Key
	

Type
	

Description

client_key

	

string
	

The unique identification key provisioned to the partner.

client_secret
	

string
	

The unique identification secret provisioned to the partner.

token
	

string
	

The access_token that bears the authorization of the TikTok user.
Response struct

If the request is successful, the response struct will be empty.
Example

curl --location --request POST 'https://open.tiktokapis.com/v2/oauth/revoke/' \
--header 'Content-Type: application/x-www-form-urlencoded' \
--header 'Cache-Control: no-cache' \
--data-urlencode 'client_key=CLIENT_KEY' \
--data-urlencode 'client_secret=CLIENT_SECRET' \
--data-urlencode 'token=ACCESS_TOKEN'

If the request is not successful, an error response body will be returned in the response, like the following.

{
    "error": "invalid_request",
    "error_description": "The request parameters are malformed.",
    "log_id": "202206221854370101130062072500FFA2"
}
