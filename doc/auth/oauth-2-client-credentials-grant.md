
# OAuth 2 Client Credentials Grant



Documentation for accessing and setting credentials for OAuthCCG.

## Auth Credentials

| Name | Type | Description | Getter |
|  --- | --- | --- | --- |
| OAuthClientId | `str` | OAuth 2 Client ID | `o_auth_client_id` |
| OAuthClientSecret | `str` | OAuth 2 Client Secret | `o_auth_client_secret` |
| OAuthToken | `OAuthToken` | Object for storing information about the OAuth token | `o_auth_token` |
| OAuthTokenProvider | `Callable[[OAuthToken, OAuth2], OAuthToken]` | Registers a callback for oAuth Token Provider used for automatic token fetching/refreshing. | `o_auth_token_provider` |
| OAuthOnTokenUpdate | `Callable[[OAuthToken], None]` | Registers a callback for token update event. | `o_auth_on_token_update` |
| OAuthClockSkew | `int` | Clock skew time in seconds applied while checking the OAuth Token expiry. | `o_auth_clock_skew` |



**Note:** Auth credentials can be set using `OAuthCCGCredentials` object, passed in as named parameter `o_auth_ccg_credentials` in the client initialization.

## Usage Example

### Client Initialization

You must initialize the client with *OAuth 2.0 Client Credentials Grant* credentials as shown in the following code snippet. This will fetch the OAuth token automatically when any of the endpoints, requiring *OAuth 2.0 Client Credentials Grant* authentication, are called.

```python
from multiauthsample.http.auth.o_auth_ccg import OAuthCCGCredentials
from multiauthsample.multiauthsample_client import MultiauthsampleClient

client = MultiauthsampleClient(
    o_auth_ccg_credentials=OAuthCCGCredentials(
        o_auth_client_id='OAuthClientId',
        o_auth_client_secret='OAuthClientSecret'
    )
)
```



Your application can also manually provide an OAuthToken using the setter `o_auth_token` in `OAuthCCGCredentials` object. This function takes in an instance of OAuthToken containing information for authorizing client requests and refreshing the token itself.

### Adding OAuth Token Update Callback

Whenever the OAuth Token gets updated, the provided callback implementation will be executed. For instance, you may use it to store your access token whenever it gets updated.

```python
from multiauthsample.http.auth.o_auth_ccg import OAuthCCGCredentials
from multiauthsample.multiauthsample_client import MultiauthsampleClient

client = MultiauthsampleClient(
    o_auth_ccg_credentials=OAuthCCGCredentials(
        o_auth_client_id='OAuthClientId',
        o_auth_client_secret='OAuthClientSecret',
        o_auth_on_token_update=(lambda o_auth_token:
                                # Add the callback handler to perform operations like save to DB or file etc.
                                # It will be triggered whenever the token gets updated
                                save_token_to_database(o_auth_token))
    )
)
```

### Adding Custom OAuth Token Provider

To authorize a client using a stored access token, set up the `o_auth_token_provider` in `OAuthCCGCredentials` along with the other auth parameters before creating the client:

```python
from multiauthsample.http.auth.o_auth_ccg import OAuthCCGCredentials
from multiauthsample.multiauthsample_client import MultiauthsampleClient

def _o_auth_token_provider(last_oauth_token, auth_manager):
    # Add the callback handler to provide a new OAuth token
    # It will be triggered whenever the last provided o_auth_token is null or expired
    o_auth_token = load_token_from_database()
    if o_auth_token is None:
        o_auth_token = auth_manager.fetch_token()
    return o_auth_token


client = MultiauthsampleClient(
    o_auth_ccg_credentials=OAuthCCGCredentials(
        o_auth_client_id='OAuthClientId',
        o_auth_client_secret='OAuthClientSecret',
        o_auth_token_provider=_o_auth_token_provider
    )
)
```


