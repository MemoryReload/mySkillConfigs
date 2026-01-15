# Claude-Code - Getting Started

**Pages:** 9

---

## Quickstart

**URL:** llms-txt#quickstart

**Contents:**
- Prerequisites
- Setup
- Create a buggy file
- Build an agent that finds and fixes bugs
  - Run your agent
  - Try other prompts
  - Customize your agent
- Key concepts
- Next steps

Get started with the Python or TypeScript Agent SDK to build AI agents that work autonomously

Use the Agent SDK to build an AI agent that reads your code, finds bugs, and fixes them, all without manual intervention.

**What you'll do:**
1. Set up a project with the Agent SDK
2. Create a file with some buggy code
3. Run an agent that finds and fixes the bugs automatically

- **Node.js 18+** or **Python 3.10+**
- An **Anthropic account** ([sign up here](https://platform.claude.com/))

<Steps>
  <Step title="Install Claude Code">
    The Agent SDK uses Claude Code as its runtime. Install it for your platform:

<Tabs>
      <Tab title="macOS/Linux/WSL">
        
      </Tab>
      <Tab title="Homebrew">
        
      </Tab>
      <Tab title="WinGet">
        
      </Tab>
    </Tabs>

After installing Claude Code onto your machine, run `claude` in your terminal and follow the prompts to authenticate. The SDK will use this authentication automatically.

<Tip>
    For more information on Claude Code installation, see [Claude Code setup](https://code.claude.com/docs/en/setup).
    </Tip>
  </Step>

<Step title="Create a project folder">
    Create a new directory for this quickstart:

For your own projects, you can run the SDK from any folder; it will have access to files in that directory and its subdirectories by default.
  </Step>

<Step title="Install the SDK">
    Install the Agent SDK package for your language:

<Tabs>
      <Tab title="TypeScript">
        
      </Tab>
      <Tab title="Python (uv)">
        [uv Python package manager](https://docs.astral.sh/uv/) is a fast Python package manager that handles virtual environments automatically:
        
      </Tab>
      <Tab title="Python (pip)">
        Create a virtual environment first, then install:
        
      </Tab>
    </Tabs>
  </Step>

<Step title="Set your API key">
    If you've already authenticated Claude Code (by running `claude` in your terminal), the SDK uses that authentication automatically.

Otherwise, you need an API key, which you can get from the [Claude Console](https://platform.claude.com/).

Create a `.env` file in your project directory and store the API key there:

<Note>
    **Using Amazon Bedrock, Google Vertex AI, or Microsoft Azure?** See the setup guides for [Bedrock](https://code.claude.com/docs/en/amazon-bedrock), [Vertex AI](https://code.claude.com/docs/en/google-vertex-ai), or [Azure AI Foundry](https://code.claude.com/docs/en/azure-ai-foundry).

Unless previously approved, Anthropic does not allow third party developers to offer claude.ai login or rate limits for their products, including agents built on the Claude Agent SDK. Please use the API key authentication methods described in this document instead.
    </Note>
  </Step>
</Steps>

## Create a buggy file

This quickstart walks you through building an agent that can find and fix bugs in code. First, you need a file with some intentional bugs for the agent to fix. Create `utils.py` in the `my-agent` directory and paste the following code:

This code has two bugs:
1. `calculate_average([])` crashes with division by zero
2. `get_user_name(None)` crashes with a TypeError

## Build an agent that finds and fixes bugs

Create `agent.py` if you're using the Python SDK, or `agent.ts` for TypeScript:

This code has three main parts:

1. **`query`**: the main entry point that creates the agentic loop. It returns an async iterator, so you use `async for` to stream messages as Claude works. See the full API in the [Python](/docs/en/agent-sdk/python#query) or [TypeScript](/docs/en/agent-sdk/typescript#query) SDK reference.

2. **`prompt`**: what you want Claude to do. Claude figures out which tools to use based on the task.

3. **`options`**: configuration for the agent. This example uses `allowedTools` to restrict Claude to `Read`, `Edit`, and `Glob`, and `permissionMode: "acceptEdits"` to auto-approve file changes. Other options include `systemPrompt`, `mcpServers`, and more. See all options for [Python](/docs/en/agent-sdk/python#claudeagentoptions) or [TypeScript](/docs/en/agent-sdk/typescript#claudeagentoptions).

The `async for` loop keeps running as Claude thinks, calls tools, observes results, and decides what to do next. Each iteration yields a message: Claude's reasoning, a tool call, a tool result, or the final outcome. The SDK handles the orchestration (tool execution, context management, retries) so you just consume the stream. The loop ends when Claude finishes the task or hits an error.

The message handling inside the loop filters for human-readable output. Without filtering, you'd see raw message objects including system initialization and internal state, which is useful for debugging but noisy otherwise.

<Note>
This example uses streaming to show progress in real-time. If you don't need live output (e.g., for background jobs or CI pipelines), you can collect all messages at once. See [Streaming vs. single-turn mode](/docs/en/agent-sdk/streaming-vs-single-mode) for details.
</Note>

Your agent is ready. Run it with the following command:

<Tabs>
  <Tab title="Python">
    
  </Tab>
  <Tab title="TypeScript">
    
  </Tab>
</Tabs>

After running, check `utils.py`. You'll see defensive code handling empty lists and null users. Your agent autonomously:

1. **Read** `utils.py` to understand the code
2. **Analyzed** the logic and identified edge cases that would crash
3. **Edited** the file to add proper error handling

This is what makes the Agent SDK different: Claude executes tools directly instead of asking you to implement them.

<Note>
If you see "Claude Code not found", [install Claude Code](#install-claude-code) and restart your terminal. For "API key not found", [set your API key](#set-your-api-key). See the [full troubleshooting guide](https://code.claude.com/docs/en/troubleshooting) for more help.
</Note>

### Try other prompts

Now that your agent is set up, try some different prompts:

- `"Add docstrings to all functions in utils.py"`
- `"Add type hints to all functions in utils.py"`
- `"Create a README.md documenting the functions in utils.py"`

### Customize your agent

You can modify your agent's behavior by changing the options. Here are a few examples:

**Add web search capability:**

**Give Claude a custom system prompt:**

**Run commands in the terminal:**

With `Bash` enabled, try: `"Write unit tests for utils.py, run them, and fix any failures"`

**Tools** control what your agent can do:

| Tools | What the agent can do |
|-------|----------------------|
| `Read`, `Glob`, `Grep` | Read-only analysis |
| `Read`, `Edit`, `Glob` | Analyze and modify code |
| `Read`, `Edit`, `Bash`, `Glob`, `Grep` | Full automation |

**Permission modes** control how much human oversight you want:

| Mode | Behavior | Use case |
|------|----------|----------|
| `acceptEdits` | Auto-approves file edits, asks for other actions | Trusted development workflows |
| `bypassPermissions` | Runs without prompts | CI/CD pipelines, automation |
| `default` | Requires a `canUseTool` callback to handle approval | Custom approval flows |

The example above uses `acceptEdits` mode, which auto-approves file operations so the agent can run without interactive prompts. If you want to prompt users for approval, use `default` mode and provide a [`canUseTool` callback](/docs/en/agent-sdk/user-input) that collects user input. For more control, see [Permissions](/docs/en/agent-sdk/permissions).

Now that you've created your first agent, learn how to extend its capabilities and tailor it to your use case:

- **[Permissions](/docs/en/agent-sdk/permissions)**: control what your agent can do and when it needs approval
- **[Hooks](/docs/en/agent-sdk/hooks)**: run custom code before or after tool calls
- **[Sessions](/docs/en/agent-sdk/sessions)**: build multi-turn agents that maintain context
- **[MCP servers](/docs/en/agent-sdk/mcp)**: connect to databases, browsers, APIs, and other external systems
- **[Hosting](/docs/en/agent-sdk/hosting)**: deploy agents to Docker, cloud, and CI/CD
- **[Example agents](https://github.com/anthropics/claude-agent-sdk-demos)**: see complete examples: email assistant, research agent, and more

**Examples:**

Example 1 (bash):
```bash
curl -fsSL https://claude.ai/install.sh | bash
```

Example 2 (bash):
```bash
brew install --cask claude-code
```

Example 3 (powershell):
```powershell
winget install Anthropic.ClaudeCode
```

Example 4 (bash):
```bash
mkdir my-agent && cd my-agent
```

---

## Features overview

**URL:** llms-txt#features-overview

**Contents:**
- Core capabilities
- Tools

Explore Claude's advanced features and capabilities.

These features enhance Claude's fundamental abilities for processing, analyzing, and generating content across various formats and use cases.

| Feature | Description | Availability |
|---------|-------------|--------------|
| [1M token context window](/docs/en/build-with-claude/context-windows#1m-token-context-window) | An extended context window that allows you to process much larger documents, maintain longer conversations, and work with more extensive codebases. | <PlatformAvailability claudeApiBeta bedrockBeta vertexAiBeta azureAiBeta /> |
| [Agent Skills](/docs/en/agents-and-tools/agent-skills/overview) | Extend Claude's capabilities with Skills. Use pre-built Skills (PowerPoint, Excel, Word, PDF) or create custom Skills with instructions and scripts. Skills use progressive disclosure to efficiently manage context. | <PlatformAvailability claudeApiBeta azureAiBeta /> |
| [Batch processing](/docs/en/build-with-claude/batch-processing) | Process large volumes of requests asynchronously for cost savings. Send batches with a large number of queries per batch. Batch API calls costs 50% less than standard API calls. | <PlatformAvailability claudeApi bedrock vertexAi /> |
| [Citations](/docs/en/build-with-claude/citations) | Ground Claude's responses in source documents. With Citations, Claude can provide detailed references to the exact sentences and passages it uses to generate responses, leading to more verifiable, trustworthy outputs. | <PlatformAvailability claudeApi bedrock vertexAi azureAi /> |
| [Context editing](/docs/en/build-with-claude/context-editing) | Automatically manage conversation context with configurable strategies. Supports clearing tool results when approaching token limits and managing thinking blocks in extended thinking conversations. | <PlatformAvailability claudeApiBeta bedrockBeta vertexAiBeta azureAiBeta /> |
| [Effort](/docs/en/build-with-claude/effort) | Control how many tokens Claude uses when responding with the effort parameter, trading off between response thoroughness and token efficiency. | <PlatformAvailability claudeApiBeta bedrockBeta vertexAiBeta azureAiBeta /> |
| [Extended thinking](/docs/en/build-with-claude/extended-thinking) | Enhanced reasoning capabilities for complex tasks, providing transparency into Claude's step-by-step thought process before delivering its final answer. | <PlatformAvailability claudeApi bedrock vertexAi azureAi /> |
| [Files API](/docs/en/build-with-claude/files) | Upload and manage files to use with Claude without re-uploading content with each request. Supports PDFs, images, and text files. | <PlatformAvailability claudeApiBeta azureAiBeta /> |
| [PDF support](/docs/en/build-with-claude/pdf-support) | Process and analyze text and visual content from PDF documents. | <PlatformAvailability claudeApi bedrock vertexAi azureAi /> |
| [Prompt caching (5m)](/docs/en/build-with-claude/prompt-caching) | Provide Claude with more background knowledge and example outputs to reduce costs and latency. | <PlatformAvailability claudeApi bedrock vertexAi azureAi /> |
| [Prompt caching (1hr)](/docs/en/build-with-claude/prompt-caching#1-hour-cache-duration) | Extended 1-hour cache duration for less frequently accessed but important context, complementing the standard 5-minute cache. | <PlatformAvailability claudeApi azureAi /> |
| [Search results](/docs/en/build-with-claude/search-results) | Enable natural citations for RAG applications by providing search results with proper source attribution. Achieve web search-quality citations for custom knowledge bases and tools. | <PlatformAvailability claudeApi bedrock vertexAi azureAi /> |
| [Structured outputs](/docs/en/build-with-claude/structured-outputs) | Guarantee schema conformance with two approaches: JSON outputs for structured data responses, and strict tool use for validated tool inputs. Available on Sonnet 4.5, Opus 4.1, Opus 4.5, and Haiku 4.5. | <PlatformAvailability claudeApiBeta azureAiBeta /> |
| [Token counting](/docs/en/api/messages-count-tokens) | Token counting enables you to determine the number of tokens in a message before sending it to Claude, helping you make informed decisions about your prompts and usage. | <PlatformAvailability claudeApi bedrock vertexAi azureAi /> |
| [Tool use](/docs/en/agents-and-tools/tool-use/overview) | Enable Claude to interact with external tools and APIs to perform a wider variety of tasks. For a list of supported tools, see [the Tools table](#tools). | <PlatformAvailability claudeApi bedrock vertexAi azureAi /> |

These features enable Claude to interact with external systems, execute code, and perform automated tasks through various tool interfaces.

| Feature | Description | Availability |
|---------|-------------|--------------|
| [Bash](/docs/en/agents-and-tools/tool-use/bash-tool) | Execute bash commands and scripts to interact with the system shell and perform command-line operations. | <PlatformAvailability claudeApi bedrock vertexAi azureAi /> |
| [Code execution](/docs/en/agents-and-tools/tool-use/code-execution-tool) | Run Python code in a sandboxed environment for advanced data analysis. | <PlatformAvailability claudeApiBeta azureAiBeta /> |
| [Programmatic tool calling](/docs/en/agents-and-tools/tool-use/programmatic-tool-calling) | Enable Claude to call your tools programmatically from within code execution containers, reducing latency and token consumption for multi-tool workflows. | <PlatformAvailability claudeApiBeta azureAiBeta /> |
| [Computer use](/docs/en/agents-and-tools/tool-use/computer-use-tool) | Control computer interfaces by taking screenshots and issuing mouse and keyboard commands. | <PlatformAvailability claudeApiBeta bedrockBeta vertexAiBeta azureAiBeta /> |
| [Fine-grained tool streaming](/docs/en/agents-and-tools/tool-use/fine-grained-tool-streaming) | Stream tool use parameters without buffering/JSON validation, reducing latency for receiving large parameters. | <PlatformAvailability claudeApi bedrock vertexAi azureAi /> |
| [MCP connector](/docs/en/agents-and-tools/mcp-connector) | Connect to remote [MCP](/docs/en/mcp) servers directly from the Messages API without a separate MCP client. | <PlatformAvailability claudeApiBeta azureAiBeta /> |
| [Memory](/docs/en/agents-and-tools/tool-use/memory-tool) | Enable Claude to store and retrieve information across conversations. Build knowledge bases over time, maintain project context, and learn from past interactions. | <PlatformAvailability claudeApiBeta bedrockBeta vertexAiBeta azureAiBeta /> |
| [Text editor](/docs/en/agents-and-tools/tool-use/text-editor-tool) | Create and edit text files with a built-in text editor interface for file manipulation tasks. | <PlatformAvailability claudeApi bedrock vertexAi azureAi /> |
| [Tool search](/docs/en/agents-and-tools/tool-use/tool-search-tool) | Scale to thousands of tools by dynamically discovering and loading tools on-demand using regex-based search, optimizing context usage and improving tool selection accuracy. | <PlatformAvailability claudeApiBeta bedrockBeta vertexAiBeta azureAiBeta /> |
| [Web fetch](/docs/en/agents-and-tools/tool-use/web-fetch-tool) | Retrieve full content from specified web pages and PDF documents for in-depth analysis. | <PlatformAvailability claudeApiBeta azureAiBeta /> |
| [Web search](/docs/en/agents-and-tools/tool-use/web-search-tool) | Augment Claude's comprehensive knowledge with current, real-world data from across the web. | <PlatformAvailability claudeApi vertexAi azureAi /> |

---

## API Overview

**URL:** llms-txt#api-overview

**Contents:**
- Prerequisites
- Available APIs
- Authentication
  - Getting API Keys
- Client SDKs
- Claude API vs Third-Party Platforms
  - Claude API
  - Third-Party Platform APIs
- Request and Response Format
  - Request Size Limits

The Claude API is a RESTful API at `https://api.anthropic.com` that provides programmatic access to Claude models. The primary API is the Messages API (`POST /v1/messages`) for conversational interactions.

<Note>
**New to Claude?** Start with [Get started](/docs/en/get-started) for prerequisites and your first API call, or see [Working with Messages](/docs/en/build-with-claude/working-with-messages) for request/response patterns and examples.
</Note>

To use the Claude API, you'll need:

- An [Anthropic Console account](https://platform.claude.com)
- An [API key](/settings/keys)

For step-by-step setup instructions, see [Get started](/docs/en/get-started).

The Claude API includes the following APIs:

**General Availability:**
- **[Messages API](/docs/en/api/messages)**: Send messages to Claude for conversational interactions (`POST /v1/messages`)
- **[Message Batches API](/docs/en/api/creating-message-batches)**: Process large volumes of Messages requests asynchronously with 50% cost reduction (`POST /v1/messages/batches`)
- **[Token Counting API](/docs/en/api/messages-count-tokens)**: Count tokens in a message before sending to manage costs and rate limits (`POST /v1/messages/count_tokens`)
- **[Models API](/docs/en/api/models-list)**: List available Claude models and their details (`GET /v1/models`)

**Beta:**
- **[Files API](/docs/en/api/files-create)**: Upload and manage files for use across multiple API calls (`POST /v1/files`, `GET /v1/files`)
- **[Skills API](/docs/en/api/skills/create-skill)**: Create and manage custom agent skills (`POST /v1/skills`, `GET /v1/skills`)

For the complete API reference with all endpoints, parameters, and response schemas, explore the API reference pages listed in the navigation. To access beta features, see [Beta headers](/docs/en/api/beta-headers).

All requests to the Claude API must include these headers:

| Header | Value | Required |
|--------|-------|----------|
| `x-api-key` | Your API key from Console | Yes |
| `anthropic-version` | API version (e.g., `2023-06-01`) | Yes |
| `content-type` | `application/json` | Yes |

If you are using the [Client SDKs](#client-sdks), the SDK will send these headers automatically. For API versioning details, see [API versions](/docs/en/api/versioning).

The API is made available via the web [Console](https://platform.claude.com/). You can use the [Workbench](https://platform.claude.com/workbench) to try out the API in the browser and then generate API keys in [Account Settings](https://platform.claude.com/settings/keys). Use [workspaces](https://platform.claude.com/settings/workspaces) to segment your API keys and [control spend](/docs/en/api/rate-limits) by use case.

Anthropic provides official SDKs that simplify API integration by handling authentication, request formatting, error handling, and more.

**Benefits**:
- Automatic header management (x-api-key, anthropic-version, content-type)
- Type-safe request and response handling
- Built-in retry logic and error handling
- Streaming support
- Request timeouts and connection management

**Example** (Python):

For a list of client SDKs and their respective installation instructions, see [Client SDKs](/docs/en/api/client-sdks).

## Claude API vs Third-Party Platforms

Claude is available through Anthropic's direct API and through partner platforms. Choose based on your infrastructure, compliance requirements, and pricing preferences.

- **Direct access** to the latest models and features first
- **Anthropic billing and support**
- **Best for**: New integrations, full feature access, direct relationship with Anthropic

### Third-Party Platform APIs

Access Claude through AWS, Google Cloud, or Microsoft Azure:
- **Integrated** with cloud provider billing and IAM
- **May have feature delays** or differences from the direct API
- **Best for**: Existing cloud commitments, specific compliance requirements, consolidated cloud billing

| Platform | Provider | Documentation |
|----------|----------|---------------|
| Amazon Bedrock | AWS | [Claude on Amazon Bedrock](/docs/en/build-with-claude/claude-on-amazon-bedrock) |
| Vertex AI | Google Cloud | [Claude on Vertex AI](/docs/en/build-with-claude/claude-on-vertex-ai) |
| Azure AI | Microsoft Azure | [Claude on Azure AI](/docs/en/build-with-claude/claude-in-microsoft-foundry) |

<Note>
For feature availability across platforms, see the [Features overview](/docs/en/build-with-claude/overview).
</Note>

## Request and Response Format

### Request Size Limits

The API has different maximum request sizes depending on the endpoint:

| Endpoint | Maximum Size |
|----------|--------------|
| Standard endpoints (Messages, Token Counting) | 32 MB |
| [Batch API](/docs/en/build-with-claude/batch-processing) | 256 MB |
| [Files API](/docs/en/build-with-claude/files) | 500 MB |

If you exceed these limits, you'll receive a 413 `request_too_large` error.

The Claude API includes the following headers in every response:

- `request-id`: A globally unique identifier for the request
- `anthropic-organization-id`: The organization ID associated with the API key used in the request

## Rate Limits and Availability

The API enforces rate limits and spend limits to prevent misuse and manage capacity. Limits are organized into usage tiers that increase automatically as you use the API. Each tier has:

- **Spend limits**: Maximum monthly cost for API usage
- **Rate limits**: Maximum number of requests per minute (RPM) and tokens per minute (TPM)

You can view your organization's current limits in the [Console](/settings/limits). For higher limits or Priority Tier (enhanced service levels with committed spend), contact sales through the Console.

For detailed information about limits, tiers, and the token bucket algorithm used for rate limiting, see [Rate limits](/docs/en/api/rate-limits).

The Claude API is available in [many countries and regions](/docs/en/api/supported-regions) worldwide. Check the supported regions page to confirm availability in your location.

Here's a minimal request using the Messages API:

For complete examples and tutorials, see [Get started](/docs/en/get-started) and [Working with Messages](/docs/en/build-with-claude/working-with-messages).

<CardGroup cols={3}>
  <Card title="Get started" icon="rocket" href="/docs/en/get-started">
    Prerequisites, step-by-step tutorial, and examples in multiple languages
  </Card>
  <Card title="Working with Messages" icon="message" href="/docs/en/build-with-claude/working-with-messages">
    Request/response patterns, multi-turn conversations, and best practices
  </Card>
  <Card title="Messages API Reference" icon="book" href="/docs/en/api/messages">
    Complete API specification: parameters, responses, and error codes
  </Card>
  <Card title="Client SDKs" icon="code" href="/docs/en/api/client-sdks">
    Installation guides for Python, TypeScript, Java, Go, C#, Ruby, and PHP
  </Card>
  <Card title="Features overview" icon="grid" href="/docs/en/build-with-claude/overview">
    Explore capabilities: caching, vision, tool use, streaming, and more
  </Card>
  <Card title="Rate limits" icon="gauge" href="/docs/en/api/rate-limits">
    Usage tiers, spend limits, and rate limiting with token bucket algorithm
  </Card>
</CardGroup>

**Examples:**

Example 1 (python):
```python
from anthropic import Anthropic

client = Anthropic()  # Reads ANTHROPIC_API_KEY from environment
message = client.messages.create(
    model="claude-sonnet-4-5",
    max_tokens=1024,
    messages=[{"role": "user", "content": "Hello, Claude"}]
)
```

Example 2 (bash):
```bash
curl https://api.anthropic.com/v1/messages \
  --header "x-api-key: $ANTHROPIC_API_KEY" \
  --header "anthropic-version: 2023-06-01" \
  --header "content-type: application/json" \
  --data '{
    "model": "claude-sonnet-4-5",
    "max_tokens": 1024,
    "messages": [
      {"role": "user", "content": "Hello, Claude"}
    ]
  }'
```

Example 3 (json):
```json
{
  "id": "msg_01XFDUDYJgAACzvnptvVoYEL",
  "type": "message",
  "role": "assistant",
  "content": [
    {
      "type": "text",
      "text": "Hello! How can I assist you today?"
    }
  ],
  "model": "claude-sonnet-4-5",
  "stop_reason": "end_turn",
  "usage": {
    "input_tokens": 12,
    "output_tokens": 8
  }
}
```

---

## Models overview

**URL:** llms-txt#models-overview

**Contents:**
- Choosing a model
  - Latest models comparison
- Prompt and output performance
- Migrating to Claude 4.5
- Get started with Claude

Claude is a family of state-of-the-art large language models developed by Anthropic. This guide introduces our models and compares their performance.

If you're unsure which model to use, we recommend starting with **Claude Sonnet 4.5**. It offers the best balance of intelligence, speed, and cost for most use cases, with exceptional performance in coding and agentic tasks.

All current Claude models support text and image input, text output, multilingual capabilities, and vision. Models are available via the Anthropic API, AWS Bedrock, and Google Vertex AI.

Once you've picked a model, [learn how to make your first API call](/docs/en/get-started).

### Latest models comparison

| Feature | Claude Sonnet 4.5 | Claude Haiku 4.5 | Claude Opus 4.5 |
|:--------|:------------------|:-----------------|:----------------|
| **Description** | Our smart model for complex agents and coding | Our fastest model with near-frontier intelligence | Premium model combining maximum intelligence with practical performance |
| **Claude API ID** | claude-sonnet-4-5-20250929 | claude-haiku-4-5-20251001 | claude-opus-4-5-20251101 |
| **Claude API alias**<sup>1</sup> | claude-sonnet-4-5 | claude-haiku-4-5 | claude-opus-4-5 |
| **AWS Bedrock ID** | anthropic.claude-sonnet-4-5-20250929-v1:0 | anthropic.claude-haiku-4-5-20251001-v1:0 | anthropic.claude-opus-4-5-20251101-v1:0 |
| **GCP Vertex AI ID** | claude-sonnet-4-5@20250929 | claude-haiku-4-5@20251001 | claude-opus-4-5@20251101 |
| **Pricing**<sup>2</sup> | \$3 / input MTok<br/>\$15 / output MTok | \$1 / input MTok<br/>\$5 / output MTok | \$5 / input MTok<br/>\$25 / output MTok |
| **[Extended thinking](/docs/en/build-with-claude/extended-thinking)** | Yes | Yes | Yes |
| **[Priority Tier](/docs/en/api/service-tiers)** | Yes | Yes | Yes |
| **Comparative latency** | Fast | Fastest | Moderate |
| **Context window** | <Tooltip tooltipContent="~150K words \ ~680K unicode characters">200K tokens</Tooltip> / <br/> <Tooltip tooltipContent="~750K words \ ~3.4M unicode characters">1M tokens</Tooltip> (beta)<sup>3</sup> | <Tooltip tooltipContent="~150K words \ ~680K unicode characters">200K tokens</Tooltip> | <Tooltip tooltipContent="~150K words \ ~680K unicode characters">200K tokens</Tooltip> |
| **Max output** | 64K tokens | 64K tokens | 64K tokens |
| **Reliable knowledge cutoff** | Jan 2025<sup>4</sup> | Feb 2025 | May 2025<sup>4</sup> |
| **Training data cutoff** | Jul 2025 | Jul 2025 | Aug 2025 |

_<sup>1 - Aliases automatically point to the most recent model snapshot. When we release new model snapshots, we migrate aliases to point to the newest version of a model, typically within a week of the new release. While aliases are useful for experimentation, we recommend using specific model versions (e.g., `claude-sonnet-4-5-20250929`) in production applications to ensure consistent behavior.</sup>_

_<sup>2 - See our [pricing page](/docs/en/about-claude/pricing) for complete pricing information including batch API discounts, prompt caching rates, extended thinking costs, and vision processing fees.</sup>_

_<sup>3 - Claude Sonnet 4.5 supports a [1M token context window](/docs/en/build-with-claude/context-windows#1m-token-context-window) when using the `context-1m-2025-08-07` beta header. [Long context pricing](/docs/en/about-claude/pricing#long-context-pricing) applies to requests exceeding 200K tokens.</sup>_

_<sup>4 - **Reliable knowledge cutoff** indicates the date through which a model's knowledge is most extensive and reliable. **Training data cutoff** is the broader date range of training data used. For example, Claude Sonnet 4.5 was trained on publicly available information through July 2025, but its knowledge is most extensive and reliable through January 2025. For more information, see [Anthropic's Transparency Hub](https://www.anthropic.com/transparency).</sup>_

<Note>Models with the same snapshot date (e.g., 20240620) are identical across all platforms and do not change. The snapshot date in the model name ensures consistency and allows developers to rely on stable performance across different environments.</Note>

<Note>Starting with **Claude Sonnet 4.5 and all future models**, AWS Bedrock and Google Vertex AI offer two endpoint types: **global endpoints** (dynamic routing for maximum availability) and **regional endpoints** (guaranteed data routing through specific geographic regions). For more information, see the [third-party platform pricing section](/docs/en/about-claude/pricing#third-party-platform-pricing).</Note>

<section title="Legacy models">

The following models are still available but we recommend migrating to current models for improved performance:

| Feature | Claude Opus 4.1 | Claude Sonnet 4 | Claude Sonnet 3.7 | Claude Opus 4 | Claude Haiku 3 |
|:--------|:----------------|:----------------|:------------------|:--------------|:---------------|
| **Claude API ID** | claude-opus-4-1-20250805 | claude-sonnet-4-20250514 | claude-3-7-sonnet-20250219 | claude-opus-4-20250514 | claude-3-haiku-20240307 |
| **Claude API alias** | claude-opus-4-1 | claude-sonnet-4-0 | claude-3-7-sonnet-latest | claude-opus-4-0 | — |
| **AWS Bedrock ID** | anthropic.claude-opus-4-1-20250805-v1:0 | anthropic.claude-sonnet-4-20250514-v1:0 | anthropic.claude-3-7-sonnet-20250219-v1:0 | anthropic.claude-opus-4-20250514-v1:0 | anthropic.claude-3-haiku-20240307-v1:0 |
| **GCP Vertex AI ID** | claude-opus-4-1@20250805 | claude-sonnet-4@20250514 | claude-3-7-sonnet@20250219 | claude-opus-4@20250514 | claude-3-haiku@20240307 |
| **Pricing** | \$15 / input MTok<br/>\$75 / output MTok | \$3 / input MTok<br/>\$15 / output MTok | \$3 / input MTok<br/>\$15 / output MTok | \$15 / input MTok<br/>\$75 / output MTok | \$0.25 / input MTok<br/>\$1.25 / output MTok |
| **[Extended thinking](/docs/en/build-with-claude/extended-thinking)** | Yes | Yes | Yes | Yes | No |
| **[Priority Tier](/docs/en/api/service-tiers)** | Yes | Yes | Yes | Yes | No |
| **Comparative latency** | Moderate | Fast | Fast | Moderate | Fast |
| **Context window** | <Tooltip tooltipContent="~150K words \ ~680K unicode characters">200K tokens</Tooltip> | <Tooltip tooltipContent="~150K words \ ~680K unicode characters">200K tokens</Tooltip> / <br/> <Tooltip tooltipContent="~750K words \ ~3.4M unicode characters">1M tokens</Tooltip> (beta)<sup>1</sup> | <Tooltip tooltipContent="~150K words \ ~680K unicode characters">200K tokens</Tooltip> | <Tooltip tooltipContent="~150K words \ ~680K unicode characters">200K tokens</Tooltip> | <Tooltip tooltipContent="~150K words \ ~680K unicode characters">200K tokens</Tooltip> |
| **Max output** | 32K tokens | 64K tokens | 64K tokens / 128K tokens (beta)<sup>4</sup> | 32K tokens | 4K tokens |
| **Reliable knowledge cutoff** | Jan 2025<sup>2</sup> | Jan 2025<sup>2</sup> | Oct 2024<sup>2</sup> | Jan 2025<sup>2</sup> | <sup>3</sup> |
| **Training data cutoff** | Mar 2025 | Mar 2025 | Nov 2024 | Mar 2025 | Aug 2023 |

_<sup>1 - Claude Sonnet 4 supports a [1M token context window](/docs/en/build-with-claude/context-windows#1m-token-context-window) when using the `context-1m-2025-08-07` beta header. [Long context pricing](/docs/en/about-claude/pricing#long-context-pricing) applies to requests exceeding 200K tokens.</sup>_

_<sup>2 - **Reliable knowledge cutoff** indicates the date through which a model's knowledge is most extensive and reliable. **Training data cutoff** is the broader date range of training data used.</sup>_

_<sup>3 - Some Haiku models have a single training data cutoff date.</sup>_

_<sup>4 - Include the beta header `output-128k-2025-02-19` in your API request to increase the maximum output token length to 128K tokens for Claude Sonnet 3.7. We strongly suggest using our [streaming Messages API](/docs/en/build-with-claude/streaming) to avoid timeouts when generating longer outputs. See our guidance on [long requests](/docs/en/api/errors#long-requests) for more details.</sup>_

## Prompt and output performance

Claude 4 models excel in:
- **Performance**: Top-tier results in reasoning, coding, multilingual tasks, long-context handling, honesty, and image processing. See the [Claude 4 blog post](http://www.anthropic.com/news/claude-4) for more information.
- **Engaging responses**: Claude models are ideal for applications that require rich, human-like interactions.

- If you prefer more concise responses, you can adjust your prompts to guide the model toward the desired output length. Refer to our [prompt engineering guides](/docs/en/build-with-claude/prompt-engineering) for details.
    - For specific Claude 4 prompting best practices, see our [Claude 4 best practices guide](/docs/en/build-with-claude/prompt-engineering/claude-4-best-practices).
- **Output quality**: When migrating from previous model generations to Claude 4, you may notice larger improvements in overall performance.

## Migrating to Claude 4.5

If you're currently using Claude 3 models, we recommend migrating to Claude 4.5 to take advantage of improved intelligence and enhanced capabilities. For detailed migration instructions, see [Migrating to Claude 4.5](/docs/en/about-claude/models/migrating-to-claude-4).

## Get started with Claude

If you're ready to start exploring what Claude can do for you, let's dive in! Whether you're a developer looking to integrate Claude into your applications or a user wanting to experience the power of AI firsthand, we've got you covered.

<Note>Looking to chat with Claude? Visit [claude.ai](http://www.claude.ai)!</Note>

<CardGroup cols={3}>
  <Card title="Intro to Claude" icon="check" href="/docs/en/intro">
    Explore Claude's capabilities and development flow.
  </Card>
  <Card title="Quickstart" icon="lightning" href="/docs/en/get-started">
    Learn how to make your first API call in minutes.
  </Card>
  <Card title="Claude Console" icon="code" href="/">
    Craft and test powerful prompts directly in your browser.
  </Card>
</CardGroup>

If you have any questions or need assistance, don't hesitate to reach out to our [support team](https://support.claude.com/) or consult the [Discord community](https://www.anthropic.com/discord).

---

## Admin API overview

**URL:** llms-txt#admin-api-overview

**Contents:**
- How the Admin API works
- Organization roles and permissions
- Key concepts
  - Organization Members

<Tip>
**The Admin API is unavailable for individual accounts.** To collaborate with teammates and add members, set up your organization in **Console → Settings → Organization**.
</Tip>

The [Admin API](/docs/en/api/admin) allows you to programmatically manage your organization's resources, including organization members, workspaces, and API keys. This provides programmatic control over administrative tasks that would otherwise require manual configuration in the [Claude Console](/).

<Check>
  **The Admin API requires special access**

The Admin API requires a special Admin API key (starting with `sk-ant-admin...`) that differs from standard API keys. Only organization members with the admin role can provision Admin API keys through the Claude Console.
</Check>

## How the Admin API works

When you use the Admin API:

1. You make requests using your Admin API key in the `x-api-key` header
2. The API allows you to manage:
   - Organization members and their roles
   - Organization member invites
   - Workspaces and their members
   - API keys

This is useful for:
- Automating user onboarding/offboarding
- Programmatically managing workspace access
- Monitoring and managing API key usage

## Organization roles and permissions

There are five organization-level roles. See more details [here](https://support.claude.com/en/articles/10186004-api-console-roles-and-permissions).

| Role | Permissions |
|------|-------------|
| user | Can use Workbench |
| claude_code_user | Can use Workbench and [Claude Code](https://code.claude.com/docs/en/overview) |
| developer | Can use Workbench and manage API keys |
| billing | Can use Workbench and manage billing details |
| admin | Can do all of the above, plus manage users |

### Organization Members

You can list [organization members](/docs/en/api/admin-api/users/get-user), update member roles, and remove members.

<CodeGroup>
```bash Shell

---

## Prompt engineering overview

**URL:** llms-txt#prompt-engineering-overview

**Contents:**
- Before prompt engineering
- When to prompt engineer
- How to prompt engineer
- Prompt engineering tutorial

<Note>
While these tips apply broadly to all Claude models, you can find prompting tips specific to extended thinking models [here](/docs/en/build-with-claude/prompt-engineering/extended-thinking-tips).
</Note>

## Before prompt engineering

This guide assumes that you have:
1. A clear definition of the success criteria for your use case
2. Some ways to empirically test against those criteria
3. A first draft prompt you want to improve

If not, we highly suggest you spend time establishing that first. Check out [Define your success criteria](/docs/en/test-and-evaluate/define-success) and [Create strong empirical evaluations](/docs/en/test-and-evaluate/develop-tests) for tips and guidance.

<Card title="Prompt generator" icon="link" href="/dashboard">
  Don't have a first draft prompt? Try the prompt generator in the Claude Console!
</Card>

## When to prompt engineer

This guide focuses on success criteria that are controllable through prompt engineering.
  Not every success criteria or failing eval is best solved by prompt engineering. For example, latency and cost can be sometimes more easily improved by selecting a different model.

<section title="Prompting vs. finetuning">

Prompt engineering is far faster than other methods of model behavior control, such as finetuning, and can often yield leaps in performance in far less time. Here are some reasons to consider prompt engineering over finetuning:<br/>
  - **Resource efficiency**: Fine-tuning requires high-end GPUs and large memory, while prompt engineering only needs text input, making it much more resource-friendly.
  - **Cost-effectiveness**: For cloud-based AI services, fine-tuning incurs significant costs. Prompt engineering uses the base model, which is typically cheaper.
  - **Maintaining model updates**: When providers update models, fine-tuned versions might need retraining. Prompts usually work across versions without changes.
  - **Time-saving**: Fine-tuning can take hours or even days. In contrast, prompt engineering provides nearly instantaneous results, allowing for quick problem-solving.
  - **Minimal data needs**: Fine-tuning needs substantial task-specific, labeled data, which can be scarce or expensive. Prompt engineering works with few-shot or even zero-shot learning.
  - **Flexibility & rapid iteration**: Quickly try various approaches, tweak prompts, and see immediate results. This rapid experimentation is difficult with fine-tuning.
  - **Domain adaptation**: Easily adapt models to new domains by providing domain-specific context in prompts, without retraining.
  - **Comprehension improvements**: Prompt engineering is far more effective than finetuning at helping models better understand and utilize external content such as retrieved documents
  - **Preserves general knowledge**: Fine-tuning risks catastrophic forgetting, where the model loses general knowledge. Prompt engineering maintains the model's broad capabilities.
  - **Transparency**: Prompts are human-readable, showing exactly what information the model receives. This transparency aids in understanding and debugging.

## How to prompt engineer

The prompt engineering pages in this section have been organized from most broadly effective techniques to more specialized techniques. When troubleshooting performance, we suggest you try these techniques in order, although the actual impact of each technique will depend on your use case.
1. [Prompt generator](/docs/en/build-with-claude/prompt-engineering/prompt-generator)
2. [Be clear and direct](/docs/en/build-with-claude/prompt-engineering/be-clear-and-direct)
3. [Use examples (multishot)](/docs/en/build-with-claude/prompt-engineering/multishot-prompting)
4. [Let Claude think (chain of thought)](/docs/en/build-with-claude/prompt-engineering/chain-of-thought)
5. [Use XML tags](/docs/en/build-with-claude/prompt-engineering/use-xml-tags)
6. [Give Claude a role (system prompts)](/docs/en/build-with-claude/prompt-engineering/system-prompts)
7. [Prefill Claude's response](/docs/en/build-with-claude/prompt-engineering/prefill-claudes-response)
8. [Chain complex prompts](/docs/en/build-with-claude/prompt-engineering/chain-prompts)
9. [Long context tips](/docs/en/build-with-claude/prompt-engineering/long-context-tips)

## Prompt engineering tutorial

If you're an interactive learner, you can dive into our interactive tutorials instead!

<CardGroup cols={2}>
  <Card title="GitHub prompting tutorial" icon="link" href="https://github.com/anthropics/prompt-eng-interactive-tutorial">
    An example-filled tutorial that covers the prompt engineering concepts found in our docs.
  </Card>
  <Card title="Google Sheets prompting tutorial" icon="link" href="https://docs.google.com/spreadsheets/d/19jzLgRruG9kjUQNKtCg1ZjdD6l6weA6qRXG5zLIAhC8">
    A lighter weight version of our prompt engineering tutorial via an interactive spreadsheet.
  </Card>
</CardGroup>

---

## Agent SDK overview

**URL:** llms-txt#agent-sdk-overview

**Contents:**
- Capabilities
  - Claude Code features
- Get started
- Compare the Agent SDK to other Claude tools
- Changelog
- Reporting bugs
- Branding guidelines
- License and terms
- Next steps

Build production AI agents with Claude Code as a library

<Note>
The Claude Code SDK has been renamed to the Claude Agent SDK. If you're migrating from the old SDK, see the [Migration Guide](/docs/en/agent-sdk/migration-guide).
</Note>

Build AI agents that autonomously read files, run commands, search the web, edit code, and more. The Agent SDK gives you the same tools, agent loop, and context management that power Claude Code, programmable in Python and TypeScript.

The Agent SDK includes built-in tools for reading files, running commands, and editing code, so your agent can start working immediately without you implementing tool execution. Dive into the quickstart or explore real agents built with the SDK:

<CardGroup cols={2}>
  <Card title="Quickstart" icon="play" href="/docs/en/agent-sdk/quickstart">
    Build a bug-fixing agent in minutes
  </Card>
  <Card title="Example agents" icon="star" href="https://github.com/anthropics/claude-agent-sdk-demos">
    Email assistant, research agent, and more
  </Card>
</CardGroup>

Everything that makes Claude Code powerful is available in the SDK:

<Tabs>
  <Tab title="Built-in tools">
    Your agent can read files, run commands, and search codebases out of the box. Key tools include:

| Tool | What it does |
    |------|--------------|
    | **Read** | Read any file in the working directory |
    | **Write** | Create new files |
    | **Edit** | Make precise edits to existing files |
    | **Bash** | Run terminal commands, scripts, git operations |
    | **Glob** | Find files by pattern (`**/*.ts`, `src/**/*.py`) |
    | **Grep** | Search file contents with regex |
    | **WebSearch** | Search the web for current information |
    | **WebFetch** | Fetch and parse web page content |
    | **[AskUserQuestion](/docs/en/agent-sdk/user-input#handle-clarifying-questions)** | Ask the user clarifying questions with multiple choice options |

This example creates an agent that searches your codebase for TODO comments:

</Tab>
  <Tab title="Hooks">
    Run custom code at key points in the agent lifecycle. SDK hooks use callback functions to validate, log, block, or transform agent behavior.

**Available hooks:** `PreToolUse`, `PostToolUse`, `Stop`, `SessionStart`, `SessionEnd`, `UserPromptSubmit`, and more.

This example logs all file changes to an audit file:

[Learn more about hooks →](/docs/en/agent-sdk/hooks)
  </Tab>
  <Tab title="Subagents">
    Spawn specialized agents to handle focused subtasks. Your main agent delegates work, and subagents report back with results.

Define custom agents with specialized instructions. Include `Task` in `allowedTools` since subagents are invoked via the Task tool:

Messages from within a subagent's context include a `parent_tool_use_id` field, letting you track which messages belong to which subagent execution.

[Learn more about subagents →](/docs/en/agent-sdk/subagents)
  </Tab>
  <Tab title="MCP">
    Connect to external systems via the Model Context Protocol: databases, browsers, APIs, and [hundreds more](https://github.com/modelcontextprotocol/servers).

This example connects the [Playwright MCP server](https://github.com/microsoft/playwright-mcp) to give your agent browser automation capabilities:

[Learn more about MCP →](/docs/en/agent-sdk/mcp)
  </Tab>
  <Tab title="Permissions">
    Control exactly which tools your agent can use. Allow safe operations, block dangerous ones, or require approval for sensitive actions.

<Note>
    For interactive approval prompts and the `AskUserQuestion` tool, see [Handle approvals and user input](/docs/en/agent-sdk/user-input).
    </Note>

This example creates a read-only agent that can analyze but not modify code:

[Learn more about permissions →](/docs/en/agent-sdk/permissions)
  </Tab>
  <Tab title="Sessions">
    Maintain context across multiple exchanges. Claude remembers files read, analysis done, and conversation history. Resume sessions later, or fork them to explore different approaches.

This example captures the session ID from the first query, then resumes to continue with full context:

[Learn more about sessions →](/docs/en/agent-sdk/sessions)
  </Tab>
</Tabs>

### Claude Code features

The SDK also supports Claude Code's filesystem-based configuration. To use these features, set `setting_sources=["project"]` (Python) or `settingSources: ['project']` (TypeScript)  in your options.

| Feature | Description | Location |
|---------|-------------|----------|
| [Skills](/docs/en/agent-sdk/skills) | Specialized capabilities defined in Markdown | `.claude/skills/SKILL.md` |
| [Slash commands](/docs/en/agent-sdk/slash-commands) | Custom commands for common tasks | `.claude/commands/*.md` |
| [Memory](/docs/en/agent-sdk/modifying-system-prompts) | Project context and instructions | `CLAUDE.md` or `.claude/CLAUDE.md` |
| [Plugins](/docs/en/agent-sdk/plugins) | Extend with custom commands, agents, and MCP servers | Programmatic via `plugins` option |

<Steps>
  <Step title="Install Claude Code">
    The SDK uses Claude Code as its runtime:

<Tabs>
      <Tab title="macOS/Linux/WSL">
        
      </Tab>
      <Tab title="Homebrew">
        
      </Tab>
      <Tab title="WinGet">
        
      </Tab>
    </Tabs>

See [Claude Code setup](https://code.claude.com/docs/en/setup) for Windows and other options.
  </Step>
  <Step title="Install the SDK">
    <Tabs>
      <Tab title="TypeScript">
        
      </Tab>
      <Tab title="Python">
        
      </Tab>
    </Tabs>
  </Step>
  <Step title="Set your API key">
    
    Get your key from the [Console](https://platform.claude.com/).

The SDK also supports authentication via third-party API providers:

- **Amazon Bedrock**: set `CLAUDE_CODE_USE_BEDROCK=1` environment variable and configure AWS credentials
    - **Google Vertex AI**: set `CLAUDE_CODE_USE_VERTEX=1` environment variable and configure Google Cloud credentials
    - **Microsoft Foundry**: set `CLAUDE_CODE_USE_FOUNDRY=1` environment variable and configure Azure credentials

<Note>
    Unless previously approved, we do not allow third party developers to offer Claude.ai login or rate limits for their products, including agents built on the Claude Agent SDK. Please use the API key authentication methods described in this document instead.
    </Note>
  </Step>
  <Step title="Run your first agent">
    This example creates an agent that lists files in your current directory using built-in tools.

</CodeGroup>
  </Step>
</Steps>

**Ready to build?** Follow the [Quickstart](/docs/en/agent-sdk/quickstart) to create an agent that finds and fixes bugs in minutes.

## Compare the Agent SDK to other Claude tools

The Claude platform offers multiple ways to build with Claude. Here's how the Agent SDK fits in:

<Tabs>
  <Tab title="Agent SDK vs Client SDK">
    The [Anthropic Client SDK](/docs/en/api/client-sdks) gives you direct API access: you send prompts and implement tool execution yourself. The **Agent SDK** gives you Claude with built-in tool execution.

With the Client SDK, you implement a tool loop. With the Agent SDK, Claude handles it:

</CodeGroup>
  </Tab>
  <Tab title="Agent SDK vs Claude Code CLI">
    Same capabilities, different interface:

| Use case | Best choice |
    |----------|-------------|
    | Interactive development | CLI |
    | CI/CD pipelines | SDK |
    | Custom applications | SDK |
    | One-off tasks | CLI |
    | Production automation | SDK |

Many teams use both: CLI for daily development, SDK for production. Workflows translate directly between them.
  </Tab>
</Tabs>

View the full changelog for SDK updates, bug fixes, and new features:

- **TypeScript SDK**: [view CHANGELOG.md](https://github.com/anthropics/claude-agent-sdk-typescript/blob/main/CHANGELOG.md)
- **Python SDK**: [view CHANGELOG.md](https://github.com/anthropics/claude-agent-sdk-python/blob/main/CHANGELOG.md)

If you encounter bugs or issues with the Agent SDK:

- **TypeScript SDK**: [report issues on GitHub](https://github.com/anthropics/claude-agent-sdk-typescript/issues)
- **Python SDK**: [report issues on GitHub](https://github.com/anthropics/claude-agent-sdk-python/issues)

## Branding guidelines

For partners integrating the Claude Agent SDK, use of Claude branding is optional. When referencing Claude in your product:

**Allowed:**
- "Claude Agent" (preferred for dropdown menus)
- "Claude" (when within a menu already labeled "Agents")
- "{YourAgentName} Powered by Claude" (if you have an existing agent name)

**Not permitted:**
- "Claude Code" or "Claude Code Agent"
- Claude Code-branded ASCII art or visual elements that mimic Claude Code

Your product should maintain its own branding and not appear to be Claude Code or any Anthropic product. For questions about branding compliance, contact our [sales team](https://www.anthropic.com/contact-sales).

Use of the Claude Agent SDK is governed by [Anthropic's Commercial Terms of Service](https://www.anthropic.com/legal/commercial-terms), including when you use it to power products and services that you make available to your own customers and end users, except to the extent a specific component or dependency is covered by a different license as indicated in that component's LICENSE file.

<CardGroup cols={2}>
  <Card title="Quickstart" icon="play" href="/docs/en/agent-sdk/quickstart">
    Build an agent that finds and fixes bugs in minutes
  </Card>
  <Card title="Example agents" icon="star" href="https://github.com/anthropics/claude-agent-sdk-demos">
    Email assistant, research agent, and more
  </Card>
  <Card title="TypeScript SDK" icon="code" href="/docs/en/agent-sdk/typescript">
    Full TypeScript API reference and examples
  </Card>
  <Card title="Python SDK" icon="code" href="/docs/en/agent-sdk/python">
    Full Python API reference and examples
  </Card>
</CardGroup>

**Examples:**

Example 1 (unknown):
```unknown

```

Example 2 (unknown):
```unknown
</CodeGroup>

The Agent SDK includes built-in tools for reading files, running commands, and editing code, so your agent can start working immediately without you implementing tool execution. Dive into the quickstart or explore real agents built with the SDK:

<CardGroup cols={2}>
  <Card title="Quickstart" icon="play" href="/docs/en/agent-sdk/quickstart">
    Build a bug-fixing agent in minutes
  </Card>
  <Card title="Example agents" icon="star" href="https://github.com/anthropics/claude-agent-sdk-demos">
    Email assistant, research agent, and more
  </Card>
</CardGroup>

## Capabilities

Everything that makes Claude Code powerful is available in the SDK:

<Tabs>
  <Tab title="Built-in tools">
    Your agent can read files, run commands, and search codebases out of the box. Key tools include:

    | Tool | What it does |
    |------|--------------|
    | **Read** | Read any file in the working directory |
    | **Write** | Create new files |
    | **Edit** | Make precise edits to existing files |
    | **Bash** | Run terminal commands, scripts, git operations |
    | **Glob** | Find files by pattern (`**/*.ts`, `src/**/*.py`) |
    | **Grep** | Search file contents with regex |
    | **WebSearch** | Search the web for current information |
    | **WebFetch** | Fetch and parse web page content |
    | **[AskUserQuestion](/docs/en/agent-sdk/user-input#handle-clarifying-questions)** | Ask the user clarifying questions with multiple choice options |

    This example creates an agent that searches your codebase for TODO comments:

    <CodeGroup>
```

Example 3 (unknown):
```unknown

```

Example 4 (unknown):
```unknown
</CodeGroup>

  </Tab>
  <Tab title="Hooks">
    Run custom code at key points in the agent lifecycle. SDK hooks use callback functions to validate, log, block, or transform agent behavior.

    **Available hooks:** `PreToolUse`, `PostToolUse`, `Stop`, `SessionStart`, `SessionEnd`, `UserPromptSubmit`, and more.

    This example logs all file changes to an audit file:

    <CodeGroup>
```

---

## Client SDKs

**URL:** llms-txt#client-sdks

**Contents:**
- Python
- TypeScript
- Java
- Go
- C#
- Ruby
- PHP
- Beta namespace in client SDKs

We provide client libraries in a number of popular languages that make it easier to work with the Claude API.

This page includes brief installation instructions and links to the open-source GitHub repositories for Anthropic's Client SDKs. For basic usage instructions, see the [API reference](/docs/en/api/overview) For detailed usage instructions, refer to each SDK's GitHub repository.

<Note>
Additional configuration is needed to use Anthropic's Client SDKs through a partner platform. If you are using Amazon Bedrock, see [this guide](/docs/en/build-with-claude/claude-on-amazon-bedrock); if you are using Google Cloud Vertex AI, see [this guide](/docs/en/build-with-claude/claude-on-vertex-ai); if you are using Microsoft Foundry, see [this guide](/docs/en/build-with-claude/claude-in-microsoft-foundry).
</Note>

[Python library GitHub repo](https://github.com/anthropics/anthropic-sdk-python)

**Requirements:** Python 3.8+

[TypeScript library GitHub repo](https://github.com/anthropics/anthropic-sdk-typescript)

<Info>
While this library is in TypeScript, it can also be used in JavaScript libraries.
</Info>

[Java library GitHub repo](https://github.com/anthropics/anthropic-sdk-java)

**Requirements:** Java 8 or later

[Go library GitHub repo](https://github.com/anthropics/anthropic-sdk-go)

**Requirements:** Go 1.22+

[C# library GitHub repo](https://github.com/anthropics/anthropic-sdk-csharp)

<Info>
The C# SDK is currently in beta.
</Info>

**Requirements:** .NET 8 or later

[Ruby library GitHub repo](https://github.com/anthropics/anthropic-sdk-ruby)

**Requirements:** Ruby 3.2.0 or later

[PHP library GitHub repo](https://github.com/anthropics/anthropic-sdk-php)

<Info>
The PHP SDK is currently in beta.
</Info>

**Requirements:** PHP 8.1.0 or higher

## Beta namespace in client SDKs

Every SDK has a `beta` namespace that is available for accessing new features that Anthropic releases in beta versions. Use this in conjunction with [beta headers](/docs/en/api/beta-headers) to access these features. Refer to each SDK's GitHub repository for specific usage examples.

**Examples:**

Example 1 (bash):
```bash
pip install anthropic
```

Example 2 (bash):
```bash
npm install @anthropic-ai/sdk
```

Example 3 (groovy):
```groovy
implementation("com.anthropic:anthropic-java:2.10.0")
```

Example 4 (xml):
```xml
<dependency>
    <groupId>com.anthropic</groupId>
    <artifactId>anthropic-java</artifactId>
    <version>2.10.0</version>
</dependency>
```

---

## Overview

**URL:** llms-txt#overview

URL: https://platform.claude.com/docs/en/resources/overview

<h2}>
    Model cards
  </h2>

<Card title="Claude Opus 4.5 System Card" icon="file" href="http://www.anthropic.com/claude-opus-4-5-system-card">
      Detailed documentation of Claude Opus 4.5.
    </Card>

<Card title="Claude Haiku 4.5 System Card" icon="file" href="http://www.anthropic.com/claude-haiku-4-5-system-card">
      Detailed documentation of Claude Haiku 4.5.
    </Card>

<Card title="Claude Sonnet 4.5 System Card" icon="file" href="http://www.anthropic.com/claude-sonnet-4-5-system-card">
      Detailed documentation of Claude Sonnet 4.5.
    </Card>

<Card title="Claude Opus 4.1 System Card" icon="file" href="http://www.anthropic.com/claude-opus-4-1-system-card">
      Detailed documentation of Claude Opus 4.1.
    </Card>

<Card title="Claude 4 System Card" icon="file" href="https://www-cdn.anthropic.com/6be99a52cb68eb70eb9572b4cafad13df32ed995.pdf">
      Detailed documentation of Claude 4 models.
    </Card>

<Card title="Claude Sonnet 3.7 System Card" icon="file" href="https://anthropic.com/claude-3-7-sonnet-system-card">
      System card for Claude Sonnet 3.7 with performance and safety details.
    </Card>

<Card title="Claude 3 Model Card" icon="file" href="https://assets.anthropic.com/m/61e7d27f8c8f5919/original/Claude-3-Model-Card.pdf">
      Detailed documentation of Claude 3 models including latest 3.5 addendum.
    </Card>

<h2}>
    Learning resources
  </h2>

<Card title="Quickstarts" icon="lightning" href="https://github.com/anthropics/anthropic-quickstarts">
      Deployable applications built with our API.
    </Card>

<Card title="Courses" icon="graduation-cap" href="https://anthropic.skilljar.com/">
      Step by step lessons on building with Claude.
    </Card>

<Card title="Cookbook" icon="fork-knife" href="https://platform.claude.com/cookbooks">
      Replicable code samples and implementations.
    </Card>

<Card title="Prompt library" icon="book" href="/docs/en/resources/prompt-library/library">
      Explore optimized prompts for a breadth of business and personal tasks.
    </Card>

<Card title="Use case guides" icon="compass" href="/docs/en/about-claude/use-case-guides/overview">
      In-depth production guides for building common use cases with Claude.
    </Card>

<Card title="Glossary" icon="book-bookmark" href="/docs/en/about-claude/glossary">
      Key terms and concepts for working with Claude and language models.
    </Card>

<h2}>
    Resources for AI ingestion
  </h2>

<Card title="API primer for Claude ingestion" icon="settings" href="/docs/en/claude_api_primer.md">
      Concise API guide meant for ingestion by Claude.
    </Card>

<Card title="Claude API Docs Overview" icon="robot" href="/docs/for-claude">
      Concise overview of Claude API documentation, optimized for LLM ingestion.
    </Card>

<Card title="llms.txt" icon="file" href="/llms.txt">
      LLM-optimized documentation index.
    </Card>

<Card title="llms-full.txt" icon="file" href="/llms-full.txt">
      Complete LLM-optimized documentation.
    </Card>

---
