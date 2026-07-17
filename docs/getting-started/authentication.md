# Authentication

Each organization accesses Pulse through its own API instance.

The base URL is specific to the organization and is provided during setup. It may follow a pattern similar to:

```text
https://<organization>.tompit.pulse.com
```
> [!IMPORTANT]
> The exact hostname can differ. Do not hard-code the example above. Use the base URL assigned to your organization.

All Pulse service paths are relative to that base URL. For example:

```text
https://<organization-base-url>/services/pulse/types/plants
```

## API token

Access to the Pulse API requires an API token.

Authorized users can generate a new token in their Pulse instance. The token is displayed only once when it is created.

Copy and store it securely before leaving the page. Pulse does not display the same token again.

If a token is lost, generate a new one and replace the old token in the integration configuration.

## Store the token securely

Treat the token as a secret.

- Store it in a secret manager or protected environment variable.
- Do not commit it to source control.
- Do not include it in application logs.
- Do not expose it in client-side code.
- Do not share it through email, chat, or documentation.
- Use separate tokens for separate environments or integrations when supported.

## Send the token

Include the API token in the `Authorization` header with every request:

```http
Authorization: Bearer <your-api-token>
```

Do not place the token in the request URL.

## Configuration example

Keep the base URL and token outside the application code.

```text
PULSE_BASE_URL=https://<organization-base-url>
PULSE_API_TOKEN=<your-api-token>
```

The application can then build service URLs from the configured base URL:

```text
{PULSE_BASE_URL}/services/pulse/types/plants
```

## Token rotation

When replacing a token:

1. Generate the new token.
2. Copy and store it securely.
3. Update the integration configuration.
4. Verify that API requests succeed with the new token.
5. Revoke or remove the previous token when supported.

See [First API request](first-api-request.md) for a simple connection test using the Plant service.