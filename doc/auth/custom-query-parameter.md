
# Custom Query Parameter



Documentation for accessing and setting credentials for apiKey.

## Auth Credentials

| Name | Type | Description | Getter |
|  --- | --- | --- | --- |
| token | `str` | - | `token` |
| api-key | `str` | - | `api_key` |



**Note:** Auth credentials can be set using `ApiKeyCredentials` object, passed in as named parameter `api_key_credentials` in the client initialization.

## Usage Example

### Client Initialization

You must provide credentials in the client as shown in the following code snippet.

```python
from multiauthsample.http.auth.api_key import ApiKeyCredentials
from multiauthsample.multiauthsample_client import MultiauthsampleClient

client = MultiauthsampleClient(
    api_key_credentials=ApiKeyCredentials(
        token='token',
        api_key='api-key'
    )
)
```


