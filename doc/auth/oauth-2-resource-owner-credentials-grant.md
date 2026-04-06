
# OAuth 2 Resource Owner Credentials Grant



Documentation for accessing and setting credentials for OAuthROPCG.

## Auth Credentials

| Name | Type | Description | Getter |
|  --- | --- | --- | --- |
| OAuthClientId | `str` | OAuth 2 Client ID | `o_auth_client_id` |
| OAuthClientSecret | `str` | OAuth 2 Client Secret | `o_auth_client_secret` |
| OAuthUsername | `str` | OAuth 2 Resource Owner Username | `o_auth_username` |
| OAuthPassword | `str` | OAuth 2 Resource Owner Password | `o_auth_password` |
| OAuthToken | `OAuthToken` | Object for storing information about the OAuth token | `o_auth_token` |



**Note:** Auth credentials can be set using `OAuthROPCGCredentials` object, passed in as named parameter `o_auth_ropcg_credentials` in the client initialization.

## Usage Example

### Client Initialization

You must initialize the client with *OAuth 2.0 Resource Owner Password Credentials Grant* credentials as shown in the following code snippet.

```python
from multiauthsample.http.auth.o_auth_ropcg import OAuthROPCGCredentials
from multiauthsample.multiauthsample_client import MultiauthsampleClient

client = MultiauthsampleClient(
    o_auth_ropcg_credentials=OAuthROPCGCredentials(
        o_auth_client_id='OAuthClientId',
        o_auth_client_secret='OAuthClientSecret',
        o_auth_username='OAuthUsername',
        o_auth_password='OAuthPassword'
    )
)
```



Your application must obtain user authorization before it can execute an endpoint call in case this SDK chooses to use *OAuth 2.0 Resource Owner Password Credentials Grant*. This authorization includes the following steps

The `fetch_token()` method will exchange the user's credentials for an *access token*. The access token is an object containing information for authorizing client requests and refreshing the token itself.

```python
try:
    token = client.o_auth_ropcg.fetch_token()
    o_auth_ropcg_credentials = client.config.o_auth_ropcg_credentials.clone_with(o_auth_token=token)
    config = client.config.clone_with(o_auth_ropcg_credentials=o_auth_ropcg_credentials)
    client = MultiauthsampleClient(config=config)
except OAuthProviderException as ex:
    # handle exception
    pass
except APIException as ex:
    # handle exception
    pass
```

The client can now make authorized endpoint calls.

### Refreshing the token

An access token may expire after sometime. To extend its lifetime, you must refresh the token.

```python
if client.o_auth_ropcg.is_token_expired():
    try:
        token = client.o_auth_ropcg.refresh_token()
        o_auth_ropcg_credentials = client.config.o_auth_ropcg_credentials.clone_with(o_auth_token=token)
        config = client.config.clone_with(o_auth_ropcg_credentials=o_auth_ropcg_credentials)
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
save_token_to_database(client.config.o_auth_ropcg_credentials.o_auth_token)
```

### Creating a client from a stored token

To authorize a client using a stored access token, just set the access token in Configuration along with the other configuration parameters before creating the client:

```python
client = MultiauthsampleClient(
    o_auth_ropcg_credentials=OAuthROPCGCredentials(
        o_auth_token=load_token_from_database()
    )
)
```

### Complete example



```python
from multiauthsample.multiauthsample_client import MultiauthsampleClient
from multiauthsample.http.auth.o_auth_ropcg import OAuthROPCGCredentials
from multiauthsample.exceptions.o_auth_provider_exception import OAuthProviderException
from multiauthsample.exceptions.api_exception import APIException

from multiauthsample.http.auth.o_auth_ropcg import OAuthROPCGCredentials
from multiauthsample.multiauthsample_client import MultiauthsampleClient

client = MultiauthsampleClient(
    o_auth_ropcg_credentials=OAuthROPCGCredentials(
        o_auth_client_id='OAuthClientId',
        o_auth_client_secret='OAuthClientSecret',
        o_auth_username='OAuthUsername',
        o_auth_password='OAuthPassword'
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
    o_auth_ropcg_credentials = client.config.o_auth_ropcg_credentials.clone_with(o_auth_token=previous_token)
    config = client.config.clone_with(o_auth_ropcg_credentials=o_auth_ropcg_credentials)
    client = MultiauthsampleClient(config=config)
else:
    # obtain new access token
    try:
        token = client.o_auth_ropcg.fetch_token()
        save_token_to_database(token)
        o_auth_ropcg_credentials = client.config.o_auth_ropcg_credentials.clone_with(o_auth_token=token)
        config = client.config.clone_with(o_auth_ropcg_credentials=o_auth_ropcg_credentials)
        client = MultiauthsampleClient(config=config)
    except OAuthProviderException as ex:
        # handle exception
        pass
    except APIException as ex:
        # handle exception
        pass

# the client is now authorized and you can use controllers to make endpoint calls
# client will automatically refresh the token when it expires and call the token update callback
```


