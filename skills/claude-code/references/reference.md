# Claude-Code - Reference

**Pages:** 46

---

## Structured outputs in the SDK

**URL:** llms-txt#structured-outputs-in-the-sdk

**Contents:**
- Why use structured outputs
- Quick start
- Defining schemas with Zod
- Quick start
- Defining schemas with Pydantic

Get validated JSON results from agent workflows

Get structured, validated JSON from agent workflows. The Agent SDK supports structured outputs through JSON Schemas, ensuring your agents return data in exactly the format you need.

<Note>
**When to use structured outputs**

Use structured outputs when you need validated JSON after an agent completes a multi-turn workflow with tools (file searches, command execution, web research, etc.).

For single API calls without tool use, see [API Structured Outputs](/docs/en/build-with-claude/structured-outputs).
</Note>

## Why use structured outputs

Structured outputs provide reliable, type-safe integration with your applications:

- **Validated structure**: Always receive valid JSON matching your schema
- **Simplified integration**: No parsing or validation code needed
- **Type safety**: Use with TypeScript or Python type hints for end-to-end safety
- **Clean separation**: Define output requirements separately from task instructions
- **Tool autonomy**: Agent chooses which tools to use while guaranteeing output format

<Tabs>
<Tab title="TypeScript">

## Defining schemas with Zod

For TypeScript projects, use Zod for type-safe schema definition and validation:

**Benefits of Zod:**
- Full TypeScript type inference
- Runtime validation with `safeParse()`
- Better error messages
- Composable schemas

</Tab>
<Tab title="Python">

## Defining schemas with Pydantic

For Python projects, use Pydantic for type-safe schema definition and validation:

```python
from pydantic import BaseModel
from claude_agent_sdk import query

class Issue(BaseModel):
    severity: str  # 'low', 'medium', 'high'
    description: str
    file: str

class AnalysisResult(BaseModel):
    summary: str
    issues: list[Issue]
    score: int

**Examples:**

Example 1 (typescript):
```typescript
import { query } from '@anthropic-ai/claude-agent-sdk'

const schema = {
  type: 'object',
  properties: {
    company_name: { type: 'string' },
    founded_year: { type: 'number' },
    headquarters: { type: 'string' }
  },
  required: ['company_name']
}

for await (const message of query({
  prompt: 'Research Anthropic and provide key company information',
  options: {
    outputFormat: {
      type: 'json_schema',
      schema: schema
    }
  }
})) {
  if (message.type === 'result' && message.structured_output) {
    console.log(message.structured_output)
    // { company_name: "Anthropic", founded_year: 2021, headquarters: "San Francisco, CA" }
  }
}
```

Example 2 (typescript):
```typescript
import { z } from 'zod'
import { zodToJsonSchema } from 'zod-to-json-schema'

// Define schema with Zod
const AnalysisResult = z.object({
  summary: z.string(),
  issues: z.array(z.object({
    severity: z.enum(['low', 'medium', 'high']),
    description: z.string(),
    file: z.string()
  })),
  score: z.number().min(0).max(100)
})

type AnalysisResult = z.infer<typeof AnalysisResult>

// Convert to JSON Schema
const schema = zodToJsonSchema(AnalysisResult, { $refStrategy: 'root' })

// Use in query
for await (const message of query({
  prompt: 'Analyze the codebase for security issues',
  options: {
    outputFormat: {
      type: 'json_schema',
      schema: schema
    }
  }
})) {
  if (message.type === 'result' && message.structured_output) {
    // Validate and get fully typed result
    const parsed = AnalysisResult.safeParse(message.structured_output)
    if (parsed.success) {
      const data: AnalysisResult = parsed.data
      console.log(`Score: ${data.score}`)
      console.log(`Found ${data.issues.length} issues`)
      data.issues.forEach(issue => {
        console.log(`[${issue.severity}] ${issue.file}: ${issue.description}`)
      })
    }
  }
}
```

Example 3 (python):
```python
from claude_agent_sdk import query

schema = {
    "type": "object",
    "properties": {
        "company_name": {"type": "string"},
        "founded_year": {"type": "number"},
        "headquarters": {"type": "string"}
    },
    "required": ["company_name"]
}

async for message in query(
    prompt="Research Anthropic and provide key company information",
    options={
        "output_format": {
            "type": "json_schema",
            "schema": schema
        }
    }
):
    if hasattr(message, 'structured_output'):
        print(message.structured_output)
        # {'company_name': 'Anthropic', 'founded_year': 2021, 'headquarters': 'San Francisco, CA'}
```

---

## PDF Processing

**URL:** llms-txt#pdf-processing

**Contents:**
- Quick start
- Advanced features

Extract text with pdfplumber:

**Form filling**: See [FORMS.md](FORMS.md) for complete guide
**API reference**: See [REFERENCE.md](REFERENCE.md) for all methods
**Examples**: See [EXAMPLES.md](EXAMPLES.md) for common patterns

bigquery-skill/
├── SKILL.md (overview and navigation)
└── reference/
    ├── finance.md (revenue, billing metrics)
    ├── sales.md (opportunities, pipeline)
    ├── product.md (API usage, features)
    └── marketing.md (campaigns, attribution)
`markdown SKILL.md

**Examples:**

Example 1 (python):
```python
import pdfplumber
with pdfplumber.open("file.pdf") as pdf:
    text = pdf.pages[0].extract_text()
```

Example 2 (unknown):
```unknown
Claude loads FORMS.md, REFERENCE.md, or EXAMPLES.md only when needed.

#### Pattern 2: Domain-specific organization

For Skills with multiple domains, organize content by domain to avoid loading irrelevant context. When a user asks about sales metrics, Claude only needs to read sales-related schemas, not finance or marketing data. This keeps token usage low and context focused.
```

Example 3 (unknown):
```unknown

```

---

## Context editing

**URL:** llms-txt#context-editing

**Contents:**
- Overview
- Server-side strategies
  - Tool result clearing
  - Thinking block clearing
- Supported models
- Tool result clearing usage
  - Advanced configuration
- Thinking block clearing usage
  - Configuration options for thinking block clearing
  - Combining strategies

Automatically manage conversation context as it grows with context editing.

Context editing allows you to automatically manage conversation context as it grows, helping you optimize costs and stay within context window limits. You can use server-side API strategies, client-side SDK features, or both together.

| Approach | Where it runs | Strategies | How it works |
|----------|---------------|------------|--------------|
| **Server-side** | API | Tool result clearing (`clear_tool_uses_20250919`)<br/>Thinking block clearing (`clear_thinking_20251015`) | Applied before the prompt reaches Claude. Clears specific content from conversation history. Each strategy can be configured independently. |
| **Client-side** | SDK | Compaction | Available in [Python and TypeScript SDKs](/docs/en/api/client-sdks) when using [`tool_runner`](/docs/en/agents-and-tools/tool-use/implement-tool-use#tool-runner-beta). Generates a summary and replaces full conversation history. See [Compaction](#client-side-compaction-sdk) below. |

## Server-side strategies

<Note>
Context editing is currently in beta with support for tool result clearing and thinking block clearing. To enable it, use the beta header `context-management-2025-06-27` in your API requests.

Please reach out through our [feedback form](https://forms.gle/YXC2EKGMhjN1c4L88) to share your feedback on this feature.
</Note>

### Tool result clearing

The `clear_tool_uses_20250919` strategy clears tool results when conversation context grows beyond your configured threshold. When activated, the API automatically clears the oldest tool results in chronological order, replacing them with placeholder text to let Claude know the tool result was removed. By default, only tool results are cleared. You can optionally clear both tool results and tool calls (the tool use parameters) by setting `clear_tool_inputs` to true.

### Thinking block clearing

The `clear_thinking_20251015` strategy manages `thinking` blocks in conversations when extended thinking is enabled. This strategy automatically clears older thinking blocks from previous turns.

<Tip>
**Default behavior**: When extended thinking is enabled without configuring the `clear_thinking_20251015` strategy, the API automatically keeps only the thinking blocks from the last assistant turn (equivalent to `keep: {type: "thinking_turns", value: 1}`).

To maximize cache hits, preserve all thinking blocks by setting `keep: "all"`.
</Tip>

<Note>
An assistant conversation turn may include multiple content blocks (e.g. when using tools) and multiple thinking blocks (e.g. with [interleaved thinking](/docs/en/build-with-claude/extended-thinking#interleaved-thinking)).
</Note>

<Tip>
**Context editing happens server-side**

Context editing is applied **server-side** before the prompt reaches Claude. Your client application maintains the full, unmodified conversation history—you do not need to sync your client state with the edited version. Continue managing your full conversation history locally as you normally would.
</Tip>

<Tip>
**Context editing and prompt caching**

Context editing's interaction with [prompt caching](/docs/en/build-with-claude/prompt-caching) varies by strategy:

- **Tool result clearing**: Invalidates cached prompt prefixes when content is cleared. To account for this, we recommend clearing enough tokens to make the cache invalidation worthwhile. Use the `clear_at_least` parameter to ensure a minimum number of tokens is cleared each time. You'll incur cache write costs each time content is cleared, but subsequent requests can reuse the newly cached prefix.

- **Thinking block clearing**: When thinking blocks are **kept** in context (not cleared), the prompt cache is preserved, enabling cache hits and reducing input token costs. When thinking blocks are **cleared**, the cache is invalidated at the point where clearing occurs. Configure the `keep` parameter based on whether you want to prioritize cache performance or context window availability.
</Tip>

Context editing is available on:

- Claude Opus 4.5 (`claude-opus-4-5-20251101`)
- Claude Opus 4.1 (`claude-opus-4-1-20250805`)
- Claude Opus 4 (`claude-opus-4-20250514`)
- Claude Sonnet 4.5 (`claude-sonnet-4-5-20250929`)
- Claude Sonnet 4 (`claude-sonnet-4-20250514`)
- Claude Haiku 4.5 (`claude-haiku-4-5-20251001`)

## Tool result clearing usage

The simplest way to enable tool result clearing is to specify only the strategy type, as all other [configuration options](#configuration-options-for-tool-result-clearing) will use their default values:

### Advanced configuration

You can customize the tool result clearing behavior with additional parameters:

## Thinking block clearing usage

Enable thinking block clearing to manage context and prompt caching effectively when extended thinking is enabled:

### Configuration options for thinking block clearing

The `clear_thinking_20251015` strategy supports the following configuration:

| Configuration option | Default | Description |
|---------------------|---------|-------------|
| `keep` | `{type: "thinking_turns", value: 1}` | Defines how many recent assistant turns with thinking blocks to preserve. Use `{type: "thinking_turns", value: N}` where N must be > 0 to keep the last N turns, or `"all"` to keep all thinking blocks. |

**Example configurations:**

### Combining strategies

You can use both thinking block clearing and tool result clearing together:

<Note>
When using multiple strategies, the `clear_thinking_20251015` strategy must be listed first in the `edits` array.
</Note>

## Configuration options for tool result clearing

| Configuration option | Default | Description |
|---------------------|---------|-------------|
| `trigger` | 100,000 input tokens | Defines when the context editing strategy activates. Once the prompt exceeds this threshold, clearing will begin. You can specify this value in either `input_tokens` or `tool_uses`. |
| `keep` | 3 tool uses | Defines how many recent tool use/result pairs to keep after clearing occurs. The API removes the oldest tool interactions first, preserving the most recent ones. |
| `clear_at_least` | None | Ensures a minimum number of tokens is cleared each time the strategy activates. If the API can't clear at least the specified amount, the strategy will not be applied. This helps determine if context clearing is worth breaking your prompt cache. |
| `exclude_tools` | None | List of tool names whose tool uses and results should never be cleared. Useful for preserving important context. |
| `clear_tool_inputs` | `false` | Controls whether the tool call parameters are cleared along with the tool results. By default, only the tool results are cleared while keeping Claude's original tool calls visible. |

## Context editing response

You can see which context edits were applied to your request using the `context_management` response field, along with helpful statistics about the content and input tokens cleared.

For streaming responses, the context edits will be included in the final `message_delta` event:

The [token counting](/docs/en/build-with-claude/token-counting) endpoint supports context management, allowing you to preview how many tokens your prompt will use after context editing is applied.

The response shows both the final token count after context management is applied (`input_tokens`) and the original token count before any clearing occurred (`original_input_tokens`).

## Using with the Memory Tool

Context editing can be combined with the [memory tool](/docs/en/agents-and-tools/tool-use/memory-tool). When your conversation context approaches the configured clearing threshold, Claude receives an automatic warning to preserve important information. This enables Claude to save tool results or context to its memory files before they're cleared from the conversation history.

This combination allows you to:

- **Preserve important context**: Claude can write essential information from tool results to memory files before those results are cleared
- **Maintain long-running workflows**: Enable agentic workflows that would otherwise exceed context limits by offloading information to persistent storage
- **Access information on demand**: Claude can look up previously cleared information from memory files when needed, rather than keeping everything in the active context window

For example, in a file editing workflow where Claude performs many operations, Claude can summarize completed changes to memory files as the context grows. When tool results are cleared, Claude retains access to that information through its memory system and can continue working effectively.

To use both features together, enable them in your API request:

## Client-side compaction (SDK)

<Note>
Compaction is available in the [Python and TypeScript SDKs](/docs/en/api/client-sdks) when using the [`tool_runner` method](/docs/en/agents-and-tools/tool-use/implement-tool-use#tool-runner-beta).
</Note>

Compaction is an SDK feature that automatically manages conversation context by generating summaries when token usage grows too large. Unlike server-side context editing strategies that clear content, compaction instructs Claude to summarize the conversation history, then replaces the full history with that summary. This allows Claude to continue working on long-running tasks that would otherwise exceed the [context window](/docs/en/build-with-claude/context-windows).

### How compaction works

When compaction is enabled, the SDK monitors token usage after each model response:

1. **Threshold check**: The SDK calculates total tokens as `input_tokens + cache_creation_input_tokens + cache_read_input_tokens + output_tokens`
2. **Summary generation**: When the threshold is exceeded, a summary prompt is injected as a user turn, and Claude generates a structured summary wrapped in `<summary></summary>` tags
3. **Context replacement**: The SDK extracts the summary and replaces the entire message history with it
4. **Continuation**: The conversation resumes from the summary, with Claude picking up where it left off

Add `compaction_control` to your `tool_runner` call:

#### What happens during compaction

As the conversation grows, the message history accumulates:

**Before compaction (approaching 100k tokens):**

When tokens exceed the threshold, the SDK injects a summary request and Claude generates a summary. The entire history is then replaced:

**After compaction (back to ~2-3k tokens):**

Claude continues working from this summary as if it were the original conversation history.

### Configuration options

| Parameter | Type | Required | Default | Description |
|-----------|------|----------|---------|-------------|
| `enabled` | boolean | Yes | - | Whether to enable automatic compaction |
| `context_token_threshold` | number | No | 100,000 | Token count at which compaction triggers |
| `model` | string | No | Same as main model | Model to use for generating summaries |
| `summary_prompt` | string | No | See below | Custom prompt for summary generation |

#### Choosing a token threshold

The threshold determines when compaction occurs. A lower threshold means more frequent compactions with smaller context windows. A higher threshold allows more context but risks hitting limits.

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

### Advanced configuration

You can customize the tool result clearing behavior with additional parameters:

<CodeGroup>
```

Example 4 (unknown):
```unknown

```

---

## Project Guidelines

**URL:** llms-txt#project-guidelines

**Contents:**
- Code Style
- Testing
- Commands

- Use TypeScript strict mode
- Prefer functional components in React
- Always include JSDoc comments for public APIs

- Run `npm test` before committing
- Maintain >80% code coverage
- Use jest for unit tests, playwright for E2E

- Build: `npm run build`
- Dev server: `npm run dev`
- Type check: `npm run typecheck`
typescript TypeScript
import { query } from "@anthropic-ai/claude-agent-sdk";

// IMPORTANT: You must specify settingSources to load CLAUDE.md
// The claude_code preset alone does NOT load CLAUDE.md files
const messages = [];

for await (const message of query({
  prompt: "Add a new React component for user profiles",
  options: {
    systemPrompt: {
      type: "preset",
      preset: "claude_code", // Use Claude Code's system prompt
    },
    settingSources: ["project"], // Required to load CLAUDE.md from project
  },
})) {
  messages.push(message);
}

// Now Claude has access to your project guidelines from CLAUDE.md
python Python
from claude_agent_sdk import query, ClaudeAgentOptions

**Examples:**

Example 1 (unknown):
```unknown
#### Using CLAUDE.md with the SDK

<CodeGroup>
```

Example 2 (unknown):
```unknown

```

---

## Code execution tool

**URL:** llms-txt#code-execution-tool

**Contents:**
- Model compatibility
- Quick start
- How code execution works
- How to use the tool
  - Execute Bash commands
  - Create and edit files directly
  - Upload and analyze your own files

Claude can analyze data, create visualizations, perform complex calculations, run system commands, create and edit files, and process uploaded
files directly within the API conversation.
The code execution tool allows Claude to run Bash commands and manipulate files, including writing code, in a secure, sandboxed environment.

<Note>
The code execution tool is currently in public beta.

To use this feature, add the `"code-execution-2025-08-25"` [beta header](/docs/en/api/beta-headers) to your API requests.

Please reach out through our [feedback form](https://forms.gle/LTAU6Xn2puCJMi1n6) to share your feedback on this feature.
</Note>

## Model compatibility

The code execution tool is available on the following models:

| Model | Tool Version |
|-------|--------------|
| Claude Opus 4.5 (`claude-opus-4-5-20251101`) | `code_execution_20250825` |
| Claude Opus 4.1 (`claude-opus-4-1-20250805`) | `code_execution_20250825` |
| Claude Opus 4 (`claude-opus-4-20250514`) | `code_execution_20250825` |
| Claude Sonnet 4.5 (`claude-sonnet-4-5-20250929`) | `code_execution_20250825` |
| Claude Sonnet 4 (`claude-sonnet-4-20250514`) | `code_execution_20250825` |
| Claude Sonnet 3.7 (`claude-3-7-sonnet-20250219`) ([deprecated](/docs/en/about-claude/model-deprecations)) | `code_execution_20250825` |
| Claude Haiku 4.5 (`claude-haiku-4-5-20251001`) | `code_execution_20250825` |
| Claude Haiku 3.5 (`claude-3-5-haiku-latest`) ([deprecated](/docs/en/about-claude/model-deprecations)) | `code_execution_20250825` |

<Note>
The current version `code_execution_20250825` supports Bash commands and file operations. A legacy version `code_execution_20250522` (Python only) is also available. See [Upgrade to latest tool version](#upgrade-to-latest-tool-version) for migration details.
</Note>

<Warning>
Older tool versions are not guaranteed to be backwards-compatible with newer models. Always use the tool version that corresponds to your model version.
</Warning>

Here's a simple example that asks Claude to perform a calculation:

## How code execution works

When you add the code execution tool to your API request:

1. Claude evaluates whether code execution would help answer your question
2. The tool automatically provides Claude with the following capabilities:
   - **Bash commands**: Execute shell commands for system operations and package management
   - **File operations**: Create, view, and edit files directly, including writing code
3. Claude can use any combination of these capabilities in a single request
4. All operations run in a secure sandbox environment
5. Claude provides results with any generated charts, calculations, or analysis

## How to use the tool

### Execute Bash commands

Ask Claude to check system information and install packages:

### Create and edit files directly

Claude can create, view, and edit files directly in the sandbox using the file manipulation capabilities:

### Upload and analyze your own files

To analyze your own data files (CSV, Excel, images, etc.), upload them via the Files API and reference them in your request:

<Note>
Using the Files API with Code Execution requires two beta headers: `"anthropic-beta": "code-execution-2025-08-25,files-api-2025-04-14"`
</Note>

The Python environment can process various file types uploaded via the Files API, including:

- CSV
- Excel (.xlsx, .xls)
- JSON
- XML
- Images (JPEG, PNG, GIF, WebP)
- Text files (.txt, .md, .py, etc)

#### Upload and analyze files

1. **Upload your file** using the [Files API](/docs/en/build-with-claude/files)
2. **Reference the file** in your message using a `container_upload` content block
3. **Include the code execution tool** in your API request

<CodeGroup>
```bash Shell

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

## How code execution works

When you add the code execution tool to your API request:

1. Claude evaluates whether code execution would help answer your question
2. The tool automatically provides Claude with the following capabilities:
   - **Bash commands**: Execute shell commands for system operations and package management
   - **File operations**: Create, view, and edit files directly, including writing code
3. Claude can use any combination of these capabilities in a single request
4. All operations run in a secure sandbox environment
5. Claude provides results with any generated charts, calculations, or analysis

## How to use the tool

### Execute Bash commands

Ask Claude to check system information and install packages:

<CodeGroup>
```

Example 4 (unknown):
```unknown

```

---

## 2. Create script

**URL:** llms-txt#2.-create-script

{"command": "cat > fetch_joke.py << 'EOF'\nimport requests\nresponse = requests.get('https://official-joke-api.appspot.com/random_joke')\njoke = response.json()\nprint(f\"Setup: {joke['setup']}\")\nprint(f\"Punchline: {joke['punchline']}\")\nEOF"}

---

## Claude Code Analytics API

**URL:** llms-txt#claude-code-analytics-api

**Contents:**
- Quick start
- Claude Code Analytics API
  - Key concepts
  - Basic examples

Programmatically access your organization's Claude Code usage analytics and productivity metrics with the Claude Code Analytics Admin API.

<Tip>
**The Admin API is unavailable for individual accounts.** To collaborate with teammates and add members, set up your organization in **Console → Settings → Organization**.
</Tip>

The Claude Code Analytics Admin API provides programmatic access to daily aggregated usage metrics for Claude Code users, enabling organizations to analyze developer productivity and build custom dashboards. This API bridges the gap between our basic [Analytics dashboard](/claude-code) and the complex OpenTelemetry integration.

This API enables you to better monitor, analyze, and optimize your Claude Code adoption:

* **Developer Productivity Analysis:** Track sessions, lines of code added/removed, commits, and pull requests created using Claude Code
* **Tool Usage Metrics:** Monitor acceptance and rejection rates for different Claude Code tools (Edit, Write, NotebookEdit)
* **Cost Analysis:** View estimated costs and token usage broken down by Claude model
* **Custom Reporting:** Export data to build executive dashboards and reports for management teams
* **Usage Justification:** Provide metrics to justify and expand Claude Code adoption internally

<Check>
  **Admin API key required**
  
  This API is part of the [Admin API](/docs/en/build-with-claude/administration-api). These endpoints require an Admin API key (starting with `sk-ant-admin...`) that differs from standard API keys. Only organization members with the admin role can provision Admin API keys through the [Claude Console](/settings/admin-keys).
</Check>

Get your organization's Claude Code analytics for a specific day:

<Tip>
  **Set a User-Agent header for integrations**
  
  If you're building an integration, set your User-Agent header to help us understand usage patterns:
  
</Tip>

## Claude Code Analytics API

Track Claude Code usage, productivity metrics, and developer activity across your organization with the `/v1/organizations/usage_report/claude_code` endpoint.

- **Daily aggregation**: Returns metrics for a single day specified by the `starting_at` parameter
- **User-level data**: Each record represents one user's activity for the specified day
- **Productivity metrics**: Track sessions, lines of code, commits, pull requests, and tool usage
- **Token and cost data**: Monitor usage and estimated costs broken down by Claude model
- **Cursor-based pagination**: Handle large datasets with stable pagination using opaque cursors
- **Data freshness**: Metrics are available with up to 1-hour delay for consistency

For complete parameter details and response schemas, see the [Claude Code Analytics API reference](/docs/en/api/admin-api/claude-code/get-claude-code-usage-report).

#### Get analytics for a specific day

#### Get analytics with pagination

**Examples:**

Example 1 (bash):
```bash
curl "https://api.anthropic.com/v1/organizations/usage_report/claude_code?\
starting_at=2025-09-08&\
limit=20" \
  --header "anthropic-version: 2023-06-01" \
  --header "x-api-key: $ADMIN_API_KEY"
```

Example 2 (unknown):
```unknown
User-Agent: YourApp/1.0.0 (https://yourapp.com)
```

Example 3 (bash):
```bash
curl "https://api.anthropic.com/v1/organizations/usage_report/claude_code?\
starting_at=2025-09-08" \
  --header "anthropic-version: 2023-06-01" \
  --header "x-api-key: $ADMIN_API_KEY"
```

---

## Claude Developer Platform

**URL:** llms-txt#claude-developer-platform

**Contents:**
  - January 12, 2026
  - January 5, 2026
  - December 19, 2025
  - December 4, 2025
  - November 24, 2025
  - November 21, 2025
  - November 19, 2025
  - November 18, 2025
  - November 14, 2025
  - October 28, 2025

Updates to the Claude Developer Platform, including the Claude API, client SDKs, and the Claude Console.

<Tip>
For release notes on Claude Apps, see the [Release notes for Claude Apps in the Claude Help Center](https://support.claude.com/en/articles/12138966-release-notes).

For updates to Claude Code, see the [complete CHANGELOG.md](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md) in the `claude-code` repository.
</Tip>

### January 12, 2026
- `console.anthropic.com` now redirects to `platform.claude.com`. The Claude Console has moved to its new home as part of our Claude brand consolidation. Existing bookmarks and links will continue working via automatic redirect. For more details, see the [September 16, 2025 announcement](#september-16-2025).

### January 5, 2026
- We've retired the Claude Opus 3 model (`claude-3-opus-20240229`). All requests to this model will now return an error. We recommend upgrading to [Claude Opus 4.5](/docs/en/about-claude/models/overview#latest-models-comparison), which offers significantly improved intelligence at a third of the cost. Researchers can request ongoing access to Claude Opus 3 on the API through the [External Researcher Access Program](https://support.claude.com/en/articles/9125743-what-is-the-external-researcher-access-program).

### December 19, 2025
- We announced the deprecation of the Claude Haiku 3.5 model. Read more in [our documentation](/docs/en/about-claude/model-deprecations).

### December 4, 2025
- [Structured outputs](/docs/en/build-with-claude/structured-outputs) now supports Claude Haiku 4.5.

### November 24, 2025
- We've launched [Claude Opus 4.5](https://www.anthropic.com/news/claude-opus-4-5), our most intelligent model combining maximum capability with practical performance. Ideal for complex specialized tasks, professional software engineering, and advanced agents. Features step-change improvements in vision, coding, and computer use at a more accessible price point than previous Opus models. Learn more in our [Models & Pricing documentation](/docs/en/about-claude/models).
- We've launched [programmatic tool calling](/docs/en/agents-and-tools/tool-use/programmatic-tool-calling) in public beta, allowing Claude to call tools from within code execution to reduce latency and token usage in multi-tool workflows.
- We've launched the [tool search tool](/docs/en/agents-and-tools/tool-use/tool-search-tool) in public beta, enabling Claude to dynamically discover and load tools on-demand from large tool catalogs.
- We've launched the [effort parameter](/docs/en/build-with-claude/effort) in public beta for Claude Opus 4.5, allowing you to control token usage by trading off between response thoroughness and efficiency.
- We've added [client-side compaction](/docs/en/build-with-claude/context-editing#client-side-compaction-sdk) to our Python and TypeScript SDKs, automatically managing conversation context through summarization when using `tool_runner`.

### November 21, 2025
- Search result content blocks are now generally available on Amazon Bedrock. Learn more in our [search results documentation](/docs/en/build-with-claude/search-results).

### November 19, 2025
- We've launched a **new documentation platform** at [platform.claude.com/docs](https://platform.claude.com/docs). Our documentation now lives side by side with the Claude Console, providing a unified developer experience. The previous docs site at docs.claude.com will redirect to the new location.

### November 18, 2025
- We've launched **Claude in Microsoft Foundry**, bringing Claude models to Azure customers with Azure billing and OAuth authentication. Access the full Messages API including extended thinking, prompt caching (5-minute and 1-hour), PDF support, Files API, Agent Skills, and tool use. Learn more in our [Microsoft Foundry documentation](/docs/en/build-with-claude/claude-in-microsoft-foundry).

### November 14, 2025
- We've launched [structured outputs](/docs/en/build-with-claude/structured-outputs) in public beta, providing guaranteed schema conformance for Claude's responses. Use JSON outputs for structured data responses or strict tool use for validated tool inputs. Available for Claude Sonnet 4.5 and Claude Opus 4.1. To enable, use the beta header `structured-outputs-2025-11-13`.

### October 28, 2025
- We announced the deprecation of the Claude Sonnet 3.7 model. Read more in [our documentation](/docs/en/about-claude/model-deprecations).
- We've retired the Claude Sonnet 3.5 models. All requests to these models will now return an error.
- We've expanded context editing with thinking block clearing (`clear_thinking_20251015`), enabling automatic management of thinking blocks. Learn more in our [context editing documentation](/docs/en/build-with-claude/context-editing).

### October 16, 2025
- We've launched [Agent Skills](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills) (`skills-2025-10-02` beta), a new way to extend Claude's capabilities. Skills are organized folders of instructions, scripts, and resources that Claude loads dynamically to perform specialized tasks. The initial release includes:
  - **Anthropic-managed Skills**: Pre-built Skills for working with PowerPoint (.pptx), Excel (.xlsx), Word (.docx), and PDF files
  - **Custom Skills**: Upload your own Skills via the Skills API (`/v1/skills` endpoints) to package domain expertise and organizational workflows
  - Skills require the [code execution tool](/docs/en/agents-and-tools/tool-use/code-execution-tool) to be enabled
  - Learn more in our [Agent Skills documentation](/docs/en/agents-and-tools/agent-skills/overview) and [API reference](/docs/en/api/skills/create-skill)

### October 15, 2025
- We've launched [Claude Haiku 4.5](https://www.anthropic.com/news/claude-haiku-4-5), our fastest and most intelligent Haiku model with near-frontier performance. Ideal for real-time applications, high-volume processing, and cost-sensitive deployments requiring strong reasoning. Learn more in our [Models & Pricing documentation](/docs/en/about-claude/models).

### September 29, 2025
- We've launched [Claude Sonnet 4.5](https://www.anthropic.com/news/claude-sonnet-4-5), our best model for complex agents and coding, with the highest intelligence across most tasks. Learn more in [What's new in Claude 4.5](/docs/en/about-claude/models/whats-new-claude-4-5).
- We've introduced [global endpoint pricing](/docs/en/about-claude/pricing#third-party-platform-pricing) for AWS Bedrock and Google Vertex AI. The Claude API (1P) pricing is unaffected.
- We've introduced a new stop reason `model_context_window_exceeded` that allows you to request the maximum possible tokens without calculating input size. Learn more in our [handling stop reasons documentation](/docs/en/build-with-claude/handling-stop-reasons).
- We've launched the memory tool in beta, enabling Claude to store and consult information across conversations. Learn more in our [memory tool documentation](/docs/en/agents-and-tools/tool-use/memory-tool).
- We've launched context editing in beta, providing strategies to automatically manage conversation context. The initial release supports clearing older tool results and calls when approaching token limits. Learn more in our [context editing documentation](/docs/en/build-with-claude/context-editing).

### September 17, 2025
- We've launched tool helpers in beta for the Python and TypeScript SDKs, simplifying tool creation and execution with type-safe input validation and a tool runner for automated tool handling in conversations. For details, see the documentation for [the Python SDK](https://github.com/anthropics/anthropic-sdk-python/blob/main/tools.md) and [the TypeScript SDK](https://github.com/anthropics/anthropic-sdk-typescript/blob/main/helpers.md#tool-helpers).

### September 16, 2025
- We've unified our developer offerings under the Claude brand. You should see updated naming and URLs across our platform and documentation, but **our developer interfaces will remain the same**. Here are some notable changes:
  - Anthropic Console ([console.anthropic.com](https://console.anthropic.com)) → Claude Console ([platform.claude.com](https://platform.claude.com)). The console will be available at both URLs until January 12, 2026. After that date, [console.anthropic.com](https://console.anthropic.com) will automatically redirect to [platform.claude.com](https://platform.claude.com).
  - Anthropic Docs ([docs.claude.com](https://docs.claude.com)) → Claude Docs ([docs.claude.com](https://docs.claude.com))
  - Anthropic Help Center ([support.claude.com](https://support.claude.com)) → Claude Help Center ([support.claude.com](https://support.claude.com))
  - API endpoints, headers, environment variables, and SDKs remain the same. Your existing integrations will continue working without any changes.

### September 10, 2025
- We've launched the web fetch tool in beta, allowing Claude to retrieve full content from specified web pages and PDF documents. Learn more in our [web fetch tool documentation](/docs/en/agents-and-tools/tool-use/web-fetch-tool).
- We've launched the [Claude Code Analytics API](/docs/en/build-with-claude/claude-code-analytics-api), enabling organizations to programmatically access daily aggregated usage metrics for Claude Code, including productivity metrics, tool usage statistics, and cost data.

### September 8, 2025
- We launched a beta version of the [C# SDK](https://github.com/anthropics/anthropic-sdk-csharp).

### September 5, 2025
- We've launched [rate limit charts](/docs/en/api/rate-limits#monitoring-your-rate-limits-in-the-console) in the Console [Usage](https://console.anthropic.com/settings/usage) page, allowing you to monitor your API rate limit usage and caching rates over time.

### September 3, 2025
- We've launched support for citable documents in client-side tool results. Learn more in our [tool use documentation](/docs/en/agents-and-tools/tool-use/implement-tool-use).

### September 2, 2025
- We've launched v2 of the [Code Execution Tool](/docs/en/agents-and-tools/tool-use/code-execution-tool) in public beta, replacing the original Python-only tool with Bash command execution and direct file manipulation capabilities, including writing code in other languages.

### August 27, 2025
- We launched a beta version of the [PHP SDK](https://github.com/anthropics/anthropic-sdk-php).

### August 26, 2025
- We've increased rate limits on the [1M token context window](/docs/en/build-with-claude/context-windows#1m-token-context-window) for Claude Sonnet 4 on the Claude API. For more information, see [Long context rate limits](/docs/en/api/rate-limits#long-context-rate-limits).
- The 1m token context window is now available on Google Cloud's Vertex AI. For more information, see [Claude on Vertex AI](/docs/en/build-with-claude/claude-on-vertex-ai).

### August 19, 2025
- Request IDs are now included directly in error response bodies alongside the existing `request-id` header. Learn more in our [error documentation](/docs/en/api/errors#error-shapes).

### August 18, 2025
- We've released the [Usage & Cost API](/docs/en/build-with-claude/usage-cost-api), allowing administrators to programmatically monitor their organization's usage and cost data.
- We've added a new endpoint to the Admin API for retrieving organization information. For details, see the [Organization Info Admin API reference](/docs/en/api/admin-api/organization/get-me).

### August 13, 2025
- We announced the deprecation of the Claude Sonnet 3.5 models (`claude-3-5-sonnet-20240620` and `claude-3-5-sonnet-20241022`). These models will be retired on October 28, 2025. We recommend migrating to Claude Sonnet 4.5 (`claude-sonnet-4-5-20250929`) for improved performance and capabilities. Read more in the [Model deprecations documentation](/docs/en/about-claude/model-deprecations).
- The 1-hour cache duration for prompt caching is now generally available. You can now use the extended cache TTL without a beta header. Learn more in our [prompt caching documentation](/docs/en/build-with-claude/prompt-caching#1-hour-cache-duration).

### August 12, 2025
- We've launched beta support for a [1M token context window](/docs/en/build-with-claude/context-windows#1m-token-context-window) in Claude Sonnet 4 on the Claude API and Amazon Bedrock.

### August 11, 2025
- Some customers might encounter 429 (`rate_limit_error`) [errors](/docs/en/api/errors) following a sharp increase in API usage due to acceleration limits on the API. Previously, 529 (`overloaded_error`) errors would occur in similar scenarios.

### August 8, 2025
- Search result content blocks are now generally available on the Claude API and Google Cloud's Vertex AI. This feature enables natural citations for RAG applications with proper source attribution. The beta header `search-results-2025-06-09` is no longer required. Learn more in our [search results documentation](/docs/en/build-with-claude/search-results).

### August 5, 2025
- We've launched [Claude Opus 4.1](https://www.anthropic.com/news/claude-opus-4-1), an incremental update to Claude Opus 4 with enhanced capabilities and performance improvements.<sup>*</sup> Learn more in our [Models & Pricing documentation](/docs/en/about-claude/models).

_<sup>* - Opus 4.1 does not allow both `temperature` and `top_p` parameters to be specified. Please use only one. </sup>_

### July 28, 2025
- We've released `text_editor_20250728`, an updated text editor tool that fixes some issues from the previous versions and adds an optional `max_characters` parameter that allows you to control the truncation length when viewing large files.

### July 24, 2025
- We've increased [rate limits](/docs/en/api/rate-limits) for Claude Opus 4 on the Claude API to give you more capacity to build and scale with Claude. For customers with [usage tier 1-4 rate limits](/docs/en/api/rate-limits#rate-limits), these changes apply immediately to your account - no action needed.

### July 21, 2025
- We've retired the Claude 2.0, Claude 2.1, and Claude Sonnet 3 models. All requests to these models will now return an error. Read more in [our documentation](/docs/en/about-claude/model-deprecations).

### July 17, 2025
- We've increased [rate limits](/docs/en/api/rate-limits) for Claude Sonnet 4 on the Claude API to give you more capacity to build and scale with Claude. For customers with [usage tier 1-4 rate limits](/docs/en/api/rate-limits#rate-limits), these changes apply immediately to your account - no action needed.

### July 3, 2025
- We've launched search result content blocks in beta, enabling natural citations for RAG applications. Tools can now return search results with proper source attribution, and Claude will automatically cite these sources in its responses - matching the citation quality of web search. This eliminates the need for document workarounds in custom knowledge base applications. Learn more in our [search results documentation](/docs/en/build-with-claude/search-results). To enable this feature, use the beta header `search-results-2025-06-09`.

### June 30, 2025
- We announced the deprecation of the Claude Opus 3 model. Read more in [our documentation](/docs/en/about-claude/model-deprecations).

### June 23, 2025
- Console users with the Developer role can now access the [Cost](https://console.anthropic.com/settings/cost) page. Previously, the Developer role allowed access to the [Usage](https://console.anthropic.com/settings/usage) page, but not the Cost page.

### June 11, 2025
- We've launched [fine-grained tool streaming](/docs/en/agents-and-tools/tool-use/fine-grained-tool-streaming) in public beta, a feature that enables Claude to stream tool use parameters without buffering / JSON validation. To enable fine-grained tool streaming, use the [beta header](/docs/en/api/beta-headers) `fine-grained-tool-streaming-2025-05-14`.

### May 22, 2025
- We've launched [Claude Opus 4 and Claude Sonnet 4](http://www.anthropic.com/news/claude-4), our latest models with extended thinking capabilities. Learn more in our [Models & Pricing documentation](/docs/en/about-claude/models).
- The default behavior of [extended thinking](/docs/en/build-with-claude/extended-thinking) in Claude 4 models returns a summary of Claude's full thinking process, with the full thinking encrypted and returned in the `signature` field of `thinking` block output.
- We've launched [interleaved thinking](/docs/en/build-with-claude/extended-thinking#interleaved-thinking) in public beta, a feature that enables Claude to think in between tool calls. To enable interleaved thinking, use the [beta header](/docs/en/api/beta-headers) `interleaved-thinking-2025-05-14`.
- We've launched the [Files API](/docs/en/build-with-claude/files) in public beta, enabling you to upload files and reference them in the Messages API and code execution tool.
- We've launched the [Code execution tool](/docs/en/agents-and-tools/tool-use/code-execution-tool) in public beta, a tool that enables Claude to execute Python code in a secure, sandboxed environment.
- We've launched the [MCP connector](/docs/en/agents-and-tools/mcp-connector) in public beta, a feature that allows you to connect to remote MCP servers directly from the Messages API. 
- To increase answer quality and decrease tool errors, we've changed the default value for the `top_p` [nucleus sampling](https://en.wikipedia.org/wiki/Top-p_sampling) parameter in the Messages API from 0.999 to 0.99 for all models. To revert this change, set `top_p` to 0.999. 
    Additionally, when extended thinking is enabled, you can now set `top_p` to values between 0.95 and 1.
- We've moved our [Go SDK](https://github.com/anthropics/anthropic-sdk-go) from beta to GA.
- We've included minute and hour level granularity to the [Usage](https://console.anthropic.com/settings/usage) page of Console alongside 429 error rates on the Usage page.

### May 21, 2025
- We've moved our [Ruby SDK](https://github.com/anthropics/anthropic-sdk-ruby) from beta to GA.

### May 7, 2025
- We've launched a web search tool in the API, allowing Claude to access up-to-date information from the web. Learn more in our [web search tool documentation](/docs/en/agents-and-tools/tool-use/web-search-tool).

### May 1, 2025
- Cache control must now be specified directly in the parent `content` block of `tool_result` and `document.source`. For backwards compatibility, if cache control is detected on the last block in `tool_result.content` or `document.source.content`, it will be automatically applied to the parent block instead. Cache control on any other blocks within `tool_result.content` and `document.source.content` will result in a validation error.

### April 9th, 2025
- We launched a beta version of the [Ruby SDK](https://github.com/anthropics/anthropic-sdk-ruby)

### March 31st, 2025
- We've moved our [Java SDK](https://github.com/anthropics/anthropic-sdk-java) from beta to GA.
- We've moved our [Go SDK](https://github.com/anthropics/anthropic-sdk-go) from alpha to beta.

### February 27th, 2025
- We've added URL source blocks for images and PDFs in the Messages API. You can now reference images and PDFs directly via URL instead of having to base64-encode them. Learn more in our [vision documentation](/docs/en/build-with-claude/vision) and [PDF support documentation](/docs/en/build-with-claude/pdf-support).
- We've added support for a `none` option to the `tool_choice` parameter in the Messages API that prevents Claude from calling any tools. Additionally, you're no longer required to provide any `tools` when including `tool_use` and `tool_result` blocks.
- We've launched an OpenAI-compatible API endpoint, allowing you to test Claude models by changing just your API key, base URL, and model name in existing OpenAI integrations. This compatibility layer supports core chat completions functionality. Learn more in our [OpenAI SDK compatibility documentation](/docs/en/api/openai-sdk).

### February 24th, 2025
- We've launched [Claude Sonnet 3.7](http://www.anthropic.com/news/claude-3-7-sonnet), our most intelligent model yet. Claude Sonnet 3.7 can produce near-instant responses or show its extended thinking step-by-step. One model, two ways to think. Learn more about all Claude models in our [Models & Pricing documentation](/docs/en/about-claude/models).
- We've added vision support to Claude Haiku 3.5, enabling the model to analyze and understand images.
- We've released a token-efficient tool use implementation, improving overall performance when using tools with Claude. Learn more in our [tool use documentation](/docs/en/agents-and-tools/tool-use/overview).
- We've changed the default temperature in the [Console](https://console.anthropic.com/workbench) for new prompts from 0 to 1 for consistency with the default temperature in the API. Existing saved prompts are unchanged.
- We've released updated versions of our tools that decouple the text edit and bash tools from the computer use system prompt:
  - `bash_20250124`: Same functionality as previous version but is independent from computer use. Does not require a beta header.
  - `text_editor_20250124`: Same functionality as previous version but is independent from computer use. Does not require a beta header.
  - `computer_20250124`: Updated computer use tool with new command options including "hold_key", "left_mouse_down", "left_mouse_up", "scroll", "triple_click", and "wait". This tool requires the "computer-use-2025-01-24" anthropic-beta header.
  Learn more in our [tool use documentation](/docs/en/agents-and-tools/tool-use/overview).

### February 10th, 2025
- We've added the `anthropic-organization-id` response header to all API responses. This header provides the organization ID associated with the API key used in the request.

### January 31st, 2025

- We've moved our [Java SDK](https://github.com/anthropics/anthropic-sdk-java) from alpha to beta.

### January 23rd, 2025

- We've launched citations capability in the API, allowing Claude to provide source attribution for information. Learn more in our [citations documentation](/docs/en/build-with-claude/citations).
- We've added support for plain text documents and custom content documents in the Messages API.

### January 21st, 2025

- We announced the deprecation of the Claude 2, Claude 2.1, and Claude Sonnet 3 models. Read more in [our documentation](/docs/en/about-claude/model-deprecations).

### January 15th, 2025

- We've updated [prompt caching](/docs/en/build-with-claude/prompt-caching) to be easier to use. Now, when you set a cache breakpoint, we'll automatically read from your longest previously cached prefix.
- You can now put words in Claude's mouth when using tools.

### January 10th, 2025

- We've optimized support for [prompt caching in the Message Batches API](/docs/en/build-with-claude/batch-processing#using-prompt-caching-with-message-batches) to improve cache hit rate.

### December 19th, 2024

- We've added support for a [delete endpoint](/docs/en/api/deleting-message-batches) in the Message Batches API

### December 17th, 2024
The following features are now generally available in the Claude API:

- [Models API](/docs/en/api/models-list): Query available models, validate model IDs, and resolve [model aliases](/docs/en/about-claude/models#model-names) to their canonical model IDs.
- [Message Batches API](/docs/en/build-with-claude/batch-processing): Process large batches of messages asynchronously at 50% of the standard API cost.
- [Token counting API](/docs/en/build-with-claude/token-counting): Calculate token counts for Messages before sending them to Claude.
- [Prompt Caching](/docs/en/build-with-claude/prompt-caching): Reduce costs by up to 90% and latency by up to 80% by caching and reusing prompt content.
- [PDF support](/docs/en/build-with-claude/pdf-support): Process PDFs to analyze both text and visual content within documents.

We also released new official SDKs:
- [Java SDK](https://github.com/anthropics/anthropic-sdk-java) (alpha)
- [Go SDK](https://github.com/anthropics/anthropic-sdk-go) (alpha)

### December 4th, 2024

- We've added the ability to group by API key to the [Usage](https://console.anthropic.com/settings/usage) and [Cost](https://console.anthropic.com/settings/cost) pages of the [Developer Console](https://console.anthropic.com)
- We've added two new **Last used at** and **Cost** columns and the ability to sort by any column in the [API keys](https://console.anthropic.com/settings/keys) page of the [Developer Console](https://console.anthropic.com)

### November 21st, 2024

- We've released the [Admin API](/docs/en/build-with-claude/administration-api), allowing users to programmatically manage their organization's resources.

### November 20th, 2024

- We've updated our rate limits for the Messages API. We've replaced the tokens per minute rate limit with new input and output tokens per minute rate limits. Read more in our [documentation](/docs/en/api/rate-limits).
- We've added support for [tool use](/docs/en/agents-and-tools/tool-use/overview) in the [Workbench](https://console.anthropic.com/workbench).

### November 13th, 2024

- We've added PDF support for all Claude Sonnet 3.5 models. Read more in our [documentation](/docs/en/build-with-claude/pdf-support).

### November 6th, 2024

- We've retired the Claude 1 and Instant models. Read more in [our documentation](/docs/en/about-claude/model-deprecations).

### November 4th, 2024

- [Claude Haiku 3.5](https://www.anthropic.com/claude/haiku) is now available on the Claude API as a text-only model.

### November 1st, 2024

- We've added PDF support for use with the new Claude Sonnet 3.5. Read more in our [documentation](/docs/en/build-with-claude/pdf-support).
- We've also added token counting, which allows you to determine the total number of tokens in a Message, prior to sending it to Claude. Read more in our [documentation](/docs/en/build-with-claude/token-counting).

### October 22nd, 2024

- We've added Anthropic-defined computer use tools to our API for use with the new Claude Sonnet 3.5. Read more in our [documentation](/docs/en/agents-and-tools/tool-use/computer-use-tool).
- Claude Sonnet 3.5, our most intelligent model yet, just got an upgrade and is now available on the Claude API. Read more [here](https://www.anthropic.com/claude/sonnet).

### October 8th, 2024

- The Message Batches API is now available in beta. Process large batches of queries asynchronously in the Claude API for 50% less cost. Read more in our [documentation](/docs/en/build-with-claude/batch-processing).
- We've loosened restrictions on the ordering of `user`/`assistant` turns in our Messages API. Consecutive `user`/`assistant` messages will be combined into a single message instead of erroring, and we no longer require the first input message to be a `user` message.
- We've deprecated the Build and Scale plans in favor of a standard feature suite (formerly referred to as Build), along with additional features that are available through sales. Read more [here](https://claude.com/platform/api).

### October 3rd, 2024

- We've added the ability to disable parallel tool use in the API. Set `disable_parallel_tool_use: true` in the `tool_choice` field to ensure that Claude uses at most one tool. Read more in our [documentation](/docs/en/agents-and-tools/tool-use/implement-tool-use#parallel-tool-use).

### September 10th, 2024

- We've added Workspaces to the [Developer Console](https://console.anthropic.com). Workspaces allow you to set custom spend or rate limits, group API keys, track usage by project, and control access with user roles. Read more in our [blog post](https://www.anthropic.com/news/workspaces).

### September 4th, 2024

- We announced the deprecation of the Claude 1 models. Read more in [our documentation](/docs/en/about-claude/model-deprecations).

### August 22nd, 2024

- We've added support for usage of the SDK in browsers by returning CORS headers in the API responses. Set `dangerouslyAllowBrowser: true` in the SDK instantiation to enable this feature.

### August 19th, 2024

- We've moved 8,192 token outputs from beta to general availability for Claude Sonnet 3.5.

### August 14th, 2024

- [Prompt caching](/docs/en/build-with-claude/prompt-caching) is now available as a beta feature in the Claude API. Cache and re-use prompts to reduce latency by up to 80% and costs by up to 90%.

- Generate outputs up to 8,192 tokens in length from Claude Sonnet 3.5 with the new `anthropic-beta: max-tokens-3-5-sonnet-2024-07-15` header.

- Automatically generate test cases for your prompts using Claude in the [Developer Console](https://console.anthropic.com).
- Compare the outputs from different prompts side by side in the new output comparison mode in the [Developer Console](https://console.anthropic.com).

- View API usage and billing broken down by dollar amount, token count, and API keys in the new [Usage](https://console.anthropic.com/settings/usage) and [Cost](https://console.anthropic.com/settings/cost) tabs in the [Developer Console](https://console.anthropic.com).
- View your current API rate limits in the new [Rate Limits](https://console.anthropic.com/settings/limits) tab in the [Developer Console](https://console.anthropic.com).

- [Claude Sonnet 3.5](http://anthropic.com/news/claude-3-5-sonnet), our most intelligent model yet, is now generally available across the Claude API, Amazon Bedrock, and Google Vertex AI.

- [Tool use](/docs/en/agents-and-tools/tool-use/overview) is now generally available across the Claude API, Amazon Bedrock, and Google Vertex AI.

- Our prompt generator tool is now available in the [Developer Console](https://console.anthropic.com). Prompt Generator makes it easy to guide Claude to generate a high-quality prompts tailored to your specific tasks. Read more in our [blog post](https://www.anthropic.com/news/prompt-generator).

Last generated: 2026-01-15T06:31:45.476Z
Total pages included: 532

---

## Get Api Key

**URL:** llms-txt#get-api-key

**Contents:**
- Retrieve
  - Path Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/admin/api_keys/retrieve

**get** `/v1/organizations/api_keys/{api_key_id}`

- `api_key_id: string`

- `APIKey = object { id, created_at, created_by, 5 more }`

- `created_at: string`

RFC 3339 datetime string indicating when the API Key was created.

- `created_by: object { id, type }`

The ID and type of the actor that created the API key.

ID of the actor that created the object.

Type of the actor that created the object.

- `partial_key_hint: string`

Partially redacted hint for the API key.

- `status: "active" or "inactive" or "archived"`

Status of the API key.

For API Keys, this is always `"api_key"`.

- `workspace_id: string`

ID of the Workspace associated with the API key, or null if the API key belongs to the default Workspace.

**Examples:**

Example 1 (http):
```http
curl https://api.anthropic.com/v1/organizations/api_keys/$API_KEY_ID \
    -H "X-Api-Key: $ANTHROPIC_ADMIN_API_KEY"
```

---

## First, upload your image to the Files API

**URL:** llms-txt#first,-upload-your-image-to-the-files-api

curl -X POST https://api.anthropic.com/v1/files \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -H "anthropic-beta: files-api-2025-04-14" \
  -F "file=@image.jpg"

---

## Alternatively, you can use vo = voyageai.Client(api_key="<your secret key>")

**URL:** llms-txt#alternatively,-you-can-use-vo-=-voyageai.client(api_key="<your-secret-key>")

**Contents:**
  - Voyage HTTP API
  - AWS Marketplace
- Quickstart example

texts = ["Sample text 1", "Sample text 2"]

result = vo.embed(texts, model="voyage-3.5", input_type="document")
print(result.embeddings[0])
print(result.embeddings[1])

[-0.013131560757756233, 0.019828535616397858, ...]   # embedding for "Sample text 1"
[-0.0069352793507277966, 0.020878976210951805, ...]  # embedding for "Sample text 2"
bash
curl https://api.voyageai.com/v1/embeddings \
  -H "Content-Type: application/json" \
  -H "Authorization: Bearer $VOYAGE_API_KEY" \
  -d '{
    "input": ["Sample text 1", "Sample text 2"],
    "model": "voyage-3.5"
  }'
json
{
  "object": "list",
  "data": [
    {
      "embedding": [-0.013131560757756233, 0.019828535616397858, ...],
      "index": 0
    },
    {
      "embedding": [-0.0069352793507277966, 0.020878976210951805, ...],
      "index": 1
    }
  ],
  "model": "voyage-3.5",
  "usage": {
    "total_tokens": 10
  }
}

python
documents = [
    "The Mediterranean diet emphasizes fish, olive oil, and vegetables, believed to reduce chronic diseases.",
    "Photosynthesis in plants converts light energy into glucose and produces essential oxygen.",
    "20th-century innovations, from radios to smartphones, centered on electronic advancements.",
    "Rivers provide water, irrigation, and habitat for aquatic species, vital for ecosystems.",
    "Apple's conference call to discuss fourth fiscal quarter results and business updates is scheduled for Thursday, November 2, 2023 at 2:00 p.m. PT / 5:00 p.m. ET.",
    "Shakespeare's works, like 'Hamlet' and 'A Midsummer Night's Dream,' endure in literature."
]

python
import voyageai

vo = voyageai.Client()

**Examples:**

Example 1 (unknown):
```unknown
`result.embeddings` will be a list of two embedding vectors, each containing 1024 floating-point numbers. After running the above code, the two embeddings will be printed on the screen:
```

Example 2 (unknown):
```unknown
When creating the embeddings, you can specify a few other arguments to the `embed()` function. 

For more information on the Voyage python package, see [the Voyage documentation](https://docs.voyageai.com/docs/embeddings#python-api).

### Voyage HTTP API

You can also get embeddings by requesting Voyage HTTP API. For example, you can send an HTTP request through the `curl` command in a terminal:
```

Example 3 (unknown):
```unknown
The response you would get is a JSON object containing the embeddings and the token usage:
```

Example 4 (unknown):
```unknown
For more information on the Voyage HTTP API, see [the Voyage documentation](https://docs.voyageai.com/reference/embeddings-api).

### AWS Marketplace

Voyage embeddings are available on [AWS Marketplace](https://aws.amazon.com/marketplace/seller-profile?id=seller-snt4gb6fd7ljg). Instructions for accessing Voyage on AWS are available [here](https://docs.voyageai.com/docs/aws-marketplace-model-package?ref=anthropic).

## Quickstart example

Now that we know how to get embeddings, let's see a brief example.

Suppose we have a small corpus of six documents to retrieve from
```

---

## Or use a custom system prompt:

**URL:** llms-txt#or-use-a-custom-system-prompt:

**Contents:**
  - Settings Sources No Longer Loaded by Default

async for message in query(
    prompt="Hello",
    options=ClaudeAgentOptions(
        system_prompt="You are a helpful coding assistant"
    )
):
    print(message)
typescript TypeScript
// BEFORE (v0.0.x) - Loaded all settings automatically
const result = query({ prompt: "Hello" });
// Would read from:
// - ~/.claude/settings.json (user)
// - .claude/settings.json (project)
// - .claude/settings.local.json (local)
// - CLAUDE.md files
// - Custom slash commands

// AFTER (v0.1.0) - No settings loaded by default
// To get the old behavior:
const result = query({
  prompt: "Hello",
  options: {
    settingSources: ["user", "project", "local"]
  }
});

// Or load only specific sources:
const result = query({
  prompt: "Hello",
  options: {
    settingSources: ["project"]  // Only project settings
  }
});
python Python

**Examples:**

Example 1 (unknown):
```unknown
</CodeGroup>

**Why this changed:** Provides better control and isolation for SDK applications. You can now build agents with custom behavior without inheriting Claude Code's CLI-focused instructions.

### Settings Sources No Longer Loaded by Default

**What changed:** The SDK no longer reads from filesystem settings (CLAUDE.md, settings.json, slash commands, etc.) by default.

**Migration:**

<CodeGroup>
```

Example 2 (unknown):
```unknown

```

---

## Set environment variables

**URL:** llms-txt#set-environment-variables

**Contents:**
  - OAuth2 Authentication
- Error Handling
- Related Resources

import os
os.environ["API_TOKEN"] = "your-token"
os.environ["API_KEY"] = "your-key"
typescript TypeScript
import { query } from "@anthropic-ai/claude-agent-sdk";

for await (const message of query({
  prompt: "Process data",
  options: {
    mcpServers: {
      "data-processor": dataServer
    }
  }
})) {
  if (message.type === "system" && message.subtype === "init") {
    // Check MCP server status
    const failedServers = message.mcp_servers.filter(
      s => s.status !== "connected"
    );
    
    if (failedServers.length > 0) {
      console.warn("Failed to connect:", failedServers);
    }
  }
  
  if (message.type === "result" && message.subtype === "error_during_execution") {
    console.error("Execution failed");
  }
}
python Python
from claude_agent_sdk import query

async for message in query(
    prompt="Process data",
    options={
        "mcpServers": {
            "data-processor": data_server
        }
    }
):
    if message["type"] == "system" and message["subtype"] == "init":
        # Check MCP server status
        failed_servers = [
            s for s in message["mcp_servers"]
            if s["status"] != "connected"
        ]
        
        if failed_servers:
            print(f"Failed to connect: {failed_servers}")
    
    if message["type"] == "result" and message["subtype"] == "error_during_execution":
        print("Execution failed")
```

- [Custom Tools Guide](/docs/en/agent-sdk/custom-tools) - Detailed guide on creating SDK MCP servers
- [TypeScript SDK Reference](/docs/en/agent-sdk/typescript)
- [Python SDK Reference](/docs/en/agent-sdk/python)
- [SDK Permissions](/docs/en/agent-sdk/permissions)
- [Common Workflows](https://code.claude.com/docs/en/common-workflows)

**Examples:**

Example 1 (unknown):
```unknown
</CodeGroup>

### OAuth2 Authentication

OAuth2 MCP authentication in-client is not currently supported.

## Error Handling

Handle MCP connection failures gracefully:

<CodeGroup>
```

Example 2 (unknown):
```unknown

```

---

## Usage and Cost API

**URL:** llms-txt#usage-and-cost-api

**Contents:**
- Partner solutions
- Quick start
- Usage API
  - Key concepts
  - Basic examples
  - Time granularity limits
- Cost API
  - Key concepts
  - Basic example
- Pagination

Programmatically access your organization's API usage and cost data with the Usage & Cost Admin API.

<Tip>
**The Admin API is unavailable for individual accounts.** To collaborate with teammates and add members, set up your organization in **Console → Settings → Organization**.
</Tip>

The Usage & Cost Admin API provides programmatic and granular access to historical API usage and cost data for your organization. This data is similar to the information available in the [Usage](/usage) and [Cost](/cost) pages of the Claude Console.

This API enables you to better monitor, analyze, and optimize your Claude implementations:

* **Accurate Usage Tracking:** Get precise token counts and usage patterns instead of relying solely on response token counting
* **Cost Reconciliation:** Match internal records with Anthropic billing for finance and accounting teams
* **Product performance and improvement:** Monitor product performance while measuring if changes to the system have improved it, or setup alerting
* **[Rate limit](/docs/en/api/rate-limits) and [Priority Tier](/docs/en/api/service-tiers#get-started-with-priority-tier) optimization:** Optimize features like [prompt caching](/docs/en/build-with-claude/prompt-caching) or specific prompts to make the most of one’s allocated capacity, or purchase dedicated capacity.
* **Advanced Analysis:** Perform deeper data analysis than what's available in Console

<Check>
  **Admin API key required**
  
  This API is part of the [Admin API](/docs/en/build-with-claude/administration-api). These endpoints require an Admin API key (starting with `sk-ant-admin...`) that differs from standard API keys. Only organization members with the admin role can provision Admin API keys through the [Claude Console](/settings/admin-keys).
</Check>

Leading observability platforms offer ready-to-use integrations for monitoring your Claude API usage and cost, without writing custom code. These integrations provide dashboards, alerting, and analytics to help you manage your API usage effectively.

<CardGroup cols={3}>
  <Card title="CloudZero" icon="chart" href="https://docs.cloudzero.com/docs/connections-anthropic">
    Cloud intelligence platform for tracking and forecasting costs
  </Card>
  <Card title="Datadog" icon="chart" href="https://docs.datadoghq.com/integrations/anthropic/">
    LLM Observability with automatic tracing and monitoring
  </Card>
  <Card title="Grafana Cloud" icon="chart" href="https://grafana.com/docs/grafana-cloud/monitor-infrastructure/integrations/integration-reference/integration-anthropic/">
    Agentless integration for easy LLM observability with out-of-the-box dashboards and alerts
  </Card>
  <Card title="Honeycomb" icon="polygon" href="https://docs.honeycomb.io/integrations/anthropic-usage-monitoring/">
    Advanced querying and visualization through OpenTelemetry
  </Card>
  <Card title="Vantage" icon="chart" href="https://docs.vantage.sh/connecting_anthropic">
    FinOps platform for LLM cost & usage observability
  </Card>
</CardGroup>

Get your organization's daily usage for the last 7 days:

<Tip>
  **Set a User-Agent header for integrations**
  
  If you're building an integration, set your User-Agent header to help us understand usage patterns:
  
</Tip>

Track token consumption across your organization with detailed breakdowns by model, workspace, and service tier with the `/v1/organizations/usage_report/messages` endpoint.

- **Time buckets**: Aggregate usage data in fixed intervals (`1m`, `1h`, or `1d`)
- **Token tracking**: Measure uncached input, cached input, cache creation, and output tokens
- **Filtering & grouping**: Filter by API key, workspace, model, service tier, or context window, and group results by these dimensions
- **Server tool usage**: Track usage of server-side tools like web search

For complete parameter details and response schemas, see the [Usage API reference](/docs/en/api/admin-api/usage-cost/get-messages-usage-report).

#### Daily usage by model

#### Hourly usage with filtering

#### Filter usage by API keys and workspaces

<Tip>
To retrieve your organization's API key IDs, use the [List API Keys](/docs/en/api/admin-api/apikeys/list-api-keys) endpoint.

To retrieve your organization's workspace IDs, use the [List Workspaces](/docs/en/api/admin-api/workspaces/list-workspaces) endpoint, or find your organization's workspace IDs in the Anthropic Console.
</Tip>

### Time granularity limits

| Granularity | Default Limit | Maximum Limit | Use Case |
|-------------|---------------|---------------|----------|
| `1m` | 60 buckets | 1440 buckets | Real-time monitoring |
| `1h` | 24 buckets | 168 buckets | Daily patterns |
| `1d` | 7 buckets | 31 buckets | Weekly/monthly reports |

Retrieve service-level cost breakdowns in USD with the `/v1/organizations/cost_report` endpoint.

- **Currency**: All costs in USD, reported as decimal strings in lowest units (cents)
- **Cost types**: Track token usage, web search, and code execution costs
- **Grouping**: Group costs by workspace or description for detailed breakdowns
- **Time buckets**: Daily granularity only (`1d`)

For complete parameter details and response schemas, see the [Cost API reference](/docs/en/api/admin-api/usage-cost/get-cost-report).

<Warning>
  Priority Tier costs use a different billing model and are not included in the cost endpoint. Track Priority Tier usage through the usage endpoint instead.
</Warning>

Both endpoints support pagination for large datasets:

1. Make your initial request
2. If `has_more` is `true`, use the `next_page` value in your next request
3. Continue until `has_more` is `false`

**Examples:**

Example 1 (bash):
```bash
curl "https://api.anthropic.com/v1/organizations/usage_report/messages?\
starting_at=2025-01-08T00:00:00Z&\
ending_at=2025-01-15T00:00:00Z&\
bucket_width=1d" \
  --header "anthropic-version: 2023-06-01" \
  --header "x-api-key: $ADMIN_API_KEY"
```

Example 2 (unknown):
```unknown
User-Agent: YourApp/1.0.0 (https://yourapp.com)
```

Example 3 (bash):
```bash
curl "https://api.anthropic.com/v1/organizations/usage_report/messages?\
starting_at=2025-01-01T00:00:00Z&\
ending_at=2025-01-08T00:00:00Z&\
group_by[]=model&\
bucket_width=1d" \
  --header "anthropic-version: 2023-06-01" \
  --header "x-api-key: $ADMIN_API_KEY"
```

Example 4 (bash):
```bash
curl "https://api.anthropic.com/v1/organizations/usage_report/messages?\
starting_at=2025-01-15T00:00:00Z&\
ending_at=2025-01-15T23:59:59Z&\
models[]=claude-sonnet-4-5-20250929&\
service_tiers[]=batch&\
context_window[]=0-200k&\
bucket_width=1h" \
  --header "anthropic-version: 2023-06-01" \
  --header "x-api-key: $ADMIN_API_KEY"
```

---

## This will automatically use the environment variable VOYAGE_API_KEY.

**URL:** llms-txt#this-will-automatically-use-the-environment-variable-voyage_api_key.

---

## Automatically fetches more pages as needed.

**URL:** llms-txt#automatically-fetches-more-pages-as-needed.

**Contents:**
  - Retrieving batch results

for message_batch in client.messages.batches.list(
    limit=20
):
    print(message_batch)
typescript TypeScript
import Anthropic from '@anthropic-ai/sdk';

const anthropic = new Anthropic();

// Automatically fetches more pages as needed.
for await (const messageBatch of anthropic.messages.batches.list({
  limit: 20
})) {
  console.log(messageBatch);
}
bash Shell
#!/bin/sh

if ! command -v jq &> /dev/null; then
    echo "Error: This script requires jq. Please install it first."
    exit 1
fi

BASE_URL="https://api.anthropic.com/v1/messages/batches"

has_more=true
after_id=""

while [ "$has_more" = true ]; do
    # Construct URL with after_id if it exists
    if [ -n "$after_id" ]; then
        url="${BASE_URL}?limit=20&after_id=${after_id}"
    else
        url="$BASE_URL?limit=20"
    fi

response=$(curl -s "$url" \
              --header "x-api-key: $ANTHROPIC_API_KEY" \
              --header "anthropic-version: 2023-06-01")

# Extract values using jq
    has_more=$(echo "$response" | jq -r '.has_more')
    after_id=$(echo "$response" | jq -r '.last_id')

# Process and print each entry in the data array
    echo "$response" | jq -c '.data[]' | while read -r entry; do
        echo "$entry" | jq '.'
    done
done
java Java
import com.anthropic.client.AnthropicClient;
import com.anthropic.client.okhttp.AnthropicOkHttpClient;
import com.anthropic.models.messages.batches.*;

public class BatchListExample {
    public static void main(String[] args) {
        AnthropicClient client = AnthropicOkHttpClient.fromEnv();

// Automatically fetches more pages as needed
        for (MessageBatch messageBatch : client.messages().batches().list(
                BatchListParams.builder()
                        .limit(20)
                        .build()
        )) {
            System.out.println(messageBatch);
        }
    }
}
bash Shell
#!/bin/sh
curl "https://api.anthropic.com/v1/messages/batches/msgbatch_01HkcTjaV5uDC8jWR4ZsDV8d" \
  --header "anthropic-version: 2023-06-01" \
  --header "x-api-key: $ANTHROPIC_API_KEY" \
  | grep -o '"results_url":[[:space:]]*"[^"]*"' \
  | cut -d'"' -f4 \
  | while read -r url; do
    curl -s "$url" \
      --header "anthropic-version: 2023-06-01" \
      --header "x-api-key: $ANTHROPIC_API_KEY" \
      | sed 's/}{/}\n{/g' \
      | while IFS= read -r line
    do
      result_type=$(echo "$line" | sed -n 's/.*"result":[[:space:]]*{[[:space:]]*"type":[[:space:]]*"\([^"]*\)".*/\1/p')
      custom_id=$(echo "$line" | sed -n 's/.*"custom_id":[[:space:]]*"\([^"]*\)".*/\1/p')
      error_type=$(echo "$line" | sed -n 's/.*"error":[[:space:]]*{[[:space:]]*"type":[[:space:]]*"\([^"]*\)".*/\1/p')

case "$result_type" in
        "succeeded")
          echo "Success! $custom_id"
          ;;
        "errored")
          if [ "$error_type" = "invalid_request" ]; then
            # Request body must be fixed before re-sending request
            echo "Validation error: $custom_id"
          else
            # Request can be retried directly
            echo "Server error: $custom_id"
          fi
          ;;
        "expired")
          echo "Expired: $line"
          ;;
      esac
    done
  done

python Python
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

```

Example 4 (unknown):
```unknown
</CodeGroup>

### Retrieving batch results

Once batch processing has ended, each Messages request in the batch will have a result. There are 4 result types:

| Result Type | Description |
|-------------|-------------|
| `succeeded` | Request was successful. Includes the message result. |
| `errored`   | Request encountered an error and a message was not created. Possible errors include invalid requests and internal server errors. You will not be billed for these requests. |
| `canceled`  | User canceled the batch before this request could be sent to the model. You will not be billed for these requests. |
| `expired`   | Batch reached its 24 hour expiration before this request could be sent to the model. You will not be billed for these requests. |

You will see an overview of your results with the batch's `request_counts`, which shows how many requests reached each of these four states.

Results of the batch are available for download at the `results_url` property on the Message Batch, and if the organization permission allows, in the Console. Because of the potentially large size of the results, it's recommended to [stream results](/docs/en/api/retrieving-message-batch-results) back rather than download them all at once.

<CodeGroup>
```

---

## Create an instance of the Claude API client

**URL:** llms-txt#create-an-instance-of-the-claude-api-client

client = anthropic.Anthropic()

---

## Update Api Key

**URL:** llms-txt#update-api-key

**Contents:**
- Update
  - Path Parameters
  - Body Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/admin/api_keys/update

**post** `/v1/organizations/api_keys/{api_key_id}`

- `api_key_id: string`

- `name: optional string`

- `status: optional "active" or "inactive" or "archived"`

Status of the API key.

- `APIKey = object { id, created_at, created_by, 5 more }`

- `created_at: string`

RFC 3339 datetime string indicating when the API Key was created.

- `created_by: object { id, type }`

The ID and type of the actor that created the API key.

ID of the actor that created the object.

Type of the actor that created the object.

- `partial_key_hint: string`

Partially redacted hint for the API key.

- `status: "active" or "inactive" or "archived"`

Status of the API key.

For API Keys, this is always `"api_key"`.

- `workspace_id: string`

ID of the Workspace associated with the API key, or null if the API key belongs to the default Workspace.

**Examples:**

Example 1 (http):
```http
curl https://api.anthropic.com/v1/organizations/api_keys/$API_KEY_ID \
    -H 'Content-Type: application/json' \
    -H "X-Api-Key: $ANTHROPIC_ADMIN_API_KEY" \
    -d '{}'
```

---

## API Keys

**URL:** llms-txt#api-keys

**Contents:**
- Retrieve
  - Path Parameters
  - Returns
  - Example
- List
  - Query Parameters
  - Returns
  - Example
- Update
  - Path Parameters

**get** `/v1/organizations/api_keys/{api_key_id}`

- `api_key_id: string`

- `APIKey = object { id, created_at, created_by, 5 more }`

- `created_at: string`

RFC 3339 datetime string indicating when the API Key was created.

- `created_by: object { id, type }`

The ID and type of the actor that created the API key.

ID of the actor that created the object.

Type of the actor that created the object.

- `partial_key_hint: string`

Partially redacted hint for the API key.

- `status: "active" or "inactive" or "archived"`

Status of the API key.

For API Keys, this is always `"api_key"`.

- `workspace_id: string`

ID of the Workspace associated with the API key, or null if the API key belongs to the default Workspace.

**get** `/v1/organizations/api_keys`

- `after_id: optional string`

ID of the object to use as a cursor for pagination. When provided, returns the page of results immediately after this object.

- `before_id: optional string`

ID of the object to use as a cursor for pagination. When provided, returns the page of results immediately before this object.

- `created_by_user_id: optional string`

Filter by the ID of the User who created the object.

- `limit: optional number`

Number of items to return per page.

Defaults to `20`. Ranges from `1` to `1000`.

- `status: optional "active" or "inactive" or "archived"`

Filter by API key status.

- `workspace_id: optional string`

Filter by Workspace ID.

- `data: array of APIKey`

- `created_at: string`

RFC 3339 datetime string indicating when the API Key was created.

- `created_by: object { id, type }`

The ID and type of the actor that created the API key.

ID of the actor that created the object.

Type of the actor that created the object.

- `partial_key_hint: string`

Partially redacted hint for the API key.

- `status: "active" or "inactive" or "archived"`

Status of the API key.

For API Keys, this is always `"api_key"`.

- `workspace_id: string`

ID of the Workspace associated with the API key, or null if the API key belongs to the default Workspace.

First ID in the `data` list. Can be used as the `before_id` for the previous page.

- `has_more: boolean`

Indicates if there are more results in the requested page direction.

Last ID in the `data` list. Can be used as the `after_id` for the next page.

**post** `/v1/organizations/api_keys/{api_key_id}`

- `api_key_id: string`

- `name: optional string`

- `status: optional "active" or "inactive" or "archived"`

Status of the API key.

- `APIKey = object { id, created_at, created_by, 5 more }`

- `created_at: string`

RFC 3339 datetime string indicating when the API Key was created.

- `created_by: object { id, type }`

The ID and type of the actor that created the API key.

ID of the actor that created the object.

Type of the actor that created the object.

- `partial_key_hint: string`

Partially redacted hint for the API key.

- `status: "active" or "inactive" or "archived"`

Status of the API key.

For API Keys, this is always `"api_key"`.

- `workspace_id: string`

ID of the Workspace associated with the API key, or null if the API key belongs to the default Workspace.

**Examples:**

Example 1 (http):
```http
curl https://api.anthropic.com/v1/organizations/api_keys/$API_KEY_ID \
    -H "X-Api-Key: $ANTHROPIC_ADMIN_API_KEY"
```

Example 2 (http):
```http
curl https://api.anthropic.com/v1/organizations/api_keys \
    -H "X-Api-Key: $ANTHROPIC_ADMIN_API_KEY"
```

Example 3 (http):
```http
curl https://api.anthropic.com/v1/organizations/api_keys/$API_KEY_ID \
    -H 'Content-Type: application/json' \
    -H "X-Api-Key: $ANTHROPIC_ADMIN_API_KEY" \
    -d '{}'
```

---

## Step 4: Download the file using Files API

**URL:** llms-txt#step-4:-download-the-file-using-files-api

curl "https://api.anthropic.com/v1/files/$FILE_ID/content" \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -H "anthropic-beta: files-api-2025-04-14" \
  --output "$FILENAME"

echo "Downloaded: $FILENAME"
python Python

**Examples:**

Example 1 (unknown):
```unknown
</CodeGroup>

**Additional Files API operations:**

<CodeGroup>
```

---

## Call your actual weather API, here is where your actual API call would go

**URL:** llms-txt#call-your-actual-weather-api,-here-is-where-your-actual-api-call-would-go

---

## The conversation continues with full context from the previous session

**URL:** llms-txt#the-conversation-continues-with-full-context-from-the-previous-session

**Contents:**
- Forking Sessions
  - When to Fork a Session
  - Forking vs Continuing
  - Example: Forking a Session

typescript TypeScript
import { query } from "@anthropic-ai/claude-agent-sdk"

// First, capture the session ID
let sessionId: string | undefined

const response = query({
  prompt: "Help me design a REST API",
  options: { model: "claude-sonnet-4-5" }
})

for await (const message of response) {
  if (message.type === 'system' && message.subtype === 'init') {
    sessionId = message.session_id
    console.log(`Original session: ${sessionId}`)
  }
}

// Fork the session to try a different approach
const forkedResponse = query({
  prompt: "Now let's redesign this as a GraphQL API instead",
  options: {
    resume: sessionId,
    forkSession: true,  // Creates a new session ID
    model: "claude-sonnet-4-5"
  }
})

for await (const message of forkedResponse) {
  if (message.type === 'system' && message.subtype === 'init') {
    console.log(`Forked session: ${message.session_id}`)
    // This will be a different session ID
  }
}

// The original session remains unchanged and can still be resumed
const originalContinued = query({
  prompt: "Add authentication to the REST API",
  options: {
    resume: sessionId,
    forkSession: false,  // Continue original session (default)
    model: "claude-sonnet-4-5"
  }
})
python Python
from claude_agent_sdk import query, ClaudeAgentOptions

**Examples:**

Example 1 (unknown):
```unknown
</CodeGroup>

The SDK automatically handles loading the conversation history and context when you resume a session, allowing Claude to continue exactly where it left off.

<Tip>
To track and revert file changes across sessions, see [File Checkpointing](/docs/en/agent-sdk/file-checkpointing).
</Tip>

## Forking Sessions

When resuming a session, you can choose to either continue the original session or fork it into a new branch. By default, resuming continues the original session. Use the `forkSession` option (TypeScript) or `fork_session` option (Python) to create a new session ID that starts from the resumed state.

### When to Fork a Session

Forking is useful when you want to:
- Explore different approaches from the same starting point
- Create multiple conversation branches without modifying the original
- Test changes without affecting the original session history
- Maintain separate conversation paths for different experiments

### Forking vs Continuing

| Behavior | `forkSession: false` (default) | `forkSession: true` |
|----------|-------------------------------|---------------------|
| **Session ID** | Same as original | New session ID generated |
| **History** | Appends to original session | Creates new branch from resume point |
| **Original Session** | Modified | Preserved unchanged |
| **Use Case** | Continue linear conversation | Branch to explore alternatives |

### Example: Forking a Session

<CodeGroup>
```

Example 2 (unknown):
```unknown

```

---

## List Api Keys

**URL:** llms-txt#list-api-keys

**Contents:**
- List
  - Query Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/admin/api_keys/list

**get** `/v1/organizations/api_keys`

- `after_id: optional string`

ID of the object to use as a cursor for pagination. When provided, returns the page of results immediately after this object.

- `before_id: optional string`

ID of the object to use as a cursor for pagination. When provided, returns the page of results immediately before this object.

- `created_by_user_id: optional string`

Filter by the ID of the User who created the object.

- `limit: optional number`

Number of items to return per page.

Defaults to `20`. Ranges from `1` to `1000`.

- `status: optional "active" or "inactive" or "archived"`

Filter by API key status.

- `workspace_id: optional string`

Filter by Workspace ID.

- `data: array of APIKey`

- `created_at: string`

RFC 3339 datetime string indicating when the API Key was created.

- `created_by: object { id, type }`

The ID and type of the actor that created the API key.

ID of the actor that created the object.

Type of the actor that created the object.

- `partial_key_hint: string`

Partially redacted hint for the API key.

- `status: "active" or "inactive" or "archived"`

Status of the API key.

For API Keys, this is always `"api_key"`.

- `workspace_id: string`

ID of the Workspace associated with the API key, or null if the API key belongs to the default Workspace.

First ID in the `data` list. Can be used as the `before_id` for the previous page.

- `has_more: boolean`

Indicates if there are more results in the requested page direction.

Last ID in the `data` list. Can be used as the `after_id` for the next page.

**Examples:**

Example 1 (http):
```http
curl https://api.anthropic.com/v1/organizations/api_keys \
    -H "X-Api-Key: $ANTHROPIC_ADMIN_API_KEY"
```

---

## Verify formatting

**URL:** llms-txt#verify-formatting

print("\n--- Verification ---")
print(f"✓ Tool results sent in single user message: {len(tool_results)} results")
print("✓ No text before tool results in content array")
print("✓ Conversation formatted correctly for future parallel tool use")
typescript TypeScript
#!/usr/bin/env node
// Test script to verify parallel tool calls with the Claude API

import { Anthropic } from '@anthropic-ai/sdk';

const anthropic = new Anthropic({
  apiKey: process.env.ANTHROPIC_API_KEY
});

// Define tools
const tools = [
  {
    name: "get_weather",
    description: "Get the current weather in a given location",
    input_schema: {
      type: "object",
      properties: {
        location: {
          type: "string",
          description: "The city and state, e.g. San Francisco, CA"
        }
      },
      required: ["location"]
    }
  },
  {
    name: "get_time",
    description: "Get the current time in a given timezone",
    input_schema: {
      type: "object",
      properties: {
        timezone: {
          type: "string",
          description: "The timezone, e.g. America/New_York"
        }
      },
      required: ["timezone"]
    }
  }
];

async function testParallelTools() {
  // Make initial request
  console.log("Requesting parallel tool calls...");
  const response = await anthropic.messages.create({
    model: "claude-sonnet-4-5",
    max_tokens: 1024,
    messages: [{
      role: "user",
      content: "What's the weather in SF and NYC, and what time is it there?"
    }],
    tools: tools
  });

// Check for parallel tool calls
  const toolUses = response.content.filter(block => block.type === "tool_use");
  console.log(`\n✓ Claude made ${toolUses.length} tool calls`);

if (toolUses.length > 1) {
    console.log("✓ Parallel tool calls detected!");
    toolUses.forEach(tool => {
      console.log(`  - ${tool.name}: ${JSON.stringify(tool.input)}`);
    });
  } else {
    console.log("✗ No parallel tool calls detected");
  }

// Simulate tool execution and format results correctly
  const toolResults = toolUses.map(toolUse => {
    let result;
    if (toolUse.name === "get_weather") {
      result = toolUse.input.location.includes("San Francisco")
        ? "San Francisco: 68°F, partly cloudy"
        : "New York: 45°F, clear skies";
    } else {
      result = toolUse.input.timezone.includes("Los_Angeles")
        ? "2:30 PM PST"
        : "5:30 PM EST";
    }

return {
      type: "tool_result",
      tool_use_id: toolUse.id,
      content: result
    };
  });

// Get final response with correct formatting
  console.log("\nGetting final response...");
  const finalResponse = await anthropic.messages.create({
    model: "claude-sonnet-4-5",
    max_tokens: 1024,
    messages: [
      { role: "user", content: "What's the weather in SF and NYC, and what time is it there?" },
      { role: "assistant", content: response.content },
      { role: "user", content: toolResults }  // All results in one message!
    ],
    tools: tools
  });

console.log(`\nClaude's response:\n${finalResponse.content[0].text}`);

// Verify formatting
  console.log("\n--- Verification ---");
  console.log(`✓ Tool results sent in single user message: ${toolResults.length} results`);
  console.log("✓ No text before tool results in content array");
  console.log("✓ Conversation formatted correctly for future parallel tool use");
}

testParallelTools().catch(console.error);
text
For maximum efficiency, whenever you need to perform multiple independent operations, invoke all relevant tools simultaneously rather than sequentially.
text
<use_parallel_tool_calls>
For maximum efficiency, whenever you perform multiple independent operations, invoke all relevant tools simultaneously rather than sequentially. Prioritize calling tools in parallel whenever possible. For example, when reading 3 files, run 3 tool calls in parallel to read all 3 files into context at the same time. When running multiple read-only commands like `ls` or `list_dir`, always run all of the commands in parallel. Err on the side of maximizing parallel tool calls rather than running too many tools sequentially.
</use_parallel_tool_calls>
python

**Examples:**

Example 1 (unknown):
```unknown

```

Example 2 (unknown):
```unknown
</CodeGroup>

This script demonstrates:
- How to properly format parallel tool calls and results
- How to verify that parallel calls are being made
- The correct message structure that encourages future parallel tool use
- Common mistakes to avoid (like text before tool results)

Run this script to test your implementation and ensure Claude is making parallel tool calls effectively.

</section>

#### Maximizing parallel tool use

While Claude 4 models have excellent parallel tool use capabilities by default, you can increase the likelihood of parallel tool execution across all models with targeted prompting:

<section title="System prompts for parallel tool use">

For Claude 4 models (Opus 4, and Sonnet 4), add this to your system prompt:
```

Example 3 (unknown):
```unknown
For even stronger parallel tool use (recommended if the default isn't sufficient), use:
```

Example 4 (unknown):
```unknown
</section>
<section title="User message prompting">

You can also encourage parallel tool use within specific user messages:
```

---

## API Reference

**URL:** llms-txt#api-reference

**Contents:**
- Contents
- Authentication and setup
- Core methods
- Workflows and feedback loops
  - Use workflows for complex tasks
- Research synthesis workflow
- PDF form filling workflow
  - Implement feedback loops
- Content review process
- Document editing process

## Contents
- Authentication and setup
- Core methods (create, read, update, delete)
- Advanced features (batch operations, webhooks)
- Error handling patterns
- Code examples

## Authentication and setup
...

## Core methods
...
`markdown
## Research synthesis workflow

Copy this checklist and track your progress:

**Step 1: Read all source documents**

Review each document in the `sources/` directory. Note the main arguments and supporting evidence.

**Step 2: Identify key themes**

Look for patterns across sources. What themes appear repeatedly? Where do sources agree or disagree?

**Step 3: Cross-reference claims**

For each major claim, verify it appears in the source material. Note which source supports each point.

**Step 4: Create structured summary**

Organize findings by theme. Include:
- Main claim
- Supporting evidence from sources
- Conflicting viewpoints (if any)

**Step 5: Verify citations**

Check that every claim references the correct source document. If citations are incomplete, return to Step 3.
`markdown
## PDF form filling workflow

Copy this checklist and check off items as you complete them:

**Step 1: Analyze the form**

Run: `python scripts/analyze_form.py input.pdf`

This extracts form fields and their locations, saving to `fields.json`.

**Step 2: Create field mapping**

Edit `fields.json` to add values for each field.

**Step 3: Validate mapping**

Run: `python scripts/validate_fields.py fields.json`

Fix any validation errors before continuing.

**Step 4: Fill the form**

Run: `python scripts/fill_form.py input.pdf fields.json output.pdf`

**Step 5: Verify output**

Run: `python scripts/verify_output.py output.pdf`

If verification fails, return to Step 2.
markdown
## Content review process

1. Draft your content following the guidelines in STYLE_GUIDE.md
2. Review against the checklist:
   - Check terminology consistency
   - Verify examples follow the standard format
   - Confirm all required sections are present
3. If issues found:
   - Note each issue with specific section reference
   - Revise the content
   - Review the checklist again
4. Only proceed when all requirements are met
5. Finalize and save the document
markdown
## Document editing process

1. Make your edits to `word/document.xml`
2. **Validate immediately**: `python ooxml/scripts/validate.py unpacked_dir/`
3. If validation fails:
   - Review the error message carefully
   - Fix the issues in the XML
   - Run validation again
4. **Only proceed when validation passes**
5. Rebuild: `python ooxml/scripts/pack.py unpacked_dir/ output.docx`
6. Test the output document
markdown
If you're doing this before August 2025, use the old API.
After August 2025, use the new API.
markdown
## Current method

Use the v2 API endpoint: `api.example.com/v2/messages`

<details>
<summary>Legacy v1 API (deprecated 2025-08)</summary>

The v1 API used: `api.example.com/v1/messages`

This endpoint is no longer supported.
</details>
`markdown
## Report structure

ALWAYS use this exact template structure:

**Examples:**

Example 1 (unknown):
```unknown
Claude can then read the complete file or jump to specific sections as needed.

For details on how this filesystem-based architecture enables progressive disclosure, see the [Runtime environment](#runtime-environment) section in the Advanced section below.

## Workflows and feedback loops

### Use workflows for complex tasks

Break complex operations into clear, sequential steps. For particularly complex workflows, provide a checklist that Claude can copy into its response and check off as it progresses.

**Example 1: Research synthesis workflow** (for Skills without code):
```

Example 2 (unknown):
```unknown
Research Progress:
- [ ] Step 1: Read all source documents
- [ ] Step 2: Identify key themes
- [ ] Step 3: Cross-reference claims
- [ ] Step 4: Create structured summary
- [ ] Step 5: Verify citations
```

Example 3 (unknown):
```unknown
This example shows how workflows apply to analysis tasks that don't require code. The checklist pattern works for any complex, multi-step process.

**Example 2: PDF form filling workflow** (for Skills with code):
```

Example 4 (unknown):
```unknown
Task Progress:
- [ ] Step 1: Analyze the form (run analyze_form.py)
- [ ] Step 2: Create field mapping (edit fields.json)
- [ ] Step 3: Validate mapping (run validate_fields.py)
- [ ] Step 4: Fill the form (run fill_form.py)
- [ ] Step 5: Verify output (run verify_output.py)
```

---

## Resume session with empty prompt, then rewind

**URL:** llms-txt#resume-session-with-empty-prompt,-then-rewind

**Contents:**
- Next steps

async with ClaudeSDKClient(ClaudeAgentOptions(
    enable_file_checkpointing=True,
    resume=session_id
)) as client:
    await client.query("")
    async for message in client.receive_response():
        await client.rewind_files(checkpoint_id)
        break
typescript TypeScript
// Resume session with empty prompt, then rewind
const rewindQuery = query({
  prompt: "",
  options: { ...opts, resume: sessionId }
});

for await (const msg of rewindQuery) {
  await rewindQuery.rewindFiles(checkpointId);
  break;
}
```

- **[Sessions](/docs/en/agent-sdk/sessions)**: learn how to resume sessions, which is required for rewinding after the stream completes. Covers session IDs, resuming conversations, and session forking.
- **[Permissions](/docs/en/agent-sdk/permissions)**: configure which tools Claude can use and how file modifications are approved. Useful if you want more control over when edits happen.
- **[TypeScript SDK reference](/docs/en/agent-sdk/typescript)**: complete API reference including all options for `query()` and the `rewindFiles()` method.
- **[Python SDK reference](/docs/en/agent-sdk/python)**: complete API reference including all options for `ClaudeAgentOptions` and the `rewind_files()` method.

**Examples:**

Example 1 (unknown):
```unknown

```

---

## Rewind file changes with checkpointing

**URL:** llms-txt#rewind-file-changes-with-checkpointing

**Contents:**
- How checkpointing works
- Implement checkpointing
- Common patterns
  - Checkpoint before risky operations
  - Multiple restore points

Track file changes during agent sessions and restore files to any previous state

File checkpointing tracks file modifications made through the Write, Edit, and NotebookEdit tools during an agent session, allowing you to rewind files to any previous state. Want to try it out? Jump to the [interactive example](#try-it-out).

With checkpointing, you can:

- **Undo unwanted changes** by restoring files to a known good state
- **Explore alternatives** by restoring to a checkpoint and trying a different approach
- **Recover from errors** when the agent makes incorrect modifications

<Warning>
Only changes made through the Write, Edit, and NotebookEdit tools are tracked. Changes made through Bash commands (like `echo > file.txt` or `sed -i`) are not captured by the checkpoint system.
</Warning>

## How checkpointing works

When you enable file checkpointing, the SDK creates backups of files before modifying them through the Write, Edit, or NotebookEdit tools. User messages in the response stream include a checkpoint UUID that you can use as a restore point.

Checkpoint works with these built-in tools that the agent uses to modify files:

| Tool | Description |
|------|-------------|
| Write | Creates a new file or overwrites an existing file with new content |
| Edit | Makes targeted edits to specific parts of an existing file |
| NotebookEdit | Modifies cells in Jupyter notebooks (`.ipynb` files) |

<Note>
File rewinding restores files on disk to a previous state. It does not rewind the conversation itself. The conversation history and context remain intact after calling `rewindFiles()` (TypeScript) or `rewind_files()` (Python).
</Note>

The checkpoint system tracks:

- Files created during the session
- Files modified during the session
- The original content of modified files

When you rewind to a checkpoint, created files are deleted and modified files are restored to their content at that point.

## Implement checkpointing

To use file checkpointing, enable it in your options, capture checkpoint UUIDs from the response stream, then call `rewindFiles()` (TypeScript) or `rewind_files()` (Python) when you need to restore.

The following example shows the complete flow: enable checkpointing, capture the checkpoint UUID and session ID from the response stream, then resume the session later to rewind files. Each step is explained in detail below.

<Step title="Set the environment variable">

File checkpointing requires the `CLAUDE_CODE_ENABLE_SDK_FILE_CHECKPOINTING` environment variable. You can set it either via command line before running your script, or directly in the SDK options.

**Option 1: Set via command line**

**Option 2: Set in SDK options**

Pass the environment variable through the `env` option when configuring the SDK:

<Step title="Enable checkpointing">

Configure your SDK options to enable checkpointing and receive checkpoint UUIDs:

| Option | Python | TypeScript | Description |
|--------|--------|------------|-------------|
| Enable checkpointing | `enable_file_checkpointing=True` | `enableFileCheckpointing: true` | Tracks file changes for rewinding |
| Receive checkpoint UUIDs | `extra_args={"replay-user-messages": None}` | `extraArgs: { 'replay-user-messages': null }` | Required to get user message UUIDs in the stream |

<Step title="Capture checkpoint UUID and session ID">

With the `replay-user-messages` option set (shown above), each user message in the response stream has a UUID that serves as a checkpoint.

For most use cases, capture the first user message UUID (`message.uuid`); rewinding to it restores all files to their original state. To store multiple checkpoints and rewind to intermediate states, see [Multiple restore points](#multiple-restore-points).

Capturing the session ID (`message.session_id`) is optional; you only need it if you want to rewind later, after the stream completes. If you're calling `rewindFiles()` immediately while still processing messages (as the example in [Checkpoint before risky operations](#checkpoint-before-risky-operations) does), you can skip capturing the session ID.

<Step title="Rewind files">

To rewind after the stream completes, resume the session with an empty prompt and call `rewind_files()` (Python) or `rewindFiles()` (TypeScript) with your checkpoint UUID. You can also rewind during the stream; see [Checkpoint before risky operations](#checkpoint-before-risky-operations) for that pattern.

If you capture the session ID and checkpoint ID, you can also rewind from the CLI:

These patterns show different ways to capture and use checkpoint UUIDs depending on your use case.

### Checkpoint before risky operations

This pattern keeps only the most recent checkpoint UUID, updating it before each agent turn. If something goes wrong during processing, you can immediately rewind to the last safe state and break out of the loop.

### Multiple restore points

If Claude makes changes across multiple turns, you might want to rewind to a specific point rather than all the way back. For example, if Claude refactors a file in turn one and adds tests in turn two, you might want to keep the refactor but undo the tests.

This pattern stores all checkpoint UUIDs in an array with metadata. After the session completes, you can rewind to any previous checkpoint:

```python Python
import asyncio
import os
from dataclasses import dataclass
from datetime import datetime
from claude_agent_sdk import ClaudeSDKClient, ClaudeAgentOptions, UserMessage, ResultMessage

**Examples:**

Example 1 (unknown):
```unknown

```

Example 2 (unknown):
```unknown
</CodeGroup>

<Steps>

<Step title="Set the environment variable">

File checkpointing requires the `CLAUDE_CODE_ENABLE_SDK_FILE_CHECKPOINTING` environment variable. You can set it either via command line before running your script, or directly in the SDK options.

**Option 1: Set via command line**
```

Example 3 (unknown):
```unknown
**Option 2: Set in SDK options**

Pass the environment variable through the `env` option when configuring the SDK:

<CodeGroup>
```

Example 4 (unknown):
```unknown

```

---

## Save to file

**URL:** llms-txt#save-to-file

**Contents:**
- File storage and limits
  - Storage limits
  - File lifecycle
- Error handling
- Usage and billing
  - Rate limits

with open("downloaded_file.txt", "w") as f:
    f.write(file_content.decode('utf-8'))
typescript TypeScript
import { Anthropic } from '@anthropic-ai/sdk';
import fs from 'fs';

const anthropic = new Anthropic();

const fileContent = await anthropic.beta.files.download(
  "file_011CNha8iCJcU1wXNR6q4V8w",
  { betas: ['files-api-2025-04-14'] },
);

// Save to file
fs.writeFileSync("downloaded_file.txt", fileContent);
json
{
  "type": "error",
  "error": {
    "type": "invalid_request_error",
    "message": "File not found: file_011CNha8iCJcU1wXNR6q4V8w"
  }
}
```

File API operations are **free**:
- Uploading files
- Downloading files
- Listing files
- Getting file metadata  
- Deleting files

File content used in `Messages` requests are priced as input tokens. You can only download files created by [skills](/docs/en/build-with-claude/skills-guide) or the [code execution tool](/docs/en/agents-and-tools/tool-use/code-execution-tool).

During the beta period:
- File-related API calls are limited to approximately 100 requests per minute
- [Contact us](mailto:sales@anthropic.com) if you need higher limits for your use case

**Examples:**

Example 1 (unknown):
```unknown

```

Example 2 (unknown):
```unknown
</CodeGroup>

<Note>
You can only download files that were created by [skills](/docs/en/build-with-claude/skills-guide) or the [code execution tool](/docs/en/agents-and-tools/tool-use/code-execution-tool). Files that you uploaded cannot be downloaded.
</Note>

---

## File storage and limits

### Storage limits

- **Maximum file size:** 500 MB per file
- **Total storage:** 100 GB per organization

### File lifecycle

- Files are scoped to the workspace of the API key. Other API keys can use files created by any other API key associated with the same workspace
- Files persist until you delete them
- Deleted files cannot be recovered
- Files are inaccessible via the API shortly after deletion, but they may persist in active `Messages` API calls and associated tool uses
- Files that users delete will be deleted in accordance with our [data retention policy](https://privacy.claude.com/en/articles/7996866-how-long-do-you-store-my-organization-s-data).

---

## Error handling

Common errors when using the Files API include:

- **File not found (404):** The specified `file_id` doesn't exist or you don't have access to it
- **Invalid file type (400):** The file type doesn't match the content block type (e.g., using an image file in a document block)
- **Exceeds context window size (400):** The file is larger than the context window size (e.g. using a 500 MB plaintext file in a `/v1/messages` request)
- **Invalid filename (400):** Filename doesn't meet the length requirements (1-255 characters) or contains forbidden characters (`<`, `>`, `:`, `"`, `|`, `?`, `*`, `\`, `/`, or unicode characters 0-31)
- **File too large (413):** File exceeds the 500 MB limit
- **Storage limit exceeded (403):** Your organization has reached the 100 GB storage limit
```

---

## Provide search results directly in the user message

**URL:** llms-txt#provide-search-results-directly-in-the-user-message

**Contents:**
- Claude's response with citations
  - Citation fields
- Multiple content blocks
- Advanced usage
  - Combining both methods

response = client.messages.create(
    model="claude-sonnet-4-5",
    max_tokens=1024,
    messages=[
        MessageParam(
            role="user",
            content=[
                SearchResultBlockParam(
                    type="search_result",
                    source="https://docs.company.com/api-reference",
                    title="API Reference - Authentication",
                    content=[
                        TextBlockParam(
                            type="text",
                            text="All API requests must include an API key in the Authorization header. Keys can be generated from the dashboard. Rate limits: 1000 requests per hour for standard tier, 10000 for premium."
                        )
                    ],
                    citations={"enabled": True}
                ),
                SearchResultBlockParam(
                    type="search_result",
                    source="https://docs.company.com/quickstart",
                    title="Getting Started Guide",
                    content=[
                        TextBlockParam(
                            type="text",
                            text="To get started: 1) Sign up for an account, 2) Generate an API key from the dashboard, 3) Install our SDK using pip install company-sdk, 4) Initialize the client with your API key."
                        )
                    ],
                    citations={"enabled": True}
                ),
                TextBlockParam(
                    type="text",
                    text="Based on these search results, how do I authenticate API requests and what are the rate limits?"
                )
            ]
        )
    ]
)

print(response.model_dump_json(indent=2))
typescript TypeScript
import { Anthropic } from '@anthropic-ai/sdk';

const anthropic = new Anthropic();

// Provide search results directly in the user message
const response = await anthropic.messages.create({
  model: "claude-sonnet-4-5",
  max_tokens: 1024,
  messages: [
    {
      role: "user",
      content: [
        {
          type: "search_result" as const,
          source: "https://docs.company.com/api-reference",
          title: "API Reference - Authentication",
          content: [
            {
              type: "text" as const,
              text: "All API requests must include an API key in the Authorization header. Keys can be generated from the dashboard. Rate limits: 1000 requests per hour for standard tier, 10000 for premium."
            }
          ],
          citations: { enabled: true }
        },
        {
          type: "search_result" as const,
          source: "https://docs.company.com/quickstart",
          title: "Getting Started Guide",
          content: [
            {
              type: "text" as const,
              text: "To get started: 1) Sign up for an account, 2) Generate an API key from the dashboard, 3) Install our SDK using pip install company-sdk, 4) Initialize the client with your API key."
            }
          ],
          citations: { enabled: true }
        },
        {
          type: "text" as const,
          text: "Based on these search results, how do I authenticate API requests and what are the rate limits?"
        }
      ]
    }
  ]
});

console.log(response);
bash Shell
#!/bin/sh
curl https://api.anthropic.com/v1/messages \
     --header "x-api-key: $ANTHROPIC_API_KEY" \
     --header "anthropic-version: 2023-06-01" \
     --header "content-type: application/json" \
     --data \
'{
    "model": "claude-sonnet-4-5",
    "max_tokens": 1024,
    "messages": [
        {
            "role": "user",
            "content": [
                {
                    "type": "search_result",
                    "source": "https://docs.company.com/api-reference",
                    "title": "API Reference - Authentication",
                    "content": [
                        {
                            "type": "text",
                            "text": "All API requests must include an API key in the Authorization header. Keys can be generated from the dashboard. Rate limits: 1000 requests per hour for standard tier, 10000 for premium."
                        }
                    ],
                    "citations": {
                        "enabled": true
                    }
                },
                {
                    "type": "search_result",
                    "source": "https://docs.company.com/quickstart",
                    "title": "Getting Started Guide",
                    "content": [
                        {
                            "type": "text",
                            "text": "To get started: 1) Sign up for an account, 2) Generate an API key from the dashboard, 3) Install our SDK using pip install company-sdk, 4) Initialize the client with your API key."
                        }
                    ],
                    "citations": {
                        "enabled": true
                    }
                },
                {
                    "type": "text",
                    "text": "Based on these search results, how do I authenticate API requests and what are the rate limits?"
                }
            ]
        }
    ]
}'
json
{
  "role": "assistant",
  "content": [
    {
      "type": "text",
      "text": "To authenticate API requests, you need to include an API key in the Authorization header",
      "citations": [
        {
          "type": "search_result_location",
          "source": "https://docs.company.com/api-reference",
          "title": "API Reference - Authentication",
          "cited_text": "All API requests must include an API key in the Authorization header",
          "search_result_index": 0,
          "start_block_index": 0,
          "end_block_index": 0
        }
      ]
    },
    {
      "type": "text",
      "text": ". You can generate API keys from your dashboard",
      "citations": [
        {
          "type": "search_result_location",
          "source": "https://docs.company.com/api-reference",
          "title": "API Reference - Authentication",
          "cited_text": "Keys can be generated from the dashboard",
          "search_result_index": 0,
          "start_block_index": 0,
          "end_block_index": 0
        }
      ]
    },
    {
      "type": "text",
      "text": ". The rate limits are 1,000 requests per hour for the standard tier and 10,000 requests per hour for the premium tier.",
      "citations": [
        {
          "type": "search_result_location",
          "source": "https://docs.company.com/api-reference",
          "title": "API Reference - Authentication",
          "cited_text": "Rate limits: 1000 requests per hour for standard tier, 10000 for premium",
          "search_result_index": 0,
          "start_block_index": 0,
          "end_block_index": 0
        }
      ]
    }
  ]
}
json
{
  "type": "search_result",
  "source": "https://docs.company.com/api-guide",
  "title": "API Documentation",
  "content": [
    {
      "type": "text",
      "text": "Authentication: All API requests require an API key."
    },
    {
      "type": "text",
      "text": "Rate Limits: The API allows 1000 requests per hour per key."
    },
    {
      "type": "text",
      "text": "Error Handling: The API returns standard HTTP status codes."
    }
  ]
}
python

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

## Claude's response with citations

Regardless of how search results are provided, Claude automatically includes citations when using information from them:
```

Example 4 (unknown):
```unknown
### Citation fields

Each citation includes:

| Field | Type | Description |
|-------|------|-------------|
| `type` | string | Always `"search_result_location"` for search result citations |
| `source` | string | The source from the original search result |
| `title` | string or null | The title from the original search result |
| `cited_text` | string | The exact text being cited |
| `search_result_index` | integer | Index of the search result (0-based) |
| `start_block_index` | integer | Starting position in the content array |
| `end_block_index` | integer | Ending position in the content array |

Note: The `search_result_index` refers to the index of the search result content block (0-based), regardless of how the search results were provided (tool call or top-level content).

## Multiple content blocks

Search results can contain multiple text blocks in the `content` array:
```

---

## Store checkpoint metadata for better tracking

**URL:** llms-txt#store-checkpoint-metadata-for-better-tracking

**Contents:**
- Try it out
- Limitations
- Troubleshooting
  - Checkpointing options not recognized
  - User messages don't have UUIDs
  - "No file checkpoint found for message" error
  - "ProcessTransport is not ready for writing" error

@dataclass
class Checkpoint:
    id: str
    description: str
    timestamp: datetime

async def main():
    options = ClaudeAgentOptions(
        enable_file_checkpointing=True,
        permission_mode="acceptEdits",
        extra_args={"replay-user-messages": None},
        env={**os.environ, "CLAUDE_CODE_ENABLE_SDK_FILE_CHECKPOINTING": "1"}
    )

checkpoints = []
    session_id = None

async with ClaudeSDKClient(options) as client:
        await client.query("Refactor the authentication module")

async for message in client.receive_response():
            if isinstance(message, UserMessage) and message.uuid:
                checkpoints.append(Checkpoint(
                    id=message.uuid,
                    description=f"After turn {len(checkpoints) + 1}",
                    timestamp=datetime.now()
                ))
            if isinstance(message, ResultMessage) and not session_id:
                session_id = message.session_id

# Later: rewind to any checkpoint by resuming the session
    if checkpoints and session_id:
        target = checkpoints[0]  # Pick any checkpoint
        async with ClaudeSDKClient(ClaudeAgentOptions(
            enable_file_checkpointing=True,
            resume=session_id
        )) as client:
            await client.query("")  # Empty prompt to open the connection
            async for message in client.receive_response():
                await client.rewind_files(target.id)
                break
        print(f"Rewound to: {target.description}")

asyncio.run(main())
typescript TypeScript
import { query } from "@anthropic-ai/claude-agent-sdk";

// Store checkpoint metadata for better tracking
interface Checkpoint {
  id: string;
  description: string;
  timestamp: Date;
}

async function main() {
  const opts = {
    enableFileCheckpointing: true,
    permissionMode: "acceptEdits" as const,
    extraArgs: { 'replay-user-messages': null },
    env: { ...process.env, CLAUDE_CODE_ENABLE_SDK_FILE_CHECKPOINTING: '1' }
  };

const response = query({
    prompt: "Refactor the authentication module",
    options: opts
  });

const checkpoints: Checkpoint[] = [];
  let sessionId: string | undefined;

for await (const message of response) {
    if (message.type === 'user' && message.uuid) {
      checkpoints.push({
        id: message.uuid,
        description: `After turn ${checkpoints.length + 1}`,
        timestamp: new Date()
      });
    }
    if ('session_id' in message && !sessionId) {
      sessionId = message.session_id;
    }
  }

// Later: rewind to any checkpoint by resuming the session
  if (checkpoints.length > 0 && sessionId) {
    const target = checkpoints[0];  // Pick any checkpoint
    const rewindQuery = query({
      prompt: "",  // Empty prompt to open the connection
      options: { ...opts, resume: sessionId }
    });

for await (const msg of rewindQuery) {
      await rewindQuery.rewindFiles(target.id);
      break;
    }
    console.log(`Rewound to: ${target.description}`);
  }
}

main();
python utils.py
def add(a, b):
    return a + b

def subtract(a, b):
    return a - b

def multiply(a, b):
    return a * b

def divide(a, b):
    if b == 0:
        raise ValueError("Cannot divide by zero")
    return a / b
typescript utils.ts
export function add(a: number, b: number): number {
  return a + b;
}

export function subtract(a: number, b: number): number {
  return a - b;
}

export function multiply(a: number, b: number): number {
  return a * b;
}

export function divide(a: number, b: number): number {
  if (b === 0) {
    throw new Error("Cannot divide by zero");
  }
  return a / b;
}
python try_checkpointing.py
import asyncio
from claude_agent_sdk import ClaudeSDKClient, ClaudeAgentOptions, UserMessage, ResultMessage

async def main():
    # Configure the SDK with checkpointing enabled
    # - enable_file_checkpointing: Track file changes for rewinding
    # - permission_mode: Auto-accept file edits without prompting
    # - extra_args: Required to receive user message UUIDs in the stream
    options = ClaudeAgentOptions(
        enable_file_checkpointing=True,
        permission_mode="acceptEdits",
        extra_args={"replay-user-messages": None}
    )

checkpoint_id = None  # Store the user message UUID for rewinding
    session_id = None     # Store the session ID for resuming

print("Running agent to add doc comments to utils.py...\n")

# Run the agent and capture checkpoint data from the response stream
    async with ClaudeSDKClient(options) as client:
        await client.query("Add doc comments to utils.py")

async for message in client.receive_response():
            # Capture the first user message UUID - this is our restore point
            if isinstance(message, UserMessage) and message.uuid and not checkpoint_id:
                checkpoint_id = message.uuid
            # Capture the session ID so we can resume later
            if isinstance(message, ResultMessage):
                session_id = message.session_id

print("Done! Open utils.py to see the added doc comments.\n")

# Ask the user if they want to rewind the changes
    if checkpoint_id and session_id:
        response = input("Rewind to remove the doc comments? (y/n): ")

if response.lower() == "y":
            # Resume the session with an empty prompt, then rewind
            async with ClaudeSDKClient(ClaudeAgentOptions(
                enable_file_checkpointing=True,
                resume=session_id
            )) as client:
                await client.query("")  # Empty prompt opens the connection
                async for message in client.receive_response():
                    await client.rewind_files(checkpoint_id)  # Restore files
                    break

print("\n✓ File restored! Open utils.py to verify the doc comments are gone.")
        else:
            print("\nKept the modified file.")

asyncio.run(main())
typescript try_checkpointing.ts
import { query } from "@anthropic-ai/claude-agent-sdk";
import * as readline from "readline";

async function main() {
  // Configure the SDK with checkpointing enabled
  // - enableFileCheckpointing: Track file changes for rewinding
  // - permissionMode: Auto-accept file edits without prompting
  // - extraArgs: Required to receive user message UUIDs in the stream
  const opts = {
    enableFileCheckpointing: true,
    permissionMode: "acceptEdits" as const,
    extraArgs: { 'replay-user-messages': null }
  };

let sessionId: string | undefined;    // Store the session ID for resuming
  let checkpointId: string | undefined; // Store the user message UUID for rewinding

console.log("Running agent to add doc comments to utils.ts...\n");

// Run the agent and capture checkpoint data from the response stream
  const response = query({
    prompt: "Add doc comments to utils.ts",
    options: opts
  });

for await (const message of response) {
    // Capture the first user message UUID - this is our restore point
    if (message.type === "user" && message.uuid && !checkpointId) {
      checkpointId = message.uuid;
    }
    // Capture the session ID so we can resume later
    if ("session_id" in message) {
      sessionId = message.session_id;
    }
  }

console.log("Done! Open utils.ts to see the added doc comments.\n");

// Ask the user if they want to rewind the changes
  if (checkpointId && sessionId) {
    const rl = readline.createInterface({
      input: process.stdin,
      output: process.stdout
    });

const answer = await new Promise<string>((resolve) => {
      rl.question("Rewind to remove the doc comments? (y/n): ", resolve);
    });
    rl.close();

if (answer.toLowerCase() === "y") {
      // Resume the session with an empty prompt, then rewind
      const rewindQuery = query({
        prompt: "",  // Empty prompt opens the connection
        options: { ...opts, resume: sessionId }
      });

for await (const msg of rewindQuery) {
        await rewindQuery.rewindFiles(checkpointId);  // Restore files
        break;
      }

console.log("\n✓ File restored! Open utils.ts to verify the doc comments are gone.");
    } else {
      console.log("\nKept the modified file.");
    }
  }
}

main();
bash
    export CLAUDE_CODE_ENABLE_SDK_FILE_CHECKPOINTING=1
    python try_checkpointing.py
    bash
    export CLAUDE_CODE_ENABLE_SDK_FILE_CHECKPOINTING=1
    npx tsx try_checkpointing.ts
    python Python

**Examples:**

Example 1 (unknown):
```unknown

```

Example 2 (unknown):
```unknown
</CodeGroup>

## Try it out

This complete example creates a small utility file, has the agent add documentation comments, shows you the changes, then asks if you want to rewind.

Before you begin, make sure you have the [Claude Agent SDK installed](/docs/en/agent-sdk/quickstart).

<Steps>

<Step title="Create a test file">

Create a new file called `utils.py` (Python) or `utils.ts` (TypeScript) and paste the following code:

<CodeGroup>
```

Example 3 (unknown):
```unknown

```

Example 4 (unknown):
```unknown
</CodeGroup>

</Step>

<Step title="Run the interactive example">

Create a new file called `try_checkpointing.py` (Python) or `try_checkpointing.ts` (TypeScript) in the same directory as your utility file, and paste the following code.

This script asks Claude to add doc comments to your utility file, then gives you the option to rewind and restore the original.

<CodeGroup>
```

---

## Get started with Claude

**URL:** llms-txt#get-started-with-claude

**Contents:**
- Prerequisites
- Call the API
- Next steps

Make your first API call to Claude and build a simple web search assistant

- An Anthropic [Console account](/)
- An [API key](/settings/keys)

<Tabs>
  <Tab title="cURL">
    <Steps>
      <Step title="Set your API key">
        Get your API key at the [Claude Console](/settings/keys) and set it as an environment variable:

<Step title="Make your first API call">
        Run this command to create a simple web search assistant:

**Example output:**
        
      </Step>
    </Steps>
  </Tab>

<Tab title="Python">
    <Steps>
      <Step title="Set your API key">
        Get your API key from the [Claude Console](/settings/keys) and set it as an environment variable:

<Step title="Install the SDK">
        Install the Anthropic Python SDK:

<Step title="Create your code">
        Save this as `quickstart.py`:

<Step title="Run your code">

**Example output:**
        
      </Step>
    </Steps>
  </Tab>

<Tab title="TypeScript">
    <Steps>
      <Step title="Set your API key">
        Get your API key from the [Claude Console](/settings/keys) and set it as an environment variable:

<Step title="Install the SDK">
        Install the Anthropic TypeScript SDK:

<Step title="Create your code">
        Save this as `quickstart.ts`:

<Step title="Run your code">

**Example output:**
        
      </Step>
    </Steps>
  </Tab>

<Tab title="Java">
    <Steps>
      <Step title="Set your API key">
        Get your API key from the [Claude Console](/settings/keys) and set it as an environment variable:

<Step title="Install the SDK">
        Add the Anthropic Java SDK to your project. First find the current version on [Maven Central](https://central.sonatype.com/artifact/com.anthropic/anthropic-java).

**Maven:**
        
      </Step>

<Step title="Create your code">
        Save this as `QuickStart.java`:

<Step title="Run your code">

**Example output:**
        
      </Step>
    </Steps>
  </Tab>
</Tabs>

Now that you have made your first Claude API request, it's time to explore what else is possible:

<CardGroup cols={3}>
  <Card title="Working with Messages" icon="messages" href="/docs/en/build-with-claude/working-with-messages">
    Learn common patterns for the Messages API.
  </Card>
  <Card title="Features Overview" icon="brain" href="/docs/en/api/overview">
    Explore Claude's advanced features and capabilities.
  </Card>
  <Card title="Client SDKs" icon="code-brackets" href="/docs/en/api/client-sdks">
    Discover Anthropic client libraries.
  </Card>
  <Card title="Claude Cookbook" icon="chef-hat" href="https://platform.claude.com/cookbooks">
    Learn with interactive Jupyter notebooks.
  </Card>
</CardGroup>

**Examples:**

Example 1 (bash):
```bash
export ANTHROPIC_API_KEY='your-api-key-here'
```

Example 2 (bash):
```bash
curl https://api.anthropic.com/v1/messages \
          -H "Content-Type: application/json" \
          -H "x-api-key: $ANTHROPIC_API_KEY" \
          -H "anthropic-version: 2023-06-01" \
          -d '{
            "model": "claude-sonnet-4-5",
            "max_tokens": 1000,
            "messages": [
              {
                "role": "user", 
                "content": "What should I search for to find the latest developments in renewable energy?"
              }
            ]
          }'
```

Example 3 (json):
```json
{
          "id": "msg_01HCDu5LRGeP2o7s2xGmxyx8",
          "type": "message", 
          "role": "assistant",
          "content": [
            {
              "type": "text",
              "text": "Here are some effective search strategies to find the latest renewable energy developments:\n\n## Search Terms to Use:\n- \"renewable energy news 2024\"\n- \"clean energy breakthrough\"\n- \"solar/wind/battery technology advances\"\n- \"green energy innovations\"\n- \"climate tech developments\"\n- \"energy storage solutions\"\n\n## Best Sources to Check:\n\n**News & Industry Sites:**\n- Renewable Energy World\n- GreenTech Media (now Wood Mackenzie)\n- Energy Storage News\n- CleanTechnica\n- PV Magazine (for solar)\n- WindPower Engineering & Development..."
            }
          ],
          "model": "claude-sonnet-4-5",
          "stop_reason": "end_turn",
          "usage": {
            "input_tokens": 21,
            "output_tokens": 305
          }
        }
```

Example 4 (bash):
```bash
export ANTHROPIC_API_KEY='your-api-key-here'
```

---

## Message Batches API

**URL:** llms-txt#message-batches-api

**Contents:**
- How the Message Batches API works
  - Batch limitations
  - Supported models
  - What can be batched
- Pricing
- How to use the Message Batches API
  - Prepare and create your batch
  - Tracking your batch
  - Listing all Message Batches

The Message Batches API is a powerful, cost-effective way to asynchronously process large volumes of [Messages](/docs/en/api/messages) requests. This approach is well-suited to tasks that do not require immediate responses, with most batches finishing in less than 1 hour while reducing costs by 50% and increasing throughput.

You can [explore the API reference directly](/docs/en/api/creating-message-batches), in addition to this guide.

## How the Message Batches API works

When you send a request to the Message Batches API:

1. The system creates a new Message Batch with the provided Messages requests.
2. The batch is then processed asynchronously, with each request handled independently.
3. You can poll for the status of the batch and retrieve results when processing has ended for all requests.

This is especially useful for bulk operations that don't require immediate results, such as:
- Large-scale evaluations: Process thousands of test cases efficiently.
- Content moderation: Analyze large volumes of user-generated content asynchronously.
- Data analysis: Generate insights or summaries for large datasets.
- Bulk content generation: Create large amounts of text for various purposes (e.g., product descriptions, article summaries).

### Batch limitations
- A Message Batch is limited to either 100,000 Message requests or 256 MB in size, whichever is reached first.
- We process each batch as fast as possible, with most batches completing within 1 hour. You will be able to access batch results when all messages have completed or after 24 hours, whichever comes first. Batches will expire if processing does not complete within 24 hours.
- Batch results are available for 29 days after creation. After that, you may still view the Batch, but its results will no longer be available for download.
- Batches are scoped to a [Workspace](/settings/workspaces). You may view all batches—and their results—that were created within the Workspace that your API key belongs to.
- Rate limits apply to both Batches API HTTP requests and the number of requests within a batch waiting to be processed. See [Message Batches API rate limits](/docs/en/api/rate-limits#message-batches-api). Additionally, we may slow down processing based on current demand and your request volume. In that case, you may see more requests expiring after 24 hours.
- Due to high throughput and concurrent processing, batches may go slightly over your Workspace's configured [spend limit](/settings/limits).

All [active models](/docs/en/about-claude/models/overview) support the Message Batches API.

### What can be batched
Any request that you can make to the Messages API can be included in a batch. This includes:

- Vision
- Tool use
- System messages
- Multi-turn conversations
- Any beta features

Since each request in the batch is processed independently, you can mix different types of requests within a single batch.

<Tip>
Since batches can take longer than 5 minutes to process, consider using the [1-hour cache duration](/docs/en/build-with-claude/prompt-caching#1-hour-cache-duration) with prompt caching for better cache hit rates when processing batches with shared context.
</Tip>

The Batches API offers significant cost savings. All usage is charged at 50% of the standard API prices.

| Model             | Batch input      | Batch output    |
|-------------------|------------------|-----------------|
| Claude Opus 4.5     | $2.50 / MTok     | $12.50 / MTok   |
| Claude Opus 4.1     | $7.50 / MTok     | $37.50 / MTok   |
| Claude Opus 4     | $7.50 / MTok     | $37.50 / MTok   |
| Claude Sonnet 4.5   | $1.50 / MTok     | $7.50 / MTok    |
| Claude Sonnet 4   | $1.50 / MTok     | $7.50 / MTok    |
| Claude Sonnet 3.7 ([deprecated](/docs/en/about-claude/model-deprecations)) | $1.50 / MTok     | $7.50 / MTok    |
| Claude Haiku 4.5  | $0.50 / MTok     | $2.50 / MTok    |
| Claude Haiku 3.5  | $0.40 / MTok     | $2 / MTok       |
| Claude Opus 3 ([deprecated](/docs/en/about-claude/model-deprecations))  | $7.50 / MTok     | $37.50 / MTok   |
| Claude Haiku 3    | $0.125 / MTok    | $0.625 / MTok   |

---
## How to use the Message Batches API

### Prepare and create your batch

A Message Batch is composed of a list of requests to create a Message. The shape of an individual request is comprised of:
- A unique `custom_id` for identifying the Messages request
- A `params` object with the standard [Messages API](/docs/en/api/messages) parameters

You can [create a batch](/docs/en/api/creating-message-batches) by passing this list into the `requests` parameter:

In this example, two separate requests are batched together for asynchronous processing. Each request has a unique `custom_id` and contains the standard parameters you'd use for a Messages API call.

<Tip>
  **Test your batch requests with the Messages API**

Validation of the `params` object for each message request is performed asynchronously, and validation errors are returned when processing of the entire batch has ended. You can ensure that you are building your input correctly by verifying your request shape with the [Messages API](/docs/en/api/messages) first.
</Tip>

When a batch is first created, the response will have a processing status of `in_progress`.

### Tracking your batch

The Message Batch's `processing_status` field indicates the stage of processing the batch is in. It starts as `in_progress`, then updates to `ended` once all the requests in the batch have finished processing, and results are ready. You can monitor the state of your batch by visiting the [Console](/settings/workspaces/default/batches), or using the [retrieval endpoint](/docs/en/api/retrieving-message-batches).

#### Polling for Message Batch completion

To poll a Message Batch, you'll need its `id`, which is provided in the response when creating a batch or by listing batches. You can implement a polling loop that checks the batch status periodically until processing has ended:

### Listing all Message Batches

You can list all Message Batches in your Workspace using the [list endpoint](/docs/en/api/listing-message-batches). The API supports pagination, automatically fetching additional pages as needed:

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

```

Example 4 (unknown):
```unknown
</CodeGroup>

In this example, two separate requests are batched together for asynchronous processing. Each request has a unique `custom_id` and contains the standard parameters you'd use for a Messages API call.

<Tip>
  **Test your batch requests with the Messages API**

Validation of the `params` object for each message request is performed asynchronously, and validation errors are returned when processing of the entire batch has ended. You can ensure that you are building your input correctly by verifying your request shape with the [Messages API](/docs/en/api/messages) first.
</Tip>

When a batch is first created, the response will have a processing status of `in_progress`.
```

---

## SDK Cost Tracking

**URL:** llms-txt#sdk-cost-tracking

**Contents:**
- Understanding Token Usage
  - Key Concepts
- Usage Reporting Structure
  - Single vs Parallel Tool Use

The Claude Agent SDK provides detailed token usage information for each interaction with Claude. This guide explains how to properly track costs and understand usage reporting, especially when dealing with parallel tool uses and multi-step conversations.

For complete API documentation, see the [TypeScript SDK reference](/docs/en/agent-sdk/typescript).

## Understanding Token Usage

When Claude processes requests, it reports token usage at the message level. This usage data is essential for tracking costs and billing users appropriately.

1. **Steps**: A step is a single request/response pair between your application and Claude
2. **Messages**: Individual messages within a step (text, tool uses, tool results)
3. **Usage**: Token consumption data attached to assistant messages

## Usage Reporting Structure

### Single vs Parallel Tool Use

When Claude executes tools, the usage reporting differs based on whether tools are executed sequentially or in parallel:

```python Python
from claude_agent_sdk import query, ClaudeAgentOptions, AssistantMessage
import asyncio

**Examples:**

Example 1 (unknown):
```unknown

```

---

## Beta headers

**URL:** llms-txt#beta-headers

**Contents:**
- How to use beta headers
  - Multiple beta features
  - Version naming conventions
- Error handling
- Getting help

Documentation for using beta headers with the Claude API

Beta headers allow you to access experimental features and new model capabilities before they become part of the standard API.

These features are subject to change and may be modified or removed in future releases.

<Info>
Beta headers are often used in conjunction with the [beta namespace in the client SDKs](/docs/en/api/client-sdks#beta-namespace-in-client-sdks)
</Info>

## How to use beta headers

To access beta features, include the `anthropic-beta` header in your API requests:

When using the SDK, you can specify beta headers in the request options:

<Warning>
Beta features are experimental and may:
- Have breaking changes without notice
- Be deprecated or removed
- Have different rate limits or pricing
- Not be available in all regions
</Warning>

### Multiple beta features

To use multiple beta features in a single request, include all feature names in the header separated by commas:

### Version naming conventions

Beta feature names typically follow the pattern: `feature-name-YYYY-MM-DD`, where the date indicates when the beta version was released. Always use the exact beta feature name as documented.

If you use an invalid or unavailable beta header, you'll receive an error response:

For questions about beta features:

1. Check the documentation for the specific feature
2. Review the [API changelog](/docs/en/api/versioning) for updates
3. Contact support for assistance with production usage

Remember that beta features are provided "as-is" and may not have the same SLA guarantees as stable API features.

**Examples:**

Example 1 (http):
```http
POST /v1/messages
Content-Type: application/json
X-API-Key: YOUR_API_KEY
anthropic-beta: BETA_FEATURE_NAME
```

Example 2 (unknown):
```unknown

```

Example 3 (unknown):
```unknown

```

Example 4 (unknown):
```unknown
</CodeGroup>

<Warning>
Beta features are experimental and may:
- Have breaking changes without notice
- Be deprecated or removed
- Have different rate limits or pricing
- Not be available in all regions
</Warning>

### Multiple beta features

To use multiple beta features in a single request, include all feature names in the header separated by commas:
```

---

## Then make the API call using the JSON file

**URL:** llms-txt#then-make-the-api-call-using-the-json-file

**Contents:**
- Next steps

curl https://api.anthropic.com/v1/messages/batches \
  -H "content-type: application/json" \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -d @request.json
python Python
message_batch = client.messages.batches.create(
    requests=[
        {
            "custom_id": "doc1",
            "params": {
                "model": "claude-sonnet-4-5",
                "max_tokens": 1024,
                "messages": [
                    {
                        "role": "user",
                        "content": [
                            {
                                "type": "document",
                                "source": {
                                    "type": "base64",
                                    "media_type": "application/pdf",
                                    "data": pdf_data
                                }
                            },
                            {
                                "type": "text",
                                "text": "Summarize this document."
                            }
                        ]
                    }
                ]
            }
        }
    ]
)
typescript TypeScript
const response = await anthropic.messages.batches.create({
  requests: [
    {
      custom_id: 'my-first-request',
      params: {
        max_tokens: 1024,
        messages: [
          {
            content: [
              {
                type: 'document',
                source: {
                  media_type: 'application/pdf',
                  type: 'base64',
                  data: pdfBase64,
                },
              },
              {
                type: 'text',
                text: 'Which model has the highest human preference win rates across each use-case?',
              },
            ],
            role: 'user',
          },
        ],
        model: 'claude-sonnet-4-5',
      },
    },
    {
      custom_id: 'my-second-request',
      params: {
        max_tokens: 1024,
        messages: [
          {
            content: [
              {
                type: 'document',
                source: {
                  media_type: 'application/pdf',
                  type: 'base64',
                  data: pdfBase64,
                },
              },
              {
                type: 'text',
                text: 'Extract 5 key insights from this document.',
              },
            ],
            role: 'user',
          },
        ],
        model: 'claude-sonnet-4-5',
      },
    }
  ],
});
console.log(response);
java Java
import java.io.IOException;
import java.nio.file.Files;
import java.nio.file.Paths;
import java.util.List;

import com.anthropic.client.AnthropicClient;
import com.anthropic.client.okhttp.AnthropicOkHttpClient;
import com.anthropic.models.messages.*;
import com.anthropic.models.messages.batches.*;

public class MessagesBatchDocumentExample {

public static void main(String[] args) throws IOException {
        AnthropicClient client = AnthropicOkHttpClient.fromEnv();

// Read PDF file as base64
        byte[] pdfBytes = Files.readAllBytes(Paths.get("pdf_base64.txt"));
        String pdfBase64 = new String(pdfBytes);

BatchCreateParams params = BatchCreateParams.builder()
                .addRequest(BatchCreateParams.Request.builder()
                        .customId("my-first-request")
                        .params(BatchCreateParams.Request.Params.builder()
                                .model(Model.CLAUDE_OPUS_4_20250514)
                                .maxTokens(1024)
                                .addUserMessageOfBlockParams(List.of(
                                        ContentBlockParam.ofDocument(
                                                DocumentBlockParam.builder()
                                                        .source(Base64PdfSource.builder()
                                                                .data(pdfBase64)
                                                                .build())
                                                        .build()
                                        ),
                                        ContentBlockParam.ofText(
                                                TextBlockParam.builder()
                                                        .text("Which model has the highest human preference win rates across each use-case?")
                                                        .build()
                                        )
                                ))
                                .build())
                        .build())
                .addRequest(BatchCreateParams.Request.builder()
                        .customId("my-second-request")
                        .params(BatchCreateParams.Request.Params.builder()
                                .model(Model.CLAUDE_OPUS_4_20250514)
                                .maxTokens(1024)
                                .addUserMessageOfBlockParams(List.of(
                                        ContentBlockParam.ofDocument(
                                        DocumentBlockParam.builder()
                                                .source(Base64PdfSource.builder()
                                                        .data(pdfBase64)
                                                        .build())
                                                .build()
                                        ),
                                        ContentBlockParam.ofText(
                                                TextBlockParam.builder()
                                                        .text("Extract 5 key insights from this document.")
                                                        .build()
                                        )
                                ))
                                .build())
                        .build())
                .build();

MessageBatch batch = client.messages().batches().create(params);
        System.out.println(batch);
    }
}
```
</CodeGroup>

<CardGroup cols={2}>
  <Card
    title="Try PDF examples"
    icon="file"
    href="https://platform.claude.com/cookbook/multimodal-getting-started-with-vision"
  >
    Explore practical examples of PDF processing in our cookbook recipe.
  </Card>

<Card
    title="View API reference"
    icon="code"
    href="/docs/en/api/messages"
  >
    See complete API documentation for PDF support.
  </Card>
</CardGroup>

**Examples:**

Example 1 (unknown):
```unknown

```

Example 2 (unknown):
```unknown

```

Example 3 (unknown):
```unknown

```

---

## - Custom slash commands

**URL:** llms-txt#--custom-slash-commands

---

## Commands run in the same session maintain state

**URL:** llms-txt#commands-run-in-the-same-session-maintain-state

**Contents:**
- Security
  - Key recommendations
- Pricing
- Common patterns
  - Development workflows
  - File operations
  - System tasks
- Limitations
- Combining with other tools
- Next steps

commands = [
    "cd /tmp",
    "echo 'Hello' > test.txt",
    "cat test.txt"  # This works because we're still in /tmp
]
python
def truncate_output(output, max_lines=100):
    lines = output.split('\n')
    if len(lines) > max_lines:
        truncated = '\n'.join(lines[:max_lines])
        return f"{truncated}\n\n... Output truncated ({len(lines)} total lines) ..."
    return output
python
import logging

def log_command(command, output, user_id):
    logging.info(f"User {user_id} executed: {command}")
    logging.info(f"Output: {output[:200]}...")  # Log first 200 chars
python
def sanitize_output(output):
    # Remove potential secrets or credentials
    import re
    # Example: Remove AWS credentials
    output = re.sub(r'aws_access_key_id\s*=\s*\S+', 'aws_access_key_id=***', output)
    output = re.sub(r'aws_secret_access_key\s*=\s*\S+', 'aws_secret_access_key=***', output)
    return output
```

<Warning>
The bash tool provides direct system access. Implement these essential safety measures:
- Running in isolated environments (Docker/VM)
- Implementing command filtering and allowlists
- Setting resource limits (CPU, memory, disk)
- Logging all executed commands
</Warning>

### Key recommendations
- Use `ulimit` to set resource constraints
- Filter dangerous commands (`sudo`, `rm -rf`, etc.)
- Run with minimal user permissions
- Monitor and log all command execution

The bash tool adds **245 input tokens** to your API calls.

Additional tokens are consumed by:
- Command outputs (stdout/stderr)
- Error messages
- Large file contents

See [tool use pricing](/docs/en/agents-and-tools/tool-use/overview#pricing) for complete pricing details.

### Development workflows
- Running tests: `pytest && coverage report`
- Building projects: `npm install && npm run build`
- Git operations: `git status && git add . && git commit -m "message"`

### File operations
- Processing data: `wc -l *.csv && ls -lh *.csv`
- Searching files: `find . -name "*.py" | xargs grep "pattern"`
- Creating backups: `tar -czf backup.tar.gz ./data`

### System tasks
- Checking resources: `df -h && free -m`
- Process management: `ps aux | grep python`
- Environment setup: `export PATH=$PATH:/new/path && echo $PATH`

- **No interactive commands**: Cannot handle `vim`, `less`, or password prompts
- **No GUI applications**: Command-line only
- **Session scope**: Persists within conversation, lost between API calls
- **Output limits**: Large outputs may be truncated
- **No streaming**: Results returned after completion

## Combining with other tools

The bash tool is most powerful when combined with the [text editor](/docs/en/agents-and-tools/tool-use/text-editor-tool) and other tools.

<CardGroup cols={2}>
  <Card
    title="Tool use overview"
    icon="tool"
    href="/docs/en/agents-and-tools/tool-use/overview"
  >
    Learn about tool use with Claude
  </Card>

<Card
    title="Text editor tool"
    icon="file"
    href="/docs/en/agents-and-tools/tool-use/text-editor-tool"
  >
    View and edit text files with Claude
  </Card>
</CardGroup>

**Examples:**

Example 1 (unknown):
```unknown
</section>

<section title="Handle large outputs">

Truncate very large outputs to prevent token limit issues:
```

Example 2 (unknown):
```unknown
</section>

<section title="Log all commands">

Keep an audit trail of executed commands:
```

Example 3 (unknown):
```unknown
</section>

<section title="Sanitize outputs">

Remove sensitive information from command outputs:
```

---

## Using the Messages API

**URL:** llms-txt#using-the-messages-api

**Contents:**
- Basic request and response
- Multiple conversational turns
- Putting words in Claude's mouth
- Vision
- Tool use and computer use
  - Capabilities

Practical patterns and examples for using the Messages API effectively

This guide covers common patterns for working with the Messages API, including basic requests, multi-turn conversations, prefill techniques, and vision capabilities. For complete API specifications, see the [Messages API reference](/docs/en/api/messages).

## Basic request and response

## Multiple conversational turns

The Messages API is stateless, which means that you always send the full conversational history to the API. You can use this pattern to build up a conversation over time. Earlier conversational turns don't necessarily need to actually originate from Claude — you can use synthetic `assistant` messages.

## Putting words in Claude's mouth

You can pre-fill part of Claude's response in the last position of the input messages list. This can be used to shape Claude's response. The example below uses `"max_tokens": 1` to get a single multiple choice answer from Claude.

For more information on prefill techniques, see our [prefill guide](/docs/en/build-with-claude/prompt-engineering/prefill-claudes-response).

Claude can read both text and images in requests. We support both `base64` and `url` source types for images, and the `image/jpeg`, `image/png`, `image/gif`, and `image/webp` media types. See our [vision guide](/docs/en/build-with-claude/vision) for more details.

## Tool use and computer use

See our [guide](/docs/en/agents-and-tools/tool-use/overview) for examples of how to use tools with the Messages API.
See our [computer use guide](/docs/en/agents-and-tools/tool-use/computer-use-tool) for examples of how to control desktop computer environments with the Messages API.
For guaranteed JSON output, see [Structured Outputs](/docs/en/build-with-claude/structured-outputs).

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
```

Example 4 (unknown):
```unknown
## Multiple conversational turns

The Messages API is stateless, which means that you always send the full conversational history to the API. You can use this pattern to build up a conversation over time. Earlier conversational turns don't necessarily need to actually originate from Claude — you can use synthetic `assistant` messages.

<CodeGroup>
```

---

## Files API

**URL:** llms-txt#files-api

**Contents:**
- Supported models
- How the Files API works
- How to use the Files API
  - Uploading a file
  - Using a file in messages
  - File types and content blocks
  - Working with other file formats

The Files API lets you upload and manage files to use with the Claude API without re-uploading content with each request. This is particularly useful when using the [code execution tool](/docs/en/agents-and-tools/tool-use/code-execution-tool) to provide inputs (e.g. datasets and documents) and then download outputs (e.g. charts). You can also use the Files API to prevent having to continually re-upload frequently used documents and images across multiple API calls. You can [explore the API reference directly](/docs/en/api/files-create), in addition to this guide.

<Note>
The Files API is currently in beta. Please reach out through our [feedback form](https://forms.gle/tisHyierGwgN4DUE9) to share your experience with the Files API.
</Note>

Referencing a `file_id` in a Messages request is supported in all models that support the given file type. For example, [images](/docs/en/build-with-claude/vision) are supported in all Claude 3+ models, [PDFs](/docs/en/build-with-claude/pdf-support) in all Claude 3.5+ models, and [various other file types](/docs/en/agents-and-tools/tool-use/code-execution-tool#supported-file-types) for the code execution tool in Claude Haiku 4.5 plus all Claude 3.7+ models.

The Files API is currently not supported on Amazon Bedrock or Google Vertex AI.

## How the Files API works

The Files API provides a simple create-once, use-many-times approach for working with files:

- **Upload files** to our secure storage and receive a unique `file_id`
- **Download files** that are created from skills or the code execution tool
- **Reference files** in [Messages](/docs/en/api/messages) requests using the `file_id` instead of re-uploading content
- **Manage your files** with list, retrieve, and delete operations

## How to use the Files API

<Note>
To use the Files API, you'll need to include the beta feature header: `anthropic-beta: files-api-2025-04-14`.
</Note>

Upload a file to be referenced in future API calls:

The response from uploading a file will include:

### Using a file in messages

Once uploaded, reference the file using its `file_id`:

### File types and content blocks

The Files API supports different file types that correspond to different content block types:

| File Type | MIME Type | Content Block Type | Use Case |
| :--- | :--- | :--- | :--- |
| PDF | `application/pdf` | `document` | Text analysis, document processing |
| Plain text | `text/plain` | `document` | Text analysis, processing |
| Images | `image/jpeg`, `image/png`, `image/gif`, `image/webp` | `image` | Image analysis, visual tasks |
| [Datasets, others](/docs/en/agents-and-tools/tool-use/code-execution-tool#supported-file-types) | Varies | `container_upload` | Analyze data, create visualizations  |

### Working with other file formats

For file types that are not supported as `document` blocks (.csv, .txt, .md, .docx, .xlsx), convert the files to plain text, and include the content directly in your message:

<CodeGroup>
```bash Shell

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

The response from uploading a file will include:
```

Example 4 (unknown):
```unknown
### Using a file in messages

Once uploaded, reference the file using its `file_id`:

<CodeGroup>
```

---

## First, upload your PDF to the Files API

**URL:** llms-txt#first,-upload-your-pdf-to-the-files-api

curl -X POST https://api.anthropic.com/v1/files \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -H "anthropic-beta: files-api-2025-04-14" \
  -F "file=@document.pdf"

---

## Slash Commands in the SDK

**URL:** llms-txt#slash-commands-in-the-sdk

**Contents:**
- Discovering Available Slash Commands
- Sending Slash Commands
- Common Slash Commands
  - `/compact` - Compact Conversation History
  - `/clear` - Clear Conversation
- Creating Custom Slash Commands
  - File Locations
  - File Format
  - Using Custom Commands in the SDK
  - Advanced Features

Learn how to use slash commands to control Claude Code sessions through the SDK

Slash commands provide a way to control Claude Code sessions with special commands that start with `/`. These commands can be sent through the SDK to perform actions like clearing conversation history, compacting messages, or getting help.

## Discovering Available Slash Commands

The Claude Agent SDK provides information about available slash commands in the system initialization message. Access this information when your session starts:

## Sending Slash Commands

Send slash commands by including them in your prompt string, just like regular text:

## Common Slash Commands

### `/compact` - Compact Conversation History

The `/compact` command reduces the size of your conversation history by summarizing older messages while preserving important context:

### `/clear` - Clear Conversation

The `/clear` command starts a fresh conversation by clearing all previous history:

## Creating Custom Slash Commands

In addition to using built-in slash commands, you can create your own custom commands that are available through the SDK. Custom commands are defined as markdown files in specific directories, similar to how subagents are configured.

Custom slash commands are stored in designated directories based on their scope:

- **Project commands**: `.claude/commands/` - Available only in the current project
- **Personal commands**: `~/.claude/commands/` - Available across all your projects

Each custom command is a markdown file where:
- The filename (without `.md` extension) becomes the command name
- The file content defines what the command does
- Optional YAML frontmatter provides configuration

Create `.claude/commands/refactor.md`:

This creates the `/refactor` command that you can use through the SDK.

#### With Frontmatter

Create `.claude/commands/security-check.md`:

### Using Custom Commands in the SDK

Once defined in the filesystem, custom commands are automatically available through the SDK:

### Advanced Features

#### Arguments and Placeholders

Custom commands support dynamic arguments using placeholders:

Create `.claude/commands/fix-issue.md`:

#### Bash Command Execution

Custom commands can execute bash commands and include their output:

Create `.claude/commands/git-commit.md`:

Include file contents using the `@` prefix:

Create `.claude/commands/review-config.md`:

### Organization with Namespacing

Organize commands in subdirectories for better structure:

The subdirectory appears in the command description but doesn't affect the command name itself.

### Practical Examples

#### Code Review Command

Create `.claude/commands/code-review.md`:

#### Test Runner Command

Create `.claude/commands/test.md`:

Use these commands through the SDK:

- [Slash Commands](https://code.claude.com/docs/en/slash-commands) - Complete slash command documentation
- [Subagents in the SDK](/docs/en/agent-sdk/subagents) - Similar filesystem-based configuration for subagents
- [TypeScript SDK reference](/docs/en/agent-sdk/typescript) - Complete API documentation
- [SDK overview](/docs/en/agent-sdk/overview) - General SDK concepts
- [CLI reference](https://code.claude.com/docs/en/cli-reference) - Command-line interface

**Examples:**

Example 1 (unknown):
```unknown

```

Example 2 (unknown):
```unknown
</CodeGroup>

## Sending Slash Commands

Send slash commands by including them in your prompt string, just like regular text:

<CodeGroup>
```

Example 3 (unknown):
```unknown

```

Example 4 (unknown):
```unknown
</CodeGroup>

## Common Slash Commands

### `/compact` - Compact Conversation History

The `/compact` command reduces the size of your conversation history by summarizing older messages while preserving important context:

<CodeGroup>
```

---

## TypeScript SDK V2 interface (preview)

**URL:** llms-txt#typescript-sdk-v2-interface-(preview)

**Contents:**
- Installation
- Quick start
  - One-shot prompt
  - Basic session
  - Multi-turn conversation
  - Session resume
  - Cleanup
- API reference
  - `unstable_v2_createSession()`
  - `unstable_v2_resumeSession()`

Preview of the simplified V2 TypeScript Agent SDK, with session-based send/stream patterns for multi-turn conversations.

<Warning>
The V2 interface is an **unstable preview**. APIs may change based on feedback before becoming stable. Some features like session forking are only available in the [V1 SDK](/docs/en/agent-sdk/typescript).
</Warning>

The V2 Claude Agent TypeScript SDK removes the need for async generators and yield coordination. This makes multi-turn conversations simpler, instead of managing generator state across turns, each turn is a separate `send()`/`stream()` cycle. The API surface reduces to three concepts:

- `createSession()` / `resumeSession()`: Start or continue a conversation
- `session.send()`: Send a message
- `session.stream()`: Get the response

The V2 interface is included in the existing SDK package:

For simple single-turn queries where you don't need to maintain a session, use `unstable_v2_prompt()`. This example sends a math question and logs the answer:

<details>
<summary>See the same operation in V1</summary>

For interactions beyond a single prompt, create a session. V2 separates sending and streaming into distinct steps:
- `send()` dispatches your message
- `stream()` streams back the response

This explicit separation makes it easier to add logic between turns (like processing responses before sending follow-ups).

The example below creates a session, sends "Hello!" to Claude, and prints the text response. It uses [`await using`](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-2.html#using-declarations-and-explicit-resource-management) (TypeScript 5.2+) to automatically close the session when the block exits. You can also call `session.close()` manually.

<details>
<summary>See the same operation in V1</summary>

In V1, both input and output flow through a single async generator. For a basic prompt this looks similar, but adding multi-turn logic requires restructuring to use an input generator.

### Multi-turn conversation

Sessions persist context across multiple exchanges. To continue a conversation, call `send()` again on the same session. Claude remembers the previous turns.

This example asks a math question, then asks a follow-up that references the previous answer:

<details>
<summary>See the same operation in V1</summary>

If you have a session ID from a previous interaction, you can resume it later. This is useful for long-running workflows or when you need to persist conversations across application restarts.

This example creates a session, stores its ID, closes it, then resumes the conversation:

<details>
<summary>See the same operation in V1</summary>

Sessions can be closed manually or automatically using [`await using`](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-5-2.html#using-declarations-and-explicit-resource-management), a TypeScript 5.2+ feature for automatic resource cleanup. If you're using an older TypeScript version or encounter compatibility issues, use manual cleanup instead.

**Automatic cleanup (TypeScript 5.2+):**

### `unstable_v2_createSession()`

Creates a new session for multi-turn conversations.

### `unstable_v2_resumeSession()`

Resumes an existing session by ID.

### `unstable_v2_prompt()`

One-shot convenience function for single-turn queries.

### Session interface

## Feature availability

Not all V1 features are available in V2 yet. The following require using the [V1 SDK](/docs/en/agent-sdk/typescript):

- Session forking (`forkSession` option)
- Some advanced streaming input patterns

Share your feedback on the V2 interface before it becomes stable. Report issues and suggestions through [GitHub Issues](https://github.com/anthropics/claude-code/issues).

- [TypeScript SDK reference (V1)](/docs/en/agent-sdk/typescript) - Full V1 SDK documentation
- [SDK overview](/docs/en/agent-sdk/overview) - General SDK concepts
- [V2 examples on GitHub](https://github.com/anthropics/claude-agent-sdk-demos/tree/main/hello-world-v2) - Working code examples

### Agent SDK > Guides

**Examples:**

Example 1 (bash):
```bash
npm install @anthropic-ai/claude-agent-sdk
```

Example 2 (typescript):
```typescript
import { unstable_v2_prompt } from '@anthropic-ai/claude-agent-sdk'

const result = await unstable_v2_prompt('What is 2 + 2?', {
  model: 'claude-sonnet-4-5-20250929'
})
console.log(result.result)
```

Example 3 (typescript):
```typescript
import { query } from '@anthropic-ai/claude-agent-sdk'

const q = query({
  prompt: 'What is 2 + 2?',
  options: { model: 'claude-sonnet-4-5-20250929' }
})

for await (const msg of q) {
  if (msg.type === 'result') {
    console.log(msg.result)
  }
}
```

Example 4 (typescript):
```typescript
import { unstable_v2_createSession } from '@anthropic-ai/claude-agent-sdk'

await using session = unstable_v2_createSession({
  model: 'claude-sonnet-4-5-20250929'
})

await session.send('Hello!')
for await (const msg of session.stream()) {
  // Filter for assistant messages to get human-readable output
  if (msg.type === 'assistant') {
    const text = msg.message.content
      .filter(block => block.type === 'text')
      .map(block => block.text)
      .join('')
    console.log(text)
  }
}
```

---

## BigQuery Data Analysis

**URL:** llms-txt#bigquery-data-analysis

**Contents:**
- Available datasets
- Quick search

## Available datasets

**Finance**: Revenue, ARR, billing → See [reference/finance.md](reference/finance.md)
**Sales**: Opportunities, pipeline, accounts → See [reference/sales.md](reference/sales.md)
**Product**: API usage, features, adoption → See [reference/product.md](reference/product.md)
**Marketing**: Campaigns, attribution, email → See [reference/marketing.md](reference/marketing.md)

Find specific metrics using grep:

**Examples:**

Example 1 (bash):
```bash
grep -i "revenue" reference/finance.md
grep -i "pipeline" reference/sales.md
grep -i "api usage" reference/product.md
```

Example 2 (unknown):
```unknown
#### Pattern 3: Conditional details

Show basic content, link to advanced content:
```

---

## Streaming Messages

**URL:** llms-txt#streaming-messages

**Contents:**
- Streaming with SDKs
- Event types
  - Ping events
  - Error events
  - Other events
- Content block delta types
  - Text delta
  - Input JSON delta
  - Thinking delta
- Full HTTP Stream response

When creating a Message, you can set `"stream": true` to incrementally stream the response using [server-sent events](https://developer.mozilla.org/en-US/Web/API/Server-sent%5Fevents/Using%5Fserver-sent%5Fevents) (SSE).

## Streaming with SDKs

Our [Python](https://github.com/anthropics/anthropic-sdk-python) and [TypeScript](https://github.com/anthropics/anthropic-sdk-typescript) SDKs offer multiple ways of streaming. The Python SDK allows both sync and async streams. See the documentation in each SDK for details.

Each server-sent event includes a named event type and associated JSON data. Each event will use an SSE event name (e.g. `event: message_stop`), and include the matching event `type` in its data.

Each stream uses the following event flow:

1. `message_start`: contains a `Message` object with empty `content`.
2. A series of content blocks, each of which have a `content_block_start`, one or more `content_block_delta` events, and a `content_block_stop` event. Each content block will have an `index` that corresponds to its index in the final Message `content` array.
3. One or more `message_delta` events, indicating top-level changes to the final `Message` object.
4. A final `message_stop` event.

<Warning>
  The token counts shown in the `usage` field of the `message_delta` event are *cumulative*.
  </Warning>

Event streams may also include any number of `ping` events.

We may occasionally send [errors](/docs/en/api/errors) in the event stream. For example, during periods of high usage, you may receive an `overloaded_error`, which would normally correspond to an HTTP 529 in a non-streaming context:

In accordance with our [versioning policy](/docs/en/api/versioning), we may add new event types, and your code should handle unknown event types gracefully.

## Content block delta types

Each `content_block_delta` event contains a `delta` of a type that updates the `content` block at a given `index`.

A `text` content block delta looks like:

The deltas for `tool_use` content blocks correspond to updates for the `input` field of the block. To support maximum granularity, the deltas are _partial JSON strings_, whereas the final `tool_use.input` is always an _object_.

You can accumulate the string deltas and parse the JSON once you receive a `content_block_stop` event, by using a library like [Pydantic](https://docs.pydantic.dev/latest/concepts/json/#partial-json-parsing) to do partial JSON parsing, or by using our [SDKs](/docs/en/api/client-sdks), which provide helpers to access parsed incremental values.

A `tool_use` content block delta looks like:

Note: Our current models only support emitting one complete key and value property from `input` at a time. As such, when using tools, there may be delays between streaming events while the model is working. Once an `input` key and value are accumulated, we emit them as multiple `content_block_delta` events with chunked partial json so that the format can automatically support finer granularity in future models.

When using [extended thinking](/docs/en/build-with-claude/extended-thinking#streaming-thinking) with streaming enabled, you'll receive thinking content via `thinking_delta` events. These deltas correspond to the `thinking` field of the `thinking` content blocks.

For thinking content, a special `signature_delta` event is sent just before the `content_block_stop` event. This signature is used to verify the integrity of the thinking block.

A typical thinking delta looks like:

The signature delta looks like:

## Full HTTP Stream response

We strongly recommend that you use our [client SDKs](/docs/en/api/client-sdks) when using streaming mode. However, if you are building a direct API integration, you will need to handle these events yourself.

A stream response is comprised of:
1. A `message_start` event
2. Potentially multiple content blocks, each of which contains:
    - A `content_block_start` event
    - Potentially multiple `content_block_delta` events
    - A `content_block_stop` event
3. A `message_delta` event
4. A `message_stop` event

There may be `ping` events dispersed throughout the response as well. See [Event types](#event-types) for more details on the format.

### Basic streaming request

### Streaming request with tool use

<Tip>
Tool use now supports fine-grained streaming for parameter values as a beta feature. For more details, see [Fine-grained tool streaming](/docs/en/agents-and-tools/tool-use/fine-grained-tool-streaming).
</Tip>

In this request, we ask Claude to use a tool to tell us the weather.

### Streaming request with extended thinking

In this request, we enable extended thinking with streaming to see Claude's step-by-step reasoning.

### Streaming request with web search tool use

In this request, we ask Claude to search the web for current weather information.

When a streaming request is interrupted due to network issues, timeouts, or other errors, you can recover by resuming from where the stream was interrupted. This approach saves you from re-processing the entire response.

The basic recovery strategy involves:

1. **Capture the partial response**: Save all content that was successfully received before the error occurred
2. **Construct a continuation request**: Create a new API request that includes the partial assistant response as the beginning of a new assistant message
3. **Resume streaming**: Continue receiving the rest of the response from where it was interrupted

### Error recovery best practices

1. **Use SDK features**: Leverage the SDK's built-in message accumulation and error handling capabilities
2. **Handle content types**: Be aware that messages can contain multiple content blocks (`text`, `tool_use`, `thinking`). Tool use and extended thinking blocks cannot be partially recovered. You can resume streaming from the most recent text block.

**Examples:**

Example 1 (unknown):
```unknown

```

Example 2 (unknown):
```unknown
</CodeGroup>

## Event types

Each server-sent event includes a named event type and associated JSON data. Each event will use an SSE event name (e.g. `event: message_stop`), and include the matching event `type` in its data.

Each stream uses the following event flow:

1. `message_start`: contains a `Message` object with empty `content`.
2. A series of content blocks, each of which have a `content_block_start`, one or more `content_block_delta` events, and a `content_block_stop` event. Each content block will have an `index` that corresponds to its index in the final Message `content` array.
3. One or more `message_delta` events, indicating top-level changes to the final `Message` object.
4. A final `message_stop` event.

  <Warning>
  The token counts shown in the `usage` field of the `message_delta` event are *cumulative*.
  </Warning>

### Ping events

Event streams may also include any number of `ping` events.

### Error events

We may occasionally send [errors](/docs/en/api/errors) in the event stream. For example, during periods of high usage, you may receive an `overloaded_error`, which would normally correspond to an HTTP 529 in a non-streaming context:
```

Example 3 (unknown):
```unknown
### Other events

In accordance with our [versioning policy](/docs/en/api/versioning), we may add new event types, and your code should handle unknown event types gracefully.

## Content block delta types

Each `content_block_delta` event contains a `delta` of a type that updates the `content` block at a given `index`.

### Text delta

A `text` content block delta looks like:
```

Example 4 (unknown):
```unknown
### Input JSON delta

The deltas for `tool_use` content blocks correspond to updates for the `input` field of the block. To support maximum granularity, the deltas are _partial JSON strings_, whereas the final `tool_use.input` is always an _object_.

You can accumulate the string deltas and parse the JSON once you receive a `content_block_stop` event, by using a library like [Pydantic](https://docs.pydantic.dev/latest/concepts/json/#partial-json-parsing) to do partial JSON parsing, or by using our [SDKs](/docs/en/api/client-sdks), which provide helpers to access parsed incremental values.

A `tool_use` content block delta looks like:
```

---

## OpenAI SDK compatibility

**URL:** llms-txt#openai-sdk-compatibility

**Contents:**
- Getting started with the OpenAI SDK
  - Quick start example
- Important OpenAI compatibility limitations
- Rate limits
- Detailed OpenAI Compatible API Support
  - Request fields
  - Response fields
  - Error message compatibility
  - Header compatibility

Anthropic provides a compatibility layer that enables you to use the OpenAI SDK to test the Claude API. With a few code changes, you can quickly evaluate Anthropic model capabilities.

<Note>
This compatibility layer is primarily intended to test and compare model capabilities, and is not considered a long-term or production-ready solution for most use cases. While we do intend to keep it fully functional and not make breaking changes, our priority is the reliability and effectiveness of the [Claude API](/docs/en/api/overview).

For more information on known compatibility limitations, see [Important OpenAI compatibility limitations](#important-openai-compatibility-limitations).

If you encounter any issues with the OpenAI SDK compatibility feature, please let us know [here](https://forms.gle/oQV4McQNiuuNbz9n8).
</Note>

<Tip>
For the best experience and access to Claude API full feature set ([PDF processing](/docs/en/build-with-claude/pdf-support), [citations](/docs/en/build-with-claude/citations), [extended thinking](/docs/en/build-with-claude/extended-thinking), and [prompt caching](/docs/en/build-with-claude/prompt-caching)), we recommend using the native [Claude API](/docs/en/api/overview).
</Tip>

## Getting started with the OpenAI SDK

To use the OpenAI SDK compatibility feature, you'll need to:

1. Use an official OpenAI SDK  
2. Change the following  
   * Update your base URL to point to the Claude API  
   * Replace your API key with an [Claude API key](/settings/keys)  
   * Update your model name to use a [Claude model](/docs/en/about-claude/models/overview)  
3. Review the documentation below for what features are supported

### Quick start example

<CodeGroup>
    
    
    
</CodeGroup>

## Important OpenAI compatibility limitations

Here are the most substantial differences from using OpenAI:

* The `strict` parameter for function calling is ignored, which means the tool use JSON is not guaranteed to follow the supplied schema. For guaranteed schema conformance, use the native [Claude API with Structured Outputs](/docs/en/build-with-claude/structured-outputs).
* Audio input is not supported; it will simply be ignored and stripped from input  
* Prompt caching is not supported, but it is supported in [the Anthropic SDK](/docs/en/api/client-sdks)  
* System/developer messages are hoisted and concatenated to the beginning of the conversation, as Anthropic only supports a single initial system message.

Most unsupported fields are silently ignored rather than producing errors. These are all documented below.

#### Output quality considerations

If you’ve done lots of tweaking to your prompt, it’s likely to be well-tuned to OpenAI specifically. Consider using our [prompt improver in the Claude Console](/dashboard) as a good starting point.

#### System / Developer message hoisting

Most of the inputs to the OpenAI SDK clearly map directly to Anthropic’s API parameters, but one distinct difference is the handling of system / developer prompts. These two prompts can be put throughout a chat conversation via OpenAI. Since Anthropic only supports an initial system message, we take all system/developer messages and concatenate them together with a single newline (`\n`) in between them. This full string is then supplied as a single system message at the start of the messages.

#### Extended thinking support

You can enable [extended thinking](/docs/en/build-with-claude/extended-thinking) capabilities by adding the `thinking` parameter. While this will improve Claude's reasoning for complex tasks, the OpenAI SDK won't return Claude's detailed thought process. For full extended thinking features, including access to Claude's step-by-step reasoning output, use the native Claude API.

<CodeGroup>
    
    
    
</CodeGroup>

Rate limits follow Anthropic's [standard limits](/docs/en/api/rate-limits) for the `/v1/messages` endpoint.

## Detailed OpenAI Compatible API Support
### Request fields
#### Simple fields
| Field | Support status |
|--------|----------------|
| `model` | Use Claude model names |
| `max_tokens` | Fully supported |
| `max_completion_tokens` | Fully supported |
| `stream` | Fully supported |
| `stream_options` | Fully supported |
| `top_p` | Fully supported |
| `parallel_tool_calls` | Fully supported |
| `stop` | All non-whitespace stop sequences work |
| `temperature` | Between 0 and 1 (inclusive). Values greater than 1 are capped at 1. |
| `n` | Must be exactly 1 |
| `logprobs` | Ignored |
| `metadata` | Ignored |
| `response_format` | Ignored. For JSON output, use [Structured Outputs](/docs/en/build-with-claude/structured-outputs) with the native Claude API |
| `prediction` | Ignored |
| `presence_penalty` | Ignored |
| `frequency_penalty` | Ignored |
| `seed` | Ignored |
| `service_tier` | Ignored |
| `audio` | Ignored |
| `logit_bias` | Ignored |
| `store` | Ignored |
| `user` | Ignored |
| `modalities` | Ignored |
| `top_logprobs` | Ignored |
| `reasoning_effort` | Ignored |

#### `tools` / `functions` fields
<section title="Show fields">

<Tabs>
<Tab title="Tools">
`tools[n].function` fields
| Field        | Support status         |
|--------------|-----------------|
| `name`       | Fully supported |
| `description`| Fully supported |
| `parameters` | Fully supported |
| `strict`     | Ignored. Use [Structured Outputs](/docs/en/build-with-claude/structured-outputs) with native Claude API for strict schema validation |
</Tab>
<Tab title="Functions">

`functions[n]` fields
<Info>
OpenAI has deprecated the `functions` field and suggests using `tools` instead.
</Info>
| Field        | Support status         |
|--------------|-----------------|
| `name`       | Fully supported |
| `description`| Fully supported |
| `parameters` | Fully supported |
| `strict`     | Ignored. Use [Structured Outputs](/docs/en/build-with-claude/structured-outputs) with native Claude API for strict schema validation |
</Tab>
</Tabs>

#### `messages` array fields
<section title="Show fields">

<Tabs>
<Tab title="Developer role">
Fields for `messages[n].role == "developer"`
<Info>
Developer messages are hoisted to beginning of conversation as part of the initial system message
</Info>
| Field | Support status |
|-------|---------|
| `content` | Fully supported, but hoisted |
| `name` | Ignored |

</Tab>
<Tab title="System role">
Fields for `messages[n].role == "system"`

<Info>
System messages are hoisted to beginning of conversation as part of the initial system message
</Info>
| Field | Support status |
|-------|---------|
| `content` | Fully supported, but hoisted |
| `name` | Ignored |

</Tab>
<Tab title="User role">
Fields for `messages[n].role == "user"`

| Field | Variant | Sub-field | Support status |
|-------|---------|-----------|----------------|
| `content` | `string` | | Fully supported |
| | `array`, `type == "text"` | | Fully supported |
| | `array`, `type == "image_url"` | `url` | Fully supported |
| | | `detail` | Ignored |
| | `array`, `type == "input_audio"` | | Ignored |
| | `array`, `type == "file"` | | Ignored |
| `name` | | | Ignored |

<Tab title="Assistant role">
Fields for `messages[n].role == "assistant"`
| Field | Variant | Support status |
|-------|---------|----------------|
| `content` | `string` | Fully supported |
| | `array`, `type == "text"` | Fully supported |
| | `array`, `type == "refusal"` | Ignored |
| `tool_calls` | | Fully supported |
| `function_call` | | Fully supported |
| `audio` | | Ignored |
| `refusal` | | Ignored |

<Tab title="Tool role">
Fields for `messages[n].role == "tool"`
| Field | Variant | Support status |
|-------|---------|----------------|
| `content` | `string` | Fully supported |
| | `array`, `type == "text"` | Fully supported |
| `tool_call_id` | | Fully supported |
| `tool_choice` | | Fully supported |
| `name` | | Ignored |
</Tab>

<Tab title="Function role">
Fields for `messages[n].role == "function"`
| Field | Variant | Support status |
|-------|---------|----------------|
| `content` | `string` | Fully supported |
| | `array`, `type == "text"` | Fully supported |
| `tool_choice` | | Fully supported |
| `name` | | Ignored |
</Tab>
</Tabs>

| Field | Support status |
|---------------------------|----------------|
| `id` | Fully supported |
| `choices[]` | Will always have a length of 1 |
| `choices[].finish_reason` | Fully supported |
| `choices[].index` | Fully supported |
| `choices[].message.role` | Fully supported |
| `choices[].message.content` | Fully supported |
| `choices[].message.tool_calls` | Fully supported |
| `object` | Fully supported |
| `created` | Fully supported |
| `model` | Fully supported |
| `finish_reason` | Fully supported |
| `content` | Fully supported |
| `usage.completion_tokens` | Fully supported |
| `usage.prompt_tokens` | Fully supported |
| `usage.total_tokens` | Fully supported |
| `usage.completion_tokens_details` | Always empty |
| `usage.prompt_tokens_details` | Always empty |
| `choices[].message.refusal` | Always empty |
| `choices[].message.audio` | Always empty |
| `logprobs` | Always empty |
| `service_tier` | Always empty |
| `system_fingerprint` | Always empty |

### Error message compatibility

The compatibility layer maintains consistent error formats with the OpenAI API. However, the detailed error messages will not be equivalent. We recommend only using the error messages for logging and debugging.

### Header compatibility

While the OpenAI SDK automatically manages headers, here is the complete list of headers supported by the Claude API for developers who need to work with them directly.

| Header | Support Status |
|---------|----------------|
| `x-ratelimit-limit-requests` | Fully supported |
| `x-ratelimit-limit-tokens` | Fully supported |
| `x-ratelimit-remaining-requests` | Fully supported |
| `x-ratelimit-remaining-tokens` | Fully supported |
| `x-ratelimit-reset-requests` | Fully supported |
| `x-ratelimit-reset-tokens` | Fully supported |
| `retry-after` | Fully supported |
| `request-id` | Fully supported |
| `openai-version` | Always `2020-10-01` |
| `authorization` | Fully supported |
| `openai-processing-ms` | Always empty |

**Examples:**

Example 1 (unknown):
```unknown

```

Example 2 (unknown):
```unknown
</CodeGroup>

## Important OpenAI compatibility limitations

#### API behavior

Here are the most substantial differences from using OpenAI:

* The `strict` parameter for function calling is ignored, which means the tool use JSON is not guaranteed to follow the supplied schema. For guaranteed schema conformance, use the native [Claude API with Structured Outputs](/docs/en/build-with-claude/structured-outputs).
* Audio input is not supported; it will simply be ignored and stripped from input  
* Prompt caching is not supported, but it is supported in [the Anthropic SDK](/docs/en/api/client-sdks)  
* System/developer messages are hoisted and concatenated to the beginning of the conversation, as Anthropic only supports a single initial system message.

Most unsupported fields are silently ignored rather than producing errors. These are all documented below.

#### Output quality considerations

If you’ve done lots of tweaking to your prompt, it’s likely to be well-tuned to OpenAI specifically. Consider using our [prompt improver in the Claude Console](/dashboard) as a good starting point.

#### System / Developer message hoisting

Most of the inputs to the OpenAI SDK clearly map directly to Anthropic’s API parameters, but one distinct difference is the handling of system / developer prompts. These two prompts can be put throughout a chat conversation via OpenAI. Since Anthropic only supports an initial system message, we take all system/developer messages and concatenate them together with a single newline (`\n`) in between them. This full string is then supplied as a single system message at the start of the messages.

#### Extended thinking support

You can enable [extended thinking](/docs/en/build-with-claude/extended-thinking) capabilities by adding the `thinking` parameter. While this will improve Claude's reasoning for complex tasks, the OpenAI SDK won't return Claude's detailed thought process. For full extended thinking features, including access to Claude's step-by-step reasoning output, use the native Claude API.

<CodeGroup>
```

Example 3 (unknown):
```unknown

```

---

## Step 3: Download the file using Files API

**URL:** llms-txt#step-3:-download-the-file-using-files-api

for file_id in extract_file_ids(response):
    file_metadata = client.beta.files.retrieve_metadata(
        file_id=file_id,
        betas=["files-api-2025-04-14"]
    )
    file_content = client.beta.files.download(
        file_id=file_id,
        betas=["files-api-2025-04-14"]
    )

# Step 4: Save to disk
    file_content.write_to_file(file_metadata.filename)
    print(f"Downloaded: {file_metadata.filename}")
typescript TypeScript
import Anthropic from '@anthropic-ai/sdk';

const client = new Anthropic();

// Step 1: Use a Skill to create a file
const response = await client.beta.messages.create({
  model: 'claude-sonnet-4-5-20250929',
  max_tokens: 4096,
  betas: ['code-execution-2025-08-25', 'skills-2025-10-02'],
  container: {
    skills: [
      {type: 'anthropic', skill_id: 'xlsx', version: 'latest'}
    ]
  },
  messages: [{
    role: 'user',
    content: 'Create an Excel file with a simple budget spreadsheet'
  }],
  tools: [{type: 'code_execution_20250825', name: 'code_execution'}]
});

// Step 2: Extract file IDs from the response
function extractFileIds(response: any): string[] {
  const fileIds: string[] = [];
  for (const item of response.content) {
    if (item.type === 'bash_code_execution_tool_result') {
      const contentItem = item.content;
      if (contentItem.type === 'bash_code_execution_result') {
        for (const file of contentItem.content) {
          if ('file_id' in file) {
            fileIds.push(file.file_id);
          }
        }
      }
    }
  }
  return fileIds;
}

// Step 3: Download the file using Files API
const fs = require('fs');
for (const fileId of extractFileIds(response)) {
  const fileMetadata = await client.beta.files.retrieve_metadata(fileId, {
    betas: ['files-api-2025-04-14']
  });
  const fileContent = await client.beta.files.download(fileId, {
    betas: ['files-api-2025-04-14']
  });

// Step 4: Save to disk
  fs.writeFileSync(fileMetadata.filename, Buffer.from(await fileContent.arrayBuffer()));
  console.log(`Downloaded: ${fileMetadata.filename}`);
}
bash Shell

**Examples:**

Example 1 (unknown):
```unknown

```

Example 2 (unknown):
```unknown

```

---
