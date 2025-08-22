# Build a basic resource server

Every secure MCP implementation starts with a resource server - the component that provides tools and resources to clients. Building this foundation first lets you understand MCP server basics before adding authentication layers. You'll create a standalone FastMCP server with public tools that will later become the protected resource server in your OAuth architecture.

This is the first tutorial in a series:

- [x] 01 - Basic resource server
- [ ] [02 - Authorization server foundation](02-authorization-server-foundation.md)
- [ ] [03 - OAuth flow implementation](03-oauth-flow-implementation.md)
- [ ] [04 - Secure resource server](04-secure-resource-server.md)
- [ ] [05 - Test client](05-test-client.md)
- [ ] [06 - Persistence, scopes, and config](06-persistence-scopes-and-config.md)

## Prerequisites

Before starting this tutorial, you need:

- Python 3.12+
- [uv](https://docs.astral.sh/uv/) package manager
- Familiarity with MCP concepts (tools, resources, prompts)

## Set up your project

Start by creating a new project directory and initializing it with uv:

```bash
mkdir mcp-oauth-tutorial
cd mcp-oauth-tutorial
uv init
```

Install the MCP Python SDK:

```bash
uv add "mcp[cli]"
```

Create the directory structure for your resource server:

```bash
mkdir -p src/resource_server
```

## Create your first MCP tool

Build a simple MCP server with one public tool that generates greetings. This tool will remain public throughout the tutorial series, demonstrating how to mix public and protected resources.

Create `src/resource_server/server.py`:

```python
# Path: src/resource_server/server.py
# TODO
```

This server implements:

- A `greet` tool that accepts a name and optional punctuation
- Basic FastMCP configuration with server name and instructions
- Logging setup for debugging

## Test with MCP Inspector

The MCP Inspector provides an interactive way to test your server without building a client. Run your server with the Inspector:

```bash
uv run mcp dev src/resource_server/server.py
```

The Inspector opens in your browser where you can:

1. View available tools in the Tools tab
2. Call the `greet` tool with different parameters
3. See the formatted responses
4. Check server logs in the terminal

Try these test cases:

- Name: "World", Punctuation: "!" > "Hello, World!"
- Name: "MCP Developer", Punctuation: "." > "Hello, MCP Developer."
- Name: "Friend" (no punctuation) > "Hello, Friend!"

## Add a second tool

Expand your server with a tool that will later require authentication. Add a `server_time` tool that returns the current server time:

```python
# Path: src/resource_server/server.py (updated with server_time tool)
# TODO
```

This new tool:

- Returns structured time information
- Will be protected in tutorial 04
- Demonstrates returning complex JSON responses

## Configure for HTTP transport

MCP servers can use different transport protocols. Prepare your server for HTTP transport, which you'll need for OAuth integration:

```python
# Path: src/resource_server/server.py (updated with HTTP transport support)
# TODO
```

Test with stdio transport (Inspector):

```bash
uv run mcp dev src/resource_server/server.py
```

Run with HTTP transport for client access:

```bash
uv run src/resource_server/server.py
```

The server now:

- Listens on port 8000 by default
- Exposes MCP endpoint at `/mcp`
- Supports both stdio (Inspector) and HTTP transports
- Can be tested with `curl http://localhost:8000/mcp`

## Understand the resource server role

Your resource server is the foundation of the OAuth architecture. It:

- **Provides MCP tools**: The actual functionality clients want to access
- **Defines access requirements**: Which tools need authentication (later)
- **Validates tokens**: Checks with the authorization server (tutorial 04)
- **Returns responses**: Sends tool results to authenticated clients

In the OAuth flow, the resource server:

1. Receives requests from clients
2. Checks if authentication is required
3. Validates bearer tokens when present
4. Executes tools and returns results

## Next steps

You've successfully:

- Created a basic MCP resource server with FastMCP
- Implemented public tools that accept parameters
- Tested your server with MCP Inspector
- Configured HTTP transport for client access
- Understood the resource server's role in OAuth

Next, build the authorization server in [02 - Authorization server foundation](02-authorization-server-foundation.md).
