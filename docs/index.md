# MCP Python SDK

A Python implementation of the Model Context Protocol (MCP) that enables applications to provide context for LLMs in a standardized way.

## Examples

The [Examples](examples-quickstart.md) section provides working code examples covering many aspects of MCP development. Each code example in these documents corresponds to an example .py file in the examples/ directory in the repository.

- [Getting started](examples-quickstart.md): Quick introduction to FastMCP and basic server patterns
- [Server development](examples-server-tools.md): Tools, resources, prompts, and structured output examples
- [Transport protocols](examples-transport-http.md): HTTP and streamable transport implementations
- [Low-level servers](examples-lowlevel-servers.md): Advanced patterns using the low-level server API
- [Authentication](examples-authentication.md): OAuth 2.1 server and client implementations
- [Client development](examples-clients.md): Complete client examples with various connection types

## API Reference

Complete API documentation is auto-generated from the source code and available in the [API Reference](reference/mcp/index.md) section.

## Code example index

### Servers

| File | Transport | Resources | Prompts | Tools | Completions | Sampling | Elicitation | Progress | Logging | Authentication | Configuration |
|---|---|---|---|---|---|---|---|---|---|---|---|
| fastmcp/complex_inputs.py | stdio | — | — | ✅ | — | — | — | — | — | — | — |
| fastmcp/desktop.py | stdio | ✅ | — | ✅ | — | — | — | — | — | — | — |
| fastmcp/echo.py | stdio | ✅ | ✅ | ✅ | — | — | — | — | — | — | — |
| fastmcp/memory.py | stdio | — | — | ✅ | — | — | — | — | — | — | ✅ |
| fastmcp/parameter_descriptions.py | stdio | — | — | ✅ | — | — | — | — | — | — | — |
| fastmcp/readme-quickstart.py | stdio | ✅ | — | ✅ | — | — | — | — | — | — | — |
| fastmcp/screenshot.py | stdio | — | — | ✅ | — | — | — | — | — | — | — |
| fastmcp/simple_echo.py | stdio | — | — | ✅ | — | — | — | — | — | — | — |
| fastmcp/text_me.py | stdio | — | — | ✅ | — | — | — | — | — | — | ✅ |
| fastmcp/unicode_example.py | stdio | — | — | ✅ | — | — | — | — | — | — | — |
| fastmcp/weather_structured.py | stdio | — | — | ✅ | — | — | — | — | — | — | — |
| servers/simple-auth/mcp_simple_auth/auth_server.py | stdio | — | — | — | — | — | — | — | — | ✅ | — |
| servers/simple-auth/mcp_simple_auth/legacy_as_server.py | streamable-http | — | — | ✅ | — | — | — | — | — | ✅ | ✅ |
| servers/simple-auth/mcp_simple_auth/server.py | streamable-http | — | — | ✅ | — | — | — | — | — | ✅ | ✅ |
| servers/simple-prompt/mcp_simple_prompt/server.py | stdio | — | ✅ | — | — | — | — | — | — | — | — |
| servers/simple-resource/mcp_simple_resource/server.py | stdio | ✅ | — | — | — | — | — | — | — | — | — |
| servers/simple-streamablehttp-stateless/mcp_simple_streamablehttp_stateless/server.py | streamable-http | — | — | ✅ | — | — | — | — | ✅ | — | ✅ |
| servers/simple-streamablehttp/mcp_simple_streamablehttp/server.py | streamable-http | — | — | ✅ | — | — | — | — | ✅ | — | ✅ |
| servers/simple-tool/mcp_simple_tool/server.py | stdio | — | — | ✅ | — | — | — | — | — | — | — |
| servers/structured_output_lowlevel.py | stdio | — | — | ✅ | — | — | — | — | — | — | — |
| snippets/servers/basic_prompt.py | stdio | — | ✅ | — | — | — | — | — | — | — | — |
| snippets/servers/basic_resource.py | stdio | ✅ | — | — | — | — | — | — | — | — | — |
| snippets/servers/basic_tool.py | stdio | — | — | ✅ | — | — | — | — | — | — | — |
| snippets/servers/completion.py | stdio | ✅ | ✅ | — | ✅ | — | — | — | — | — | — |
| snippets/servers/direct_execution.py | stdio | — | — | ✅ | — | — | — | — | — | — | — |
| snippets/servers/elicitation.py | stdio | — | — | ✅ | — | — | ✅ | — | — | — | — |
| snippets/servers/fastmcp_quickstart.py | stdio | ✅ | ✅ | ✅ | — | — | — | — | — | — | — |
| snippets/servers/images.py | stdio | — | — | ✅ | — | — | — | — | — | — | — |
| snippets/servers/lifespan_example.py | stdio | — | — | ✅ | — | — | — | — | — | — | ✅ |
| snippets/servers/lowlevel/basic.py | stdio | — | ✅ | — | — | — | — | — | — | — | — |
| snippets/servers/lowlevel/lifespan.py | stdio | — | — | ✅ | — | — | — | — | — | — | ✅ |
| snippets/servers/lowlevel/structured_output.py | stdio | — | — | ✅ | — | — | — | — | — | — | — |
| snippets/servers/notifications.py | stdio | — | — | ✅ | — | — | — | — | ✅ | — | — |
| snippets/servers/oauth_server.py | streamable-http | — | — | ✅ | — | — | — | — | — | ✅ | — |
| snippets/servers/sampling.py | stdio | — | — | ✅ | — | ✅ | — | — | — | — | — |
| snippets/servers/streamable_config.py | streamable-http | — | — | ✅ | — | — | — | — | — | — | ✅ |
| snippets/servers/streamable_starlette_mount.py | streamable-http | — | — | ✅ | — | — | — | — | — | — | ✅ |
| snippets/servers/structured_output.py | stdio | — | — | ✅ | — | — | — | — | — | — | — |
| snippets/servers/tool_progress.py | stdio | — | — | ✅ | — | — | — | ✅ | ✅ | — | — |

### Clients

| File | Transport | Resources | Prompts | Tools | Completions | Sampling | Authentication |
|---|---|---|---|---|---|---|---|
| clients/simple-auth-client/mcp_simple_auth_client/main.py | streamable-http | — | — | ✅ | — | — | ✅ |
| clients/simple-chatbot/mcp_simple_chatbot/main.py | stdio | — | — | ✅ | — | — | — |
| snippets/clients/completion_client.py | stdio | ✅ | ✅ | — | ✅ | — | — |
| snippets/clients/display_utilities.py | stdio | ✅ | — | ✅ | — | — | — |
| snippets/clients/oauth_client.py | streamable-http | ✅ | — | ✅ | — | — | ✅ |
| snippets/clients/parsing_tool_results.py | stdio | — | — | ✅ | — | — | — |
| snippets/clients/stdio_client.py | stdio | ✅ | ✅ | ✅ | — | ✅ | — |
| snippets/clients/streamable_basic.py | streamable-http | — | — | ✅ | — | — | — |

Notes:

- **Resources** for clients indicates the example uses the Resources API (reading resources or listing resource templates).
- **Completions** refers to the completion/complete API for argument autocompletion.
- **Sampling** indicates the example exercises the sampling/createMessage flow (server-initiated in server examples; client-provided callback in stdio_client).
- **Authentication** indicates OAuth support is implemented in the example.
- Em dash (—) indicates **not demonstrated** in the example.
