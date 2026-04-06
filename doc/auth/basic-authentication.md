
# Basic Authentication



Documentation for accessing and setting credentials for basicAuth.

## Auth Credentials

| Name | Type | Description | Getter |
|  --- | --- | --- | --- |
| Username | `str` | The username to use with basic authentication | `username` |
| Password | `str` | The password to use with basic authentication | `password` |



**Note:** Auth credentials can be set using `BasicAuthCredentials` object, passed in as named parameter `basic_auth_credentials` in the client initialization.

## Usage Example

### Client Initialization

You must provide credentials in the client as shown in the following code snippet.

```python
from multiauthsample.http.auth.basic_auth import BasicAuthCredentials
from multiauthsample.multiauthsample_client import MultiauthsampleClient

client = MultiauthsampleClient(
    basic_auth_credentials=BasicAuthCredentials(
        username='Username',
        password='Password'
    )
)
```


