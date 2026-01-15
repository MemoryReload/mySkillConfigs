# Claude-Code - Agents

**Pages:** 19

---

## Guides to common use cases

**URL:** llms-txt#guides-to-common-use-cases

Claude is designed to excel in a variety of tasks. Explore these in-depth production guides to learn how to build common use cases with Claude.

<CardGroup cols={2}>
  <Card title="Ticket routing" icon="headset" href="/docs/en/about-claude/use-case-guides/ticket-routing">
    Best practices for using Claude to classify and route customer support tickets at scale.
  </Card>
  <Card title="Customer support agent" icon="robot" href="/docs/en/about-claude/use-case-guides/customer-support-chat">
    Build intelligent, context-aware chatbots with Claude to enhance customer support interactions.
  </Card>
  <Card title="Content moderation" icon="verified" href="/docs/en/about-claude/use-case-guides/content-moderation">
    Techniques and best practices for using Claude to perform content filtering and general content moderation.
  </Card>
  <Card title="Legal summarization" icon="book" href="/docs/en/about-claude/use-case-guides/legal-summarization">
    Summarize legal documents using Claude to extract key information and expedite research.
  </Card>
</CardGroup>

---

## Agent SDK reference - TypeScript

**URL:** llms-txt#agent-sdk-reference---typescript

**Contents:**
- Installation
- Functions
  - `query()`
  - `tool()`
  - `createSdkMcpServer()`
- Types
  - `Options`
  - `Query`
  - `AgentDefinition`
  - `SettingSource`

Complete API reference for the TypeScript Agent SDK, including all functions, types, and interfaces.

<script src="/components/typescript-sdk-type-links.js" defer />

<Note>
**Try the new V2 interface (preview):** A simplified interface with `send()` and `receive()` patterns is now available, making multi-turn conversations easier. [Learn more about the TypeScript V2 preview](/docs/en/agent-sdk/typescript-v2-preview)
</Note>

The primary function for interacting with Claude Code. Creates an async generator that streams messages as they arrive.

| Parameter | Type | Description |
| :-------- | :--- | :---------- |
| `prompt` | `string \| AsyncIterable<`[`SDKUserMessage`](#sdkusermessage)`>` | The input prompt as a string or async iterable for streaming mode |
| `options` | [`Options`](#options) | Optional configuration object (see Options type below) |

Returns a [`Query`](#query-1) object that extends `AsyncGenerator<`[`SDKMessage`](#sdkmessage)`, void>` with additional methods.

Creates a type-safe MCP tool definition for use with SDK MCP servers.

| Parameter | Type | Description |
| :-------- | :--- | :---------- |
| `name` | `string` | The name of the tool |
| `description` | `string` | A description of what the tool does |
| `inputSchema` | `Schema extends ZodRawShape` | Zod schema defining the tool's input parameters |
| `handler` | `(args, extra) => Promise<`[`CallToolResult`](#calltoolresult)`>` | Async function that executes the tool logic |

### `createSdkMcpServer()`

Creates an MCP server instance that runs in the same process as your application.

| Parameter | Type | Description |
| :-------- | :--- | :---------- |
| `options.name` | `string` | The name of the MCP server |
| `options.version` | `string` | Optional version string |
| `options.tools` | `Array<SdkMcpToolDefinition>` | Array of tool definitions created with [`tool()`](#tool) |

Configuration object for the `query()` function.

| Property | Type | Default | Description |
| :------- | :--- | :------ | :---------- |
| `abortController` | `AbortController` | `new AbortController()` | Controller for cancelling operations |
| `additionalDirectories` | `string[]` | `[]` | Additional directories Claude can access |
| `agents` | `Record<string, [`AgentDefinition`](#agentdefinition)>` | `undefined` | Programmatically define subagents |
| `allowDangerouslySkipPermissions` | `boolean` | `false` | Enable bypassing permissions. Required when using `permissionMode: 'bypassPermissions'` |
| `allowedTools` | `string[]` | All tools | List of allowed tool names |
| `betas` | [`SdkBeta`](#sdkbeta)`[]` | `[]` | Enable beta features (e.g., `['context-1m-2025-08-07']`) |
| `canUseTool` | [`CanUseTool`](#canusetool) | `undefined` | Custom permission function for tool usage |
| `continue` | `boolean` | `false` | Continue the most recent conversation |
| `cwd` | `string` | `process.cwd()` | Current working directory |
| `disallowedTools` | `string[]` | `[]` | List of disallowed tool names |
| `enableFileCheckpointing` | `boolean` | `false` | Enable file change tracking for rewinding. See [File checkpointing](/docs/en/agent-sdk/file-checkpointing) |
| `env` | `Dict<string>` | `process.env` | Environment variables |
| `executable` | `'bun' \| 'deno' \| 'node'` | Auto-detected | JavaScript runtime to use |
| `executableArgs` | `string[]` | `[]` | Arguments to pass to the executable |
| `extraArgs` | `Record<string, string \| null>` | `{}` | Additional arguments |
| `fallbackModel` | `string` | `undefined` | Model to use if primary fails |
| `forkSession` | `boolean` | `false` | When resuming with `resume`, fork to a new session ID instead of continuing the original session |
| `hooks` | `Partial<Record<`[`HookEvent`](#hookevent)`, `[`HookCallbackMatcher`](#hookcallbackmatcher)`[]>>` | `{}` | Hook callbacks for events |
| `includePartialMessages` | `boolean` | `false` | Include partial message events |
| `maxBudgetUsd` | `number` | `undefined` | Maximum budget in USD for the query |
| `maxThinkingTokens` | `number` | `undefined` | Maximum tokens for thinking process |
| `maxTurns` | `number` | `undefined` | Maximum conversation turns |
| `mcpServers` | `Record<string, [`McpServerConfig`](#mcpserverconfig)>` | `{}` | MCP server configurations |
| `model` | `string` | Default from CLI | Claude model to use |
| `outputFormat` | `{ type: 'json_schema', schema: JSONSchema }` | `undefined` | Define output format for agent results. See [Structured outputs](/docs/en/agent-sdk/structured-outputs) for details |
| `pathToClaudeCodeExecutable` | `string` | Uses built-in executable | Path to Claude Code executable |
| `permissionMode` | [`PermissionMode`](#permissionmode) | `'default'` | Permission mode for the session |
| `permissionPromptToolName` | `string` | `undefined` | MCP tool name for permission prompts |
| `plugins` | [`SdkPluginConfig`](#sdkpluginconfig)`[]` | `[]` | Load custom plugins from local paths. See [Plugins](/docs/en/agent-sdk/plugins) for details |
| `resume` | `string` | `undefined` | Session ID to resume |
| `resumeSessionAt` | `string` | `undefined` | Resume session at a specific message UUID |
| `sandbox` | [`SandboxSettings`](#sandboxsettings) | `undefined` | Configure sandbox behavior programmatically. See [Sandbox settings](#sandboxsettings) for details |
| `settingSources` | [`SettingSource`](#settingsource)`[]` | `[]` (no settings) | Control which filesystem settings to load. When omitted, no settings are loaded. **Note:** Must include `'project'` to load CLAUDE.md files |
| `stderr` | `(data: string) => void` | `undefined` | Callback for stderr output |
| `strictMcpConfig` | `boolean` | `false` | Enforce strict MCP validation |
| `systemPrompt` | `string \| { type: 'preset'; preset: 'claude_code'; append?: string }` | `undefined` (empty prompt) | System prompt configuration. Pass a string for custom prompt, or `{ type: 'preset', preset: 'claude_code' }` to use Claude Code's system prompt. When using the preset object form, add `append` to extend the system prompt with additional instructions |
| `tools` | `string[] \| { type: 'preset'; preset: 'claude_code' }` | `undefined` | Tool configuration. Pass an array of tool names or use the preset to get Claude Code's default tools |

Interface returned by the `query()` function.

| Method | Description |
| :----- | :---------- |
| `interrupt()` | Interrupts the query (only available in streaming input mode) |
| `rewindFiles(userMessageUuid)` | Restores files to their state at the specified user message. Requires `enableFileCheckpointing: true`. See [File checkpointing](/docs/en/agent-sdk/file-checkpointing) |
| `setPermissionMode()` | Changes the permission mode (only available in streaming input mode) |
| `setModel()` | Changes the model (only available in streaming input mode) |
| `setMaxThinkingTokens()` | Changes the maximum thinking tokens (only available in streaming input mode) |
| `supportedCommands()` | Returns available slash commands |
| `supportedModels()` | Returns available models with display info |
| `mcpServerStatus()` | Returns status of connected MCP servers |
| `accountInfo()` | Returns account information |

### `AgentDefinition`

Configuration for a subagent defined programmatically.

| Field | Required | Description |
|:------|:---------|:------------|
| `description` | Yes | Natural language description of when to use this agent |
| `tools` | No | Array of allowed tool names. If omitted, inherits all tools |
| `prompt` | Yes | The agent's system prompt |
| `model` | No | Model override for this agent. If omitted, uses the main model |

Controls which filesystem-based configuration sources the SDK loads settings from.

| Value | Description | Location |
|:------|:------------|:---------|
| `'user'` | Global user settings | `~/.claude/settings.json` |
| `'project'` | Shared project settings (version controlled) | `.claude/settings.json` |
| `'local'` | Local project settings (gitignored) | `.claude/settings.local.json` |

#### Default behavior

When `settingSources` is **omitted** or **undefined**, the SDK does **not** load any filesystem settings. This provides isolation for SDK applications.

#### Why use settingSources?

**Load all filesystem settings (legacy behavior):**

**Load only specific setting sources:**

**Testing and CI environments:**

**SDK-only applications:**

**Loading CLAUDE.md project instructions:**

#### Settings precedence

When multiple sources are loaded, settings are merged with this precedence (highest to lowest):
1. Local settings (`.claude/settings.local.json`)
2. Project settings (`.claude/settings.json`)
3. User settings (`~/.claude/settings.json`)

Programmatic options (like `agents`, `allowedTools`) always override filesystem settings.

Custom permission function type for controlling tool usage.

### `PermissionResult`

Result of a permission check.

### `McpServerConfig`

Configuration for MCP servers.

#### `McpStdioServerConfig`

#### `McpSSEServerConfig`

#### `McpHttpServerConfig`

#### `McpSdkServerConfigWithInstance`

### `SdkPluginConfig`

Configuration for loading plugins in the SDK.

| Field | Type | Description |
|:------|:-----|:------------|
| `type` | `'local'` | Must be `'local'` (only local plugins currently supported) |
| `path` | `string` | Absolute or relative path to the plugin directory |

For complete information on creating and using plugins, see [Plugins](/docs/en/agent-sdk/plugins).

Union type of all possible messages returned by the query.

### `SDKAssistantMessage`

Assistant response message.

### `SDKUserMessageReplay`

Replayed user message with required UUID.

### `SDKResultMessage`

Final result message.

### `SDKSystemMessage`

System initialization message.

### `SDKPartialAssistantMessage`

Streaming partial message (only when `includePartialMessages` is true).

### `SDKCompactBoundaryMessage`

Message indicating a conversation compaction boundary.

### `SDKPermissionDenial`

Information about a denied tool use.

For a comprehensive guide on using hooks with examples and common patterns, see the [Hooks guide](/docs/en/agent-sdk/hooks).

Available hook events.

Hook callback function type.

### `HookCallbackMatcher`

Hook configuration with optional matcher.

Union type of all hook input types.

Base interface that all hook input types extend.

#### `PreToolUseHookInput`

#### `PostToolUseHookInput`

#### `PostToolUseFailureHookInput`

#### `NotificationHookInput`

#### `UserPromptSubmitHookInput`

#### `SessionStartHookInput`

#### `SessionEndHookInput`

#### `SubagentStartHookInput`

#### `SubagentStopHookInput`

#### `PreCompactHookInput`

#### `PermissionRequestHookInput`

#### `AsyncHookJSONOutput`

#### `SyncHookJSONOutput`

Documentation of input schemas for all built-in Claude Code tools. These types are exported from `@anthropic-ai/claude-agent-sdk` and can be used for type-safe tool interactions.

**Note:** This is a documentation-only type for clarity. It represents the union of all tool input types.

**Tool name:** `Task`

Launches a new agent to handle complex, multi-step tasks autonomously.

**Tool name:** `AskUserQuestion`

Asks the user clarifying questions during execution. See [Handle approvals and user input](/docs/en/agent-sdk/user-input#handle-clarifying-questions) for usage details.

**Tool name:** `Bash`

Executes bash commands in a persistent shell session with optional timeout and background execution.

**Tool name:** `BashOutput`

Retrieves output from a running or completed background bash shell.

**Tool name:** `Edit`

Performs exact string replacements in files.

**Tool name:** `Read`

Reads files from the local filesystem, including text, images, PDFs, and Jupyter notebooks.

**Tool name:** `Write`

Writes a file to the local filesystem, overwriting if it exists.

**Tool name:** `Glob`

Fast file pattern matching that works with any codebase size.

**Tool name:** `Grep`

Powerful search tool built on ripgrep with regex support.

**Tool name:** `KillBash`

Kills a running background bash shell by its ID.

**Tool name:** `NotebookEdit`

Edits cells in Jupyter notebook files.

**Tool name:** `WebFetch`

Fetches content from a URL and processes it with an AI model.

**Tool name:** `WebSearch`

Searches the web and returns formatted results.

**Tool name:** `TodoWrite`

Creates and manages a structured task list for tracking progress.

**Tool name:** `ExitPlanMode`

Exits planning mode and prompts the user to approve the plan.

**Tool name:** `ListMcpResources`

Lists available MCP resources from connected servers.

**Tool name:** `ReadMcpResource`

Reads a specific MCP resource from a server.

Documentation of output schemas for all built-in Claude Code tools. These types represent the actual response data returned by each tool.

**Note:** This is a documentation-only type for clarity. It represents the union of all tool output types.

**Tool name:** `Task`

Returns the final result from the subagent after completing the delegated task.

**Tool name:** `AskUserQuestion`

Returns the questions asked and the user's answers.

**Tool name:** `Bash`

Returns command output with exit status. Background commands return immediately with a shellId.

**Tool name:** `BashOutput`

Returns incremental output from background shells.

**Tool name:** `Edit`

Returns confirmation of successful edits with replacement count.

**Tool name:** `Read`

Returns file contents in format appropriate to file type.

**Tool name:** `Write`

Returns confirmation after successfully writing the file.

**Tool name:** `Glob`

Returns file paths matching the glob pattern, sorted by modification time.

**Tool name:** `Grep`

Returns search results in the format specified by output_mode.

**Tool name:** `KillBash`

Returns confirmation after terminating the background shell.

**Tool name:** `NotebookEdit`

Returns confirmation after modifying the Jupyter notebook.

**Tool name:** `WebFetch`

Returns the AI's analysis of the fetched web content.

**Tool name:** `WebSearch`

Returns formatted search results from the web.

**Tool name:** `TodoWrite`

Returns confirmation with current task statistics.

**Tool name:** `ExitPlanMode`

Returns confirmation after exiting plan mode.

**Tool name:** `ListMcpResources`

Returns list of available MCP resources.

**Tool name:** `ReadMcpResource`

Returns the contents of the requested MCP resource.

### `PermissionUpdate`

Operations for updating permissions.

### `PermissionBehavior`

### `PermissionUpdateDestination`

### `PermissionRuleValue`

Available beta features that can be enabled via the `betas` option. See [Beta headers](/docs/en/api/beta-headers) for more information.

| Value | Description | Compatible Models |
|:------|:------------|:------------------|
| `'context-1m-2025-08-07'` | Enables 1 million token [context window](/docs/en/build-with-claude/context-windows) | Claude Sonnet 4, Claude Sonnet 4.5 |

Information about an available slash command.

Information about an available model.

### `McpServerStatus`

Status of a connected MCP server.

Account information for the authenticated user.

Per-model usage statistics returned in result messages.

### `NonNullableUsage`

A version of [`Usage`](#usage) with all nullable fields made non-nullable.

Token usage statistics (from `@anthropic-ai/sdk`).

MCP tool result type (from `@modelcontextprotocol/sdk/types.js`).

Custom error class for abort operations.

## Sandbox Configuration

### `SandboxSettings`

Configuration for sandbox behavior. Use this to enable command sandboxing and configure network restrictions programmatically.

| Property | Type | Default | Description |
| :------- | :--- | :------ | :---------- |
| `enabled` | `boolean` | `false` | Enable sandbox mode for command execution |
| `autoAllowBashIfSandboxed` | `boolean` | `false` | Auto-approve bash commands when sandbox is enabled |
| `excludedCommands` | `string[]` | `[]` | Commands that always bypass sandbox restrictions (e.g., `['docker']`). These run unsandboxed automatically without model involvement |
| `allowUnsandboxedCommands` | `boolean` | `false` | Allow the model to request running commands outside the sandbox. When `true`, the model can set `dangerouslyDisableSandbox` in tool input, which falls back to the [permissions system](#permissions-fallback-for-unsandboxed-commands) |
| `network` | [`NetworkSandboxSettings`](#networksandboxsettings) | `undefined` | Network-specific sandbox configuration |
| `ignoreViolations` | [`SandboxIgnoreViolations`](#sandboxignoreviolations) | `undefined` | Configure which sandbox violations to ignore |
| `enableWeakerNestedSandbox` | `boolean` | `false` | Enable a weaker nested sandbox for compatibility |

<Note>
**Filesystem and network access restrictions** are NOT configured via sandbox settings. Instead, they are derived from [permission rules](https://code.claude.com/docs/en/settings#permission-settings):

- **Filesystem read restrictions**: Read deny rules
- **Filesystem write restrictions**: Edit allow/deny rules
- **Network restrictions**: WebFetch allow/deny rules

Use sandbox settings for command execution sandboxing, and permission rules for filesystem and network access control.
</Note>

### `NetworkSandboxSettings`

Network-specific configuration for sandbox mode.

| Property | Type | Default | Description |
| :------- | :--- | :------ | :---------- |
| `allowLocalBinding` | `boolean` | `false` | Allow processes to bind to local ports (e.g., for dev servers) |
| `allowUnixSockets` | `string[]` | `[]` | Unix socket paths that processes can access (e.g., Docker socket) |
| `allowAllUnixSockets` | `boolean` | `false` | Allow access to all Unix sockets |
| `httpProxyPort` | `number` | `undefined` | HTTP proxy port for network requests |
| `socksProxyPort` | `number` | `undefined` | SOCKS proxy port for network requests |

### `SandboxIgnoreViolations`

Configuration for ignoring specific sandbox violations.

| Property | Type | Default | Description |
| :------- | :--- | :------ | :---------- |
| `file` | `string[]` | `[]` | File path patterns to ignore violations for |
| `network` | `string[]` | `[]` | Network patterns to ignore violations for |

### Permissions Fallback for Unsandboxed Commands

When `allowUnsandboxedCommands` is enabled, the model can request to run commands outside the sandbox by setting `dangerouslyDisableSandbox: true` in the tool input. These requests fall back to the existing permissions system, meaning your `canUseTool` handler will be invoked, allowing you to implement custom authorization logic.

<Note>
**`excludedCommands` vs `allowUnsandboxedCommands`:**
- `excludedCommands`: A static list of commands that always bypass the sandbox automatically (e.g., `['docker']`). The model has no control over this.
- `allowUnsandboxedCommands`: Lets the model decide at runtime whether to request unsandboxed execution by setting `dangerouslyDisableSandbox: true` in the tool input.
</Note>

This pattern enables you to:

- **Audit model requests**: Log when the model requests unsandboxed execution
- **Implement allowlists**: Only permit specific commands to run unsandboxed
- **Add approval workflows**: Require explicit authorization for privileged operations

<Warning>
Commands running with `dangerouslyDisableSandbox: true` have full system access. Ensure your `canUseTool` handler validates these requests carefully.
</Warning>

- [SDK overview](/docs/en/agent-sdk/overview) - General SDK concepts
- [Python SDK reference](/docs/en/agent-sdk/python) - Python SDK documentation
- [CLI reference](https://code.claude.com/docs/en/cli-reference) - Command-line interface
- [Common workflows](https://code.claude.com/docs/en/common-workflows) - Step-by-step guides

**Examples:**

Example 1 (bash):
```bash
npm install @anthropic-ai/claude-agent-sdk
```

Example 2 (typescript):
```typescript
function query({
  prompt,
  options
}: {
  prompt: string | AsyncIterable<SDKUserMessage>;
  options?: Options;
}): Query
```

Example 3 (typescript):
```typescript
function tool<Schema extends ZodRawShape>(
  name: string,
  description: string,
  inputSchema: Schema,
  handler: (args: z.infer<ZodObject<Schema>>, extra: unknown) => Promise<CallToolResult>
): SdkMcpToolDefinition<Schema>
```

Example 4 (typescript):
```typescript
function createSdkMcpServer(options: {
  name: string;
  version?: string;
  tools?: Array<SdkMcpToolDefinition<any>>;
}): McpSdkServerConfigWithInstance
```

---

## Hosting the Agent SDK

**URL:** llms-txt#hosting-the-agent-sdk

**Contents:**
- Hosting Requirements
  - Container-Based Sandboxing
  - System Requirements
- Understanding the SDK Architecture
- Sandbox Provider Options
- Production Deployment Patterns
  - Pattern 1: Ephemeral Sessions
  - Pattern 2: Long-Running Sessions
  - Pattern 3: Hybrid Sessions
  - Pattern 4: Single Containers

Deploy and host Claude Agent SDK in production environments

The Claude Agent SDK differs from traditional stateless LLM APIs in that it maintains conversational state and executes commands in a persistent environment. This guide covers the architecture, hosting considerations, and best practices for deploying SDK-based agents in production.

<Info>
For security hardening beyond basic sandboxing—including network controls, credential management, and isolation options—see [Secure Deployment](/docs/en/agent-sdk/secure-deployment).
</Info>

## Hosting Requirements

### Container-Based Sandboxing

For security and isolation, the SDK should run inside a sandboxed container environment. This provides process isolation, resource limits, network control, and ephemeral filesystems.

The SDK also supports [programmatic sandbox configuration](/docs/en/agent-sdk/typescript#sandbox-settings) for command execution.

### System Requirements

Each SDK instance requires:

- **Runtime dependencies**
  - Python 3.10+ (for Python SDK) or Node.js 18+ (for TypeScript SDK)
  - Node.js (required by Claude Code CLI)
  - Claude Code CLI: `npm install -g @anthropic-ai/claude-code`

- **Resource allocation**
  - Recommended: 1GiB RAM, 5GiB of disk, and 1 CPU (vary this based on your task as needed)

- **Network access**
  - Outbound HTTPS to `api.anthropic.com`
  - Optional: Access to MCP servers or external tools

## Understanding the SDK Architecture

Unlike stateless API calls, the Claude Agent SDK operates as a **long-running process** that:
- **Executes commands** in a persistent shell environment
- **Manages file operations** within a working directory
- **Handles tool execution** with context from previous interactions

## Sandbox Provider Options

Several providers specialize in secure container environments for AI code execution:

- **[Modal Sandbox](https://modal.com/docs/guide/sandbox)** - [demo implementation](https://modal.com/docs/examples/claude-slack-gif-creator)
- **[Cloudflare Sandboxes](https://github.com/cloudflare/sandbox-sdk)**
- **[Daytona](https://www.daytona.io/)**
- **[E2B](https://e2b.dev/)**
- **[Fly Machines](https://fly.io/docs/machines/)**
- **[Vercel Sandbox](https://vercel.com/docs/functions/sandbox)**

For self-hosted options (Docker, gVisor, Firecracker) and detailed isolation configuration, see [Isolation Technologies](/docs/en/agent-sdk/secure-deployment#isolation-technologies).

## Production Deployment Patterns

### Pattern 1: Ephemeral Sessions

Create a new container for each user task, then destroy it when complete.

Best for one-off tasks, the user may still interact with the AI while the task is completing, but once completed the container is destroyed.

**Examples:**
- Bug Investigation & Fix: Debug and resolve a specific issue with relevant context
- Invoice Processing: Extract and structure data from receipts/invoices for accounting systems
- Translation Tasks: Translate documents or content batches between languages
- Image/Video Processing: Apply transformations, optimizations, or extract metadata from media files

### Pattern 2: Long-Running Sessions

Maintain persistent container instances for long running tasks. Often times running _multiple_ Claude Agent processes inside of the container based on demand.

Best for proactive agents that take action without the users input, agents that serve content or agents that process high amounts of messages.

**Examples:**
- Email Agent: Monitors incoming emails and autonomously triages, responds, or takes actions based on content
- Site Builder: Hosts custom websites per user with live editing capabilities served through container ports
- High-Frequency Chat Bots: Handles continuous message streams from platforms like Slack where rapid response times are critical

### Pattern 3: Hybrid Sessions

Ephemeral containers that are hydrated with history and state, possibly from a database or from the SDK's session resumption features.

Best for containers with intermittent interaction from the user that kicks off work and spins down when the work is completed but can be continued.

**Examples:**
- Personal Project Manager: Helps manage ongoing projects with intermittent check-ins, maintains context of tasks, decisions, and progress
- Deep Research: Conducts multi-hour research tasks, saves findings and resumes investigation when user returns
- Customer Support Agent: Handles support tickets that span multiple interactions, loads ticket history and customer context

### Pattern 4: Single Containers

Run multiple Claude Agent SDK processes in one global container.

Best for agents that must collaborate closely together. This is likely the least popular pattern because you will have to prevent agents from overwriting each other.

**Examples:**
- **Simulations**: Agents that interact with each other in simulations such as video games.

---

## INFO: Compaction complete. New token usage: 2500

**URL:** llms-txt#info:-compaction-complete.-new-token-usage:-2500

**Contents:**
  - When to use compaction

typescript TypeScript
// The SDK logs compaction events to the console
// You'll see messages like:
// Token usage 105000 has exceeded the threshold of 100000. Performing compaction.
// Compaction complete. New token usage: 2500
```

### When to use compaction

- Long-running agent tasks that process many files or data sources
- Research workflows that accumulate large amounts of information
- Multi-step tasks with clear, measurable progress
- Tasks that produce artifacts (files, reports) that persist outside the conversation

**Less ideal use cases:**

- Tasks requiring precise recall of early conversation details
- Workflows using server-side tools extensively
- Tasks that need to maintain exact state across many variables

**Examples:**

Example 1 (unknown):
```unknown

```

---

## Factory function that returns an AgentDefinition

**URL:** llms-txt#factory-function-that-returns-an-agentdefinition

---

## What's new in Claude 4.5

**URL:** llms-txt#what's-new-in-claude-4.5

**Contents:**
- Key improvements in Opus 4.5 over Opus 4.1
  - Maximum intelligence
  - Effort parameter
  - Computer use excellence
  - Practical performance
  - Thinking block preservation
- Key improvements in Sonnet 4.5 over Sonnet 4
  - Coding excellence
  - Agent capabilities
  - Communication and interaction style

Claude 4.5 introduces three models designed for different use cases:

- **Claude Opus 4.5**: Our most intelligent model combining maximum capability with practical performance. Features a more accessible price point than previous Opus models. Available with a 200k token context window.
- **Claude Sonnet 4.5**: Our best model for complex agents and coding, with the highest intelligence across most tasks. Available with a 200k and 1M (beta) token context window.
- **Claude Haiku 4.5**: Our fastest and most intelligent Haiku model with near-frontier performance. Available with a 200k token context window.

## Key improvements in Opus 4.5 over Opus 4.1

### Maximum intelligence

Claude Opus 4.5 represents our most intelligent model, combining maximum capability with practical performance. It delivers step-change improvements across reasoning, coding, and complex problem-solving tasks while maintaining the high-quality outputs expected from the Opus family.

Claude Opus 4.5 is the only model that supports the [effort parameter](/docs/en/build-with-claude/effort), allowing you to control how many tokens Claude uses when responding. This gives you the ability to trade off between response thoroughness and token efficiency with a single model.

The effort parameter affects all tokens in the response, including text responses, tool calls, and extended thinking. You can choose between:
- **High effort**: Maximum thoroughness for complex analysis and detailed explanations
- **Medium effort**: Balanced approach for most production use cases
- **Low effort**: Most token-efficient responses for high-volume automation

### Computer use excellence

Claude Opus 4.5 introduces [enhanced computer use capabilities](/docs/en/agents-and-tools/tool-use/computer-use-tool) with a new zoom action that enables detailed inspection of specific screen regions at full resolution. This allows Claude to examine fine-grained UI elements, small text, and detailed visual information that might be unclear in standard screenshots.

The zoom capability is particularly valuable for:
- Inspecting small UI elements and controls
- Reading fine print or detailed text
- Analyzing complex interfaces with dense information
- Verifying precise visual details before taking actions

### Practical performance

Claude Opus 4.5 delivers flagship intelligence at a [more accessible price point](/docs/en/about-claude/pricing) than previous Opus models, making advanced AI capabilities available for a broader range of applications and use cases.

### Thinking block preservation

Claude Opus 4.5 [automatically preserves all previous thinking blocks](/docs/en/build-with-claude/extended-thinking#thinking-block-preservation-in-claude-opus-4-5) throughout conversations, maintaining reasoning continuity across extended multi-turn interactions and tool use sessions. This ensures Claude can effectively leverage its full reasoning history when working on complex, long-running tasks.

## Key improvements in Sonnet 4.5 over Sonnet 4

### Coding excellence

Claude Sonnet 4.5 is our best coding model to date, with significant improvements across the entire development lifecycle:

- **SWE-bench Verified performance**: Advanced state-of-the-art on coding benchmarks
- **Enhanced planning and system design**: Better architectural decisions and code organization
- **Improved security engineering**: More robust security practices and vulnerability detection
- **Better instruction following**: More precise adherence to coding specifications and requirements

<Note>
Claude Sonnet 4.5 performs significantly better on coding tasks when [extended thinking](/docs/en/build-with-claude/extended-thinking) is enabled. Extended thinking is disabled by default, but we recommend enabling it for complex coding work. Be aware that extended thinking impacts [prompt caching efficiency](/docs/en/build-with-claude/prompt-caching#caching-with-thinking-blocks). See the [migration guide](/docs/en/about-claude/models/migrating-to-claude-4#extended-thinking-recommendations) for configuration details.
</Note>

### Agent capabilities

Claude Sonnet 4.5 introduces major advances in agent capabilities:

- **Extended autonomous operation**: Sonnet 4.5 can work independently for hours while maintaining clarity and focus on incremental progress. The model makes steady advances on a few tasks at a time rather than attempting everything at once. It provides fact-based progress updates that accurately reflect what has been accomplished.
- **Context awareness**: Claude now tracks its token usage throughout conversations, receiving updates after each tool call. This awareness helps prevent premature task abandonment and enables more effective execution on long-running tasks. See [Context awareness](/docs/en/build-with-claude/context-windows#context-awareness-in-claude-sonnet-4-5) for technical details and [prompting guidance](/docs/en/build-with-claude/prompt-engineering/claude-4-best-practices#context-awareness-and-multi-window-workflows).
- **Enhanced tool usage**: The model more effectively uses parallel tool calls, firing off multiple speculative searches simultaneously during research and reading several files at once to build context faster. Improved coordination across multiple tools and information sources enables the model to effectively leverage a wide range of capabilities in agentic search and coding workflows.
- **Advanced context management**: Sonnet 4.5 maintains exceptional state tracking in external files, preserving goal-orientation across sessions. Combined with more effective context window usage and our new context management API features, the model optimally handles information across extended sessions to maintain coherence over time.

<Note>Context awareness is available in Claude Sonnet 4, Sonnet 4.5, Haiku 4.5, Opus 4, Opus 4.1, and Opus 4.5.</Note>

### Communication and interaction style

Claude Sonnet 4.5 has a refined communication approach that is concise, direct, and natural. It provides fact-based progress updates and may skip verbose summaries after tool calls to maintain workflow momentum (though this can be adjusted with prompting).

For detailed guidance on working with this communication style, see [Claude 4 best practices](/docs/en/build-with-claude/prompt-engineering/claude-4-best-practices).

### Creative content generation

Claude Sonnet 4.5 excels at creative content tasks:

- **Presentations and animations**: Matches or exceeds Claude Opus 4.1 and Opus 4.5 for creating slides and visual content
- **Creative flair**: Produces polished, professional output with strong instruction following
- **First-try quality**: Generates usable, well-designed content in initial attempts

## Key improvements in Haiku 4.5 over Haiku 3.5

Claude Haiku 4.5 represents a transformative leap for the Haiku model family, bringing frontier capabilities to our fastest model class:

### Near-frontier intelligence with blazing speed

Claude Haiku 4.5 delivers near-frontier performance matching Sonnet 4 at significantly lower cost and faster speed:

- **Near-frontier intelligence**: Matches Sonnet 4 performance across reasoning, coding, and complex tasks
- **Enhanced speed**: More than twice the speed of Sonnet 4, with optimizations for output tokens per second (OTPS)
- **Optimal cost-performance**: Near-frontier intelligence at one-third the cost, ideal for high-volume deployments

### Extended thinking capabilities

Claude Haiku 4.5 is the **first Haiku model** to support extended thinking, bringing advanced reasoning capabilities to the Haiku family:

- **Reasoning at speed**: Access to Claude's internal reasoning process for complex problem-solving
- **Thinking Summarization**: Summarized thinking output for production-ready deployments
- **Interleaved thinking**: Think between tool calls for more sophisticated multi-step workflows
- **Budget control**: Configure thinking token budgets to balance reasoning depth with speed

Extended thinking must be enabled explicitly by adding a `thinking` parameter to your API requests. See the [Extended thinking documentation](/docs/en/build-with-claude/extended-thinking) for implementation details.

<Note>
Claude Haiku 4.5 performs significantly better on coding and reasoning tasks when [extended thinking](/docs/en/build-with-claude/extended-thinking) is enabled. Extended thinking is disabled by default, but we recommend enabling it for complex problem-solving, coding work, and multi-step reasoning. Be aware that extended thinking impacts [prompt caching efficiency](/docs/en/build-with-claude/prompt-caching#caching-with-thinking-blocks). See the [migration guide](/docs/en/about-claude/models/migrating-to-claude-4#extended-thinking-recommendations) for configuration details.
</Note>

<Note>Available in Claude Sonnet 3.7, Sonnet 4, Sonnet 4.5, Haiku 4.5, Opus 4, Opus 4.1, and Opus 4.5.</Note>

### Context awareness

Claude Haiku 4.5 features **context awareness**, enabling the model to track its remaining context window throughout a conversation:

- **Token budget tracking**: Claude receives real-time updates on remaining context capacity after each tool call
- **Better task persistence**: The model can execute tasks more effectively by understanding available working space
- **Multi-context-window workflows**: Improved handling of state transitions across extended sessions

This is the first Haiku model with native context awareness capabilities. For prompting guidance, see [Claude 4 best practices](/docs/en/build-with-claude/prompt-engineering/claude-4-best-practices#context-awareness-and-multi-window-workflows).

<Note>Available in Claude Sonnet 4, Sonnet 4.5, Haiku 4.5, Opus 4, Opus 4.1, and Opus 4.5.</Note>

### Strong coding and tool use

Claude Haiku 4.5 delivers robust coding capabilities expected from modern Claude models:

- **Coding proficiency**: Strong performance across code generation, debugging, and refactoring tasks
- **Full tool support**: Compatible with all Claude 4 tools including bash, code execution, text editor, web search, and computer use
- **Enhanced computer use**: Optimized for autonomous desktop interaction and browser automation workflows
- **Parallel tool execution**: Efficient coordination across multiple tools for complex workflows

Haiku 4.5 is designed for use cases that demand both intelligence and efficiency:

- **Real-time applications**: Fast response times for interactive user experiences
- **High-volume processing**: Cost-effective intelligence for large-scale deployments
- **Free tier implementations**: Premium model quality at accessible pricing
- **Sub-agent architectures**: Fast, intelligent agents for multi-agent systems
- **Computer use at scale**: Cost-effective autonomous desktop and browser automation

### Programmatic tool calling (Beta)

[Programmatic tool calling](/docs/en/agents-and-tools/tool-use/programmatic-tool-calling) allows Claude to write code that calls your tools programmatically within a code execution container, rather than requiring round trips through the model for each tool invocation. This significantly reduces latency for multi-tool workflows and decreases token consumption by allowing Claude to filter or process data before it reaches the model's context window.

Key benefits:
- **Reduced latency**: Eliminate model round-trips between tool calls
- **Token efficiency**: Process and filter tool results programmatically before returning to Claude
- **Complex workflows**: Support loops, conditional logic, and batch processing

<Note>Available in Claude Opus 4.5 and Claude Sonnet 4.5. Requires [beta header](/docs/en/api/beta-headers): `advanced-tool-use-2025-11-20`</Note>

### Tool search tool (Beta)

The [tool search tool](/docs/en/agents-and-tools/tool-use/tool-search-tool) enables Claude to work with hundreds or thousands of tools by dynamically discovering and loading them on-demand. Instead of loading all tool definitions into the context window upfront, Claude searches your tool catalog and loads only the tools it needs.

Two search variants are available:
- **Regex** (`tool_search_tool_regex_20251119`): Claude constructs regex patterns to search tool names, descriptions, and arguments
- **BM25** (`tool_search_tool_bm25_20251119`): Claude uses natural language queries to search for tools

This approach solves two critical challenges:
- **Context efficiency**: Save 10-20K tokens by not loading all tool definitions upfront
- **Tool selection accuracy**: Maintain high accuracy even with 100+ available tools

<Note>Available in Claude Opus 4.5 and Claude Sonnet 4.5. Requires [beta header](/docs/en/api/beta-headers): `advanced-tool-use-2025-11-20`</Note>

### Effort parameter (Beta)

The [effort parameter](/docs/en/build-with-claude/effort) allows you to control how many tokens Claude uses when responding, trading off between response thoroughness and token efficiency:

The effort parameter affects all tokens in the response, including text responses, tool calls, and extended thinking. Lower effort levels produce more concise responses with minimal explanations, while higher effort provides detailed reasoning and comprehensive answers.

<Note>Available exclusively in Claude Opus 4.5. Requires [beta header](/docs/en/api/beta-headers): `effort-2025-11-24`</Note>

### Tool use examples (Beta)

[Tool use examples](/docs/en/agents-and-tools/tool-use/implement-tool-use#providing-tool-use-examples) allow you to provide concrete examples of valid tool inputs to help Claude understand how to use your tools more effectively. This is particularly useful for complex tools with nested objects, optional parameters, or format-sensitive inputs.

Examples are included in the prompt alongside your tool schema, showing Claude concrete patterns for well-formed tool calls. Each example must be valid according to the tool's `input_schema`.

<Note>Available in Claude Sonnet 4.5, Haiku 4.5, Opus 4.5, Opus 4.1, and Opus 4. Requires [beta header](/docs/en/api/beta-headers): `advanced-tool-use-2025-11-20`.</Note>

### Memory tool (Beta)

The new [memory tool](/docs/en/agents-and-tools/tool-use/memory-tool) enables Claude to store and retrieve information outside the context window:

This allows for:
- Building knowledge bases over time
- Maintaining project state across sessions
- Preserving effectively unlimited context through file-based storage

<Note>Available in Claude Sonnet 4, Sonnet 4.5, Haiku 4.5, Opus 4, Opus 4.1, and Opus 4.5. Requires [beta header](/docs/en/api/beta-headers): `context-management-2025-06-27`</Note>

Use [context editing](/docs/en/build-with-claude/context-editing) for intelligent context management through automatic tool call clearing:

This feature automatically removes older tool calls and results when approaching token limits, helping manage context in long-running agent sessions.

<Note>Available in Claude Sonnet 4, Sonnet 4.5, Haiku 4.5, Opus 4, Opus 4.1, and Opus 4.5. Requires [beta header](/docs/en/api/beta-headers): `context-management-2025-06-27`</Note>

### Enhanced stop reasons

Claude 4.5 models introduce a new `model_context_window_exceeded` stop reason that explicitly indicates when generation stopped due to hitting the context window limit, rather than the requested `max_tokens` limit. This makes it easier to handle context window limits in your application logic.

### Improved tool parameter handling

Claude 4.5 models include a bug fix that preserves intentional formatting in tool call string parameters. Previously, trailing newlines in string parameters were sometimes incorrectly stripped. This fix ensures that tools requiring precise formatting (like text editors) receive parameters exactly as intended.

<Note>
This is a behind-the-scenes improvement with no API changes required. However, tools with string parameters may now receive values with trailing newlines that were previously stripped.
</Note>

### Token count optimizations

Claude 4.5 models include automatic optimizations to improve model performance. These optimizations may add small amounts of tokens to requests, but **you are not billed for these system-added tokens**.

## Features introduced in Claude 4

The following features were introduced in Claude 4 and are available across Claude 4 models, including Claude Sonnet 4.5 and Claude Haiku 4.5.

### New refusal stop reason

Claude 4 models introduce a new `refusal` stop reason for content that the model declines to generate for safety reasons:

When using Claude 4 models, you should update your application to [handle `refusal` stop reasons](/docs/en/test-and-evaluate/strengthen-guardrails/handle-streaming-refusals).

### Summarized thinking

With extended thinking enabled, the Messages API for Claude 4 models returns a summary of Claude's full thinking process. Summarized thinking provides the full intelligence benefits of extended thinking, while preventing misuse.

While the API is consistent across Claude 3.7 and 4 models, streaming responses for extended thinking might return in a "chunky" delivery pattern, with possible delays between streaming events.

<Note>
Summarization is processed by a different model than the one you target in your requests. The thinking model does not see the summarized output.
</Note>

For more information, see the [Extended thinking documentation](/docs/en/build-with-claude/extended-thinking#summarized-thinking).

### Interleaved thinking

Claude 4 models support interleaving tool use with extended thinking, allowing for more natural conversations where tool uses and responses can be mixed with regular messages.

<Note>
Interleaved thinking is in beta. To enable interleaved thinking, add [the beta header](/docs/en/api/beta-headers) `interleaved-thinking-2025-05-14` to your API request.
</Note>

For more information, see the [Extended thinking documentation](/docs/en/build-with-claude/extended-thinking#interleaved-thinking).

### Behavioral differences

Claude 4 models have notable behavioral changes that may affect how you structure prompts:

#### Communication style changes

- **More concise and direct**: Claude 4 models communicate more efficiently, with less verbose explanations
- **More natural tone**: Responses are slightly more conversational and less machine-like
- **Efficiency-focused**: May skip detailed summaries after completing actions to maintain workflow momentum (you can prompt for more detail if needed)

#### Instruction following

Claude 4 models are trained for precise instruction following and require more explicit direction:

- **Be explicit about actions**: Use direct language like "Make these changes" or "Implement this feature" rather than "Can you suggest changes" if you want Claude to take action
- **State desired behaviors clearly**: Claude will follow instructions precisely, so being specific about what you want helps achieve better results

For comprehensive guidance on working with these models, see [Claude 4 prompt engineering best practices](/docs/en/build-with-claude/prompt-engineering/claude-4-best-practices).

### Updated text editor tool

The text editor tool has been updated for Claude 4 models with the following changes:

- **Tool type**: `text_editor_20250728`
- **Tool name**: `str_replace_based_edit_tool`
- The `undo_edit` command is no longer supported

<Note>
The `str_replace_editor` text editor tool remains the same for Claude Sonnet 3.7.
</Note>

If you're migrating from Claude Sonnet 3.7 and using the text editor tool:

**Examples:**

Example 1 (python):
```python
tools=[
    {
        "type": "code_execution_20250825",
        "name": "code_execution"
    },
    {
        "name": "query_database",
        "description": "Execute a SQL query against the sales database. Returns a list of rows as JSON objects.",
        "input_schema": {...},
        "allowed_callers": ["code_execution_20250825"]  # Enable programmatic calling
    }
]
```

Example 2 (python):
```python
tools=[
    {
        "type": "tool_search_tool_regex_20251119",
        "name": "tool_search_tool_regex"
    },
    {
        "name": "get_weather",
        "description": "Get the weather at a specific location",
        "input_schema": {...},
        "defer_loading": True  # Load on-demand via search
    }
]
```

Example 3 (python):
```python
response = client.beta.messages.create(
    model="claude-opus-4-5-20251101",
    betas=["effort-2025-11-24"],
    max_tokens=4096,
    messages=[{"role": "user", "content": "..."}],
    output_config={
        "effort": "medium"  # "low", "medium", or "high"
    }
)
```

Example 4 (python):
```python
tools=[
    {
        "name": "get_weather",
        "description": "Get the current weather in a given location",
        "input_schema": {...},
        "input_examples": [
            {
                "location": "San Francisco, CA",
                "unit": "fahrenheit"
            },
            {
                "location": "Tokyo, Japan",
                "unit": "celsius"
            },
            {
                "location": "New York, NY"  # Demonstrates optional 'unit' parameter
            }
        ]
    }
]
```

---

## Custom Tools

**URL:** llms-txt#custom-tools

**Contents:**
- Creating Custom Tools

Build and integrate custom tools to extend Claude Agent SDK functionality

Custom tools allow you to extend Claude Code's capabilities with your own functionality through in-process MCP servers, enabling Claude to interact with external services, APIs, or perform specialized operations.

## Creating Custom Tools

Use the `createSdkMcpServer` and `tool` helper functions to define type-safe custom tools:

```python Python
from claude_agent_sdk import tool, create_sdk_mcp_server, ClaudeSDKClient, ClaudeAgentOptions
from typing import Any
import aiohttp

**Examples:**

Example 1 (unknown):
```unknown

```

---

## Using Agent Skills with the API

**URL:** llms-txt#using-agent-skills-with-the-api

**Contents:**
- Quick Links
- Overview
  - Using Skills
  - Prerequisites
- Using Skills in Messages
  - Container Parameter
  - Downloading Generated Files

Learn how to use Agent Skills to extend Claude's capabilities through the API.

Agent Skills extend Claude's capabilities through organized folders of instructions, scripts, and resources. This guide shows you how to use both pre-built and custom Skills with the Claude API.

<Note>
For complete API reference including request/response schemas and all parameters, see:
- [Skill Management API Reference](/docs/en/api/skills/list-skills) - CRUD operations for Skills
- [Skill Versions API Reference](/docs/en/api/skills/list-skill-versions) - Version management
</Note>

<CardGroup cols={2}>
  <Card
    title="Get started with Agent Skills"
    icon="rocket"
    href="/docs/en/agents-and-tools/agent-skills/quickstart"
  >
    Create your first Skill
  </Card>
  <Card
    title="Create Custom Skills"
    icon="hammer"
    href="/docs/en/agents-and-tools/agent-skills/best-practices"
  >
    Best practices for authoring Skills
  </Card>
</CardGroup>

<Note>
For a deep dive into the architecture and real-world applications of Agent Skills, read our engineering blog: [Equipping agents for the real world with Agent Skills](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills).
</Note>

Skills integrate with the Messages API through the code execution tool. Whether using pre-built Skills managed by Anthropic or custom Skills you've uploaded, the integration shape is identical—both require code execution and use the same `container` structure.

Skills integrate identically in the Messages API regardless of source. You specify Skills in the `container` parameter with a `skill_id`, `type`, and optional `version`, and they execute in the code execution environment.

**You can use Skills from two sources:**

| Aspect | Anthropic Skills | Custom Skills |
|--------|------------------|---------------|
| **Type value** | `anthropic` | `custom` |
| **Skill IDs** | Short names: `pptx`, `xlsx`, `docx`, `pdf` | Generated: `skill_01AbCdEfGhIjKlMnOpQrStUv` |
| **Version format** | Date-based: `20251013` or `latest` | Epoch timestamp: `1759178010641129` or `latest` |
| **Management** | Pre-built and maintained by Anthropic | Upload and manage via [Skills API](/docs/en/api/skills/create-skill) |
| **Availability** | Available to all users | Private to your workspace |

Both skill sources are returned by the [List Skills endpoint](/docs/en/api/skills/list-skills) (use the `source` parameter to filter). The integration shape and execution environment are identical—the only difference is where the Skills come from and how they're managed.

To use Skills, you need:

1. **Anthropic API key** from the [Console](/settings/keys)
2. **Beta headers**:
   - `code-execution-2025-08-25` - Enables code execution (required for Skills)
   - `skills-2025-10-02` - Enables Skills API
   - `files-api-2025-04-14` - For uploading/downloading files to/from container
3. **Code execution tool** enabled in your requests

## Using Skills in Messages

### Container Parameter

Skills are specified using the `container` parameter in the Messages API. You can include up to 8 Skills per request.

The structure is identical for both Anthropic and custom Skills—specify the required `type` and `skill_id`, and optionally include `version` to pin to a specific version:

### Downloading Generated Files

When Skills create documents (Excel, PowerPoint, PDF, Word), they return `file_id` attributes in the response. You must use the Files API to download these files.

**How it works:**
1. Skills create files during code execution
2. Response includes `file_id` for each created file
3. Use Files API to download the actual file content
4. Save locally or process as needed

**Example: Creating and downloading an Excel file**

<CodeGroup>
```python Python
import anthropic

client = anthropic.Anthropic()

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

### Downloading Generated Files

When Skills create documents (Excel, PowerPoint, PDF, Word), they return `file_id` attributes in the response. You must use the Files API to download these files.

**How it works:**
1. Skills create files during code execution
2. Response includes `file_id` for each created file
3. Use Files API to download the actual file content
4. Save locally or process as needed

**Example: Creating and downloading an Excel file**

<CodeGroup>
```

---

## Subagents in the SDK

**URL:** llms-txt#subagents-in-the-sdk

**Contents:**
- Overview
- Benefits of using subagents
  - Context management
  - Parallelization
  - Specialized instructions and knowledge
  - Tool restrictions
- Creating subagents
  - Programmatic definition (recommended)
  - AgentDefinition configuration
  - Filesystem-based definition (alternative)

Define and invoke subagents to isolate context, run tasks in parallel, and apply specialized instructions in your Claude Agent SDK applications.

Subagents are separate agent instances that your main agent can spawn to handle focused subtasks.
Use subagents to isolate context for focused subtasks, run multiple analyses in parallel, and apply specialized instructions without bloating the main agent's prompt.

This guide explains how to define and use subagents in the SDK using the `agents` parameter.

You can create subagents in three ways:

- **Programmatically**: use the `agents` parameter in your `query()` options ([TypeScript](/docs/en/agent-sdk/typescript#agentdefinition), [Python](/docs/en/agent-sdk/python#agentdefinition))
- **Filesystem-based**: define agents as markdown files in `.claude/agents/` directories (see the [Claude Code documentation](https://code.claude.com/docs/en/sub-agents))
- **Built-in general-purpose**: Claude can invoke the built-in `general-purpose` subagent at any time via the Task tool without you defining anything

This guide focuses on the programmatic approach, which is recommended for SDK applications.

When you define subagents, Claude decides whether to invoke them based on each subagent's `description` field. Write clear descriptions that explain when the subagent should be used, and Claude will automatically delegate appropriate tasks. You can also explicitly request a subagent by name in your prompt (e.g., "Use the code-reviewer agent to...").

## Benefits of using subagents

### Context management
Subagents maintain separate context from the main agent, preventing information overload and keeping interactions focused. This isolation ensures that specialized tasks don't pollute the main conversation context with irrelevant details.

**Example**: a `research-assistant` subagent can explore dozens of files and documentation pages without cluttering the main conversation with all the intermediate search results, returning only the relevant findings.

### Parallelization
Multiple subagents can run concurrently, dramatically speeding up complex workflows.

**Example**: during a code review, you can run `style-checker`, `security-scanner`, and `test-coverage` subagents simultaneously, reducing review time from minutes to seconds.

### Specialized instructions and knowledge
Each subagent can have tailored system prompts with specific expertise, best practices, and constraints.

**Example**: a `database-migration` subagent can have detailed knowledge about SQL best practices, rollback strategies, and data integrity checks that would be unnecessary noise in the main agent's instructions.

### Tool restrictions
Subagents can be limited to specific tools, reducing the risk of unintended actions.

**Example**: a `doc-reviewer` subagent might only have access to Read and Grep tools, ensuring it can analyze but never accidentally modify your documentation files.

## Creating subagents

### Programmatic definition (recommended)

Define subagents directly in your code using the `agents` parameter. This example creates two subagents: a code reviewer with read-only access and a test runner that can execute commands. The `Task` tool must be included in `allowedTools` since Claude invokes subagents through the Task tool.

### AgentDefinition configuration

| Field | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `description` | `string` | Yes | Natural language description of when to use this agent |
| `prompt` | `string` | Yes | The agent's system prompt defining its role and behavior |
| `tools` | `string[]` | No | Array of allowed tool names. If omitted, inherits all tools |
| `model` | `'sonnet' \| 'opus' \| 'haiku' \| 'inherit'` | No | Model override for this agent. Defaults to main model if omitted |

<Note>
Subagents cannot spawn their own subagents. Don't include `Task` in a subagent's `tools` array.
</Note>

### Filesystem-based definition (alternative)

You can also define subagents as markdown files in `.claude/agents/` directories. See the [Claude Code subagents documentation](https://code.claude.com/docs/en/sub-agents) for details on this approach. Programmatically defined agents take precedence over filesystem-based agents with the same name.

<Note>
Even without defining custom subagents, Claude can spawn the built-in `general-purpose` subagent when `Task` is in your `allowedTools`. This is useful for delegating research or exploration tasks without creating specialized agents.
</Note>

## Invoking subagents

### Automatic invocation

Claude automatically decides when to invoke subagents based on the task and each subagent's `description`. For example, if you define a `performance-optimizer` subagent with the description "Performance optimization specialist for query tuning", Claude will invoke it when your prompt mentions optimizing queries.

Write clear, specific descriptions so Claude can match tasks to the right subagent.

### Explicit invocation

To guarantee Claude uses a specific subagent, mention it by name in your prompt:

This bypasses automatic matching and directly invokes the named subagent.

### Dynamic agent configuration

You can create agent definitions dynamically based on runtime conditions. This example creates a security reviewer with different strictness levels, using a more powerful model for strict reviews.

<CodeGroup>
```python Python
import asyncio
from claude_agent_sdk import query, ClaudeAgentOptions, AgentDefinition

**Examples:**

Example 1 (unknown):
```unknown

```

Example 2 (unknown):
```unknown
</CodeGroup>

### AgentDefinition configuration

| Field | Type | Required | Description |
|:------|:-----|:---------|:------------|
| `description` | `string` | Yes | Natural language description of when to use this agent |
| `prompt` | `string` | Yes | The agent's system prompt defining its role and behavior |
| `tools` | `string[]` | No | Array of allowed tool names. If omitted, inherits all tools |
| `model` | `'sonnet' \| 'opus' \| 'haiku' \| 'inherit'` | No | Model override for this agent. Defaults to main model if omitted |

<Note>
Subagents cannot spawn their own subagents. Don't include `Task` in a subagent's `tools` array.
</Note>

### Filesystem-based definition (alternative)

You can also define subagents as markdown files in `.claude/agents/` directories. See the [Claude Code subagents documentation](https://code.claude.com/docs/en/sub-agents) for details on this approach. Programmatically defined agents take precedence over filesystem-based agents with the same name.

<Note>
Even without defining custom subagents, Claude can spawn the built-in `general-purpose` subagent when `Task` is in your `allowedTools`. This is useful for delegating research or exploration tasks without creating specialized agents.
</Note>

## Invoking subagents

### Automatic invocation

Claude automatically decides when to invoke subagents based on the task and each subagent's `description`. For example, if you define a `performance-optimizer` subagent with the description "Performance optimization specialist for query tuning", Claude will invoke it when your prompt mentions optimizing queries.

Write clear, specific descriptions so Claude can match tasks to the right subagent.

### Explicit invocation

To guarantee Claude uses a specific subagent, mention it by name in your prompt:
```

Example 3 (unknown):
```unknown
This bypasses automatic matching and directly invokes the named subagent.

### Dynamic agent configuration

You can create agent definitions dynamically based on runtime conditions. This example creates a security reviewer with different strictness levels, using a more powerful model for strict reviews.

<CodeGroup>
```

---

## Agent SDK reference - Python

**URL:** llms-txt#agent-sdk-reference---python

**Contents:**
- Installation
- Choosing Between `query()` and `ClaudeSDKClient`
  - Quick Comparison
  - When to Use `query()` (New Session Each Time)
  - When to Use `ClaudeSDKClient` (Continuous Conversation)
- Functions
  - `query()`
  - `tool()`
  - `create_sdk_mcp_server()`

Complete API reference for the Python Agent SDK, including all functions, types, and classes.

## Choosing Between `query()` and `ClaudeSDKClient`

The Python SDK provides two ways to interact with Claude Code:

| Feature             | `query()`                     | `ClaudeSDKClient`                  |
| :------------------ | :---------------------------- | :--------------------------------- |
| **Session**         | Creates new session each time | Reuses same session                |
| **Conversation**    | Single exchange               | Multiple exchanges in same context |
| **Connection**      | Managed automatically         | Manual control                     |
| **Streaming Input** | ✅ Supported                  | ✅ Supported                       |
| **Interrupts**      | ❌ Not supported              | ✅ Supported                       |
| **Hooks**           | ❌ Not supported              | ✅ Supported                       |
| **Custom Tools**    | ❌ Not supported              | ✅ Supported                       |
| **Continue Chat**   | ❌ New session each time      | ✅ Maintains conversation          |
| **Use Case**        | One-off tasks                 | Continuous conversations           |

### When to Use `query()` (New Session Each Time)

- One-off questions where you don't need conversation history
- Independent tasks that don't require context from previous exchanges
- Simple automation scripts
- When you want a fresh start each time

### When to Use `ClaudeSDKClient` (Continuous Conversation)

- **Continuing conversations** - When you need Claude to remember context
- **Follow-up questions** - Building on previous responses
- **Interactive applications** - Chat interfaces, REPLs
- **Response-driven logic** - When next action depends on Claude's response
- **Session control** - Managing conversation lifecycle explicitly

Creates a new session for each interaction with Claude Code. Returns an async iterator that yields messages as they arrive. Each call to `query()` starts fresh with no memory of previous interactions.

| Parameter | Type                         | Description                                                                |
| :-------- | :--------------------------- | :------------------------------------------------------------------------- |
| `prompt`  | `str \| AsyncIterable[dict]` | The input prompt as a string or async iterable for streaming mode          |
| `options` | `ClaudeAgentOptions \| None` | Optional configuration object (defaults to `ClaudeAgentOptions()` if None) |

Returns an `AsyncIterator[Message]` that yields messages from the conversation.

#### Example - With options

Decorator for defining MCP tools with type safety.

| Parameter      | Type                     | Description                                             |
| :------------- | :----------------------- | :------------------------------------------------------ |
| `name`         | `str`                    | Unique identifier for the tool                          |
| `description`  | `str`                    | Human-readable description of what the tool does        |
| `input_schema` | `type \| dict[str, Any]` | Schema defining the tool's input parameters (see below) |

#### Input Schema Options

1. **Simple type mapping** (recommended):

2. **JSON Schema format** (for complex validation):

A decorator function that wraps the tool implementation and returns an `SdkMcpTool` instance.

### `create_sdk_mcp_server()`

Create an in-process MCP server that runs within your Python application.

| Parameter | Type                            | Default   | Description                                           |
| :-------- | :------------------------------ | :-------- | :---------------------------------------------------- |
| `name`    | `str`                           | -         | Unique identifier for the server                      |
| `version` | `str`                           | `"1.0.0"` | Server version string                                 |
| `tools`   | `list[SdkMcpTool[Any]] \| None` | `None`    | List of tool functions created with `@tool` decorator |

Returns an `McpSdkServerConfig` object that can be passed to `ClaudeAgentOptions.mcp_servers`.

```python
from claude_agent_sdk import tool, create_sdk_mcp_server

@tool("add", "Add two numbers", {"a": float, "b": float})
async def add(args):
    return {
        "content": [{
            "type": "text",
            "text": f"Sum: {args['a'] + args['b']}"
        }]
    }

@tool("multiply", "Multiply two numbers", {"a": float, "b": float})
async def multiply(args):
    return {
        "content": [{
            "type": "text",
            "text": f"Product: {args['a'] * args['b']}"
        }]
    }

calculator = create_sdk_mcp_server(
    name="calculator",
    version="2.0.0",
    tools=[add, multiply]  # Pass decorated functions
)

**Examples:**

Example 1 (bash):
```bash
pip install claude-agent-sdk
```

Example 2 (python):
```python
async def query(
    *,
    prompt: str | AsyncIterable[dict[str, Any]],
    options: ClaudeAgentOptions | None = None
) -> AsyncIterator[Message]
```

Example 3 (python):
```python
import asyncio
from claude_agent_sdk import query, ClaudeAgentOptions

async def main():
    options = ClaudeAgentOptions(
        system_prompt="You are an expert Python developer",
        permission_mode='acceptEdits',
        cwd="/home/user/project"
    )

    async for message in query(
        prompt="Create a Python web server",
        options=options
    ):
        print(message)


asyncio.run(main())
```

Example 4 (python):
```python
def tool(
    name: str,
    description: str,
    input_schema: type | dict[str, Any]
) -> Callable[[Callable[[Any], Awaitable[dict[str, Any]]]], SdkMcpTool[Any]]
```

---

## Intro to Claude

**URL:** llms-txt#intro-to-claude

**Contents:**
- Get started
- Develop with Claude
- Key capabilities
- Support
  - Models & pricing

Claude is a highly performant, trustworthy, and intelligent AI platform built by Anthropic. Claude excels at tasks involving language, reasoning, analysis, coding, and more.

The latest generation of Claude models:

**Claude Opus 4.5** - Most intelligent model, and an industry-leader for coding,
agents, and computer use. [Learn more](https://www.anthropic.com/news/claude-opus-4-5).

**Claude Sonnet 4.5** - Balanced performance and practicality for most uses,
including coding and agents. [Learn
more](https://www.anthropic.com/news/claude-sonnet-4-5).

**Claude Haiku 4.5** - Fastest model with near-frontier intelligence. [Learn more](https://www.anthropic.com/news/claude-haiku-4-5).

<Note>
Looking to chat with Claude? Visit [claude.ai](http://www.claude.ai)!
</Note>

If you’re new to Claude, start here to learn the essentials and make your first API call.

<CardGroup cols={3}>
  <Card title="Get started" icon="check" href="/docs/en/get-started">
    Set up your development environment for building with Claude.
  </Card>
  <Card title="Learn about Claude" icon="settings" href="/docs/en/about-claude/models/overview">
    Learn about the family of Claude models.
  </Card>
  <Card title="Prompt Library" icon="books" href="/docs/en/resources/prompt-library/library">
    Explore example prompts for inspiration.
  </Card>
</CardGroup>

## Develop with Claude

Anthropic has best-in-class developer tools to build scalable applications with Claude.

<CardGroup cols={3}>
  <Card title="Developer Console" icon="computer" href="/">
    Enjoy easier, more powerful prompting in your browser with the Workbench and the prompt generator tool. 
  </Card>
  <Card title="API Reference" icon="code" href="/docs/en/api/overview">
    Explore, implement, and scale with the Claude API and SDKs. 
  </Card>
  <Card title="Claude Cookbook" icon="chef-hat" href="https://platform.claude.com/cookbooks">
    Learn with interactive Jupyter notebooks that demonstrate uploading PDFs, embeddings, and more.
  </Card>
</CardGroup>

Claude can assist with many tasks that involve text, code, and images.

<CardGroup cols={2}>
  <Card title="Text and code generation" icon="text-aa" href="/docs/en/build-with-claude/text-generation">
    Summarize text, answer questions, extract data, translate text, and explain and generate code. 
  </Card>
  <Card title="Vision" icon="image" href="/docs/en/build-with-claude/vision">
    Process and analyze visual input and generate text and code from images. 
  </Card>
</CardGroup>

<CardGroup cols={2}>
  <Card title="Help Center" icon="help" href="https://support.claude.com/en/">
    Find answers to frequently asked account and billing questions.
  </Card>

<Card title="Service Status" icon="chart" href="https://status.claude.com">
    Check the status of Anthropic services.
  </Card>
</CardGroup>

---

## Get started with Agent Skills in the API

**URL:** llms-txt#get-started-with-agent-skills-in-the-api

**Contents:**
- Prerequisites
- What are Agent Skills?
- Step 1: List available Skills

Learn how to use Agent Skills to create documents with the Claude API in under 10 minutes.

This tutorial shows you how to use Agent Skills to create a PowerPoint presentation. You'll learn how to enable Skills, make a simple request, and access the generated file.

- [Anthropic API key](/settings/keys)
- Python 3.7+ or curl installed
- Basic familiarity with making API requests

## What are Agent Skills?

Pre-built Agent Skills extend Claude's capabilities with specialized expertise for tasks like creating documents, analyzing data, and processing files. Anthropic provides the following pre-built Agent Skills in the API:

- **PowerPoint (pptx)**: Create and edit presentations
- **Excel (xlsx)**: Create and analyze spreadsheets
- **Word (docx)**: Create and edit documents
- **PDF (pdf)**: Generate PDF documents

<Note>
**Want to create custom Skills?** See the [Agent Skills Cookbook](https://platform.claude.com/cookbook/skills-notebooks-01-skills-introduction) for examples of building your own Skills with domain-specific expertise.
</Note>

## Step 1: List available Skills

First, let's see what Skills are available. We'll use the Skills API to list all Anthropic-managed Skills:

<CodeGroup>
```python Python
import anthropic

client = anthropic.Anthropic()

---

## Agent Skills

**URL:** llms-txt#agent-skills

**Contents:**
- Why use Skills
- Using Skills
- How Skills work
  - Three types of Skill content, three levels of loading
  - Level 1: Metadata (always loaded)
  - Level 2: Instructions (loaded when triggered)

Agent Skills are modular capabilities that extend Claude's functionality. Each Skill packages instructions, metadata, and optional resources (scripts, templates) that Claude uses automatically when relevant.

Skills are reusable, filesystem-based resources that provide Claude with domain-specific expertise: workflows, context, and best practices that transform general-purpose agents into specialists. Unlike prompts (conversation-level instructions for one-off tasks), Skills load on-demand and eliminate the need to repeatedly provide the same guidance across multiple conversations.

**Key benefits**:
- **Specialize Claude**: Tailor capabilities for domain-specific tasks
- **Reduce repetition**: Create once, use automatically
- **Compose capabilities**: Combine Skills to build complex workflows

<Note>
For a deep dive into the architecture and real-world applications of Agent Skills, read our engineering blog: [Equipping agents for the real world with Agent Skills](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills).
</Note>

Anthropic provides pre-built Agent Skills for common document tasks (PowerPoint, Excel, Word, PDF), and you can create your own custom Skills. Both work the same way. Claude automatically uses them when relevant to your request.

**Pre-built Agent Skills** are available to all users on claude.ai and via the Claude API. See the [Available Skills](#available-skills) section below for the complete list.

**Custom Skills** let you package domain expertise and organizational knowledge. They're available across Claude's products: create them in Claude Code, upload them via the API, or add them in claude.ai settings.

<Note>
**Get started:**
- For pre-built Agent Skills: See the [quickstart tutorial](/docs/en/agents-and-tools/agent-skills/quickstart) to start using PowerPoint, Excel, Word, and PDF skills in the API
- For custom Skills: See the [Agent Skills Cookbook](https://platform.claude.com/cookbook/skills-notebooks-01-skills-introduction) to learn how to create your own Skills
</Note>

Skills leverage Claude's VM environment to provide capabilities beyond what's possible with prompts alone. Claude operates in a virtual machine with filesystem access, allowing Skills to exist as directories containing instructions, executable code, and reference materials, organized like an onboarding guide you'd create for a new team member.

This filesystem-based architecture enables **progressive disclosure**: Claude loads information in stages as needed, rather than consuming context upfront.

### Three types of Skill content, three levels of loading

Skills can contain three types of content, each loaded at different times:

### Level 1: Metadata (always loaded)

**Content type: Instructions**. The Skill's YAML frontmatter provides discovery information:

Claude loads this metadata at startup and includes it in the system prompt. This lightweight approach means you can install many Skills without context penalty; Claude only knows each Skill exists and when to use it.

### Level 2: Instructions (loaded when triggered)

**Content type: Instructions**. The main body of SKILL.md contains procedural knowledge: workflows, best practices, and guidance:

**Examples:**

Example 1 (yaml):
```yaml
---
name: pdf-processing
description: Extract text and tables from PDF files, fill forms, merge documents. Use when working with PDF files or when the user mentions PDFs, forms, or document extraction.
---
```

---

## After (Claude Opus 4.5)

**URL:** llms-txt#after-(claude-opus-4.5)

**Contents:**
  - Migrating between Claude 4.5 models
- Need help?

model="claude-opus-4-5-20251101"
```

Claude Opus 4.5 is our most intelligent model, combining maximum capability with practical performance. It features step-change improvements in vision, coding, and computer use at a more accessible price point than Opus 4.1. Ideal for complex specialized tasks and professional software engineering.

<Note>
For codebases with many model references, a [Claude Code plugin](https://github.com/anthropics/claude-code/tree/main/plugins/claude-opus-4-5-migration) is available to automate migration to Opus 4.5.
</Note>

### Migrating between Claude 4.5 models

**No breaking changes.** All API calls will work without modification.

Simply update your model ID.

- Check our [API documentation](/docs/en/api/overview) for detailed specifications
- Review [model capabilities](/docs/en/about-claude/models/overview) for performance comparisons
- Review [API release notes](/docs/en/release-notes/api) for API updates
- Contact support if you encounter any issues during migration

---

## Migrate to Claude Agent SDK

**URL:** llms-txt#migrate-to-claude-agent-sdk

**Contents:**
- Overview
- What's Changed
- Migration Steps
  - For TypeScript/JavaScript Projects
  - For Python Projects

Guide for migrating the Claude Code TypeScript and Python SDKs to the Claude Agent SDK

The Claude Code SDK has been renamed to the **Claude Agent SDK** and its documentation has been reorganized. This change reflects the SDK's broader capabilities for building AI agents beyond just coding tasks.

| Aspect                   | Old                         | New                              |
| :----------------------- | :-------------------------- | :------------------------------- |
| **Package Name (TS/JS)** | `@anthropic-ai/claude-code` | `@anthropic-ai/claude-agent-sdk` |
| **Python Package**       | `claude-code-sdk`           | `claude-agent-sdk`               |
| **Documentation Location** | Claude Code docs | API Guide → Agent SDK section |

<Note>
**Documentation Changes:** The Agent SDK documentation has moved from the Claude Code docs to the API Guide under a dedicated [Agent SDK](/docs/en/agent-sdk/overview) section. The Claude Code docs now focus on the CLI tool and automation features.
</Note>

### For TypeScript/JavaScript Projects

**1. Uninstall the old package:**

**2. Install the new package:**

**3. Update your imports:**

Change all imports from `@anthropic-ai/claude-code` to `@anthropic-ai/claude-agent-sdk`:

**4. Update package.json dependencies:**

If you have the package listed in your `package.json`, update it:

That's it! No other code changes are required.

### For Python Projects

**1. Uninstall the old package:**

**2. Install the new package:**

**3. Update your imports:**

Change all imports from `claude_code_sdk` to `claude_agent_sdk`:

**Examples:**

Example 1 (bash):
```bash
npm uninstall @anthropic-ai/claude-code
```

Example 2 (bash):
```bash
npm install @anthropic-ai/claude-agent-sdk
```

Example 3 (typescript):
```typescript
// Before
import { query, tool, createSdkMcpServer } from "@anthropic-ai/claude-code";

// After
import {
  query,
  tool,
  createSdkMcpServer,
} from "@anthropic-ai/claude-agent-sdk";
```

Example 4 (json):
```json
// Before
{
  "dependencies": {
    "@anthropic-ai/claude-code": "^1.0.0"
  }
}

// After
{
  "dependencies": {
    "@anthropic-ai/claude-agent-sdk": "^0.1.0"
  }
}
```

---

## Securely deploying AI agents

**URL:** llms-txt#securely-deploying-ai-agents

**Contents:**
- What are we protecting against?
- Built-in security features
- Security principles
  - Security boundaries
  - Least privilege
  - Defense in depth
- Isolation technologies
  - Sandbox runtime
  - Containers
  - gVisor

A guide to securing Claude Code and Agent SDK deployments with isolation, credential management, and network controls

Claude Code and the Agent SDK are powerful tools that can execute code, access files, and interact with external services on your behalf. Like any tool with these capabilities, deploying them thoughtfully ensures you get the benefits while maintaining appropriate controls.

Unlike traditional software that follows predetermined code paths, these tools generate their actions dynamically based on context and goals. This flexibility is what makes them useful, but it also means their behavior can be influenced by the content they process: files, webpages, or user input. This is sometimes called prompt injection. For example, if a repository's README contains unusual instructions, Claude Code might incorporate those into its actions in ways the operator didn't anticipate. This guide covers practical ways to reduce this risk.

The good news is that securing an agent deployment doesn't require exotic infrastructure. The same principles that apply to running any semi-trusted code apply here: isolation, least privilege, and defense in depth. Claude Code includes several security features that help with common concerns, and this guide walks through these along with additional hardening options for those who need them.

Not every deployment needs maximum security. A developer running Claude Code on their laptop has different requirements than a company processing customer data in a multi-tenant environment. This guide presents options ranging from Claude Code's built-in security features to hardened production architectures, so you can choose what fits your situation.

## What are we protecting against?

Agents can take unintended actions due to prompt injection (instructions embedded in content they process) or model error. Claude models are designed to resist this, and as we analyzed in our [model card](https://assets.anthropic.com/m/64823ba7485345a7/Claude-Opus-4-5-System-Card.pdf), we believe Claude Opus 4.5 is the most robust frontier model available.

Defense in depth is still good practice though. For example, if an agent processes a malicious file that instructs it to send customer data to an external server, network controls can block that request entirely.

## Built-in security features

Claude Code includes several security features that address common concerns. See the [security documentation](https://code.claude.com/docs/en/security) for full details.

- **Permissions system**: Every tool and bash command can be configured to allow, block, or prompt the user for approval. Use glob patterns to create rules like "allow all npm commands" or "block any command with sudo". Organizations can set policies that apply across all users. See [access control and permissions](https://code.claude.com/docs/en/iam#access-control-and-permissions).
- **Static analysis**: Before executing bash commands, Claude Code runs static analysis to identify potentially risky operations. Commands that modify system files or access sensitive directories are flagged and require explicit user approval.
- **Web search summarization**: Search results are summarized rather than passing raw content directly into the context, reducing the risk of prompt injection from malicious web content.
- **Sandbox mode**: Bash commands can run in a sandboxed environment that restricts filesystem and network access. See the [sandboxing documentation](https://code.claude.com/docs/en/sandboxing) for details.

## Security principles

For deployments that require additional hardening beyond Claude Code's defaults, these principles guide the available options.

### Security boundaries

A security boundary separates components with different trust levels. For high-security deployments, you can place sensitive resources (like credentials) outside the boundary containing the agent. If something goes wrong in the agent's environment, resources outside that boundary remain protected.

For example, rather than giving an agent direct access to an API key, you could run a proxy outside the agent's environment that injects the key into requests. The agent can make API calls, but it never sees the credential itself. This pattern is useful for multi-tenant deployments or when processing untrusted content.

When needed, you can restrict the agent to only the capabilities required for its specific task:

| Resource | Restriction options |
|----------|---------------------|
| Filesystem | Mount only needed directories, prefer read-only |
| Network | Restrict to specific endpoints via proxy |
| Credentials | Inject via proxy rather than exposing directly |
| System capabilities | Drop Linux capabilities in containers |

For high-security environments, layering multiple controls provides additional protection. Options include:

- Container isolation
- Network restrictions
- Filesystem controls
- Request validation at a proxy

The right combination depends on your threat model and operational requirements.

## Isolation technologies

Different isolation technologies offer different tradeoffs between security strength, performance, and operational complexity.

<Info>
In all of these configurations, Claude Code (or your Agent SDK application) runs inside the isolation boundary—the sandbox, container, or VM. The security controls described below restrict what the agent can access from within that boundary.
</Info>

| Technology | Isolation strength | Performance overhead | Complexity |
|------------|-------------------|---------------------|------------|
| Sandbox runtime | Good (secure defaults) | Very low | Low |
| Containers (Docker) | Setup dependent | Low | Medium |
| gVisor | Excellent (with correct setup) | Medium/High | Medium |
| VMs (Firecracker, QEMU) | Excellent (with correct setup) | High | Medium/High |

For lightweight isolation without containers, [sandbox-runtime](https://github.com/anthropic-experimental/sandbox-runtime) enforces filesystem and network restrictions at the OS level.

The main advantage is simplicity: no Docker configuration, container images, or networking setup required. The proxy and filesystem restrictions are built in. You provide a settings file specifying allowed domains and paths.

**How it works:**
- **Filesystem**: Uses OS primitives (`bubblewrap` on Linux, `sandbox-exec` on macOS) to restrict read/write access to configured paths
- **Network**: Removes network namespace (Linux) or uses Seatbelt profiles (macOS) to route network traffic through a built-in proxy
- **Configuration**: JSON-based allowlists for domains and filesystem paths

Then create a configuration file specifying allowed paths and domains.

**Security considerations:**

1. **Same-host kernel**: Unlike VMs, sandboxed processes share the host kernel. A kernel vulnerability could theoretically enable escape. For some threat models this is acceptable, but if you need kernel-level isolation, use gVisor or a separate VM.

2. **No TLS inspection**: The proxy allowlists domains but doesn't inspect encrypted traffic. If the agent has permissive credentials for an allowed domain, ensure it isn't possible to use that domain to trigger other network requests or to exfiltrate data.

For many single-developer and CI/CD use cases, sandbox-runtime raises the bar significantly with minimal setup. The sections below cover containers and VMs for deployments requiring stronger isolation.

Containers provide isolation through Linux namespaces. Each container has its own view of the filesystem, process tree, and network stack, while sharing the host kernel.

A security-hardened container configuration might look like this:

Here's what each option does:

| Option | Purpose |
|--------|---------|
| `--cap-drop ALL` | Removes Linux capabilities like `NET_ADMIN` and `SYS_ADMIN` that could enable privilege escalation |
| `--security-opt no-new-privileges` | Prevents processes from gaining privileges through setuid binaries |
| `--security-opt seccomp=...` | Restricts available syscalls; Docker's default blocks ~44, custom profiles can block more |
| `--read-only` | Makes the container's root filesystem immutable, preventing the agent from persisting changes |
| `--tmpfs /tmp:...` | Provides a writable temporary directory that's cleared when the container stops |
| `--network none` | Removes all network interfaces; the agent communicates through the mounted Unix socket below |
| `--memory 2g` | Limits memory usage to prevent resource exhaustion |
| `--pids-limit 100` | Limits process count to prevent fork bombs |
| `--user 1000:1000` | Runs as a non-root user |
| `-v ...:/workspace:ro` | Mounts code read-only so the agent can analyze but not modify it. **Avoid mounting sensitive host directories like `~/.ssh`, `~/.aws`, or `~/.config`** |
| `-v .../proxy.sock:...` | Mounts a Unix socket connected to a proxy running outside the container (see below) |

**Unix socket architecture:**

With `--network none`, the container has no network interfaces at all. The only way for the agent to reach the outside world is through the mounted Unix socket, which connects to a proxy running on the host. This proxy can enforce domain allowlists, inject credentials, and log all traffic.

This is the same architecture used by [sandbox-runtime](https://github.com/anthropic-experimental/sandbox-runtime). Even if the agent is compromised via prompt injection, it cannot exfiltrate data to arbitrary servers—it can only communicate through the proxy, which controls what domains are reachable. For more details, see the [Claude Code sandboxing blog post](https://www.anthropic.com/engineering/claude-code-sandboxing).

**Additional hardening options:**

| Option | Purpose |
|--------|---------|
| `--userns-remap` | Maps container root to unprivileged host user; requires daemon configuration but limits damage from container escape |
| `--ipc private` | Isolates inter-process communication to prevent cross-container attacks |

Standard containers share the host kernel: when code inside a container makes a system call, it goes directly to the same kernel that runs the host. This means a kernel vulnerability could allow container escape. gVisor addresses this by intercepting system calls in userspace before they reach the host kernel, implementing its own compatibility layer that handles most syscalls without involving the real kernel.

If an agent runs malicious code (perhaps due to prompt injection), that code runs in the container and could attempt kernel exploits. With gVisor, the attack surface is much smaller: the malicious code would need to exploit gVisor's userspace implementation first and would have limited access to the real kernel.

To use gVisor with Docker, install the `runsc` runtime and configure the daemon:

Then run containers with:

**Performance considerations:**

| Workload | Overhead |
|----------|----------|
| CPU-bound computation | ~0% (no syscall interception) |
| Simple syscalls | ~2× slower |
| File I/O intensive | Up to 10-200× slower for heavy open/close patterns |

For multi-tenant environments or when processing untrusted content, the additional isolation is often worth the overhead.

VMs provide hardware-level isolation through CPU virtualization extensions. Each VM runs its own kernel, creating a strong boundary—a vulnerability in the guest kernel doesn't directly compromise the host. However, VMs aren't automatically "more secure" than alternatives like gVisor. VM security depends heavily on the hypervisor and device emulation code.

Firecracker is designed for lightweight microVM isolation—it can boot VMs in under 125ms with less than 5 MiB memory overhead, stripping away unnecessary device emulation to reduce attack surface.

With this approach, the agent VM has no external network interface. Instead, it communicates through `vsock` (virtual sockets). All traffic routes through vsock to a proxy on the host, which enforces allowlists and injects credentials before forwarding requests.

### Cloud deployments

For cloud deployments, you can combine any of the above isolation technologies with cloud-native network controls:

1. Run agent containers in a private subnet with no internet gateway
2. Configure cloud firewall rules (AWS Security Groups, GCP VPC firewall) to block all egress except to your proxy
3. Run a proxy (such as [Envoy](https://www.envoyproxy.io/) with its `credential_injector` filter) that validates requests, enforces domain allowlists, injects credentials, and forwards to external APIs
4. Assign minimal IAM permissions to the agent's service account, routing sensitive access through the proxy where possible
5. Log all traffic at the proxy for audit purposes

## Credential management

Agents often need credentials to call APIs, access repositories, or interact with cloud services. The challenge is providing this access without exposing the credentials themselves.

### The proxy pattern

The recommended approach is to run a proxy outside the agent's security boundary that injects credentials into outgoing requests. The agent sends requests without credentials, the proxy adds them, and forwards the request to its destination.

This pattern has several benefits:

1. The agent never sees the actual credentials
2. The proxy can enforce an allowlist of permitted endpoints
3. The proxy can log all requests for auditing
4. Credentials are stored in one secure location rather than distributed to each agent

### Configuring Claude Code to use a proxy

Claude Code supports two methods for routing sampling requests through a proxy:

**Option 1: ANTHROPIC_BASE_URL (simple but only for sampling API requests)**

This tells Claude Code and the Agent SDK to send sampling requests to your proxy instead of the Anthropic API directly. Your proxy receives plaintext HTTP requests, can inspect and modify them (including injecting credentials), then forwards to the real API.

**Option 2: HTTP_PROXY / HTTPS_PROXY (system-wide)**

Claude Code and the Agent SDK respect these standard environment variables, routing all HTTP traffic through the proxy. For HTTPS, the proxy creates an encrypted CONNECT tunnel: it cannot see or modify request contents without TLS interception.

### Implementing a proxy

You can build your own proxy or use an existing one:

- [Envoy Proxy](https://www.envoyproxy.io/) — production-grade proxy with `credential_injector` filter for adding auth headers
- [mitmproxy](https://mitmproxy.org/) — TLS-terminating proxy for inspecting and modifying HTTPS traffic
- [Squid](http://www.squid-cache.org/) — caching proxy with access control lists
- [LiteLLM](https://github.com/BerriAI/litellm) — LLM gateway with credential injection and rate limiting

### Credentials for other services

Beyond sampling from the Anthropic API, agents often need authenticated access to other services—git repositories, databases, internal APIs. There are two main approaches:

Provide access through an MCP server or custom tool that routes requests to a service running outside the agent's security boundary. The agent calls the tool, but the actual authenticated request happens outside—the tool calls to a proxy which injects the credentials.

For example, a git MCP server could accept commands from the agent but forward them to a git proxy running on the host, which adds authentication before contacting the remote repository. The agent never sees the credentials.

Advantages:
- **No TLS interception**: The external service makes authenticated requests directly
- **Credentials stay outside**: The agent only sees the tool interface, not the underlying credentials

#### Traffic forwarding

For Anthropic API calls, `ANTHROPIC_BASE_URL` lets you route requests to a proxy that can inspect and modify them in plaintext. But for other HTTPS services (GitHub, npm registries, internal APIs), the traffic is often encrypted end-to-end—even if you route it through a proxy via `HTTP_PROXY`, the proxy only sees an opaque TLS tunnel and can't inject credentials.

To modify HTTPS traffic to arbitrary services, without using a custom tool, you need a TLS-terminating proxy that decrypts traffic, inspects or modifies it, then re-encrypts it before forwarding. This requires:

1. Running the proxy outside the agent's container
2. Installing the proxy's CA certificate in the agent's trust store (so the agent trusts the proxy's certificates)
3. Configuring `HTTP_PROXY`/`HTTPS_PROXY` to route traffic through the proxy

This approach handles any HTTP-based service without writing custom tools, but adds complexity around certificate management.

Note that not all programs respect `HTTP_PROXY`/`HTTPS_PROXY`. Most tools (curl, pip, npm, git) do, but some may bypass these variables and connect directly. For example, Node.js `fetch()` ignores these variables by default; in Node 24+ you can set `NODE_USE_ENV_PROXY=1` to enable support. For comprehensive coverage, you can use [proxychains](https://github.com/haad/proxychains) to intercept network calls, or configure iptables to redirect outbound traffic to a transparent proxy.

<Info>
A **transparent proxy** intercepts traffic at the network level, so the client doesn't need to be configured to use it. Regular proxies require clients to explicitly connect and speak HTTP CONNECT or SOCKS. Transparent proxies (like Squid or mitmproxy in transparent mode) can handle raw redirected TCP connections.
</Info>

Both approaches still require the TLS-terminating proxy and trusted CA certificate—they just ensure traffic actually reaches the proxy.

## Filesystem configuration

Filesystem controls determine what files the agent can read and write.

### Read-only code mounting

When the agent needs to analyze code but not modify it, mount the directory read-only:

<Warning>
Even read-only access to a code directory can expose credentials. Common files to exclude or sanitize before mounting:

| File | Risk |
|------|------|
| `.env`, `.env.local` | API keys, database passwords, secrets |
| `~/.git-credentials` | Git passwords/tokens in plaintext |
| `~/.aws/credentials` | AWS access keys |
| `~/.config/gcloud/application_default_credentials.json` | Google Cloud ADC tokens |
| `~/.azure/` | Azure CLI credentials |
| `~/.docker/config.json` | Docker registry auth tokens |
| `~/.kube/config` | Kubernetes cluster credentials |
| `.npmrc`, `.pypirc` | Package registry tokens |
| `*-service-account.json` | GCP service account keys |
| `*.pem`, `*.key` | Private keys |

Consider copying only the source files needed, or using `.dockerignore`-style filtering.
</Warning>

### Writable locations

If the agent needs to write files, you have a few options depending on whether you want changes to persist:

For ephemeral workspaces in containers, use `tmpfs` mounts that exist only in memory and are cleared when the container stops:

If you want to review changes before persisting them, an overlay filesystem lets the agent write without modifying underlying files—changes are stored in a separate layer you can inspect, apply, or discard. For fully persistent output, mount a dedicated volume but keep it separate from sensitive directories.

- [Claude Code security documentation](https://code.claude.com/docs/en/security)
- [Hosting the Agent SDK](/docs/en/agent-sdk/hosting)
- [Handling permissions](/docs/en/agent-sdk/permissions)
- [Sandbox runtime](https://github.com/anthropic-experimental/sandbox-runtime)
- [The Lethal Trifecta for AI Agents](https://simonwillison.net/2025/Jun/16/the-lethal-trifecta/)
- [OWASP Top 10 for LLM Applications](https://owasp.org/www-project-top-10-for-large-language-model-applications/)
- [Docker Security Best Practices](https://docs.docker.com/engine/security/)
- [gVisor Documentation](https://gvisor.dev/docs/)
- [Firecracker Documentation](https://firecracker-microvm.github.io/)

**Examples:**

Example 1 (bash):
```bash
npm install @anthropic-ai/sandbox-runtime
```

Example 2 (bash):
```bash
docker run \
  --cap-drop ALL \
  --security-opt no-new-privileges \
  --security-opt seccomp=/path/to/seccomp-profile.json \
  --read-only \
  --tmpfs /tmp:rw,noexec,nosuid,size=100m \
  --tmpfs /home/agent:rw,noexec,nosuid,size=500m \
  --network none \
  --memory 2g \
  --cpus 2 \
  --pids-limit 100 \
  --user 1000:1000 \
  -v /path/to/code:/workspace:ro \
  -v /var/run/proxy.sock:/var/run/proxy.sock:ro \
  agent-image
```

Example 3 (json):
```json
// /etc/docker/daemon.json
{
  "runtimes": {
    "runsc": {
      "path": "/usr/local/bin/runsc"
    }
  }
}
```

Example 4 (bash):
```bash
docker run --runtime=runsc agent-image
```

---

## Agent Skills in the SDK

**URL:** llms-txt#agent-skills-in-the-sdk

**Contents:**
- Overview
- How Skills Work with the SDK
- Using Skills with the SDK
- Skill Locations
- Creating Skills
- Tool Restrictions
- Discovering Available Skills
- Testing Skills
- Troubleshooting
  - Skills Not Found

Extend Claude with specialized capabilities using Agent Skills in the Claude Agent SDK

Agent Skills extend Claude with specialized capabilities that Claude autonomously invokes when relevant. Skills are packaged as `SKILL.md` files containing instructions, descriptions, and optional supporting resources.

For comprehensive information about Skills, including benefits, architecture, and authoring guidelines, see the [Agent Skills overview](/docs/en/agents-and-tools/agent-skills/overview).

## How Skills Work with the SDK

When using the Claude Agent SDK, Skills are:

1. **Defined as filesystem artifacts**: Created as `SKILL.md` files in specific directories (`.claude/skills/`)
2. **Loaded from filesystem**: Skills are loaded from configured filesystem locations. You must specify `settingSources` (TypeScript) or `setting_sources` (Python) to load Skills from the filesystem
3. **Automatically discovered**: Once filesystem settings are loaded, Skill metadata is discovered at startup from user and project directories; full content loaded when triggered
4. **Model-invoked**: Claude autonomously chooses when to use them based on context
5. **Enabled via allowed_tools**: Add `"Skill"` to your `allowed_tools` to enable Skills

Unlike subagents (which can be defined programmatically), Skills must be created as filesystem artifacts. The SDK does not provide a programmatic API for registering Skills.

<Note>
**Default behavior**: By default, the SDK does not load any filesystem settings. To use Skills, you must explicitly configure `settingSources: ['user', 'project']` (TypeScript) or `setting_sources=["user", "project"]` (Python) in your options.
</Note>

## Using Skills with the SDK

To use Skills with the SDK, you need to:

1. Include `"Skill"` in your `allowed_tools` configuration
2. Configure `settingSources`/`setting_sources` to load Skills from the filesystem

Once configured, Claude automatically discovers Skills from the specified directories and invokes them when relevant to the user's request.

Skills are loaded from filesystem directories based on your `settingSources`/`setting_sources` configuration:

- **Project Skills** (`.claude/skills/`): Shared with your team via git - loaded when `setting_sources` includes `"project"`
- **User Skills** (`~/.claude/skills/`): Personal Skills across all projects - loaded when `setting_sources` includes `"user"`
- **Plugin Skills**: Bundled with installed Claude Code plugins

Skills are defined as directories containing a `SKILL.md` file with YAML frontmatter and Markdown content. The `description` field determines when Claude invokes your Skill.

**Example directory structure**:

For complete guidance on creating Skills, including SKILL.md structure, multi-file Skills, and examples, see:
- [Agent Skills in Claude Code](https://code.claude.com/docs/en/skills): Complete guide with examples
- [Agent Skills Best Practices](/docs/en/agents-and-tools/agent-skills/best-practices): Authoring guidelines and naming conventions

<Note>
The `allowed-tools` frontmatter field in SKILL.md is only supported when using Claude Code CLI directly. **It does not apply when using Skills through the SDK**.

When using the SDK, control tool access through the main `allowedTools` option in your query configuration.
</Note>

To restrict tools for Skills in SDK applications, use the `allowedTools` option:

<Note>
Import statements from the first example are assumed in the following code snippets.
</Note>

## Discovering Available Skills

To see which Skills are available in your SDK application, simply ask Claude:

Claude will list the available Skills based on your current working directory and installed plugins.

Test Skills by asking questions that match their descriptions:

Claude automatically invokes the relevant Skill if the description matches your request.

**Check settingSources configuration**: Skills are only loaded when you explicitly configure `settingSources`/`setting_sources`. This is the most common issue:

**Examples:**

Example 1 (unknown):
```unknown

```

Example 2 (unknown):
```unknown
</CodeGroup>

## Skill Locations

Skills are loaded from filesystem directories based on your `settingSources`/`setting_sources` configuration:

- **Project Skills** (`.claude/skills/`): Shared with your team via git - loaded when `setting_sources` includes `"project"`
- **User Skills** (`~/.claude/skills/`): Personal Skills across all projects - loaded when `setting_sources` includes `"user"`
- **Plugin Skills**: Bundled with installed Claude Code plugins

## Creating Skills

Skills are defined as directories containing a `SKILL.md` file with YAML frontmatter and Markdown content. The `description` field determines when Claude invokes your Skill.

**Example directory structure**:
```

Example 3 (unknown):
```unknown
For complete guidance on creating Skills, including SKILL.md structure, multi-file Skills, and examples, see:
- [Agent Skills in Claude Code](https://code.claude.com/docs/en/skills): Complete guide with examples
- [Agent Skills Best Practices](/docs/en/agents-and-tools/agent-skills/best-practices): Authoring guidelines and naming conventions

## Tool Restrictions

<Note>
The `allowed-tools` frontmatter field in SKILL.md is only supported when using Claude Code CLI directly. **It does not apply when using Skills through the SDK**.

When using the SDK, control tool access through the main `allowedTools` option in your query configuration.
</Note>

To restrict tools for Skills in SDK applications, use the `allowedTools` option:

<Note>
Import statements from the first example are assumed in the following code snippets.
</Note>

<CodeGroup>
```

Example 4 (unknown):
```unknown

```

---

## Todo Lists

**URL:** llms-txt#todo-lists

**Contents:**
  - Todo Lifecycle
  - When Todos Are Used
- Examples
  - Monitoring Todo Changes
  - Real-time Progress Display

Track and display todos using the Claude Agent SDK for organized task management

Todo tracking provides a structured way to manage tasks and display progress to users. The Claude Agent SDK includes built-in todo functionality that helps organize complex workflows and keep users informed about task progression.

Todos follow a predictable lifecycle:
1. **Created** as `pending` when tasks are identified
2. **Activated** to `in_progress` when work begins
3. **Completed** when the task finishes successfully
4. **Removed** when all tasks in a group are completed

### When Todos Are Used

The SDK automatically creates todos for:
- **Complex multi-step tasks** requiring 3 or more distinct actions
- **User-provided task lists** when multiple items are mentioned
- **Non-trivial operations** that benefit from progress tracking
- **Explicit requests** when users ask for todo organization

### Monitoring Todo Changes

### Real-time Progress Display

```python Python
from claude_agent_sdk import query, AssistantMessage, ToolUseBlock
from typing import List, Dict

class TodoTracker:
    def __init__(self):
        self.todos: List[Dict] = []
    
    def display_progress(self):
        if not self.todos:
            return
        
        completed = len([t for t in self.todos if t["status"] == "completed"])
        in_progress = len([t for t in self.todos if t["status"] == "in_progress"])
        total = len(self.todos)
        
        print(f"\nProgress: {completed}/{total} completed")
        print(f"Currently working on: {in_progress} task(s)\n")
        
        for i, todo in enumerate(self.todos):
            icon = "✅" if todo["status"] == "completed" else \
                  "🔧" if todo["status"] == "in_progress" else "❌"
            text = todo["activeForm"] if todo["status"] == "in_progress" else todo["content"]
            print(f"{i + 1}. {icon} {text}")
    
    async def track_query(self, prompt: str):
        async for message in query(
            prompt=prompt,
            options={"max_turns": 20}
        ):
            if isinstance(message, AssistantMessage):
                for block in message.content:
                    if isinstance(block, ToolUseBlock) and block.name == "TodoWrite":
                        self.todos = block.input["todos"]
                        self.display_progress()

**Examples:**

Example 1 (unknown):
```unknown

```

Example 2 (unknown):
```unknown
</CodeGroup>

### Real-time Progress Display

<CodeGroup>
```

Example 3 (unknown):
```unknown

```

---

## Customer support agent

**URL:** llms-txt#customer-support-agent

**Contents:**
- Before building with Claude
  - Decide whether to use Claude for support chat
  - Define your ideal chat interaction
  - Break the interaction into unique tasks
  - Establish success criteria
- How to implement Claude as a customer service agent
  - Choose the right Claude model
  - Build a strong prompt
  - Add dynamic and agentic capabilities with tool use
  - Deploy your prompts

This guide walks through how to leverage Claude's advanced conversational capabilities to handle customer inquiries in real time, providing 24/7 support, reducing wait times, and managing high support volumes with accurate responses and positive interactions.

## Before building with Claude

### Decide whether to use Claude for support chat

Here are some key indicators that you should employ an LLM like Claude to automate portions of your customer support process:

<section title="High volume of repetitive queries">

Claude excels at handling a large number of similar questions efficiently, freeing up human agents for more complex issues.
  
</section>
  <section title="Need for quick information synthesis">

Claude can quickly retrieve, process, and combine information from vast knowledge bases, while human agents may need time to research or consult multiple sources.
  
</section>
  <section title="24/7 availability requirement">

Claude can provide round-the-clock support without fatigue, whereas staffing human agents for continuous coverage can be costly and challenging.
  
</section>
  <section title="Rapid scaling during peak periods">

Claude can handle sudden increases in query volume without the need for hiring and training additional staff.
  
</section>
  <section title="Consistent brand voice">

You can instruct Claude to consistently represent your brand's tone and values, whereas human agents may vary in their communication styles.
  
</section>

Some considerations for choosing Claude over other LLMs:

- You prioritize natural, nuanced conversation: Claude's sophisticated language understanding allows for more natural, context-aware conversations that feel more human-like than chats with other LLMs.
- You often receive complex and open-ended queries: Claude can handle a wide range of topics and inquiries without generating canned responses or requiring extensive programming of permutations of user utterances.
- You need scalable multilingual support: Claude's multilingual capabilities allow it to engage in conversations in over 200 languages without the need for separate chatbots or extensive translation processes for each supported language.

### Define your ideal chat interaction

Outline an ideal customer interaction to define how and when you expect the customer to interact with Claude. This outline will help to determine the technical requirements of your solution.

Here is an example chat interaction for car insurance customer support:

* **Customer**: Initiates support chat experience
   * **Claude**: Warmly greets customer and initiates conversation
* **Customer**: Asks about insurance for their new electric car
   * **Claude**: Provides relevant information about electric vehicle coverage
* **Customer**: Asks questions related to unique needs for electric vehicle insurances
   * **Claude**: Responds with accurate and informative answers and provides links to the sources
* **Customer**: Asks off-topic questions unrelated to insurance or cars
   * **Claude**: Clarifies it does not discuss unrelated topics and steers the user back to car insurance
* **Customer**: Expresses interest in an insurance quote
   * **Claude**: Ask a set of questions to determine the appropriate quote, adapting to their responses
   * **Claude**: Sends a request to use the quote generation API tool along with necessary information collected from the user
   * **Claude**: Receives the response information from the API tool use, synthesizes the information into a natural response, and presents the provided quote to the user
* **Customer**: Asks follow up questions
   * **Claude**: Answers follow up questions as needed
   * **Claude**: Guides the customer to the next steps in the insurance process and closes out the conversation

<Tip>In the real example that you write for your own use case, you might find it useful to write out the actual words in this interaction so that you can also get a sense of the ideal tone, response length, and level of detail you want Claude to have.</Tip>

### Break the interaction into unique tasks

Customer support chat is a collection of multiple different tasks, from question answering to information retrieval to taking action on requests, wrapped up in a single customer interaction. Before you start building, break down your ideal customer interaction into every task you want Claude to be able to perform. This ensures you can prompt and evaluate Claude for every task, and gives you a good sense of the range of interactions you need to account for when writing test cases.

<Tip>Customers sometimes find it helpful to visualize this as an interaction flowchart of possible conversation inflection points depending on user requests.</Tip>

Here are the key tasks associated with the example insurance interaction above:

1. Greeting and general guidance
   - Warmly greet the customer and initiate conversation
   - Provide general information about the company and interaction

2. Product Information
   - Provide information about electric vehicle coverage
   <Note>This will require that Claude have the necessary information in its context, and might imply that a [RAG integration](https://platform.claude.com/cookbook/capabilities-retrieval-augmented-generation-guide) is necessary.</Note>
   - Answer questions related to unique electric vehicle insurance needs
   - Answer follow-up questions about the quote or insurance details
   - Offer links to sources when appropriate

3. Conversation Management
   - Stay on topic (car insurance)
   - Redirect off-topic questions back to relevant subjects

4. Quote Generation
   - Ask appropriate questions to determine quote eligibility
   - Adapt questions based on customer responses
   - Submit collected information to quote generation API
   - Present the provided quote to the customer

### Establish success criteria

Work with your support team to [define clear success criteria](/docs/en/test-and-evaluate/define-success) and write [detailed evaluations](/docs/en/test-and-evaluate/develop-tests) with measurable benchmarks and goals.

Here are criteria and benchmarks that can be used to evaluate how successfully Claude performs the defined tasks:

<section title="Query comprehension accuracy">

This metric evaluates how accurately Claude understands customer inquiries across various topics. Measure this by reviewing a sample of conversations and assessing whether Claude has the correct interpretation of customer intent, critical next steps, what successful resolution looks like, and more. Aim for a comprehension accuracy of 95% or higher.
  
</section>
  <section title="Response relevance">

This assesses how well Claude's response addresses the customer's specific question or issue. Evaluate a set of conversations and rate the relevance of each response (using LLM-based grading for scale). Target a relevance score of 90% or above.
  
</section>
  <section title="Response accuracy">

Assess the correctness of general company and product information provided to the user, based on the information provided to Claude in context. Target 100% accuracy in this introductory information.
  
</section>
  <section title="Citation provision relevance">

Track the frequency and relevance of links or sources offered. Target providing relevant sources in 80% of interactions where additional information could be beneficial.
  
</section>
  <section title="Topic adherence">

Measure how well Claude stays on topic, such as the topic of car insurance in our example implementation. Aim for 95% of responses to be directly related to car insurance or the customer's specific query.
  
</section>
  <section title="Content generation effectiveness">

Measure how successful Claude is at determining when to generate informational content and how relevant that content is. For example, in our implementation, we would be determining how well Claude understands when to generate a quote and how accurate that quote is. Target 100% accuracy, as this is vital information for a successful customer interaction.
  
</section>
  <section title="Escalation efficiency">

This measures Claude's ability to recognize when a query needs human intervention and escalate appropriately. Track the percentage of correctly escalated conversations versus those that should have been escalated but weren't. Aim for an escalation accuracy of 95% or higher.
  
</section>

Here are criteria and benchmarks that can be used to evaluate the business impact of employing Claude for support:

<section title="Sentiment maintenance">

This assesses Claude's ability to maintain or improve customer sentiment throughout the conversation. Use sentiment analysis tools to measure sentiment at the beginning and end of each conversation. Aim for maintained or improved sentiment in 90% of interactions.
  
</section>
  <section title="Deflection rate">

The percentage of customer inquiries successfully handled by the chatbot without human intervention. Typically aim for 70-80% deflection rate, depending on the complexity of inquiries.
  
</section>
  <section title="Customer satisfaction score">

A measure of how satisfied customers are with their chatbot interaction. Usually done through post-interaction surveys. Aim for a CSAT score of 4 out of 5 or higher.
  
</section>
  <section title="Average handle time">

The average time it takes for the chatbot to resolve an inquiry. This varies widely based on the complexity of issues, but generally, aim for a lower AHT compared to human agents.
  
</section>

## How to implement Claude as a customer service agent

### Choose the right Claude model

The choice of model depends on the trade-offs between cost, accuracy, and response time.

For customer support chat, Claude Sonnet 4.5 is well suited to balance intelligence, latency, and cost. However, for instances where you have conversation flow with multiple prompts including RAG, tool use, and/or long-context prompts, Claude Haiku 4.5 may be more suitable to optimize for latency.

### Build a strong prompt

Using Claude for customer support requires Claude having enough direction and context to respond appropriately, while having enough flexibility to handle a wide range of customer inquiries.

Let's start by writing the elements of a strong prompt, starting with a system prompt:

<Tip>While you may be tempted to put all your information inside a system prompt as a way to separate instructions from the user conversation, Claude actually works best with the bulk of its prompt content written inside the first `User` turn (with the only exception being role prompting). Read more at [Giving Claude a role with a system prompt](/docs/en/build-with-claude/prompt-engineering/system-prompts).</Tip>

It's best to break down complex prompts into subsections and write one part at a time. For each task, you might find greater success by following a step by step process to define the parts of the prompt Claude would need to do the task well. For this car insurance customer support example, we'll be writing piecemeal all the parts for a prompt starting with the "Greeting and general guidance" task. This also makes debugging your prompt easier as you can more quickly adjust individual parts of the overall prompt.

We'll put all of these pieces in a file called `config.py`.

We'll then do the same for our car insurance and electric car insurance information.

Now that we have our static content, let's add at least 4-5 sample "good" interactions to guide Claude's responses. These examples should be representative of your ideal customer interaction and can include guardrails, tool calls, etc.

You will also want to include any important instructions outlining Do's and Don'ts for how Claude should interact with the customer. 
This may draw from brand guardrails or support policies.

Now let’s combine all these sections into a single string to use as our prompt.

### Add dynamic and agentic capabilities with tool use

Claude is capable of taking actions and retrieving information dynamically using client-side tool use functionality. Start by listing any external tools or APIs the prompt should utilize.

For this example, we will start with one tool for calculating the quote.

<Tip>As a reminder, this tool will not perform the actual calculation, it will just signal to the application that a tool should be used with whatever arguments specified.</Tip>

Example insurance quote calculator:

### Deploy your prompts

It's hard to know how well your prompt works without deploying it in a test production setting and [running evaluations](/docs/en/test-and-evaluate/develop-tests) so let's build a small application using our prompt, the Anthropic SDK, and streamlit for a user interface.

In a file called `chatbot.py`, start by setting up the ChatBot class, which will encapsulate the interactions with the Anthropic SDK.

The class should have two main methods: `generate_message` and `process_user_input`.

### Build your user interface

Test deploying this code with Streamlit using a main method. This `main()` function sets up a Streamlit-based chat interface.

We'll do this in a file called `app.py`

Run the program with:

### Evaluate your prompts

Prompting often requires testing and optimization for it to be production ready. To determine the readiness of your solution, evaluate the chatbot performance using a systematic process combining quantitative and qualitative methods. Creating a [strong empirical evaluation](/docs/en/test-and-evaluate/develop-tests#building-evals-and-test-cases) based on your defined success criteria will allow you to optimize your prompts.

<Tip>The [Claude Console](/dashboard) now features an Evaluation tool that allows you to test your prompts under various scenarios.</Tip>

### Improve performance

In complex scenarios, it may be helpful to consider additional strategies to improve performance beyond standard [prompt engineering techniques](/docs/en/build-with-claude/prompt-engineering/overview) & [guardrail implementation strategies](/docs/en/test-and-evaluate/strengthen-guardrails/reduce-hallucinations). Here are some common scenarios:

#### Reduce long context latency with RAG

When dealing with large amounts of static and dynamic context, including all information in the prompt can lead to high costs, slower response times, and reaching context window limits. In this scenario, implementing Retrieval Augmented Generation (RAG) techniques can significantly improve performance and efficiency.

By using [embedding models like Voyage](/docs/en/build-with-claude/embeddings) to convert information into vector representations, you can create a more scalable and responsive system. This approach allows for dynamic retrieval of relevant information based on the current query, rather than including all possible context in every prompt.

Implementing RAG for support use cases [RAG recipe](https://platform.claude.com/cookbook/capabilities-retrieval-augmented-generation-guide) has been shown to increase accuracy, reduce response times, and reduce API costs in systems with extensive context requirements.

#### Integrate real-time data with tool use

When dealing with queries that require real-time information, such as account balances or policy details, embedding-based RAG approaches are not sufficient. Instead, you can leverage tool use to significantly enhance your chatbot's ability to provide accurate, real-time responses. For example, you can use tool use to look up customer information, retrieve order details, and cancel orders on behalf of the customer.

This approach, [outlined in our tool use: customer service agent recipe](https://platform.claude.com/cookbook/tool-use-customer-service-agent), allows you to seamlessly integrate live data into your Claude's responses and provide a more personalized and efficient customer experience.

#### Strengthen input and output guardrails

When deploying a chatbot, especially in customer service scenarios, it's crucial to prevent risks associated with misuse, out-of-scope queries, and inappropriate responses. While Claude is inherently resilient to such scenarios, here are additional steps to strengthen your chatbot guardrails:

- [Reduce hallucination](/docs/en/test-and-evaluate/strengthen-guardrails/reduce-hallucinations): Implement fact-checking mechanisms and [citations](https://platform.claude.com/cookbook/misc-using-citations) to ground responses in provided information.
- Cross-check information: Verify that the agent's responses align with your company's policies and known facts.
- Avoid contractual commitments: Ensure the agent doesn't make promises or enter into agreements it's not authorized to make.
- [Mitigate jailbreaks](/docs/en/test-and-evaluate/strengthen-guardrails/mitigate-jailbreaks): Use methods like harmlessness screens and input validation to prevent users from exploiting model vulnerabilities, aiming to generate inappropriate content.
- Avoid mentioning competitors: Implement a competitor mention filter to maintain brand focus and not mention any competitor's products or services.
- [Keep Claude in character](/docs/en/test-and-evaluate/strengthen-guardrails/keep-claude-in-character): Prevent Claude from changing their style of context, even during long, complex interactions.
- Remove Personally Identifiable Information (PII): Unless explicitly required and authorized, strip out any PII from responses.

#### Reduce perceived response time with streaming

When dealing with potentially lengthy responses, implementing streaming can significantly improve user engagement and satisfaction. In this scenario, users receive the answer progressively instead of waiting for the entire response to be generated.

Here is how to implement streaming:
1. Use the [Anthropic Streaming API](/docs/en/build-with-claude/streaming) to support streaming responses.
2. Set up your frontend to handle incoming chunks of text.
3. Display each chunk as it arrives, simulating real-time typing.
4. Implement a mechanism to save the full response, allowing users to view it if they navigate away and return.

In some cases, streaming enables the use of more advanced models with higher base latencies, as the progressive display mitigates the impact of longer processing times.

#### Scale your Chatbot

As the complexity of your Chatbot grows, your application architecture can evolve to match. Before you add further layers to your architecture, consider the following less exhaustive options:

- Ensure that you are making the most out of your prompts and optimizing through prompt engineering. Use our [prompt engineering guides](/docs/en/build-with-claude/prompt-engineering/overview) to write the most effective prompts.
- Add additional [tools](/docs/en/build-with-claude/tool-use) to the prompt (which can include [prompt chains](/docs/en/build-with-claude/prompt-engineering/chain-prompts)) and see if you can achieve the functionality required.

If your Chatbot handles incredibly varied tasks, you may want to consider adding a [separate intent classifier](https://platform.claude.com/cookbook/capabilities-classification-guide) to route the initial customer query. For the existing application, this would involve creating a decision tree that would route customer queries through the classifier and then to specialized conversations (with their own set of tools and system prompts). Note, this method requires an additional call to Claude that can increase latency.

### Integrate Claude into your support workflow

While our examples have focused on Python functions callable within a Streamlit environment, deploying Claude for real-time support chatbot requires an API service.

Here's how you can approach this:

1. Create an API wrapper: Develop a simple API wrapper around your classification function. For example, you can use Flask API or Fast API to wrap your code into a HTTP Service. Your HTTP service could accept the user input and return the Assistant response in its entirety. Thus, your service could have the following characteristics:
   - Server-Sent Events (SSE): SSE allows for real-time streaming of responses from the server to the client. This is crucial for providing a smooth, interactive experience when working with LLMs.
   - Caching: Implementing caching can significantly improve response times and reduce unnecessary API calls.
   - Context retention: Maintaining context when a user navigates away and returns is important for continuity in conversations.

2. Build a web interface: Implement a user-friendly web UI for interacting with the Claude-powered agent.

<CardGroup cols={2}>
  <Card title="Retrieval Augmented Generation (RAG) cookbook" icon="link" href="https://platform.claude.com/cookbook/capabilities-retrieval-augmented-generation-guide">
    Visit our RAG cookbook recipe for more example code and detailed guidance.
  </Card>
  <Card title="Citations cookbook" icon="link" href="https://platform.claude.com/cookbook/misc-using-citations">
    Explore our Citations cookbook recipe for how to ensure accuracy and explainability of information.
  </Card>
</CardGroup>

**Examples:**

Example 1 (python):
```python
IDENTITY = """You are Eva, a friendly and knowledgeable AI assistant for Acme Insurance 
Company. Your role is to warmly welcome customers and provide information on 
Acme's insurance offerings, which include car insurance and electric car 
insurance. You can also help customers get quotes for their insurance needs."""
```

Example 2 (python):
```python
STATIC_GREETINGS_AND_GENERAL = """
<static_context>
Acme Auto Insurance: Your Trusted Companion on the Road

About:
At Acme Insurance, we understand that your vehicle is more than just a mode of transportation—it's your ticket to life's adventures. 
Since 1985, we've been crafting auto insurance policies that give drivers the confidence to explore, commute, and travel with peace of mind.
Whether you're navigating city streets or embarking on cross-country road trips, Acme is there to protect you and your vehicle. 
Our innovative auto insurance policies are designed to adapt to your unique needs, covering everything from fender benders to major collisions.
With Acme's award-winning customer service and swift claim resolution, you can focus on the joy of driving while we handle the rest. 
We're not just an insurance provider—we're your co-pilot in life's journeys.
Choose Acme Auto Insurance and experience the assurance that comes with superior coverage and genuine care. Because at Acme, we don't just 
insure your car—we fuel your adventures on the open road.

Note: We also offer specialized coverage for electric vehicles, ensuring that drivers of all car types can benefit from our protection.

Acme Insurance offers the following products:
- Car insurance
- Electric car insurance
- Two-wheeler insurance

Business hours: Monday-Friday, 9 AM - 5 PM EST
Customer service number: 1-800-123-4567
</static_context>
"""
```

Example 3 (python):
```python
STATIC_CAR_INSURANCE="""
<static_context>
Car Insurance Coverage:
Acme's car insurance policies typically cover:
1. Liability coverage: Pays for bodily injury and property damage you cause to others.
2. Collision coverage: Pays for damage to your car in an accident.
3. Comprehensive coverage: Pays for damage to your car from non-collision incidents.
4. Medical payments coverage: Pays for medical expenses after an accident.
5. Uninsured/underinsured motorist coverage: Protects you if you're hit by a driver with insufficient insurance.

Optional coverages include:
- Rental car reimbursement
- Roadside assistance
- New car replacement
</static_context>
"""

STATIC_ELECTRIC_CAR_INSURANCE="""
<static_context>
Electric Car Insurance:
Our specialized electric car insurance goes beyond traditional auto coverage, 
offering tailored protection for your high-tech investment. In addition to standard coverages, we offer:
- Safeguarding expensive battery systems
- Covering home charging equipment
- Adapting to the rapidly evolving world of electric vehicles

Whether you're driving a Rivian, Nissan Leaf, Chevrolet Bolt, 
or any other electric vehicle, our knowledgeable agents are ready to craft a policy 
that gives you the confidence to enjoy every electron-powered mile.
</static_context>
"""
```

Example 4 (python):
```python
EXAMPLES="""
Here are a few examples of how you can interact with customers:

<example 1>
H: Hi, do you offer commercial insurance for small businesses?

A: Ah! Unfortunately, we don't offer commercial insurance at this time. 
However, we do provide a range of personal insurance products including car insurance and electric car insurance. 
Would you like to know more about these offerings?
</example 1>

<example 2>
H: Hi, what are your business hours?

A: Our business hours are Monday to Friday, 9 AM to 5 PM EST. 
If you have any questions or need assistance, feel free to reach out during those times. 
How else can I assist you today?
</example 2>

<example 3>
H: I'm shopping for car insurance. What does Acme offer?

A: Great choice! Acme's car insurance policies typically include:
- Liability coverage: Protects you if you're at fault in an accident
- Collision coverage: Pays for damage to your car in an accident
- Comprehensive coverage: Covers non-collision incidents like theft or vandalism
- Medical payments coverage: Pays for medical expenses after an accident
- Uninsured/underinsured motorist coverage: Protects you from drivers with insufficient insurance

We also offer optional coverages like rental car reimbursement, roadside assistance, and new car replacement. 
Would you like more details on any of these coverages, or do you have specific questions about insuring your vehicle?
</example 3>

<example 4>
H: I'd like to hear about your electric car insurance.

A: Certainly! Our electric car insurance is specially designed to meet the unique needs of electric vehicle owners. 
In addition to all the standard coverages we offer for regular cars, our electric car policies include:

- Battery coverage: Protection against damage or degradation of your vehicle's expensive battery system
- Tech package: Covers repairs or replacement of advanced electronics and onboard computer systems
- Charging station coverage: Protects both home and portable charging equipment
- Cybersecurity protection: Coverage against hacking or digital threats
- Specialized roadside assistance: Includes flatbed towing to protect the vehicle's undercarriage

We cover a wide range of electric vehicles, including popular models from Tesla, Rivian, Nissan, Chevrolet, and more. 
Is there a specific electric vehicle you're looking to insure, or would you like more details on any of these coverages?
</example 4>

<example 5>
H: I'd like to get a quote for my car insurance.

A: Certainly! I'd be happy to help you get a quote for your car insurance. 
To provide you with an accurate quote, I'll need to collect some information about your vehicle and the primary driver. 
Let's start with the basics:

1. What is the make and model of your vehicle?
2. What year was it manufactured?
3. Approximately how many miles have you driven?
4. What is the age of the primary driver?

Once you provide this information, I'll use our quoting tool to generate a personalized insurance quote for you.
</example 5>
"""
```

---
