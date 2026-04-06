
# Client Class Documentation

The following parameters are configurable for the API Client:

| Parameter | Type | Description |
|  --- | --- | --- |
| access_token | `str` |  |
| port | `str` | *Default*: `"80"` |
| suites | `SuiteCodeEnum` | *Default*: `1` |
| environment | [`Environment`](../README.md#environments) | The API environment. <br> **Default: `Environment.TESTING`** |
| http_client_instance | `Union[Session, HttpClientProvider]` | The Http Client passed from the sdk user for making requests |
| override_http_client_configuration | `bool` | The value which determines to override properties of the passed Http Client from the sdk user |
| http_call_back | `HttpCallBack` | The callback value that is invoked before and after an HTTP call is made to an endpoint |
| timeout | `float` | The value to use for connection timeout. <br> **Default: 60** |
| max_retries | `int` | The number of times to retry an endpoint call if it fails. <br> **Default: 0** |
| backoff_factor | `float` | A backoff factor to apply between attempts after the second try. <br> **Default: 2** |
| retry_statuses | `Array of int` | The http statuses on which retry is to be done. <br> **Default: [408, 413, 429, 500, 502, 503, 504, 521, 522, 524]** |
| retry_methods | `Array of string` | The http methods on which retry is to be done. <br> **Default: ["GET", "PUT"]** |
| proxy_settings | [`ProxySettings`](../doc/proxy-settings.md) | Optional proxy configuration to route HTTP requests through a proxy server. |
| basic_auth_credentials | [`BasicAuthCredentials`](auth/basic-authentication.md) | The credential object for Basic Authentication |
| api_key_credentials | [`ApiKeyCredentials`](auth/custom-query-parameter.md) | The credential object for Custom Query Parameter |
| api_header_credentials | [`ApiHeaderCredentials`](auth/custom-header-signature.md) | The credential object for Custom Header Signature |
| o_auth_ccg_credentials | [`OAuthCCGCredentials`](auth/oauth-2-client-credentials-grant.md) | The credential object for OAuth 2 Client Credentials Grant |
| o_auth_acg_credentials | [`OAuthACGCredentials`](auth/oauth-2-authorization-code-grant.md) | The credential object for OAuth 2 Authorization Code Grant |
| o_auth_ropcg_credentials | [`OAuthROPCGCredentials`](auth/oauth-2-resource-owner-credentials-grant.md) | The credential object for OAuth 2 Resource Owner Credentials Grant |
| o_auth_bearer_token_credentials | [`OAuthBearerTokenCredentials`](auth/oauth-2-bearer-token.md) | The credential object for OAuth 2 Bearer token |

The API client can be initialized as follows:

## Code-Based Client Initialization

```python
from multiauthsample.configuration import Environment
from multiauthsample.http.auth.api_header import ApiHeaderCredentials
from multiauthsample.http.auth.api_key import ApiKeyCredentials
from multiauthsample.http.auth.basic_auth import BasicAuthCredentials
from multiauthsample.http.auth.o_auth_acg import OAuthACGCredentials
from multiauthsample.http.auth.o_auth_bearer_token import OAuthBearerTokenCredentials
from multiauthsample.http.auth.o_auth_ccg import OAuthCCGCredentials
from multiauthsample.http.auth.o_auth_ropcg import OAuthROPCGCredentials
from multiauthsample.models.o_auth_scope_o_auth_acg_enum import OAuthScopeOAuthACGEnum
from multiauthsample.models.suite_code_enum import SuiteCodeEnum
from multiauthsample.multiauthsample_client import MultiauthsampleClient

client = MultiauthsampleClient(
    access_token='accessToken',
    basic_auth_credentials=BasicAuthCredentials(
        username='Username',
        password='Password'
    ),
    api_key_credentials=ApiKeyCredentials(
        token='token',
        api_key='api-key'
    ),
    api_header_credentials=ApiHeaderCredentials(
        token='token',
        api_key='api-key'
    ),
    o_auth_ccg_credentials=OAuthCCGCredentials(
        o_auth_client_id='OAuthClientId',
        o_auth_client_secret='OAuthClientSecret'
    ),
    o_auth_acg_credentials=OAuthACGCredentials(
        o_auth_client_id='OAuthClientId',
        o_auth_client_secret='OAuthClientSecret',
        o_auth_redirect_uri='OAuthRedirectUri',
        o_auth_scopes=[
            OAuthScopeOAuthACGEnum.READ_SCOPE
        ]
    ),
    o_auth_ropcg_credentials=OAuthROPCGCredentials(
        o_auth_client_id='OAuthClientId',
        o_auth_client_secret='OAuthClientSecret',
        o_auth_username='OAuthUsername',
        o_auth_password='OAuthPassword'
    ),
    o_auth_bearer_token_credentials=OAuthBearerTokenCredentials(
        access_token='AccessToken'
    ),
    environment=Environment.TESTING,
    port='80',
    suites=SuiteCodeEnum.HEARTS
)
```

## Environment-Based Client Initialization

```python
from multiauthsample.multiauthsample_client import MultiauthsampleClient

# Specify the path to your .env file if it’s located outside the project’s root directory.
client = MultiauthsampleClient.from_environment(dotenv_path='/path/to/.env')
```

See the [Environment-Based Client Initialization](../doc/environment-based-client-initialization.md) section for details.

## MultiAuth-Sample Client

The gateway for the SDK. This class acts as a factory for the Controllers and also holds the configuration of the SDK.

## Controllers

| Name | Description |
|  --- | --- |
| authentication | Gets AuthenticationController |
| o_auth_authorization | Gets OAuthAuthorizationController |

