Implements a complete OAuth 2.1-secured MCP server tutorial reference implementation with two-service architecture.

### What's added

- Resource Server (RS): FastMCP-based MCP server with protected tools and token verification
- Authorization Server (AS): Separate OAuth 2.1 server with PKCE support and token management
- Client implementation: Example demonstrating the complete three-party OAuth flow
- Progressive tutorial structure: Code evolves from basic MCP server to fully secured multi-service system

### Key features

- Physical separation of concerns with RS (port 8000) and AS (port 9000) as independent services
- PKCE-based OAuth 2.1 implementation for secure authorization
- Token introspection for service-to-service validation
- Mixed access patterns (public and protected tools)
- Comprehensive documentation and tutorial progression

This reference implementation serves as the complete "answer key" for the MCP authentication tutorial series, demonstrating best practices for securing MCP servers with OAuth 2.1.