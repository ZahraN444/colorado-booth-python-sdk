
# Environment-Based Client Initialization

The SDK client can also be initialized directly from environment variables using the `from_environment()` class method. This allows the SDK to automatically read configuration values from the runtime environment or a .env file.

## Example

```python
from multiauthsample.multiauthsample_client import MultiauthsampleClient

# Specify the path to your .env file if it’s located outside the project’s root directory.
client = MultiauthsampleClient.from_environment(dotenv_path='/path/to/.env')
```

You can also specify a path to a `.env` file by passing it to the `from_environment()` method:

The same method can accept keyword arguments to override any values read from the environment, and the arguments to override values should follow the same approach as code-based client initialization.

```python
from multiauthsample.multiauthsample_client import MultiauthsampleClient

client = MultiauthsampleClient.from_environment(
    dotenv_path='/path/to/.env',
    timeout=0,  # overrides timeout from environment variable
)
```

Values provided through arguments take precedence over those defined in environment variables.

## Example `.env` File

```python
PORT=80
SUITES=1
ACCESS_TOKEN=
ENVIRONMENT=testing

BASIC_AUTH_USERNAME=ExampleUsername
BASIC_AUTH_PASSWORD=ExamplePassword
API_KEY_TOKEN=
API_KEY_API_KEY=
API_HEADER_TOKEN=
API_HEADER_API_KEY=
O_AUTH_CCG_O_AUTH_CLIENT_ID=ExampleOAuthClientId
O_AUTH_CCG_O_AUTH_CLIENT_SECRET=ExampleOAuthClientSecret
O_AUTH_CCG_O_AUTH_CLOCK_SKEW=5
O_AUTH_ACG_O_AUTH_CLIENT_ID=ExampleOAuthClientId
O_AUTH_ACG_O_AUTH_CLIENT_SECRET=ExampleOAuthClientSecret
O_AUTH_ACG_O_AUTH_REDIRECT_URI=ExampleOAuthRedirectUri
O_AUTH_ACG_O_AUTH_SCOPES=ExampleOAuthScopes
O_AUTH_ROPCG_O_AUTH_CLIENT_ID=ExampleOAuthClientId
O_AUTH_ROPCG_O_AUTH_CLIENT_SECRET=ExampleOAuthClientSecret
O_AUTH_ROPCG_O_AUTH_USERNAME=ExampleOAuthUsername
O_AUTH_ROPCG_O_AUTH_PASSWORD=ExampleOAuthPassword
O_AUTH_BEARER_TOKEN_ACCESS_TOKEN=ExampleAccessToken

TIMEOUT=60
MAX_RETRIES=3
BACKOFF_FACTOR=2
RETRY_STATUSES=408,413
RETRY_METHODS=GET,PUT,DELETE

# Proxy Configuration
PROXY_ADDRESS=http://localhost:3000
PROXY_PORT=8080
PROXY_USERNAME=username
PROXY_PASSWORD=password
```

## Note

- If an environment variable is not defined, the default SDK configuration value will be used.

