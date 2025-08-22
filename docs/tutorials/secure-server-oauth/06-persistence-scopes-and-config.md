# Add persistence, scopes, and configuration

Production OAuth deployments require features beyond the basic flow—persistent storage ensures tokens survive restarts, scopes enable fine-grained permissions, and proper configuration management supports different environments. These enhancements transform your tutorial implementation into a foundation for real-world MCP servers with OAuth protection.

This is the sixth tutorial in a series:

- [ ] [01 - Basic resource server](01-basic-resource-server.md)
- [ ] [02 - Authorization server foundation](02-authorization-server-foundation.md)
- [ ] [03 - OAuth flow implementation](03-oauth-flow-implementation.md)
- [ ] [04 - Secure resource server](04-secure-resource-server.md)
- [ ] [05 - Test client](05-test-client.md)
- [x] 06 - Persistence, scopes, and config

## Prerequisites

Before starting this tutorial, you need:

- Completed [tutorial 05](05-test-client.md) with working OAuth system
- All three components (AS, RS, client) running successfully
- Python 3.12+ and [uv](https://docs.astral.sh/uv/) installed
- Basic understanding of databases and configuration management

## What this tutorial would cover

This tutorial would extend your OAuth implementation with production-ready features:

### Persistent storage

Replace in-memory storage with database persistence:

- **SQLite for development**: Simple file-based storage
- **PostgreSQL for production**: Scalable relational database
- **Token storage**: Access tokens, refresh tokens, authorization codes
- **Client storage**: Registered clients and their credentials
- **User storage**: User accounts and authentication data
- **Session management**: Active sessions and their states

### OAuth scopes

Implement fine-grained permissions:

- **Scope definition**: Define available scopes (e.g., `read:tools`, `write:resources`)
- **Tool-level scopes**: Require specific scopes for different tools
- **Scope negotiation**: Clients request scopes, users approve subsets
- **Token scopes**: Include granted scopes in tokens
- **Scope validation**: RS checks token scopes match tool requirements
- **Scope inheritance**: Hierarchical scopes for complex permissions

### Configuration management

Support multiple environments:

- **Environment variables**: Override settings per deployment
- **Configuration files**: YAML/TOML for complex settings
- **Secret management**: Secure storage for sensitive values
- **Feature flags**: Enable/disable features dynamically
- **Multi-environment**: Development, staging, production configs
- **Hot reloading**: Update configuration without restarts

### Refresh tokens

Enable long-lived sessions:

- **Refresh token issuance**: Return refresh tokens with access tokens
- **Token rotation**: Issue new refresh tokens on use
- **Refresh flow**: Exchange refresh token for new access token
- **Revocation**: Invalidate refresh tokens and their descendants
- **Security considerations**: Refresh token binding and replay protection

### Enhanced security

Additional security measures:

- **Rate limiting**: Prevent brute force attacks
- **Token binding**: Bind tokens to client certificates
- **Introspection authentication**: Require credentials for introspection
- **Consent tracking**: Remember user consent decisions
- **Audit logging**: Track all authentication events
- **Anomaly detection**: Identify suspicious patterns

### Monitoring and observability

Track system health:

- **Metrics collection**: Token issuance, validation, errors
- **Distributed tracing**: Track requests across services
- **Health checks**: Liveness and readiness endpoints
- **Performance monitoring**: Response times, throughput
- **Alert configuration**: Notify on critical events

## Implementation approach

The implementation would follow these principles:

### Database schema

```sql
-- Example tables for persistence
CREATE TABLE clients (
    id UUID PRIMARY KEY,
    client_id VARCHAR(255) UNIQUE,
    client_secret VARCHAR(255),
    metadata JSONB,
    created_at TIMESTAMP
);

CREATE TABLE tokens (
    id UUID PRIMARY KEY,
    token VARCHAR(255) UNIQUE,
    client_id UUID REFERENCES clients(id),
    user_id VARCHAR(255),
    scopes TEXT[],
    expires_at TIMESTAMP
);
```

### Scope configuration

```python
# Example scope definitions
SCOPES = {
    "read:tools": "Read access to MCP tools",
    "write:resources": "Write access to resources",
    "admin": "Full administrative access"
}

TOOL_SCOPES = {
    "server_time": ["read:tools"],
    "update_config": ["admin"],
}
```

### Environment configuration

```yaml
# config.yaml example
development:
  database_url: "sqlite:///dev.db"
  token_lifetime: 3600
  enable_introspection: true

production:
  database_url: "${DATABASE_URL}"
  token_lifetime: 900
  enable_introspection: false
```

## Benefits of these enhancements

Adding these features provides:

- **Reliability**: Survive restarts and crashes
- **Scalability**: Support multiple instances
- **Security**: Fine-grained access control
- **Flexibility**: Adapt to different environments
- **Observability**: Understand system behavior
- **Compliance**: Meet regulatory requirements

## Next steps

With these enhancements, your OAuth implementation would be ready for:

- Production deployment with real users
- Integration with existing identity providers
- Compliance with enterprise security requirements
- Scaling to handle thousands of clients
- Extension with custom authentication methods

The complete tutorial series has taken you from a basic MCP server to a secure, OAuth-protected system ready for real-world deployment.
