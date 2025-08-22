# Build a test client

Testing OAuth integration requires a client that orchestrates the complete three-party flow between itself, the resource server, and the authorization server. Building this client demonstrates how MCP's OAuth support handles authentication automatically while you focus on calling tools. You'll create a client that performs dynamic registration, handles the authorization flow, and accesses both public and protected tools.

This is the fifth tutorial in a series:

- [ ] [01 - Basic resource server](01-basic-resource-server.md)
- [ ] [02 - Authorization server foundation](02-authorization-server-foundation.md)
- [ ] [03 - OAuth flow implementation](03-oauth-flow-implementation.md)
- [ ] [04 - Secure resource server](04-secure-resource-server.md)
- [x] 05 - Test client
- [ ] [06 - Persistence, scopes, and config](06-persistence-scopes-and-config.md)

## Prerequisites

Before starting this tutorial, you need:

- Completed [tutorial 04](04-secure-resource-server.md) with secured RS
- Working authorization server from tutorial 03
- Python 3.12+ and [uv](https://docs.astral.sh/uv/) installed
- Understanding of OAuth client concepts

## Understand the three-party flow

The client orchestrates interactions between all three parties:

1. **Client → RS**: Attempts to access protected resource
2. **RS → Client**: Returns 401 with OAuth metadata
3. **Client → AS**: Discovers metadata and registers dynamically
4. **Client → AS**: Initiates authorization with PKCE
5. **User → AS**: Authenticates via browser
6. **AS → Client**: Returns authorization code
7. **Client → AS**: Exchanges code for access token
8. **Client → RS**: Retries with bearer token
9. **RS → AS**: Validates token via introspection
10. **RS → Client**: Returns protected resource

The MCP SDK handles most of this automatically through [`OAuthClientProvider`][mcp.client.auth.OAuthClientProvider].

## Set up token storage

Create simple in-memory storage for tokens and client credentials.

Create `src/client.py`:

```python
# Path: src/client.py (token storage)
# TODO
```

Token storage manages:

- Access tokens received from AS
- Client credentials from dynamic registration
- Token refresh (future enhancement)
- Persistence (could be file/database in production)

## Implement OAuth redirect handling

Handle the OAuth callback that receives the authorization code.

Add to `src/client.py`:

```python
# Path: src/client.py (redirect handlers)
# TODO
```

Redirect handling includes:

- Browser opener: Launches user's browser for login
- Callback receiver: Local HTTP server for redirect
- State validation: Ensures callback matches request
- Code extraction: Pulls authorization code from URL

## Create the OAuth client provider

Configure the OAuth provider that manages the authentication flow.

Add to `src/client.py`:

```python
# Path: src/client.py (OAuth provider setup)
# TODO
```

[`OAuthClientProvider`][mcp.client.auth.OAuthClientProvider] configuration:

- Server URL: Resource server's MCP endpoint
- Client metadata: Redirect URI and auth method
- Storage: Token and client credential persistence
- Handlers: Browser and callback functions
- Timeout: Maximum time for auth flow

## Connect to the resource server

Use streamable HTTP transport with OAuth authentication.

Add to `src/client.py`:

```python
# Path: src/client.py (main client logic)
# TODO
```

Client connection:

- [`streamablehttp_client`][mcp.client.streamable_http.streamablehttp_client]: HTTP transport for MCP
- OAuth auth: Automatically handles authentication
- [`ClientSession`][mcp.client.session.ClientSession]: MCP protocol session
- Tool discovery: Lists available tools
- Tool invocation: Calls public and protected tools

## Test the complete flow

Run all three components and watch the OAuth flow in action.

### Start all servers

Terminal 1 - Authorization Server:

```bash
uv run src/authorization_server/server.py
```

Terminal 2 - Resource Server:

```bash
uv run src/resource_server/server.py
```

### Run the client

Terminal 3 - Client:

```bash
uv run src/client.py
```

Expected flow:

1. Client connects to RS
2. Browser opens for authentication
3. Enter demo credentials (demo_user/demo_password)
4. Browser shows "Authentication complete"
5. Client receives token
6. Client calls public tool (greet)
7. Client calls protected tool (server_time)
8. Both tools return results

### Monitor the flow

Watch server logs to see:

**Authorization Server**:

- Dynamic client registration
- Authorization request with PKCE
- Login form serving
- Code issuance
- Token exchange
- Introspection requests

**Resource Server**:

- Public tool calls (no auth)
- Protected tool rejection (401)
- Token validation via AS
- Successful protected calls

## Handle authentication errors

The client should gracefully handle various error conditions:

```python
# Path: src/client.py (error handling example)
# TODO
```

Common errors:

- **Network issues**: AS or RS unavailable
- **Invalid credentials**: Wrong username/password
- **Token expiration**: Access token timeout
- **Registration failure**: Client metadata rejection
- **PKCE mismatch**: Verifier doesn't match challenge

## Understand the SDK automation

The MCP SDK's [`OAuthClientProvider`][mcp.client.auth.OAuthClientProvider] automates:

**Discovery**:

- Fetches RS metadata from `/.well-known/oauth-protected-resource`
- Discovers AS metadata from `/.well-known/oauth-authorization-server`
- Extracts endpoints and capabilities

**Registration**:

- Dynamically registers client if needed
- Stores client credentials securely
- Reuses existing registration when available

**Authentication**:

- Generates PKCE verifier and challenge
- Constructs authorization URL with parameters
- Handles redirect and code extraction
- Exchanges code for tokens

**Token management**:

- Adds bearer token to requests
- Handles 401 responses automatically
- Could support refresh (future enhancement)

## Customize client behavior

You can customize various aspects of the OAuth flow:

**Client metadata**:

```python
# Path: src/client.py (custom metadata example)
# TODO
```

Options include:

- `client_name`: Display name for consent
- `client_uri`: Client information URL
- `logo_uri`: Client logo for AS UI
- `scope`: Requested permissions
- `token_endpoint_auth_method`: Authentication method

**Redirect handling**:

- Custom redirect URI (must match registration)
- Alternative to browser (e.g., display URL)
- Custom callback server implementation

**Storage backends**:

- File-based for persistence
- Database for multi-user
- Encrypted for security

## Next steps

You've successfully:

- Built a complete OAuth client for MCP
- Implemented the three-party authentication flow
- Handled dynamic registration and PKCE
- Accessed both public and protected tools
- Understood the SDK's OAuth automation

Next, enhance with persistence and scopes in [06 - Persistence, scopes, and config](06-persistence-scopes-and-config.md).