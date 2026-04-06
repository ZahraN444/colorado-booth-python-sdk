
# OAuth 2 Authorization Code Grant



Documentation for accessing and setting credentials for OAuthACG.

## Auth Credentials

| Name | Type | Description | Getter |
|  --- | --- | --- | --- |
| OAuthClientId | `str` | OAuth 2 Client ID | `o_auth_client_id` |
| OAuthClientSecret | `str` | OAuth 2 Client Secret | `o_auth_client_secret` |
| OAuthRedirectUri | `str` | OAuth 2 Redirection endpoint or Callback Uri | `o_auth_redirect_uri` |
| OAuthToken | `OAuthToken` | Object for storing information about the OAuth token | `o_auth_token` |
| OAuthScopes | `List[OAuthScopeOAuthACGEnum]` | List of scopes that apply to the OAuth token | `o_auth_scopes` |



**Note:** Auth credentials can be set using `OAuthACGCredentials` object, passed in as named parameter `o_auth_acg_credentials` in the client initialization.

## Usage Example

### 1\. Client Initialization

You must initialize the client with *OAuth 2.0 Authorization Code Grant* credentials as shown in the following code snippet.

```python
from multiauthsample.http.auth.o_auth_acg import OAuthACGCredentials
from multiauthsample.models.o_auth_scope_o_auth_acg_enum import OAuthScopeOAuthACGEnum
from multiauthsample.multiauthsample_client import MultiauthsampleClient

client = MultiauthsampleClient(
    o_auth_acg_credentials=OAuthACGCredentials(
        o_auth_client_id='OAuthClientId',
        o_auth_client_secret='OAuthClientSecret',
        o_auth_redirect_uri='OAuthRedirectUri',
        o_auth_scopes=[
            OAuthScopeOAuthACGEnum.READ_SCOPE
        ]
    )
)
```



Your application must obtain user authorization before it can execute an endpoint call in case this SDK chooses to use *OAuth 2.0 Authorization Code Grant*. This authorization includes the following steps

### 2\. Obtain user consent

To obtain user's consent, you must redirect the user to the authorization page.The `get_authorization_url()` method creates the URL to the authorization page. You must have initialized the client with scopes for which you need permission to access.

```python
auth_url = client.o_auth_acg.get_authorization_url()
```

### 3\. Handle the OAuth server response

Once the user responds to the consent request, the OAuth 2.0 server responds to your application's access request by redirecting the user to the redirect URI specified set in `Configuration`.

If the user approves the request, the authorization code will be sent as the `code` query string:

```
https://example.com/oauth/callback?code=XXXXXXXXXXXXXXXXXXXXXXXXX
```

If the user does not approve the request, the response contains an `error` query string:

```
https://example.com/oauth/callback?error=access_denied
```

### 4\. Authorize the client using the code

After the server receives the code, it can exchange this for an *access token*. The access token is an object containing information for authorizing client requests and refreshing the token itself.

```python
try:
    token = client.o_auth_acg.fetch_token('code')
    o_auth_acg_credentials = client.config.o_auth_acg_credentials.clone_with(o_auth_token=token)
    config = client.config.clone_with(o_auth_acg_credentials=o_auth_acg_credentials)
    client = MultiauthsampleClient(config=config)
except OAuthProviderException as ex:
    # handle exception
    pass
except APIException as ex:
    # handle exception
    pass
```

### Scopes

Scopes enable your application to only request access to the resources it needs while enabling users to control the amount of access they grant to your application. Available scopes are defined in the [`OAuthScopeOAuthACGEnum`](../../doc/models/o-auth-scope-o-auth-acg-enum.md) enumeration.

| Scope Name | Description |
|  --- | --- |
| `READ_SCOPE` | Read request for files |

### Refreshing the token

An access token may expire after sometime. To extend its lifetime, you must refresh the token.

```python
if client.o_auth_acg.is_token_expired():
    try:
        token = client.o_auth_acg.refresh_token()
        o_auth_acg_credentials = client.config.o_auth_acg_credentials.clone_with(o_auth_token=token)
        config = client.config.clone_with(o_auth_acg_credentials=o_auth_acg_credentials)
        client = MultiauthsampleClient(config=config)
    except OAuthProviderException as ex:
       # handle exception
       pass
```

If a token expires, an exception will be thrown before the next endpoint call requiring authentication.

### Storing an access token for reuse

It is recommended that you store the access token for reuse.

```python
# store token
save_token_to_database(client.config.o_auth_acg_credentials.o_auth_token)
```

### Creating a client from a stored token

To authorize a client using a stored access token, just set the access token in Configuration along with the other configuration parameters before creating the client:

```python
client = MultiauthsampleClient(
    o_auth_acg_credentials=OAuthACGCredentials(
        o_auth_token=load_token_from_database()
    )
)
```

### Complete example



```python
from multiauthsample.multiauthsample_client import MultiauthsampleClient
from multiauthsample.http.auth.o_auth_acg import OAuthACGCredentials
from multiauthsample.models.o_auth_scope_o_auth_acg_enum import OAuthScopeOAuthACGEnum
from multiauthsample.exceptions.o_auth_provider_exception import OAuthProviderException

client = MultiauthsampleClient(
    o_auth_acg_credentials=OAuthACGCredentials(
        o_auth_client_id='OAuthClientId',
        o_auth_client_secret='OAuthClientSecret',
        o_auth_redirect_uri='OAuthRedirectUri',
        o_auth_scopes=[
            OAuthScopeOAuthACGEnum.READ_SCOPE
        ]
    )
)
# function for storing token to database
def save_token_to_database(token):
    # code to save the token to database
    pass

# function for loading token from database
def load_token_from_database():
    # load token from database and return it (return None if no token exists)
    pass

# obtain access token, needed for client to be authorized
previous_token = load_token_from_database()
if previous_token:
    # restore previous access token
    o_auth_acg_credentials = client.config.o_auth_acg_credentials.clone_with(o_auth_token=previous_token)
    config = client.config.clone_with(o_auth_acg_credentials=o_auth_acg_credentials)
    client = MultiauthsampleClient(config=config)
else:
    # redirect user to a page that handles authorization
    pass

# the client is now authorized and you can use controllers to make endpoint calls
```


