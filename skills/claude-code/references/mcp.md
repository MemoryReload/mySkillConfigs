# Claude-Code - Mcp

**Pages:** 6

---

## Remote MCP servers

**URL:** llms-txt#remote-mcp-servers

**Contents:**
- Connecting to remote MCP servers
- Remote MCP server examples
  - Claude on 3rd-party platforms

Several companies have deployed remote MCP servers that developers can connect to via the Anthropic MCP connector API. These servers expand the capabilities available to developers and end users by providing remote access to various services and tools through the MCP protocol.

<Note>
    The remote MCP servers listed below are third-party services designed to work with the Claude API. These servers
    are not owned, operated, or endorsed by Anthropic. Users should only connect to remote MCP servers they trust and
    should review each server's security practices and terms before connecting.
</Note>

## Connecting to remote MCP servers

To connect to a remote MCP server:

1. Review the documentation for the specific server you want to use.
2. Ensure you have the necessary authentication credentials.
3. Follow the server-specific connection instructions provided by each company.

For more information about using remote MCP servers with the Claude API, see the [MCP connector docs](/docs/en/agents-and-tools/mcp-connector).

## Remote MCP server examples

<MCPServersTable platform="mcpConnector" />

<Note>
**Looking for more?** [Find hundreds more MCP servers on GitHub](https://github.com/modelcontextprotocol/servers).
</Note>

### Claude on 3rd-party platforms

---

## .mcp.json with environment variables

**URL:** llms-txt#.mcp.json-with-environment-variables

{
  "mcpServers": {
    "secure-api": {
      "type": "sse",
      "url": "https://api.example.com/mcp",
      "headers": {
        "Authorization": "Bearer ${API_TOKEN}",
        "X-API-Key": "${API_KEY:-default-key}"
      }
    }
  }
}

---

## .mcp.json configuration

**URL:** llms-txt#.mcp.json-configuration

**Contents:**
  - HTTP/SSE Servers

{
  "mcpServers": {
    "my-tool": {
      "command": "python",
      "args": ["./my_mcp_server.py"],
      "env": {
        "DEBUG": "${DEBUG:-false}"
      }
    }
  }
}
typescript TypeScript
// SSE server configuration
{
  "mcpServers": {
    "remote-api": {
      "type": "sse",
      "url": "https://api.example.com/mcp/sse",
      "headers": {
        "Authorization": "Bearer ${API_TOKEN}"
      }
    }
  }
}

// HTTP server configuration
{
  "mcpServers": {
    "http-service": {
      "type": "http",
      "url": "https://api.example.com/mcp",
      "headers": {
        "X-API-Key": "${API_KEY}"
      }
    }
  }
}
python Python

**Examples:**

Example 1 (unknown):
```unknown
</CodeGroup>

### HTTP/SSE Servers

Remote servers with network communication:

<CodeGroup>
```

Example 2 (unknown):
```unknown

```

---

## MCP connector

**URL:** llms-txt#mcp-connector

**Contents:**
- Key features
- Limitations
- Using the MCP connector in the Messages API
  - Basic example
- MCP server configuration
  - Field descriptions
- MCP toolset configuration
  - Basic structure
  - Field descriptions
  - Tool configuration options

Claude's Model Context Protocol (MCP) connector feature enables you to connect to remote MCP servers directly from the Messages API without a separate MCP client.

<Note>
  **Current version**: This feature requires the beta header: `"anthropic-beta": "mcp-client-2025-11-20"`

The previous version (`mcp-client-2025-04-04`) is deprecated. See the [deprecated version documentation](#deprecated-version-mcp-client-2025-04-04) below.
</Note>

- **Direct API integration**: Connect to MCP servers without implementing an MCP client
- **Tool calling support**: Access MCP tools through the Messages API
- **Flexible tool configuration**: Enable all tools, allowlist specific tools, or denylist unwanted tools
- **Per-tool configuration**: Configure individual tools with custom settings
- **OAuth authentication**: Support for OAuth Bearer tokens for authenticated servers
- **Multiple servers**: Connect to multiple MCP servers in a single request

- Of the feature set of the [MCP specification](https://modelcontextprotocol.io/introduction#explore-mcp), only [tool calls](https://modelcontextprotocol.io/docs/concepts/tools) are currently supported.
- The server must be publicly exposed through HTTP (supports both Streamable HTTP and SSE transports). Local STDIO servers cannot be connected directly.
- The MCP connector is currently not supported on Amazon Bedrock and Google Vertex.

## Using the MCP connector in the Messages API

The MCP connector uses two components:

1. **MCP Server Definition** (`mcp_servers` array): Defines server connection details (URL, authentication)
2. **MCP Toolset** (`tools` array): Configures which tools to enable and how to configure them

This example enables all tools from an MCP server with default configuration:

## MCP server configuration

Each MCP server in the `mcp_servers` array defines the connection details:

### Field descriptions

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| `type` | string | Yes | Currently only "url" is supported |
| `url` | string | Yes | The URL of the MCP server. Must start with https:// |
| `name` | string | Yes | A unique identifier for this MCP server. Must be referenced by exactly one MCPToolset in the `tools` array. |
| `authorization_token` | string | No | OAuth authorization token if required by the MCP server. See [MCP specification](https://modelcontextprotocol.io/specification/2025-11-25/basic/authorization). |

## MCP toolset configuration

The MCPToolset lives in the `tools` array and configures which tools from the MCP server are enabled and how they should be configured.

### Field descriptions

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| `type` | string | Yes | Must be "mcp_toolset" |
| `mcp_server_name` | string | Yes | Must match a server name defined in the `mcp_servers` array |
| `default_config` | object | No | Default configuration applied to all tools in this set. Individual tool configs in `configs` will override these defaults. |
| `configs` | object | No | Per-tool configuration overrides. Keys are tool names, values are configuration objects. |
| `cache_control` | object | No | Cache breakpoint configuration for this toolset |

### Tool configuration options

Each tool (whether configured in `default_config` or in `configs`) supports the following fields:

| Property | Type | Default | Description |
|----------|------|---------|-------------|
| `enabled` | boolean | `true` | Whether this tool is enabled |
| `defer_loading` | boolean | `false` | If true, tool description is not sent to the model initially. Used with [Tool Search Tool](/docs/en/agents-and-tools/tool-use/tool-search-tool). |

### Configuration merging

Configuration values merge with this precedence (highest to lowest):

1. Tool-specific settings in `configs`
2. Set-level `default_config`
3. System defaults

Results in:
- `search_events`: `enabled: false` (from configs), `defer_loading: true` (from default_config)
- All other tools: `enabled: true` (system default), `defer_loading: true` (from default_config)

## Common configuration patterns

### Enable all tools with default configuration

The simplest pattern - enable all tools from a server:

### Allowlist - Enable only specific tools

Set `enabled: false` as the default, then explicitly enable specific tools:

### Denylist - Disable specific tools

Enable all tools by default, then explicitly disable unwanted tools:

### Mixed - Allowlist with per-tool configuration

Combine allowlisting with custom configuration for each tool:

In this example:
- `search_events` is enabled with `defer_loading: false`
- `list_events` is enabled with `defer_loading: true` (inherited from default_config)
- All other tools are disabled

The API enforces these validation rules:

- **Server must exist**: The `mcp_server_name` in an MCPToolset must match a server defined in the `mcp_servers` array
- **Server must be used**: Every MCP server defined in `mcp_servers` must be referenced by exactly one MCPToolset
- **Unique toolset per server**: Each MCP server can only be referenced by one MCPToolset
- **Unknown tool names**: If a tool name in `configs` doesn't exist on the MCP server, a backend warning is logged but no error is returned (MCP servers may have dynamic tool availability)

## Response content types

When Claude uses MCP tools, the response will include two new content block types:

### MCP Tool Use Block

### MCP Tool Result Block

## Multiple MCP servers

You can connect to multiple MCP servers by including multiple server definitions in `mcp_servers` and a corresponding MCPToolset for each in the `tools` array:

For MCP servers that require OAuth authentication, you'll need to obtain an access token. The MCP connector beta supports passing an `authorization_token` parameter in the MCP server definition.
API consumers are expected to handle the OAuth flow and obtain the access token prior to making the API call, as well as refreshing the token as needed.

### Obtaining an access token for testing

The MCP inspector can guide you through the process of obtaining an access token for testing purposes.

1. Run the inspector with the following command. You need Node.js installed on your machine.

2. In the sidebar on the left, for "Transport type", select either "SSE" or "Streamable HTTP".
3. Enter the URL of the MCP server.
4. In the right area, click on the "Open Auth Settings" button after "Need to configure authentication?".
5. Click "Quick OAuth Flow" and authorize on the OAuth screen.
6. Follow the steps in the "OAuth Flow Progress" section of the inspector and click "Continue" until you reach "Authentication complete".
7. Copy the `access_token` value.
8. Paste it into the `authorization_token` field in your MCP server configuration.

### Using the access token

Once you've obtained an access token using either OAuth flow above, you can use it in your MCP server configuration:

For detailed explanations of the OAuth flow, refer to the [Authorization section](https://modelcontextprotocol.io/specification/2025-11-25/basic/authorization) in the MCP specification.

If you're using the deprecated `mcp-client-2025-04-04` beta header, follow this guide to migrate to the new version.

1. **New beta header**: Change from `mcp-client-2025-04-04` to `mcp-client-2025-11-20`
2. **Tool configuration moved**: Tool configuration now lives in the `tools` array as MCPToolset objects, not in the MCP server definition
3. **More flexible configuration**: New pattern supports allowlisting, denylisting, and per-tool configuration

**Before (deprecated):**

### Common migration patterns

| Old pattern | New pattern |
|-------------|-------------|
| No `tool_configuration` (all tools enabled) | MCPToolset with no `default_config` or `configs` |
| `tool_configuration.enabled: false` | MCPToolset with `default_config.enabled: false` |
| `tool_configuration.allowed_tools: [...]` | MCPToolset with `default_config.enabled: false` and specific tools enabled in `configs` |

## Deprecated version: mcp-client-2025-04-04

<Note type="warning">
  This version is deprecated. Please migrate to `mcp-client-2025-11-20` using the [migration guide](#migration-guide) above.
</Note>

The previous version of the MCP connector included tool configuration directly in the MCP server definition:

### Deprecated field descriptions

| Property | Type | Description |
|----------|------|-------------|
| `tool_configuration` | object | **Deprecated**: Use MCPToolset in the `tools` array instead |
| `tool_configuration.enabled` | boolean | **Deprecated**: Use `default_config.enabled` in MCPToolset |
| `tool_configuration.allowed_tools` | array | **Deprecated**: Use allowlist pattern with `configs` in MCPToolset |

**Examples:**

Example 1 (unknown):
```unknown

```

Example 2 (unknown):
```unknown

```

Example 3 (unknown):
```unknown
</CodeGroup>

## MCP server configuration

Each MCP server in the `mcp_servers` array defines the connection details:
```

Example 4 (unknown):
```unknown
### Field descriptions

| Property | Type | Required | Description |
|----------|------|----------|-------------|
| `type` | string | Yes | Currently only "url" is supported |
| `url` | string | Yes | The URL of the MCP server. Must start with https:// |
| `name` | string | Yes | A unique identifier for this MCP server. Must be referenced by exactly one MCPToolset in the `tools` array. |
| `authorization_token` | string | No | OAuth authorization token if required by the MCP server. See [MCP specification](https://modelcontextprotocol.io/specification/2025-11-25/basic/authorization). |

## MCP toolset configuration

The MCPToolset lives in the `tools` array and configures which tools from the MCP server are enabled and how they should be configured.

### Basic structure
```

---

## Create an SDK MCP server with the custom tool

**URL:** llms-txt#create-an-sdk-mcp-server-with-the-custom-tool

**Contents:**
- Using Custom Tools
  - Tool Name Format
  - Configuring Allowed Tools

custom_server = create_sdk_mcp_server(
    name="my-custom-tools",
    version="1.0.0",
    tools=[get_weather]  # Pass the decorated function
)
typescript TypeScript
import { query } from "@anthropic-ai/claude-agent-sdk";

// Use the custom tools in your query with streaming input
async function* generateMessages() {
  yield {
    type: "user" as const,
    message: {
      role: "user" as const,
      content: "What's the weather in San Francisco?"
    }
  };
}

for await (const message of query({
  prompt: generateMessages(),  // Use async generator for streaming input
  options: {
    mcpServers: {
      "my-custom-tools": customServer  // Pass as object/dictionary, not array
    },
    // Optionally specify which tools Claude can use
    allowedTools: [
      "mcp__my-custom-tools__get_weather",  // Allow the weather tool
      // Add other tools as needed
    ],
    maxTurns: 3
  }
})) {
  if (message.type === "result" && message.subtype === "success") {
    console.log(message.result);
  }
}
python Python
from claude_agent_sdk import ClaudeSDKClient, ClaudeAgentOptions
import asyncio

**Examples:**

Example 1 (unknown):
```unknown
</CodeGroup>

## Using Custom Tools

Pass the custom server to the `query` function via the `mcpServers` option as a dictionary/object.

<Note>
**Important:** Custom MCP tools require streaming input mode. You must use an async generator/iterable for the `prompt` parameter - a simple string will not work with MCP servers.
</Note>

### Tool Name Format

When MCP tools are exposed to Claude, their names follow a specific format:
- Pattern: `mcp__{server_name}__{tool_name}`
- Example: A tool named `get_weather` in server `my-custom-tools` becomes `mcp__my-custom-tools__get_weather`

### Configuring Allowed Tools

You can control which tools Claude can use via the `allowedTools` option:

<CodeGroup>
```

Example 2 (unknown):
```unknown

```

---

## MCP in the SDK

**URL:** llms-txt#mcp-in-the-sdk

**Contents:**
- Overview
- Configuration
  - Basic Configuration
  - Using MCP Servers in SDK
- Transport Types
  - stdio Servers

Extend Claude Code with custom tools using Model Context Protocol servers

Model Context Protocol (MCP) servers extend Claude Code with custom tools and capabilities. MCPs can run as external processes, connect via HTTP/SSE, or execute directly within your SDK application.

### Basic Configuration

Configure MCP servers in `.mcp.json` at your project root:

### Using MCP Servers in SDK

External processes communicating via stdin/stdout:

**Examples:**

Example 1 (unknown):
```unknown

```

Example 2 (unknown):
```unknown
</CodeGroup>

### Using MCP Servers in SDK

<CodeGroup>
```

Example 3 (unknown):
```unknown

```

Example 4 (unknown):
```unknown
</CodeGroup>

## Transport Types

### stdio Servers

External processes communicating via stdin/stdout:

<CodeGroup>
```

---
