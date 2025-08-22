# Create an authorization server foundation

OAuth 2.1 requires a separate authorization server to handle authentication and token management independently from your resource server. Building this separation from the start ensures clean service boundaries and follows OAuth best practices. You'll create a standalone authorization server with a two-layer architecture that clearly separates web routing from OAuth logic.

This is the second tutorial in a series:

- [ ] [01 - Basic resource server](01-basic-resource-server.md)
- [x] 02 - Authorization server foundation
- [ ] [03 - OAuth flow implementation](03-oauth-flow-implementation.md)
- [ ] [04 - Secure resource server](04-secure-resource-server.md)
- [ ] [05 - Test client](05-test-client.md)
- [ ] [06 - Persistence, scopes, and config](06-persistence-scopes-and-config.md)

## Prerequisites

Before starting this tutorial, you need:

- Completed [tutorial 01](01-basic-resource-server.md) with a working resource server
- Python 3.12+ and [uv](https://docs.astral.sh/uv/) installed
- Basic understanding of HTTP servers and REST APIs
- Familiarity with OAuth concepts (authorization, tokens, clients)

## Understand the authorization server role

The authorization server (AS) is the security gateway in OAuth 2.1. It:

- **Authenticates users**: Verifies user credentials through login forms
- **Issues tokens**: Creates access tokens after successful authentication
- **Validates tokens**: Provides introspection endpoints for resource servers
- **Manages clients**: Handles dynamic client registration and credentials
- **Exposes metadata**: Publishes OAuth configuration for automatic discovery

The AS communicates with:

- **Clients**: Applications requesting access on behalf of users
- **Resource servers**: Services that need to verify token validity
- **Users**: People authenticating to grant access to their resources

## Set up the authorization server structure

Create a clean two-layer architecture that separates concerns:

```bash
mkdir -p src/authorization_server
```

The two-layer structure:

- **Web layer** (`server.py`): HTTP routes, request handling, server setup
- **Logic layer** (`auth_provider.py`): OAuth flows, token management, validation

This separation:

- Makes the code easier to test and maintain
- Clearly defines service boundaries
- Allows swapping implementations without changing routes
- Follows MCP SDK patterns for auth providers

## Create the logic layer stub

Start with a minimal auth provider that implements the required interface. This stub will be expanded in tutorial 03.

Create `src/authorization_server/auth_provider.py`:

```python
# Path: src/authorization_server/auth_provider.py
# TODO
```

This stub provider:

- Implements the [`AuthenticationProvider`][mcp.server.auth.provider.AuthenticationProvider] interface from MCP SDK
- Returns placeholder metadata for OAuth discovery
- Will be expanded with real authentication in tutorial 03
- Demonstrates the provider pattern for OAuth logic

## Build the web layer

Create the HTTP server that exposes OAuth endpoints using the provider.

Create `src/authorization_server/server.py`:

```python
# Path: src/authorization_server/server.py
# TODO
```

The web layer:

- Uses Starlette for HTTP routing (lightweight ASGI framework)
- Leverages MCP SDK's `create_auth_routes` for OAuth endpoints
- Exposes metadata at `/.well-known/oauth-authorization-server`
- Enables dynamic client registration for the tutorial
- Keeps all HTTP concerns separate from OAuth logic

## Add server configuration

Create a settings class to manage configuration:

```python
# Path: src/authorization_server/server.py (updated with settings)
# TODO
```

Configuration includes:

- Server port (9000 to avoid conflicts with resource server)
- Issuer URL (the AS's public identifier)
- Demo credentials (for tutorial simplicity)
- Token lifetime (how long tokens remain valid)

## Test the metadata endpoint

Run your authorization server and verify it exposes OAuth metadata:

```bash
uv run src/authorization_server/server.py
```

Test the discovery endpoint:

```bash
curl http://localhost:9000/.well-known/oauth-authorization-server | python -m json.tool
```

You should see OAuth metadata including:

- `issuer`: The AS identifier
- `authorization_endpoint`: Where clients send users to authenticate
- `token_endpoint`: Where clients exchange codes for tokens
- `registration_endpoint`: Dynamic client registration (if enabled)
- Supported flows and features

## Understand the service boundary

The authorization server is completely separate from your resource server:

**Physical separation**:

- Different processes (ports 9000 vs 8000)
- Different codebases (no shared imports)
- Communication only via HTTP

**Logical separation**:

- AS handles authentication and tokens
- RS provides MCP tools and resources
- AS doesn't know about MCP tools
- RS doesn't know how authentication works

This separation is important for:

- Security (compromise of one doesn't affect the other)
- Scalability (can run on different machines)
- Flexibility (can swap AS implementations)
- Standards compliance (OAuth 2.1 architecture)

## Verify the foundation

Your authorization server foundation should:

1. Run independently on port 9000
2. Expose OAuth metadata at the well-known URL
3. Have a clean two-layer structure
4. Use the MCP SDK's auth provider pattern
5. Be ready for OAuth flow implementation

Test checklist:

- [ ] Server starts without errors
- [ ] Metadata endpoint returns valid JSON
- [ ] No import dependencies on resource server
- [ ] Clean separation between web and logic layers

## Next steps

You've successfully:

- Created a standalone authorization server with clean architecture
- Implemented the provider pattern for OAuth logic
- Exposed OAuth 2.1 discovery metadata
- Established service boundaries between AS and RS
- Prepared the foundation for authentication flows

Next, implement the complete OAuth flow in [03 - OAuth flow implementation](03-oauth-flow-implementation.md).