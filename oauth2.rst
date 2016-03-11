OAuth2
======

Authorization Request
----------------------

To access a user's account, the user must first authorize your application so that you can get an access token. 
The simplest way to do this is to get and then redirect to Imgur's authorization url.

::

        var client = new ImgurClient("CLIENT_ID");
        var endpoint = new OAuth2Endpoint(client);
        var authorizationUrl = endpoint.GetAuthorizationUrl(OAuth2ResponseType.TOKEN_TYPE);
		
The authorization url will look similar to this: 

https://api.imgur.com/oauth2/authorize/?client_id=CLIENT_ID&response_type=TOKEN_TYPE

Once you have the url, your application must redirect to it. How you redirect will depend on your application.
Some common scenarios are listed here.

Using Windows Universal Apps
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

::

        Windows.System.Launcher.LaunchUriAsync(new Uri(authorizationUrl));
		
Using ASP.NET MVC
~~~~~~~~~~~~~~~~~

::

		return Redirect(authorizationUrl);

Authorization Response
----------------------

Once the user authorizes the application, Imgur will then redirect back to your application's Redirect URL. 
The Redirect URL is configured on Imgur under your account settings_.

.. _settings: https://imgur.com/account/settings/apps

The Redirect URL can be a localhost entry or an external url. In either case you will need a web server at this address with the ability to parse the url components.

Code Response
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If you requested a Code OAuth2ResponseType (OAuth2ResponseType.Code) then the response from Imgur is pretty easy to parse.

For example, if your Redirect URL is https://localhost:12345/ImgurResponse then a successful authorisation response from Imgur to this Redirect URL will look similar to this:

https://localhost:12345/ImgurResponse?code=a1fe7151b8d1011377776363e18300a12dd2e13e

The response contains the code value that should be parsed and stored by your application.

Once you have parsed it, a request to Imgur must be sent to get the OAuth2 token. Note you must use the Client Secret for this.

::

        var client = new ImgurClient("CLIENT_ID", "CLIENT_SECRET");
        var endpoint = new OAuth2Endpoint(client);
		var token = await endpoint.GetTokenByCodeAsync(code);

Token Response
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

A Token OAuth2ResponseType (OAuth2ResponseType.Token) contains all the data you need to create a token.

For example, if your Redirect URL is https://localhost:12345/ImgurResponse then a successful authorisation response from Imgur to this Redirect URL will look similar to this:

https://localhost:12345/ImgurResponse# access_token=ACCESS_TOKEN_VALUE &expires_in=2419200 &token_type=bearer &refresh_token=REFRESH_TOKEN_VALUE &account_username=Bob &account_id=122345

The response will contain several values that should be parsed and stored by your application.

-  access_token - The user's access token for this session.
-  refresh_token - The user's refresh token which should be used to refresh the access_token when it expires.
-  token_type - The type of token that should be used for authorization.
-  account_id - The user's account id.
-  account_username - The user's account username.
-  expires_in - The time in seconds when the user's access token expires. Default is one month - 2419200.

Note the hash "#" in the url. This is a fragment and is not returned to server side code. You may need to use JavaScript to parse the values instead of server side code.

Creating an OAuth2 token from the Redirect URL.
-----------------------------------------------

Using the Redirect URL values, an OAuth2 Token can be created.

::

        var token = new OAuth2Token("ACCESS_TOKEN", "REFRESH_TOKEN", "TOKEN_TYPE", 
                                    "ACCOUNT_ID", "ACCOUNT_USERNAME", EXPIRES_IN);

The token should be stored by your application. This will save your application from constructing a new token on each endpoint request.

Using the OAuth2 token.
-----------------------

Using the OAuth2 token can be done in two ways:

1. You may use it in the client's constructor:

::

        var client = new ImgurClient("CLIENT_ID", token);
::

        var client = new ImgurClient("CLIENT_ID", "CLIENT_SECRET", token);

2. You may also switch or set the token explicitly using the client's SetOAuth2Token method:

::

        var client = new ImgurClient("CLIENT_ID");
        client.SetOAuth2Token(token);

Getting an OAuth2 token from the Refresh Token.
-----------------------------------------------

If the access token has expired but you still have the refresh token, you can request a new OAuth2 token.
Note that you must use the Client Secret to get the refresh token.

::

        var client = new ImgurClient("CLIENT_ID", "CLIENT_SECRET");
        var endpoint = new OAuth2Endpoint(client);
        var token = endpoint.GetTokenByRefreshTokenAsync("REFRESH_TOKEN");

More information on Imgur's OAuth2 implementation can be found at https://api.imgur.com/oauth2