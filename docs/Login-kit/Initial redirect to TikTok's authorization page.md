https://developers.tiktok.com/doc/login-kit-web/


Initial redirect to TikTok's authorization page

To make the initial redirect request to TikTok's authorization server, the following query parameters below must be added to the Authorization Page URL using the application/x-www-form-urlencoded format.

For example, you can use an online URL encoder to encode parameters. Select UTF-8 as the destination character set.

Parameter
	

Type
	

Description

client_key
	

String
	

The unique identification key provisioned to the partner.

scope
	

String
	

A comma (,) separated string of authorization scope(s). These scope(s) are assigned to your application on the TikTok for Developers website. They handle what content your application can and cannot access. If a scope is toggleable, the user can deny access to one scope while granting access to others.

redirect_uri
	

String
	

The redirect URI that you requested for your application. It must match one of the redirect URIs you registered for the app.

state
	

String
	

The state is used to maintain the state of your request and callback. This value will be included when redirecting the user back to the client. Check if the state returned in the callback matches what you sent earlier to prevent cross-site request forgery.

The state can also include customized parameters that you want TikTok service to return.

response_type
	

String
	

This value should always be set to code.

Redirect your users to the authorization page URL and supply the necessary query parameters. Note that the page can only be accessed through HTTPS.

Type
	

Description

URL
	

https://www.tiktok.com/v2/auth/authorize/

Query parameters

	

client_key=<client_key>&response_type=code&scope=<scope>&redirect_uri=<redirect_uri>&state=<state>
