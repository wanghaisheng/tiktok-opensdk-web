https://developers.tiktok.com/doc/login-kit-web/

Manage authorization response

If the user authorizes access, they will be redirected to redirect_uri with the following query parameters appended using application/x-www-form-urlencoded format:

Parameter
	

Type
	

Description

code

	

String
	

Authorization code that is used in getting an access token.

scopes

	

String
	

A comma-separated (,) string of authorization scope(s), which the user has granted.

state

	

String

	

A unique, non-guessable string when making the initial authorization request. This value allows you to prevent CSRF attacks by confirming that the value coming from the response matches the one you sent.

error

	

String

	

If this field is set, it means that the current user is not eligible for using third-party login or authorization. The partner is responsible for handling the error gracefully.

error_description
	

String
	

If this field is set, it will be a human-readable description about the error.
