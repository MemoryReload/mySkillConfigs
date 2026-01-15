# Claude-Code - Deployment

**Pages:** 4

---

## See https://docs.claude.com/claude/reference/claude-on-amazon-bedrock

**URL:** llms-txt#see-https://docs.claude.com/claude/reference/claude-on-amazon-bedrock

---

## Claude on Amazon Bedrock

**URL:** llms-txt#claude-on-amazon-bedrock

**Contents:**
- Install and configure the AWS CLI
- Install an SDK for accessing Bedrock
- Accessing Bedrock
  - Subscribe to Anthropic models
  - List available models
  - Making requests
- Activity logging
- Feature support
  - PDF Support on Bedrock
  - 1M token context window

Anthropic's Claude models are now generally available through Amazon Bedrock.

Calling Claude through Bedrock slightly differs from how you would call Claude when using Anthropic's client SDK's. This guide will walk you through the process of completing an API call to Claude on Bedrock in either Python or TypeScript.

Note that this guide assumes you have already signed up for an [AWS account](https://portal.aws.amazon.com/billing/signup) and configured programmatic access.

## Install and configure the AWS CLI

1. [Install a version of the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html) at or newer than version `2.13.23`
2. Configure your AWS credentials using the AWS configure command (see [Configure the AWS CLI](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-configure.html)) or find your credentials by navigating to "Command line or programmatic access" within your AWS dashboard and following the directions in the popup modal.
3. Verify that your credentials are working:

## Install an SDK for accessing Bedrock

Anthropic's [client SDKs](/docs/en/api/client-sdks) support Bedrock. You can also use an AWS SDK like `boto3` directly.

### Subscribe to Anthropic models

Go to the [AWS Console > Bedrock > Model Access](https://console.aws.amazon.com/bedrock/home?region=us-west-2#/modelaccess) and request access to Anthropic models. Note that Anthropic model availability varies by region. See [AWS documentation](https://docs.aws.amazon.com/bedrock/latest/userguide/models-regions.html) for latest information.

| Model | Base Bedrock model ID | `global` | `us` | `eu` | `jp` | `apac` |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| Claude Sonnet 4.5 | anthropic.claude-sonnet-4-5-20250929-v1:0 | Yes | Yes | Yes | Yes | No |
| Claude Sonnet 4 | anthropic.claude-sonnet-4-20250514-v1:0 | Yes | Yes | Yes | No | Yes |
| Claude Sonnet 3.7 <Tooltip tooltipContent="Deprecated as of October 28, 2025.">⚠️</Tooltip> | anthropic.claude-3-7-sonnet-20250219-v1:0 | No | Yes | Yes | No | Yes |
| Claude Opus 4.5 | anthropic.claude-opus-4-5-20251101-v1:0 | Yes | Yes | Yes | No | No |
| Claude Opus 4.1 | anthropic.claude-opus-4-1-20250805-v1:0 | No | Yes | No | No | No |
| Claude Opus 4 | anthropic.claude-opus-4-20250514-v1:0 | No | Yes | No | No | No |
| Claude Haiku 4.5 | anthropic.claude-haiku-4-5-20251001-v1:0 | Yes | Yes | Yes | No | No |
| Claude Haiku 3.5 <Tooltip tooltipContent="Deprecated as of December 19, 2025.">⚠️</Tooltip> | anthropic.claude-3-5-haiku-20241022-v1:0 | No | Yes | No | No | No |
| Claude Haiku 3 | anthropic.claude-3-haiku-20240307-v1:0 | No | Yes | Yes | No | Yes |

For more information about regional vs global model IDs, see the [Global vs regional endpoints](#global-vs-regional-endpoints) section below.

### List available models

The following examples show how to print a list of all the Claude models available through Bedrock:

The following examples show how to generate text from Claude on Bedrock:

See our [client SDKs](/docs/en/api/client-sdks) for more details, and the official Bedrock docs [here](https://docs.aws.amazon.com/bedrock/).

Bedrock provides an [invocation logging service](https://docs.aws.amazon.com/bedrock/latest/userguide/model-invocation-logging.html) that allows customers to log the prompts and completions associated with your usage.

Anthropic recommends that you log your activity on at least a 30-day rolling basis in order to understand your activity and investigate any potential misuse.

<Note>
Turning on this service does not give AWS or Anthropic any access to your content.
</Note>

## Feature support
You can find all the features currently supported on Bedrock [here](/docs/en/api/overview).

### PDF Support on Bedrock

PDF support is available on Amazon Bedrock through both the Converse API and InvokeModel API. For detailed information about PDF processing capabilities and limitations, see the [PDF support documentation](/docs/en/build-with-claude/pdf-support#amazon-bedrock-pdf-support).

**Important considerations for Converse API users:**
- Visual PDF analysis (charts, images, layouts) requires citations to be enabled
- Without citations, only basic text extraction is available
- For full control without forced citations, use the InvokeModel API

For more details on the two document processing modes and their limitations, refer to the [PDF support guide](/docs/en/build-with-claude/pdf-support#amazon-bedrock-pdf-support).

### 1M token context window

Claude Sonnet 4 and 4.5 support the [1M token context window](/docs/en/build-with-claude/context-windows#1m-token-context-window) on Amazon Bedrock.

<Note>
The 1M token context window is currently in beta. To use the extended context window, include the `context-1m-2025-08-07` beta header in your [Bedrock API requests](https://docs.aws.amazon.com/bedrock/latest/userguide/model-parameters-anthropic-claude-messages-request-response.html).
</Note>

## Global vs regional endpoints

Starting with **Claude Sonnet 4.5 and all future models**, Amazon Bedrock offers two endpoint types:

- **Global endpoints**: Dynamic routing for maximum availability
- **Regional endpoints**: Guaranteed data routing through specific geographic regions

Regional endpoints include a 10% pricing premium over global endpoints.

<Note>
This applies to Claude Sonnet 4.5 and future models only. Older models (Claude Sonnet 4, Opus 4, and earlier) maintain their existing pricing structures.
</Note>

### When to use each option

**Global endpoints (recommended):**
- Provide maximum availability and uptime
- Dynamically route requests to regions with available capacity
- No pricing premium
- Best for applications where data residency is flexible

**Regional endpoints (CRIS):**
- Route traffic through specific geographic regions
- Required for data residency and compliance requirements
- Available for US, EU, Japan, and Australia
- 10% pricing premium reflects infrastructure costs for dedicated regional capacity

**Using global endpoints (default for Sonnet 4.5 and 4):**

The model IDs for Claude Sonnet 4.5 and 4 already include the `global.` prefix:

**Using regional endpoints (CRIS):**

To use regional endpoints, remove the `global.` prefix from the model ID:

<CodeGroup>
```python Python
from anthropic import AnthropicBedrock

client = AnthropicBedrock(aws_region="us-west-2")

**Examples:**

Example 1 (unknown):
```unknown
## Install an SDK for accessing Bedrock

Anthropic's [client SDKs](/docs/en/api/client-sdks) support Bedrock. You can also use an AWS SDK like `boto3` directly.

<CodeGroup>
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

## Accessing Bedrock

### Subscribe to Anthropic models

Go to the [AWS Console > Bedrock > Model Access](https://console.aws.amazon.com/bedrock/home?region=us-west-2#/modelaccess) and request access to Anthropic models. Note that Anthropic model availability varies by region. See [AWS documentation](https://docs.aws.amazon.com/bedrock/latest/userguide/models-regions.html) for latest information.

#### API model IDs

| Model | Base Bedrock model ID | `global` | `us` | `eu` | `jp` | `apac` |
| :---- | :---- | :---- | :---- | :---- | :---- | :---- |
| Claude Sonnet 4.5 | anthropic.claude-sonnet-4-5-20250929-v1:0 | Yes | Yes | Yes | Yes | No |
| Claude Sonnet 4 | anthropic.claude-sonnet-4-20250514-v1:0 | Yes | Yes | Yes | No | Yes |
| Claude Sonnet 3.7 <Tooltip tooltipContent="Deprecated as of October 28, 2025.">⚠️</Tooltip> | anthropic.claude-3-7-sonnet-20250219-v1:0 | No | Yes | Yes | No | Yes |
| Claude Opus 4.5 | anthropic.claude-opus-4-5-20251101-v1:0 | Yes | Yes | Yes | No | No |
| Claude Opus 4.1 | anthropic.claude-opus-4-1-20250805-v1:0 | No | Yes | No | No | No |
| Claude Opus 4 | anthropic.claude-opus-4-20250514-v1:0 | No | Yes | No | No | No |
| Claude Haiku 4.5 | anthropic.claude-haiku-4-5-20251001-v1:0 | Yes | Yes | Yes | No | No |
| Claude Haiku 3.5 <Tooltip tooltipContent="Deprecated as of December 19, 2025.">⚠️</Tooltip> | anthropic.claude-3-5-haiku-20241022-v1:0 | No | Yes | No | No | No |
| Claude Haiku 3 | anthropic.claude-3-haiku-20240307-v1:0 | No | Yes | Yes | No | Yes |

For more information about regional vs global model IDs, see the [Global vs regional endpoints](#global-vs-regional-endpoints) section below.

### List available models

The following examples show how to print a list of all the Claude models available through Bedrock:

<CodeGroup>
```

---

## Fine-grained tool streaming

**URL:** llms-txt#fine-grained-tool-streaming

**Contents:**
- How to use fine-grained tool streaming
- Handling invalid JSON in tool responses

Tool use now supports fine-grained [streaming](/docs/en/build-with-claude/streaming) for parameter values. This allows developers to stream tool use parameters without buffering / JSON validation, reducing the latency to begin receiving large parameters.

Fine-grained tool streaming is available via the Claude API, AWS Bedrock, Google Cloud's Vertex AI, and Microsoft Foundry.

<Note>
Fine-grained tool streaming is a beta feature. Please make sure to evaluate your responses before using it in production.

Please use [this form](https://forms.gle/D4Fjr7GvQRzfTZT96) to provide feedback on the quality of the model responses, the API itself, or the quality of the documentation—we cannot wait to hear from you!
</Note>

<Warning>
When using fine-grained tool streaming, you may potentially receive invalid or partial JSON inputs. Please make sure to account for these edge cases in your code.
</Warning>
## How to use fine-grained tool streaming
To use this beta feature, simply add the beta header `fine-grained-tool-streaming-2025-05-14` to a tool use request and turn on streaming.

Here's an example of how to use fine-grained tool streaming with the API:

In this example, fine-grained tool streaming enables Claude to stream the lines of a long poem into the tool call `make_file` without buffering to validate if the `lines_of_text` parameter is valid JSON. This means you can see the parameter stream as it arrives, without having to wait for the entire parameter to buffer and validate.

<Note>
With fine-grained tool streaming, tool use chunks start streaming faster, and are often longer and contain fewer word breaks. This is due to differences in chunking behavior.

Without fine-grained streaming (15s delay):

With fine-grained streaming (3s delay):

<Warning>
Because fine-grained streaming sends parameters without buffering or JSON validation, there is no guarantee that the resulting stream will complete in a valid JSON string.
Particularly, if the [stop reason](/docs/en/build-with-claude/handling-stop-reasons) `max_tokens` is reached, the stream may end midway through a parameter and may be incomplete. You will generally have to write specific support to handle when `max_tokens` is reached.
</Warning>

## Handling invalid JSON in tool responses

When using fine-grained tool streaming, you may receive invalid or incomplete JSON from the model. If you need to pass this invalid JSON back to the model in an error response block, you may wrap it in a JSON object to ensure proper handling (with a reasonable key). For example:

This approach helps the model understand that the content is invalid JSON while preserving the original malformed data for debugging purposes.

<Note>
When wrapping invalid JSON, make sure to properly escape any quotes or special characters in the invalid JSON string to maintain valid JSON structure in the wrapper object.
</Note>

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

In this example, fine-grained tool streaming enables Claude to stream the lines of a long poem into the tool call `make_file` without buffering to validate if the `lines_of_text` parameter is valid JSON. This means you can see the parameter stream as it arrives, without having to wait for the entire parameter to buffer and validate.

<Note>
With fine-grained tool streaming, tool use chunks start streaming faster, and are often longer and contain fewer word breaks. This is due to differences in chunking behavior.

Example: 

Without fine-grained streaming (15s delay):
```

Example 4 (unknown):
```unknown
With fine-grained streaming (3s delay):
```

---

## Claude on Vertex AI

**URL:** llms-txt#claude-on-vertex-ai

**Contents:**
- Install an SDK for accessing Vertex AI
- Accessing Vertex AI
  - Model Availability
  - Making requests
- Activity logging
- Feature support
- Global vs regional endpoints
  - When to use each option
  - Implementation
  - Additional resources

Anthropic's Claude models are now generally available through [Vertex AI](https://cloud.google.com/vertex-ai).

The Vertex API for accessing Claude is nearly-identical to the [Messages API](/docs/en/api/messages) and supports all of the same options, with two key differences:

* In Vertex, `model` is not passed in the request body. Instead, it is specified in the Google Cloud endpoint URL.
* In Vertex, `anthropic_version` is passed in the request body (rather than as a header), and must be set to the value `vertex-2023-10-16`.

Vertex is also supported by Anthropic's official [client SDKs](/docs/en/api/client-sdks). This guide will walk you through the process of making a request to Claude on Vertex AI in either Python or TypeScript.

Note that this guide assumes you have already have a GCP project that is able to use Vertex AI. See [using the Claude 3 models from Anthropic](https://cloud.google.com/vertex-ai/generative-ai/docs/partner-models/use-claude) for more information on the setup required, as well as a full walkthrough.

## Install an SDK for accessing Vertex AI

First, install Anthropic's [client SDK](/docs/en/api/client-sdks) for your language of choice.

## Accessing Vertex AI

### Model Availability

Note that Anthropic model availability varies by region. Search for "Claude" in the [Vertex AI Model Garden](https://cloud.google.com/model-garden) or go to [Use Claude 3](https://cloud.google.com/vertex-ai/generative-ai/docs/partner-models/use-claude) for the latest information.

| Model                          | Vertex AI API model ID |
| ------------------------------ | ------------------------ |
| Claude Sonnet 4.5              | claude-sonnet-4-5@20250929 |
| Claude Sonnet 4                | claude-sonnet-4@20250514 |
| Claude Sonnet 3.7 <Tooltip tooltipContent="Deprecated as of October 28, 2025.">⚠️</Tooltip> | claude-3-7-sonnet@20250219 |
| Claude Opus 4.5                | claude-opus-4-5@20251101 |
| Claude Opus 4.1                | claude-opus-4-1@20250805 |
| Claude Opus 4                  | claude-opus-4@20250514   |
| Claude Haiku 4.5               | claude-haiku-4-5@20251001 |
| Claude Haiku 3.5 <Tooltip tooltipContent="Deprecated as of December 19, 2025.">⚠️</Tooltip> | claude-3-5-haiku@20241022 |
| Claude Haiku 3                 | claude-3-haiku@20240307  |

Before running requests you may need to run `gcloud auth application-default login` to authenticate with GCP.

The following examples shows how to generate text from Claude on Vertex AI:
<CodeGroup>

See our [client SDKs](/docs/en/api/client-sdks) and the official [Vertex AI docs](https://cloud.google.com/vertex-ai/docs) for more details.

Vertex provides a [request-response logging service](https://cloud.google.com/vertex-ai/generative-ai/docs/multimodal/request-response-logging) that allows customers to log the prompts and completions associated with your usage.

Anthropic recommends that you log your activity on at least a 30-day rolling basis in order to understand your activity and investigate any potential misuse.

<Note>
Turning on this service does not give Google or Anthropic any access to your content.
</Note>

## Feature support
You can find all the features currently supported on Vertex [here](/docs/en/api/overview).

## Global vs regional endpoints

Starting with **Claude Sonnet 4.5 and all future models**, Google Vertex AI offers two endpoint types:

- **Global endpoints**: Dynamic routing for maximum availability
- **Regional endpoints**: Guaranteed data routing through specific geographic regions

Regional endpoints include a 10% pricing premium over global endpoints.

<Note>
This applies to Claude Sonnet 4.5 and future models only. Older models (Claude Sonnet 4, Opus 4, and earlier) maintain their existing pricing structures.
</Note>

### When to use each option

**Global endpoints (recommended):**
- Provide maximum availability and uptime
- Dynamically route requests to regions with available capacity
- No pricing premium
- Best for applications where data residency is flexible
- Only supports pay-as-you-go traffic (provisioned throughput requires regional endpoints)

**Regional endpoints:**
- Route traffic through specific geographic regions
- Required for data residency and compliance requirements
- Support both pay-as-you-go and provisioned throughput
- 10% pricing premium reflects infrastructure costs for dedicated regional capacity

**Using global endpoints (recommended):**

Set the `region` parameter to `"global"` when initializing the client:

**Using regional endpoints:**

Specify a specific region like `"us-east1"` or `"europe-west1"`:

### Additional resources

- **Google Vertex AI pricing:** [cloud.google.com/vertex-ai/generative-ai/pricing](https://cloud.google.com/vertex-ai/generative-ai/pricing)
- **Claude models documentation:** [Claude on Vertex AI](https://cloud.google.com/vertex-ai/generative-ai/docs/partner-models/claude)
- **Google blog post:** [Global endpoint for Claude models](https://cloud.google.com/blog/products/ai-machine-learning/global-endpoint-for-claude-models-generally-available-on-vertex-ai)
- **Anthropic pricing details:** [Pricing documentation](/docs/en/about-claude/pricing#third-party-platform-pricing)

### Prompt engineering

**Examples:**

Example 1 (unknown):
```unknown

```

Example 2 (unknown):
```unknown
</CodeGroup>

## Accessing Vertex AI

### Model Availability

Note that Anthropic model availability varies by region. Search for "Claude" in the [Vertex AI Model Garden](https://cloud.google.com/model-garden) or go to [Use Claude 3](https://cloud.google.com/vertex-ai/generative-ai/docs/partner-models/use-claude) for the latest information.

#### API model IDs

| Model                          | Vertex AI API model ID |
| ------------------------------ | ------------------------ |
| Claude Sonnet 4.5              | claude-sonnet-4-5@20250929 |
| Claude Sonnet 4                | claude-sonnet-4@20250514 |
| Claude Sonnet 3.7 <Tooltip tooltipContent="Deprecated as of October 28, 2025.">⚠️</Tooltip> | claude-3-7-sonnet@20250219 |
| Claude Opus 4.5                | claude-opus-4-5@20251101 |
| Claude Opus 4.1                | claude-opus-4-1@20250805 |
| Claude Opus 4                  | claude-opus-4@20250514   |
| Claude Haiku 4.5               | claude-haiku-4-5@20251001 |
| Claude Haiku 3.5 <Tooltip tooltipContent="Deprecated as of December 19, 2025.">⚠️</Tooltip> | claude-3-5-haiku@20241022 |
| Claude Haiku 3                 | claude-3-haiku@20240307  |

### Making requests

Before running requests you may need to run `gcloud auth application-default login` to authenticate with GCP.

The following examples shows how to generate text from Claude on Vertex AI:
<CodeGroup>
```

Example 3 (unknown):
```unknown

```

Example 4 (unknown):
```unknown

```

---
