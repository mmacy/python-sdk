# Implement the OAuth flow

Authentication requires more than just endpointsyou need the complete OAuth 2.1 Authorization Code flow with PKCE to securely authenticate users and issue tokens. Building on your authorization server foundation, you'll implement token generation, user authentication, and the full authorization flow that connects clients with protected resources.

This is the third tutorial in a series:

- [ ] [01 - Basic resource server](01-basic-resource-server.md)
- [ ] [02 - Authorization server foundation](02-authorization-server-foundation.md)
- [x] 03 - OAuth flow implementation
- [ ] [04 - Secure resource server](04-secure-resource-server.md)
- [ ] [05 - Test client](05-test-client.md)
- [ ] [06 - Persistence, scopes, and config](06-persistence-scopes-and-config.md)

## Prerequisites

Before starting this tutorial, you need:

- Completed [tutorial 02](02-authorization-server-foundation.md) with AS foundation
- Python 3.12+ and [uv](https://docs.astral.sh/uv/) installed
- Understanding of OAuth 2.1 Authorization Code flow
- Basic knowledge of PKCE (Proof Key for Code Exchange)

## Understand the Authorization Code flow with PKCE

The Authorization Code flow with PKCE is the recommended OAuth 2.1 flow for all clients:

1. **Client prepares PKCE**: Generates code verifier and challenge
2. **Authorization request**: Client sends user to AS with challenge
3. **User authenticates**: AS verifies credentials via login form
4. **Authorization code**: AS returns code to client redirect URI
5. **Token exchange**: Client exchanges code + verifier for tokens
6. **Token validation**: AS verifies PKCE and issues access token

PKCE prevents authorization code interception attacks by:

- Requiring a dynamically generated secret (verifier)
- Sending only the hash (challenge) in the authorization request
- Validating the verifier during token exchange

## Implement the authentication provider

Expand your stub provider with full OAuth logic including PKCE validation, token generation, and user authentication.

Update `src/authorization_server/auth_provider.py`:

```python
# Path: src/authorization_server/auth_provider.py (complete implementation)
# TODO
```

The provider implements:

- [`OAuthAuthorizationServerProvider`][mcp.server.auth.provider.OAuthAuthorizationServerProvider] protocol
- In-memory storage for clients, codes, and tokens
- PKCE challenge storage and validation
- Demo user authentication (username/password)
- Token generation with configurable lifetime

## Add authorization endpoint logic

The authorization endpoint starts the OAuth flow by validating the client request and redirecting to login.

Key implementation in `auth_provider.py`:

```python
# Path: src/authorization_server/auth_provider.py (authorize method)
# TODO
```

This method:

- Validates client_id and redirect_uri
- Stores PKCE code_challenge for later validation
- Generates state parameter for CSRF protection
- Redirects to login page with state

## Create the login UI

Add a simple HTML login form for user authentication.

Update `src/authorization_server/server.py` with login routes:

```python
# Path: src/authorization_server/server.py (login routes)
# TODO
```

The login flow:

- Displays username/password form
- Validates credentials against demo users
- Issues authorization code on success
- Redirects back to client with code
- Maintains state throughout the flow

## Implement token exchange

Complete the token endpoint to exchange authorization codes for access tokens with PKCE validation.

Key implementation in `auth_provider.py`:

```python
# Path: src/authorization_server/auth_provider.py (exchange_authorization_code method)
# TODO
```

Token exchange validates:

- Authorization code exists and hasn't expired
- Client credentials match
- PKCE verifier matches stored challenge
- Redirect URI matches original request

On success, it returns:

- Access token with expiration
- Token type ("Bearer")
- Expires_in value in seconds
- Optional scope information

## Add token introspection

Implement token introspection so the resource server can validate tokens.

```python
# Path: src/authorization_server/auth_provider.py (introspect_token method)
# TODO
```

Introspection returns:

- `active`: Whether token is valid
- `client_id`: Which client owns the token
- `exp`: Expiration timestamp
- `scope`: Granted scopes (if any)

## Test the complete flow

Test your OAuth implementation manually using a browser and curl.

### Start the authorization server

```bash
uv run src/authorization_server/server.py
```

### Simulate client registration

```bash
curl -X POST http://localhost:9000/register \
  -H "Content-Type: application/json" \
  -d '{
    "client_name": "Test Client",
    "redirect_uris": ["http://localhost:8080/callback"]
  }'
```

Save the returned `client_id` and `client_secret`.

### Start the authorization flow

Generate PKCE values:

```bash
# Generate code verifier (43-128 characters)
CODE_VERIFIER=$(openssl rand -base64 32 | tr -d "=+/" | cut -c -43)
echo "Verifier: $CODE_VERIFIER"

# Generate code challenge (SHA256 hash, base64url encoded)
CODE_CHALLENGE=$(echo -n $CODE_VERIFIER | openssl dgst -sha256 -binary | base64 | tr -d "=+/" | tr '/+' '_-')
echo "Challenge: $CODE_CHALLENGE"
```

Open in browser with your client_id:

```
http://localhost:9000/authorize?client_id=YOUR_CLIENT_ID&redirect_uri=http://localhost:8080/callback&response_type=code&code_challenge=YOUR_CHALLENGE&code_challenge_method=S256
```

### Complete authentication

1. Enter demo credentials (demo_user/demo_password)
2. Copy the authorization code from redirect URL
3. Exchange code for token:

```bash
curl -X POST http://localhost:9000/token \
  -u "YOUR_CLIENT_ID:YOUR_CLIENT_SECRET" \
  -d "grant_type=authorization_code" \
  -d "code=YOUR_AUTH_CODE" \
  -d "redirect_uri=http://localhost:8080/callback" \
  -d "code_verifier=YOUR_VERIFIER"
```

You should receive an access token response:

```json
{
  "access_token": "at_...",
  "token_type": "Bearer",
  "expires_in": 3600
}
```

### Verify token introspection

```bash
curl -X POST http://localhost:9000/introspect \
  -d "token=YOUR_ACCESS_TOKEN"
```

## Understand security considerations

Your implementation includes important security features:

**PKCE protection**:

- Prevents authorization code interception
- Required for all OAuth 2.1 clients
- Validates code_verifier against stored challenge

**State parameter**:

- Prevents CSRF attacks
- Links authorization request to login
- Must be unpredictable and unique

**Token expiration**:

- Access tokens expire after configured lifetime
- Expired tokens fail introspection
- Clients must handle token refresh (future enhancement)

**Demo limitations**:

- In-memory storage (not persistent)
- Fixed demo credentials (not production-ready)
- No rate limiting or brute force protection
- Missing refresh tokens and revocation

## Next steps

You've successfully:

- Implemented the complete OAuth 2.1 Authorization Code flow with PKCE
- Added user authentication with login UI
- Created token generation and introspection
- Tested the full authentication flow
- Understood security considerations

Next, protect your resource server in [04 - Secure resource server](04-secure-resource-server.md).