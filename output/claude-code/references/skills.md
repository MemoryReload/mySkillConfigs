# Claude-Code - Skills

**Pages:** 81

---

## Correct - Skills will be loaded

**URL:** llms-txt#correct---skills-will-be-loaded

options = ClaudeAgentOptions(
    setting_sources=["user", "project"],  # Required to load Skills
    allowed_tools=["Skill"]
)
typescript TypeScript
// Wrong - Skills won't be loaded
const options = {
  allowedTools: ["Skill"]
};

// Correct - Skills will be loaded
const options = {
  settingSources: ["user", "project"],  // Required to load Skills
  allowedTools: ["Skill"]
};
python Python

**Examples:**

Example 1 (unknown):
```unknown

```

Example 2 (unknown):
```unknown
</CodeGroup>

For more details on `settingSources`/`setting_sources`, see the [TypeScript SDK reference](/docs/en/agent-sdk/typescript#settingsource) or [Python SDK reference](/docs/en/agent-sdk/python#settingsource).

**Check working directory**: The SDK loads Skills relative to the `cwd` option. Ensure it points to a directory containing `.claude/skills/`:

<CodeGroup>
```

---

## Delete Skill (Beta)

**URL:** llms-txt#delete-skill-(beta)

**Contents:**
- Delete
  - Path Parameters
  - Header Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/beta/skills/delete

**delete** `/v1/skills/{skill_id}`

Unique identifier for the skill.

The format and length of IDs may change over time.

### Header Parameters

- `"anthropic-beta": optional array of AnthropicBeta`

Optional header to specify the beta version(s) you want to use.

- `UnionMember0 = string`

- `UnionMember1 = "message-batches-2024-09-24" or "prompt-caching-2024-07-31" or "computer-use-2024-10-22" or 16 more`

- `"message-batches-2024-09-24"`

- `"prompt-caching-2024-07-31"`

- `"computer-use-2024-10-22"`

- `"computer-use-2025-01-24"`

- `"pdfs-2024-09-25"`

- `"token-counting-2024-11-01"`

- `"token-efficient-tools-2025-02-19"`

- `"output-128k-2025-02-19"`

- `"files-api-2025-04-14"`

- `"mcp-client-2025-04-04"`

- `"mcp-client-2025-11-20"`

- `"dev-full-thinking-2025-05-14"`

- `"interleaved-thinking-2025-05-14"`

- `"code-execution-2025-05-22"`

- `"extended-cache-ttl-2025-04-11"`

- `"context-1m-2025-08-07"`

- `"context-management-2025-06-27"`

- `"model-context-window-exceeded-2025-08-26"`

- `"skills-2025-10-02"`

Unique identifier for the skill.

The format and length of IDs may change over time.

For Skills, this is always `"skill_deleted"`.

**Examples:**

Example 1 (http):
```http
curl https://api.anthropic.com/v1/skills/$SKILL_ID \
    -X DELETE \
    -H 'anthropic-version: 2023-06-01' \
    -H 'anthropic-beta: skills-2025-10-02' \
    -H "X-Api-Key: $ANTHROPIC_API_KEY"
```

---

## Create custom DCF analysis Skill

**URL:** llms-txt#create-custom-dcf-analysis-skill

DCF_SKILL=$(curl -X POST "https://api.anthropic.com/v1/skills" \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -H "anthropic-beta: skills-2025-10-02" \
  -F "display_title=DCF Analysis" \
  -F "files[]=@dcf_skill/SKILL.md;filename=dcf_skill/SKILL.md")

DCF_SKILL_ID=$(echo "$DCF_SKILL" | jq -r '.id')

---

## Create Skill (Beta) (Java)

**URL:** llms-txt#create-skill-(beta)-(java)

**Contents:**
- Create
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/java/beta/skills/create

`SkillCreateResponse beta().skills().create(SkillCreateParamsparams = SkillCreateParams.none(), RequestOptionsrequestOptions = RequestOptions.none())`

**post** `/v1/skills`

- `SkillCreateParams params`

- `Optional<List<AnthropicBeta>> betas`

Optional header to specify the beta version(s) you want to use.

- `MESSAGE_BATCHES_2024_09_24("message-batches-2024-09-24")`

- `PROMPT_CACHING_2024_07_31("prompt-caching-2024-07-31")`

- `COMPUTER_USE_2024_10_22("computer-use-2024-10-22")`

- `COMPUTER_USE_2025_01_24("computer-use-2025-01-24")`

- `PDFS_2024_09_25("pdfs-2024-09-25")`

- `TOKEN_COUNTING_2024_11_01("token-counting-2024-11-01")`

- `TOKEN_EFFICIENT_TOOLS_2025_02_19("token-efficient-tools-2025-02-19")`

- `OUTPUT_128K_2025_02_19("output-128k-2025-02-19")`

- `FILES_API_2025_04_14("files-api-2025-04-14")`

- `MCP_CLIENT_2025_04_04("mcp-client-2025-04-04")`

- `MCP_CLIENT_2025_11_20("mcp-client-2025-11-20")`

- `DEV_FULL_THINKING_2025_05_14("dev-full-thinking-2025-05-14")`

- `INTERLEAVED_THINKING_2025_05_14("interleaved-thinking-2025-05-14")`

- `CODE_EXECUTION_2025_05_22("code-execution-2025-05-22")`

- `EXTENDED_CACHE_TTL_2025_04_11("extended-cache-ttl-2025-04-11")`

- `CONTEXT_1M_2025_08_07("context-1m-2025-08-07")`

- `CONTEXT_MANAGEMENT_2025_06_27("context-management-2025-06-27")`

- `MODEL_CONTEXT_WINDOW_EXCEEDED_2025_08_26("model-context-window-exceeded-2025-08-26")`

- `SKILLS_2025_10_02("skills-2025-10-02")`

- `Optional<String> displayTitle`

Display title for the skill.

This is a human-readable label that is not included in the prompt sent to the model.

- `Optional<List<String>> files`

Files to upload for the skill.

All files must be in the same top-level directory and must include a SKILL.md file at the root of that directory.

- `class SkillCreateResponse:`

Unique identifier for the skill.

The format and length of IDs may change over time.

ISO 8601 timestamp of when the skill was created.

- `Optional<String> displayTitle`

Display title for the skill.

This is a human-readable label that is not included in the prompt sent to the model.

- `Optional<String> latestVersion`

The latest version identifier for the skill.

This represents the most recent version of the skill that has been created.

This may be one of the following values:

* `"custom"`: the skill was created by a user
    * `"anthropic"`: the skill was created by Anthropic

For Skills, this is always `"skill"`.

ISO 8601 timestamp of when the skill was last updated.

**Examples:**

Example 1 (java):
```java
package com.anthropic.example;

import com.anthropic.client.AnthropicClient;
import com.anthropic.client.okhttp.AnthropicOkHttpClient;
import com.anthropic.models.beta.skills.SkillCreateParams;
import com.anthropic.models.beta.skills.SkillCreateResponse;

public final class Main {
    private Main() {}

    public static void main(String[] args) {
        AnthropicClient client = AnthropicOkHttpClient.fromEnv();

        SkillCreateResponse skill = client.beta().skills().create();
    }
}
```

---

## Get Skill Version (Beta) (Java)

**URL:** llms-txt#get-skill-version-(beta)-(java)

**Contents:**
- Retrieve
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/java/beta/skills/versions/retrieve

`VersionRetrieveResponse beta().skills().versions().retrieve(VersionRetrieveParamsparams, RequestOptionsrequestOptions = RequestOptions.none())`

**get** `/v1/skills/{skill_id}/versions/{version}`

- `VersionRetrieveParams params`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `Optional<String> version`

Version identifier for the skill.

Each version is identified by a Unix epoch timestamp (e.g., "1759178010641129").

- `Optional<List<AnthropicBeta>> betas`

Optional header to specify the beta version(s) you want to use.

- `MESSAGE_BATCHES_2024_09_24("message-batches-2024-09-24")`

- `PROMPT_CACHING_2024_07_31("prompt-caching-2024-07-31")`

- `COMPUTER_USE_2024_10_22("computer-use-2024-10-22")`

- `COMPUTER_USE_2025_01_24("computer-use-2025-01-24")`

- `PDFS_2024_09_25("pdfs-2024-09-25")`

- `TOKEN_COUNTING_2024_11_01("token-counting-2024-11-01")`

- `TOKEN_EFFICIENT_TOOLS_2025_02_19("token-efficient-tools-2025-02-19")`

- `OUTPUT_128K_2025_02_19("output-128k-2025-02-19")`

- `FILES_API_2025_04_14("files-api-2025-04-14")`

- `MCP_CLIENT_2025_04_04("mcp-client-2025-04-04")`

- `MCP_CLIENT_2025_11_20("mcp-client-2025-11-20")`

- `DEV_FULL_THINKING_2025_05_14("dev-full-thinking-2025-05-14")`

- `INTERLEAVED_THINKING_2025_05_14("interleaved-thinking-2025-05-14")`

- `CODE_EXECUTION_2025_05_22("code-execution-2025-05-22")`

- `EXTENDED_CACHE_TTL_2025_04_11("extended-cache-ttl-2025-04-11")`

- `CONTEXT_1M_2025_08_07("context-1m-2025-08-07")`

- `CONTEXT_MANAGEMENT_2025_06_27("context-management-2025-06-27")`

- `MODEL_CONTEXT_WINDOW_EXCEEDED_2025_08_26("model-context-window-exceeded-2025-08-26")`

- `SKILLS_2025_10_02("skills-2025-10-02")`

- `class VersionRetrieveResponse:`

Unique identifier for the skill version.

The format and length of IDs may change over time.

ISO 8601 timestamp of when the skill version was created.

- `String description`

Description of the skill version.

This is extracted from the SKILL.md file in the skill upload.

Directory name of the skill version.

This is the top-level directory name that was extracted from the uploaded files.

Human-readable name of the skill version.

This is extracted from the SKILL.md file in the skill upload.

Identifier for the skill that this version belongs to.

For Skill Versions, this is always `"skill_version"`.

Version identifier for the skill.

Each version is identified by a Unix epoch timestamp (e.g., "1759178010641129").

**Examples:**

Example 1 (java):
```java
package com.anthropic.example;

import com.anthropic.client.AnthropicClient;
import com.anthropic.client.okhttp.AnthropicOkHttpClient;
import com.anthropic.models.beta.skills.versions.VersionRetrieveParams;
import com.anthropic.models.beta.skills.versions.VersionRetrieveResponse;

public final class Main {
    private Main() {}

    public static void main(String[] args) {
        AnthropicClient client = AnthropicOkHttpClient.fromEnv();

        VersionRetrieveParams params = VersionRetrieveParams.builder()
            .skillId("skill_id")
            .version("version")
            .build();
        VersionRetrieveResponse version = client.beta().skills().versions().retrieve(params);
    }
}
```

---

## Check project Skills

**URL:** llms-txt#check-project-skills

ls .claude/skills/*/SKILL.md

---

## Ensure your cwd points to the directory containing .claude/skills/

**URL:** llms-txt#ensure-your-cwd-points-to-the-directory-containing-.claude/skills/

options = ClaudeAgentOptions(
    cwd="/path/to/project",  # Must contain .claude/skills/
    setting_sources=["user", "project"],  # Required to load Skills
    allowed_tools=["Skill"]
)
typescript TypeScript
// Ensure your cwd points to the directory containing .claude/skills/
const options = {
  cwd: "/path/to/project",  // Must contain .claude/skills/
  settingSources: ["user", "project"],  // Required to load Skills
  allowedTools: ["Skill"]
};
bash

**Examples:**

Example 1 (unknown):
```unknown

```

Example 2 (unknown):
```unknown
</CodeGroup>

See the "Using Skills with the SDK" section above for the complete pattern.

**Verify filesystem location**:
```

---

## List Skills (Beta) (Kotlin)

**URL:** llms-txt#list-skills-(beta)-(kotlin)

**Contents:**
- List
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/kotlin/beta/skills/list

`beta().skills().list(SkillListParamsparams = SkillListParams.none(), RequestOptionsrequestOptions = RequestOptions.none()) : SkillListPage`

- `params: SkillListParams`

- `limit: Optional<Long>`

Number of results to return per page.

Maximum value is 100. Defaults to 20.

- `page: Optional<String>`

Pagination token for fetching a specific page of results.

Pass the value from a previous response's `next_page` field to get the next page of results.

- `source: Optional<String>`

Filter skills by source.

If provided, only skills from the specified source will be returned:

* `"custom"`: only return user-created skills
    * `"anthropic"`: only return Anthropic-created skills

- `betas: Optional<List<AnthropicBeta>>`

Optional header to specify the beta version(s) you want to use.

- `MESSAGE_BATCHES_2024_09_24("message-batches-2024-09-24")`

- `PROMPT_CACHING_2024_07_31("prompt-caching-2024-07-31")`

- `COMPUTER_USE_2024_10_22("computer-use-2024-10-22")`

- `COMPUTER_USE_2025_01_24("computer-use-2025-01-24")`

- `PDFS_2024_09_25("pdfs-2024-09-25")`

- `TOKEN_COUNTING_2024_11_01("token-counting-2024-11-01")`

- `TOKEN_EFFICIENT_TOOLS_2025_02_19("token-efficient-tools-2025-02-19")`

- `OUTPUT_128K_2025_02_19("output-128k-2025-02-19")`

- `FILES_API_2025_04_14("files-api-2025-04-14")`

- `MCP_CLIENT_2025_04_04("mcp-client-2025-04-04")`

- `MCP_CLIENT_2025_11_20("mcp-client-2025-11-20")`

- `DEV_FULL_THINKING_2025_05_14("dev-full-thinking-2025-05-14")`

- `INTERLEAVED_THINKING_2025_05_14("interleaved-thinking-2025-05-14")`

- `CODE_EXECUTION_2025_05_22("code-execution-2025-05-22")`

- `EXTENDED_CACHE_TTL_2025_04_11("extended-cache-ttl-2025-04-11")`

- `CONTEXT_1M_2025_08_07("context-1m-2025-08-07")`

- `CONTEXT_MANAGEMENT_2025_06_27("context-management-2025-06-27")`

- `MODEL_CONTEXT_WINDOW_EXCEEDED_2025_08_26("model-context-window-exceeded-2025-08-26")`

- `SKILLS_2025_10_02("skills-2025-10-02")`

- `class SkillListResponse:`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `createdAt: String`

ISO 8601 timestamp of when the skill was created.

- `displayTitle: Optional<String>`

Display title for the skill.

This is a human-readable label that is not included in the prompt sent to the model.

- `latestVersion: Optional<String>`

The latest version identifier for the skill.

This represents the most recent version of the skill that has been created.

This may be one of the following values:

* `"custom"`: the skill was created by a user
    * `"anthropic"`: the skill was created by Anthropic

For Skills, this is always `"skill"`.

- `updatedAt: String`

ISO 8601 timestamp of when the skill was last updated.

**Examples:**

Example 1 (kotlin):
```kotlin
package com.anthropic.example

import com.anthropic.client.AnthropicClient
import com.anthropic.client.okhttp.AnthropicOkHttpClient
import com.anthropic.models.beta.skills.SkillListPage
import com.anthropic.models.beta.skills.SkillListParams

fun main() {
    val client: AnthropicClient = AnthropicOkHttpClient.fromEnv()

    val page: SkillListPage = client.beta().skills().list()
}
```

---

## Delete Skill (Beta) (Java)

**URL:** llms-txt#delete-skill-(beta)-(java)

**Contents:**
- Delete
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/java/beta/skills/delete

`SkillDeleteResponse beta().skills().delete(SkillDeleteParamsparams = SkillDeleteParams.none(), RequestOptionsrequestOptions = RequestOptions.none())`

**delete** `/v1/skills/{skill_id}`

- `SkillDeleteParams params`

- `Optional<String> skillId`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `Optional<List<AnthropicBeta>> betas`

Optional header to specify the beta version(s) you want to use.

- `MESSAGE_BATCHES_2024_09_24("message-batches-2024-09-24")`

- `PROMPT_CACHING_2024_07_31("prompt-caching-2024-07-31")`

- `COMPUTER_USE_2024_10_22("computer-use-2024-10-22")`

- `COMPUTER_USE_2025_01_24("computer-use-2025-01-24")`

- `PDFS_2024_09_25("pdfs-2024-09-25")`

- `TOKEN_COUNTING_2024_11_01("token-counting-2024-11-01")`

- `TOKEN_EFFICIENT_TOOLS_2025_02_19("token-efficient-tools-2025-02-19")`

- `OUTPUT_128K_2025_02_19("output-128k-2025-02-19")`

- `FILES_API_2025_04_14("files-api-2025-04-14")`

- `MCP_CLIENT_2025_04_04("mcp-client-2025-04-04")`

- `MCP_CLIENT_2025_11_20("mcp-client-2025-11-20")`

- `DEV_FULL_THINKING_2025_05_14("dev-full-thinking-2025-05-14")`

- `INTERLEAVED_THINKING_2025_05_14("interleaved-thinking-2025-05-14")`

- `CODE_EXECUTION_2025_05_22("code-execution-2025-05-22")`

- `EXTENDED_CACHE_TTL_2025_04_11("extended-cache-ttl-2025-04-11")`

- `CONTEXT_1M_2025_08_07("context-1m-2025-08-07")`

- `CONTEXT_MANAGEMENT_2025_06_27("context-management-2025-06-27")`

- `MODEL_CONTEXT_WINDOW_EXCEEDED_2025_08_26("model-context-window-exceeded-2025-08-26")`

- `SKILLS_2025_10_02("skills-2025-10-02")`

- `class SkillDeleteResponse:`

Unique identifier for the skill.

The format and length of IDs may change over time.

For Skills, this is always `"skill_deleted"`.

**Examples:**

Example 1 (java):
```java
package com.anthropic.example;

import com.anthropic.client.AnthropicClient;
import com.anthropic.client.okhttp.AnthropicOkHttpClient;
import com.anthropic.models.beta.skills.SkillDeleteParams;
import com.anthropic.models.beta.skills.SkillDeleteResponse;

public final class Main {
    private Main() {}

    public static void main(String[] args) {
        AnthropicClient client = AnthropicOkHttpClient.fromEnv();

        SkillDeleteResponse skill = client.beta().skills().delete("skill_id");
    }
}
```

---

## Create Skill Version (Beta) (Kotlin)

**URL:** llms-txt#create-skill-version-(beta)-(kotlin)

**Contents:**
- Create
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/kotlin/beta/skills/versions/create

`beta().skills().versions().create(VersionCreateParamsparams = VersionCreateParams.none(), RequestOptionsrequestOptions = RequestOptions.none()) : VersionCreateResponse`

**post** `/v1/skills/{skill_id}/versions`

- `params: VersionCreateParams`

- `skillId: Optional<String>`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `betas: Optional<List<AnthropicBeta>>`

Optional header to specify the beta version(s) you want to use.

- `MESSAGE_BATCHES_2024_09_24("message-batches-2024-09-24")`

- `PROMPT_CACHING_2024_07_31("prompt-caching-2024-07-31")`

- `COMPUTER_USE_2024_10_22("computer-use-2024-10-22")`

- `COMPUTER_USE_2025_01_24("computer-use-2025-01-24")`

- `PDFS_2024_09_25("pdfs-2024-09-25")`

- `TOKEN_COUNTING_2024_11_01("token-counting-2024-11-01")`

- `TOKEN_EFFICIENT_TOOLS_2025_02_19("token-efficient-tools-2025-02-19")`

- `OUTPUT_128K_2025_02_19("output-128k-2025-02-19")`

- `FILES_API_2025_04_14("files-api-2025-04-14")`

- `MCP_CLIENT_2025_04_04("mcp-client-2025-04-04")`

- `MCP_CLIENT_2025_11_20("mcp-client-2025-11-20")`

- `DEV_FULL_THINKING_2025_05_14("dev-full-thinking-2025-05-14")`

- `INTERLEAVED_THINKING_2025_05_14("interleaved-thinking-2025-05-14")`

- `CODE_EXECUTION_2025_05_22("code-execution-2025-05-22")`

- `EXTENDED_CACHE_TTL_2025_04_11("extended-cache-ttl-2025-04-11")`

- `CONTEXT_1M_2025_08_07("context-1m-2025-08-07")`

- `CONTEXT_MANAGEMENT_2025_06_27("context-management-2025-06-27")`

- `MODEL_CONTEXT_WINDOW_EXCEEDED_2025_08_26("model-context-window-exceeded-2025-08-26")`

- `SKILLS_2025_10_02("skills-2025-10-02")`

- `files: Optional<List<String>>`

Files to upload for the skill.

All files must be in the same top-level directory and must include a SKILL.md file at the root of that directory.

- `class VersionCreateResponse:`

Unique identifier for the skill version.

The format and length of IDs may change over time.

- `createdAt: String`

ISO 8601 timestamp of when the skill version was created.

- `description: String`

Description of the skill version.

This is extracted from the SKILL.md file in the skill upload.

- `directory: String`

Directory name of the skill version.

This is the top-level directory name that was extracted from the uploaded files.

Human-readable name of the skill version.

This is extracted from the SKILL.md file in the skill upload.

Identifier for the skill that this version belongs to.

For Skill Versions, this is always `"skill_version"`.

Version identifier for the skill.

Each version is identified by a Unix epoch timestamp (e.g., "1759178010641129").

**Examples:**

Example 1 (kotlin):
```kotlin
package com.anthropic.example

import com.anthropic.client.AnthropicClient
import com.anthropic.client.okhttp.AnthropicOkHttpClient
import com.anthropic.models.beta.skills.versions.VersionCreateParams
import com.anthropic.models.beta.skills.versions.VersionCreateResponse

fun main() {
    val client: AnthropicClient = AnthropicOkHttpClient.fromEnv()

    val version: VersionCreateResponse = client.beta().skills().versions().create("skill_id")
}
```

---

## List Skills (Beta) (TypeScript)

**URL:** llms-txt#list-skills-(beta)-(typescript)

**Contents:**
- List
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/typescript/beta/skills/list

`client.beta.skills.list(SkillListParamsparams?, RequestOptionsoptions?): PageCursor<SkillListResponse>`

- `params: SkillListParams`

Query param: Number of results to return per page.

Maximum value is 100. Defaults to 20.

- `page?: string | null`

Query param: Pagination token for fetching a specific page of results.

Pass the value from a previous response's `next_page` field to get the next page of results.

- `source?: string | null`

Query param: Filter skills by source.

If provided, only skills from the specified source will be returned:

* `"custom"`: only return user-created skills
    * `"anthropic"`: only return Anthropic-created skills

- `betas?: Array<AnthropicBeta>`

Header param: Optional header to specify the beta version(s) you want to use.

- `"message-batches-2024-09-24" | "prompt-caching-2024-07-31" | "computer-use-2024-10-22" | 16 more`

- `"message-batches-2024-09-24"`

- `"prompt-caching-2024-07-31"`

- `"computer-use-2024-10-22"`

- `"computer-use-2025-01-24"`

- `"pdfs-2024-09-25"`

- `"token-counting-2024-11-01"`

- `"token-efficient-tools-2025-02-19"`

- `"output-128k-2025-02-19"`

- `"files-api-2025-04-14"`

- `"mcp-client-2025-04-04"`

- `"mcp-client-2025-11-20"`

- `"dev-full-thinking-2025-05-14"`

- `"interleaved-thinking-2025-05-14"`

- `"code-execution-2025-05-22"`

- `"extended-cache-ttl-2025-04-11"`

- `"context-1m-2025-08-07"`

- `"context-management-2025-06-27"`

- `"model-context-window-exceeded-2025-08-26"`

- `"skills-2025-10-02"`

- `SkillListResponse`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `created_at: string`

ISO 8601 timestamp of when the skill was created.

- `display_title: string | null`

Display title for the skill.

This is a human-readable label that is not included in the prompt sent to the model.

- `latest_version: string | null`

The latest version identifier for the skill.

This represents the most recent version of the skill that has been created.

This may be one of the following values:

* `"custom"`: the skill was created by a user
    * `"anthropic"`: the skill was created by Anthropic

For Skills, this is always `"skill"`.

- `updated_at: string`

ISO 8601 timestamp of when the skill was last updated.

**Examples:**

Example 1 (typescript):
```typescript
import Anthropic from '@anthropic-ai/sdk';

const client = new Anthropic({
  apiKey: process.env['ANTHROPIC_API_KEY'], // This is the default and can be omitted
});

// Automatically fetches more pages as needed.
for await (const skillListResponse of client.beta.skills.list()) {
  console.log(skillListResponse.id);
}
```

---

## List Skills (Beta) (Ruby)

**URL:** llms-txt#list-skills-(beta)-(ruby)

**Contents:**
- List
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/ruby/beta/skills/list

`beta.skills.list(**kwargs) -> PageCursor<SkillListResponse>`

Number of results to return per page.

Maximum value is 100. Defaults to 20.

Pagination token for fetching a specific page of results.

Pass the value from a previous response's `next_page` field to get the next page of results.

Filter skills by source.

If provided, only skills from the specified source will be returned:

* `"custom"`: only return user-created skills
  * `"anthropic"`: only return Anthropic-created skills

- `anthropic_beta: Array[AnthropicBeta]`

Optional header to specify the beta version(s) you want to use.

- `:"message-batches-2024-09-24" | :"prompt-caching-2024-07-31" | :"computer-use-2024-10-22" | 16 more`

- `:"message-batches-2024-09-24"`

- `:"prompt-caching-2024-07-31"`

- `:"computer-use-2024-10-22"`

- `:"computer-use-2025-01-24"`

- `:"pdfs-2024-09-25"`

- `:"token-counting-2024-11-01"`

- `:"token-efficient-tools-2025-02-19"`

- `:"output-128k-2025-02-19"`

- `:"files-api-2025-04-14"`

- `:"mcp-client-2025-04-04"`

- `:"mcp-client-2025-11-20"`

- `:"dev-full-thinking-2025-05-14"`

- `:"interleaved-thinking-2025-05-14"`

- `:"code-execution-2025-05-22"`

- `:"extended-cache-ttl-2025-04-11"`

- `:"context-1m-2025-08-07"`

- `:"context-management-2025-06-27"`

- `:"model-context-window-exceeded-2025-08-26"`

- `:"skills-2025-10-02"`

- `class SkillListResponse`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `created_at: String`

ISO 8601 timestamp of when the skill was created.

- `display_title: String`

Display title for the skill.

This is a human-readable label that is not included in the prompt sent to the model.

- `latest_version: String`

The latest version identifier for the skill.

This represents the most recent version of the skill that has been created.

This may be one of the following values:

* `"custom"`: the skill was created by a user
    * `"anthropic"`: the skill was created by Anthropic

For Skills, this is always `"skill"`.

- `updated_at: String`

ISO 8601 timestamp of when the skill was last updated.

**Examples:**

Example 1 (ruby):
```ruby
require "anthropic"

anthropic = Anthropic::Client.new(api_key: "my-anthropic-api-key")

page = anthropic.beta.skills.list

puts(page)
```

---

## Skill authoring best practices

**URL:** llms-txt#skill-authoring-best-practices

**Contents:**
- Core principles
  - Concise is key
- Extract PDF text
- Extract PDF text
  - Set appropriate degrees of freedom
- Code review process
- Generate report
- Database migration
  - Test with all models you plan to use
- Skill structure

Learn how to write effective Skills that Claude can discover and use successfully.

Good Skills are concise, well-structured, and tested with real usage. This guide provides practical authoring decisions to help you write Skills that Claude can discover and use effectively.

For conceptual background on how Skills work, see the [Skills overview](/docs/en/agents-and-tools/agent-skills/overview).

The [context window](/docs/en/build-with-claude/context-windows) is a public good. Your Skill shares the context window with everything else Claude needs to know, including:
- The system prompt
- Conversation history
- Other Skills' metadata
- Your actual request

Not every token in your Skill has an immediate cost. At startup, only the metadata (name and description) from all Skills is pre-loaded. Claude reads SKILL.md only when the Skill becomes relevant, and reads additional files only as needed. However, being concise in SKILL.md still matters: once Claude loads it, every token competes with conversation history and other context.

**Default assumption**: Claude is already very smart

Only add context Claude doesn't already have. Challenge each piece of information:
- "Does Claude really need this explanation?"
- "Can I assume Claude knows this?"
- "Does this paragraph justify its token cost?"

**Good example: Concise** (approximately 50 tokens):
python
import pdfplumber

with pdfplumber.open("file.pdf") as pdf:
    text = pdf.pages[0].extract_text()
`

**Bad example: Too verbose** (approximately 150 tokens):

The concise version assumes Claude knows what PDFs are and how libraries work.

### Set appropriate degrees of freedom

Match the level of specificity to the task's fragility and variability.

**High freedom** (text-based instructions):

Use when:
- Multiple approaches are valid
- Decisions depend on context
- Heuristics guide the approach

**Medium freedom** (pseudocode or scripts with parameters):

Use when:
- A preferred pattern exists
- Some variation is acceptable
- Configuration affects behavior

Example:
python
def generate_report(data, format="markdown", include_charts=True):
    # Process data
    # Generate output in specified format
    # Optionally include visualizations
`

**Low freedom** (specific scripts, few or no parameters):

Use when:
- Operations are fragile and error-prone
- Consistency is critical
- A specific sequence must be followed

Example:
bash
python scripts/migrate.py --verify --backup
`

**Analogy**: Think of Claude as a robot exploring a path:
- **Narrow bridge with cliffs on both sides**: There's only one safe way forward. Provide specific guardrails and exact instructions (low freedom). Example: database migrations that must run in exact sequence.
- **Open field with no hazards**: Many paths lead to success. Give general direction and trust Claude to find the best route (high freedom). Example: code reviews where context determines the best approach.

### Test with all models you plan to use

Skills act as additions to models, so effectiveness depends on the underlying model. Test your Skill with all the models you plan to use it with.

**Testing considerations by model**:
- **Claude Haiku** (fast, economical): Does the Skill provide enough guidance?
- **Claude Sonnet** (balanced): Is the Skill clear and efficient?
- **Claude Opus** (powerful reasoning): Does the Skill avoid over-explaining?

What works perfectly for Opus might need more detail for Haiku. If you plan to use your Skill across multiple models, aim for instructions that work well with all of them.

<Note>
**YAML Frontmatter**: The SKILL.md frontmatter requires two fields:

`name`:
- Maximum 64 characters
- Must contain only lowercase letters, numbers, and hyphens
- Cannot contain XML tags
- Cannot contain reserved words: "anthropic", "claude"

`description`:
- Must be non-empty
- Maximum 1024 characters
- Cannot contain XML tags
- Should describe what the Skill does and when to use it

For complete Skill structure details, see the [Skills overview](/docs/en/agents-and-tools/agent-skills/overview#skill-structure).
</Note>

### Naming conventions

Use consistent naming patterns to make Skills easier to reference and discuss. We recommend using **gerund form** (verb + -ing) for Skill names, as this clearly describes the activity or capability the Skill provides.

Remember that the `name` field must use lowercase letters, numbers, and hyphens only.

**Good naming examples (gerund form)**:
- `processing-pdfs`
- `analyzing-spreadsheets`
- `managing-databases`
- `testing-code`
- `writing-documentation`

**Acceptable alternatives**:
- Noun phrases: `pdf-processing`, `spreadsheet-analysis`
- Action-oriented: `process-pdfs`, `analyze-spreadsheets`

**Avoid**:
- Vague names: `helper`, `utils`, `tools`
- Overly generic: `documents`, `data`, `files`
- Reserved words: `anthropic-helper`, `claude-tools`
- Inconsistent patterns within your skill collection

Consistent naming makes it easier to:
- Reference Skills in documentation and conversations
- Understand what a Skill does at a glance
- Organize and search through multiple Skills
- Maintain a professional, cohesive skill library

### Writing effective descriptions

The `description` field enables Skill discovery and should include both what the Skill does and when to use it.

<Warning>
**Always write in third person**. The description is injected into the system prompt, and inconsistent point-of-view can cause discovery problems.

- **Good:** "Processes Excel files and generates reports"
- **Avoid:** "I can help you process Excel files"
- **Avoid:** "You can use this to process Excel files"
</Warning>

**Be specific and include key terms**. Include both what the Skill does and specific triggers/contexts for when to use it.

Each Skill has exactly one description field. The description is critical for skill selection: Claude uses it to choose the right Skill from potentially 100+ available Skills. Your description must provide enough detail for Claude to know when to select this Skill, while the rest of SKILL.md provides the implementation details.

**PDF Processing skill:**

**Excel Analysis skill:**

**Git Commit Helper skill:**

Avoid vague descriptions like these:

### Progressive disclosure patterns

SKILL.md serves as an overview that points Claude to detailed materials as needed, like a table of contents in an onboarding guide. For an explanation of how progressive disclosure works, see [How Skills work](/docs/en/agents-and-tools/agent-skills/overview#how-skills-work) in the overview.

**Practical guidance:**
- Keep SKILL.md body under 500 lines for optimal performance
- Split content into separate files when approaching this limit
- Use the patterns below to organize instructions, code, and resources effectively

#### Visual overview: From simple to complex

A basic Skill starts with just a SKILL.md file containing metadata and instructions:

![Simple SKILL.md file showing YAML frontmatter and markdown body](/docs/images/agent-skills-simple-file.png)

As your Skill grows, you can bundle additional content that Claude loads only when needed:

![Bundling additional reference files like reference.md and forms.md.](/docs/images/agent-skills-bundling-content.png)

The complete Skill directory structure might look like this:

#### Pattern 1: High-level guide with references

````markdown
---
name: pdf-processing
description: Extracts text and tables from PDF files, fills forms, and merges documents. Use when working with PDF files or when the user mentions PDFs, forms, or document extraction.
---

**Examples:**

Example 1 (markdown):
```markdown
## Extract PDF text

Use pdfplumber for text extraction:
```

Example 2 (unknown):
```unknown

```

Example 3 (markdown):
```markdown
## Extract PDF text

PDF (Portable Document Format) files are a common file format that contains
text, images, and other content. To extract text from a PDF, you'll need to
use a library. There are many libraries available for PDF processing, but we
recommend pdfplumber because it's easy to use and handles most cases well.
First, you'll need to install it using pip. Then you can use the code below...
```

Example 4 (markdown):
```markdown
## Code review process

1. Analyze the code structure and organization
2. Check for potential bugs or edge cases
3. Suggest improvements for readability and maintainability
4. Verify adherence to project conventions
```

---

## Get Skill Version (Beta) (Python)

**URL:** llms-txt#get-skill-version-(beta)-(python)

**Contents:**
- Retrieve
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/python/beta/skills/versions/retrieve

`beta.skills.versions.retrieve(strversion, VersionRetrieveParams**kwargs)  -> VersionRetrieveResponse`

**get** `/v1/skills/{skill_id}/versions/{version}`

Unique identifier for the skill.

The format and length of IDs may change over time.

Version identifier for the skill.

Each version is identified by a Unix epoch timestamp (e.g., "1759178010641129").

- `betas: Optional[List[AnthropicBetaParam]]`

Optional header to specify the beta version(s) you want to use.

- `UnionMember0 = str`

- `UnionMember1 = Literal["message-batches-2024-09-24", "prompt-caching-2024-07-31", "computer-use-2024-10-22", 16 more]`

- `"message-batches-2024-09-24"`

- `"prompt-caching-2024-07-31"`

- `"computer-use-2024-10-22"`

- `"computer-use-2025-01-24"`

- `"pdfs-2024-09-25"`

- `"token-counting-2024-11-01"`

- `"token-efficient-tools-2025-02-19"`

- `"output-128k-2025-02-19"`

- `"files-api-2025-04-14"`

- `"mcp-client-2025-04-04"`

- `"mcp-client-2025-11-20"`

- `"dev-full-thinking-2025-05-14"`

- `"interleaved-thinking-2025-05-14"`

- `"code-execution-2025-05-22"`

- `"extended-cache-ttl-2025-04-11"`

- `"context-1m-2025-08-07"`

- `"context-management-2025-06-27"`

- `"model-context-window-exceeded-2025-08-26"`

- `"skills-2025-10-02"`

- `class VersionRetrieveResponse: …`

Unique identifier for the skill version.

The format and length of IDs may change over time.

ISO 8601 timestamp of when the skill version was created.

Description of the skill version.

This is extracted from the SKILL.md file in the skill upload.

Directory name of the skill version.

This is the top-level directory name that was extracted from the uploaded files.

Human-readable name of the skill version.

This is extracted from the SKILL.md file in the skill upload.

Identifier for the skill that this version belongs to.

For Skill Versions, this is always `"skill_version"`.

Version identifier for the skill.

Each version is identified by a Unix epoch timestamp (e.g., "1759178010641129").

**Examples:**

Example 1 (python):
```python
import os
from anthropic import Anthropic

client = Anthropic(
    api_key=os.environ.get("ANTHROPIC_API_KEY"),  # This is the default and can be omitted
)
version = client.beta.skills.versions.retrieve(
    version="version",
    skill_id="skill_id",
)
print(version.id)
```

---

## Delete Skill (Beta) (Python)

**URL:** llms-txt#delete-skill-(beta)-(python)

**Contents:**
- Delete
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/python/beta/skills/delete

`beta.skills.delete(strskill_id, SkillDeleteParams**kwargs)  -> SkillDeleteResponse`

**delete** `/v1/skills/{skill_id}`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `betas: Optional[List[AnthropicBetaParam]]`

Optional header to specify the beta version(s) you want to use.

- `UnionMember0 = str`

- `UnionMember1 = Literal["message-batches-2024-09-24", "prompt-caching-2024-07-31", "computer-use-2024-10-22", 16 more]`

- `"message-batches-2024-09-24"`

- `"prompt-caching-2024-07-31"`

- `"computer-use-2024-10-22"`

- `"computer-use-2025-01-24"`

- `"pdfs-2024-09-25"`

- `"token-counting-2024-11-01"`

- `"token-efficient-tools-2025-02-19"`

- `"output-128k-2025-02-19"`

- `"files-api-2025-04-14"`

- `"mcp-client-2025-04-04"`

- `"mcp-client-2025-11-20"`

- `"dev-full-thinking-2025-05-14"`

- `"interleaved-thinking-2025-05-14"`

- `"code-execution-2025-05-22"`

- `"extended-cache-ttl-2025-04-11"`

- `"context-1m-2025-08-07"`

- `"context-management-2025-06-27"`

- `"model-context-window-exceeded-2025-08-26"`

- `"skills-2025-10-02"`

- `class SkillDeleteResponse: …`

Unique identifier for the skill.

The format and length of IDs may change over time.

For Skills, this is always `"skill_deleted"`.

**Examples:**

Example 1 (python):
```python
import os
from anthropic import Anthropic

client = Anthropic(
    api_key=os.environ.get("ANTHROPIC_API_KEY"),  # This is the default and can be omitted
)
skill = client.beta.skills.delete(
    skill_id="skill_id",
)
print(skill.id)
```

---

## Delete Skill (Beta) (Kotlin)

**URL:** llms-txt#delete-skill-(beta)-(kotlin)

**Contents:**
- Delete
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/kotlin/beta/skills/delete

`beta().skills().delete(SkillDeleteParamsparams = SkillDeleteParams.none(), RequestOptionsrequestOptions = RequestOptions.none()) : SkillDeleteResponse`

**delete** `/v1/skills/{skill_id}`

- `params: SkillDeleteParams`

- `skillId: Optional<String>`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `betas: Optional<List<AnthropicBeta>>`

Optional header to specify the beta version(s) you want to use.

- `MESSAGE_BATCHES_2024_09_24("message-batches-2024-09-24")`

- `PROMPT_CACHING_2024_07_31("prompt-caching-2024-07-31")`

- `COMPUTER_USE_2024_10_22("computer-use-2024-10-22")`

- `COMPUTER_USE_2025_01_24("computer-use-2025-01-24")`

- `PDFS_2024_09_25("pdfs-2024-09-25")`

- `TOKEN_COUNTING_2024_11_01("token-counting-2024-11-01")`

- `TOKEN_EFFICIENT_TOOLS_2025_02_19("token-efficient-tools-2025-02-19")`

- `OUTPUT_128K_2025_02_19("output-128k-2025-02-19")`

- `FILES_API_2025_04_14("files-api-2025-04-14")`

- `MCP_CLIENT_2025_04_04("mcp-client-2025-04-04")`

- `MCP_CLIENT_2025_11_20("mcp-client-2025-11-20")`

- `DEV_FULL_THINKING_2025_05_14("dev-full-thinking-2025-05-14")`

- `INTERLEAVED_THINKING_2025_05_14("interleaved-thinking-2025-05-14")`

- `CODE_EXECUTION_2025_05_22("code-execution-2025-05-22")`

- `EXTENDED_CACHE_TTL_2025_04_11("extended-cache-ttl-2025-04-11")`

- `CONTEXT_1M_2025_08_07("context-1m-2025-08-07")`

- `CONTEXT_MANAGEMENT_2025_06_27("context-management-2025-06-27")`

- `MODEL_CONTEXT_WINDOW_EXCEEDED_2025_08_26("model-context-window-exceeded-2025-08-26")`

- `SKILLS_2025_10_02("skills-2025-10-02")`

- `class SkillDeleteResponse:`

Unique identifier for the skill.

The format and length of IDs may change over time.

For Skills, this is always `"skill_deleted"`.

**Examples:**

Example 1 (kotlin):
```kotlin
package com.anthropic.example

import com.anthropic.client.AnthropicClient
import com.anthropic.client.okhttp.AnthropicOkHttpClient
import com.anthropic.models.beta.skills.SkillDeleteParams
import com.anthropic.models.beta.skills.SkillDeleteResponse

fun main() {
    val client: AnthropicClient = AnthropicOkHttpClient.fromEnv()

    val skill: SkillDeleteResponse = client.beta().skills().delete("skill_id")
}
```

---

## Create Skill Version (Beta) (Python)

**URL:** llms-txt#create-skill-version-(beta)-(python)

**Contents:**
- Create
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/python/beta/skills/versions/create

`beta.skills.versions.create(strskill_id, VersionCreateParams**kwargs)  -> VersionCreateResponse`

**post** `/v1/skills/{skill_id}/versions`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `files: Optional[SequenceNotStr[FileTypes]]`

Files to upload for the skill.

All files must be in the same top-level directory and must include a SKILL.md file at the root of that directory.

- `betas: Optional[List[AnthropicBetaParam]]`

Optional header to specify the beta version(s) you want to use.

- `UnionMember0 = str`

- `UnionMember1 = Literal["message-batches-2024-09-24", "prompt-caching-2024-07-31", "computer-use-2024-10-22", 16 more]`

- `"message-batches-2024-09-24"`

- `"prompt-caching-2024-07-31"`

- `"computer-use-2024-10-22"`

- `"computer-use-2025-01-24"`

- `"pdfs-2024-09-25"`

- `"token-counting-2024-11-01"`

- `"token-efficient-tools-2025-02-19"`

- `"output-128k-2025-02-19"`

- `"files-api-2025-04-14"`

- `"mcp-client-2025-04-04"`

- `"mcp-client-2025-11-20"`

- `"dev-full-thinking-2025-05-14"`

- `"interleaved-thinking-2025-05-14"`

- `"code-execution-2025-05-22"`

- `"extended-cache-ttl-2025-04-11"`

- `"context-1m-2025-08-07"`

- `"context-management-2025-06-27"`

- `"model-context-window-exceeded-2025-08-26"`

- `"skills-2025-10-02"`

- `class VersionCreateResponse: …`

Unique identifier for the skill version.

The format and length of IDs may change over time.

ISO 8601 timestamp of when the skill version was created.

Description of the skill version.

This is extracted from the SKILL.md file in the skill upload.

Directory name of the skill version.

This is the top-level directory name that was extracted from the uploaded files.

Human-readable name of the skill version.

This is extracted from the SKILL.md file in the skill upload.

Identifier for the skill that this version belongs to.

For Skill Versions, this is always `"skill_version"`.

Version identifier for the skill.

Each version is identified by a Unix epoch timestamp (e.g., "1759178010641129").

**Examples:**

Example 1 (python):
```python
import os
from anthropic import Anthropic

client = Anthropic(
    api_key=os.environ.get("ANTHROPIC_API_KEY"),  # This is the default and can be omitted
)
version = client.beta.skills.versions.create(
    skill_id="skill_id",
)
print(version.id)
```

---

## Create Skill (Beta) (Kotlin)

**URL:** llms-txt#create-skill-(beta)-(kotlin)

**Contents:**
- Create
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/kotlin/beta/skills/create

`beta().skills().create(SkillCreateParamsparams = SkillCreateParams.none(), RequestOptionsrequestOptions = RequestOptions.none()) : SkillCreateResponse`

**post** `/v1/skills`

- `params: SkillCreateParams`

- `betas: Optional<List<AnthropicBeta>>`

Optional header to specify the beta version(s) you want to use.

- `MESSAGE_BATCHES_2024_09_24("message-batches-2024-09-24")`

- `PROMPT_CACHING_2024_07_31("prompt-caching-2024-07-31")`

- `COMPUTER_USE_2024_10_22("computer-use-2024-10-22")`

- `COMPUTER_USE_2025_01_24("computer-use-2025-01-24")`

- `PDFS_2024_09_25("pdfs-2024-09-25")`

- `TOKEN_COUNTING_2024_11_01("token-counting-2024-11-01")`

- `TOKEN_EFFICIENT_TOOLS_2025_02_19("token-efficient-tools-2025-02-19")`

- `OUTPUT_128K_2025_02_19("output-128k-2025-02-19")`

- `FILES_API_2025_04_14("files-api-2025-04-14")`

- `MCP_CLIENT_2025_04_04("mcp-client-2025-04-04")`

- `MCP_CLIENT_2025_11_20("mcp-client-2025-11-20")`

- `DEV_FULL_THINKING_2025_05_14("dev-full-thinking-2025-05-14")`

- `INTERLEAVED_THINKING_2025_05_14("interleaved-thinking-2025-05-14")`

- `CODE_EXECUTION_2025_05_22("code-execution-2025-05-22")`

- `EXTENDED_CACHE_TTL_2025_04_11("extended-cache-ttl-2025-04-11")`

- `CONTEXT_1M_2025_08_07("context-1m-2025-08-07")`

- `CONTEXT_MANAGEMENT_2025_06_27("context-management-2025-06-27")`

- `MODEL_CONTEXT_WINDOW_EXCEEDED_2025_08_26("model-context-window-exceeded-2025-08-26")`

- `SKILLS_2025_10_02("skills-2025-10-02")`

- `displayTitle: Optional<String>`

Display title for the skill.

This is a human-readable label that is not included in the prompt sent to the model.

- `files: Optional<List<String>>`

Files to upload for the skill.

All files must be in the same top-level directory and must include a SKILL.md file at the root of that directory.

- `class SkillCreateResponse:`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `createdAt: String`

ISO 8601 timestamp of when the skill was created.

- `displayTitle: Optional<String>`

Display title for the skill.

This is a human-readable label that is not included in the prompt sent to the model.

- `latestVersion: Optional<String>`

The latest version identifier for the skill.

This represents the most recent version of the skill that has been created.

This may be one of the following values:

* `"custom"`: the skill was created by a user
    * `"anthropic"`: the skill was created by Anthropic

For Skills, this is always `"skill"`.

- `updatedAt: String`

ISO 8601 timestamp of when the skill was last updated.

**Examples:**

Example 1 (kotlin):
```kotlin
package com.anthropic.example

import com.anthropic.client.AnthropicClient
import com.anthropic.client.okhttp.AnthropicOkHttpClient
import com.anthropic.models.beta.skills.SkillCreateParams
import com.anthropic.models.beta.skills.SkillCreateResponse

fun main() {
    val client: AnthropicClient = AnthropicOkHttpClient.fromEnv()

    val skill: SkillCreateResponse = client.beta().skills().create()
}
```

---

## Create Skill (Beta) (Python)

**URL:** llms-txt#create-skill-(beta)-(python)

**Contents:**
- Create
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/python/beta/skills/create

`beta.skills.create(SkillCreateParams**kwargs)  -> SkillCreateResponse`

**post** `/v1/skills`

- `display_title: Optional[str]`

Display title for the skill.

This is a human-readable label that is not included in the prompt sent to the model.

- `files: Optional[SequenceNotStr[FileTypes]]`

Files to upload for the skill.

All files must be in the same top-level directory and must include a SKILL.md file at the root of that directory.

- `betas: Optional[List[AnthropicBetaParam]]`

Optional header to specify the beta version(s) you want to use.

- `UnionMember0 = str`

- `UnionMember1 = Literal["message-batches-2024-09-24", "prompt-caching-2024-07-31", "computer-use-2024-10-22", 16 more]`

- `"message-batches-2024-09-24"`

- `"prompt-caching-2024-07-31"`

- `"computer-use-2024-10-22"`

- `"computer-use-2025-01-24"`

- `"pdfs-2024-09-25"`

- `"token-counting-2024-11-01"`

- `"token-efficient-tools-2025-02-19"`

- `"output-128k-2025-02-19"`

- `"files-api-2025-04-14"`

- `"mcp-client-2025-04-04"`

- `"mcp-client-2025-11-20"`

- `"dev-full-thinking-2025-05-14"`

- `"interleaved-thinking-2025-05-14"`

- `"code-execution-2025-05-22"`

- `"extended-cache-ttl-2025-04-11"`

- `"context-1m-2025-08-07"`

- `"context-management-2025-06-27"`

- `"model-context-window-exceeded-2025-08-26"`

- `"skills-2025-10-02"`

- `class SkillCreateResponse: …`

Unique identifier for the skill.

The format and length of IDs may change over time.

ISO 8601 timestamp of when the skill was created.

- `display_title: Optional[str]`

Display title for the skill.

This is a human-readable label that is not included in the prompt sent to the model.

- `latest_version: Optional[str]`

The latest version identifier for the skill.

This represents the most recent version of the skill that has been created.

This may be one of the following values:

* `"custom"`: the skill was created by a user
    * `"anthropic"`: the skill was created by Anthropic

For Skills, this is always `"skill"`.

ISO 8601 timestamp of when the skill was last updated.

**Examples:**

Example 1 (python):
```python
import os
from anthropic import Anthropic

client = Anthropic(
    api_key=os.environ.get("ANTHROPIC_API_KEY"),  # This is the default and can be omitted
)
skill = client.beta.skills.create()
print(skill.id)
```

---

## Delete Skill Version (Beta) (TypeScript)

**URL:** llms-txt#delete-skill-version-(beta)-(typescript)

**Contents:**
- Delete
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/typescript/beta/skills/versions/delete

`client.beta.skills.versions.delete(stringversion, VersionDeleteParamsparams, RequestOptionsoptions?): VersionDeleteResponse`

**delete** `/v1/skills/{skill_id}/versions/{version}`

Version identifier for the skill.

Each version is identified by a Unix epoch timestamp (e.g., "1759178010641129").

- `params: VersionDeleteParams`

Path param: Unique identifier for the skill.

The format and length of IDs may change over time.

- `betas?: Array<AnthropicBeta>`

Header param: Optional header to specify the beta version(s) you want to use.

- `"message-batches-2024-09-24" | "prompt-caching-2024-07-31" | "computer-use-2024-10-22" | 16 more`

- `"message-batches-2024-09-24"`

- `"prompt-caching-2024-07-31"`

- `"computer-use-2024-10-22"`

- `"computer-use-2025-01-24"`

- `"pdfs-2024-09-25"`

- `"token-counting-2024-11-01"`

- `"token-efficient-tools-2025-02-19"`

- `"output-128k-2025-02-19"`

- `"files-api-2025-04-14"`

- `"mcp-client-2025-04-04"`

- `"mcp-client-2025-11-20"`

- `"dev-full-thinking-2025-05-14"`

- `"interleaved-thinking-2025-05-14"`

- `"code-execution-2025-05-22"`

- `"extended-cache-ttl-2025-04-11"`

- `"context-1m-2025-08-07"`

- `"context-management-2025-06-27"`

- `"model-context-window-exceeded-2025-08-26"`

- `"skills-2025-10-02"`

- `VersionDeleteResponse`

Version identifier for the skill.

Each version is identified by a Unix epoch timestamp (e.g., "1759178010641129").

For Skill Versions, this is always `"skill_version_deleted"`.

**Examples:**

Example 1 (typescript):
```typescript
import Anthropic from '@anthropic-ai/sdk';

const client = new Anthropic({
  apiKey: process.env['ANTHROPIC_API_KEY'], // This is the default and can be omitted
});

const version = await client.beta.skills.versions.delete('version', { skill_id: 'skill_id' });

console.log(version.id);
```

---

## List Skill Versions (Beta) (Ruby)

**URL:** llms-txt#list-skill-versions-(beta)-(ruby)

**Contents:**
- List
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/ruby/beta/skills/versions/list

`beta.skills.versions.list(skill_id, **kwargs) -> PageCursor<VersionListResponse>`

**get** `/v1/skills/{skill_id}/versions`

Unique identifier for the skill.

The format and length of IDs may change over time.

Number of items to return per page.

Defaults to `20`. Ranges from `1` to `1000`.

Optionally set to the `next_page` token from the previous response.

- `anthropic_beta: Array[AnthropicBeta]`

Optional header to specify the beta version(s) you want to use.

- `:"message-batches-2024-09-24" | :"prompt-caching-2024-07-31" | :"computer-use-2024-10-22" | 16 more`

- `:"message-batches-2024-09-24"`

- `:"prompt-caching-2024-07-31"`

- `:"computer-use-2024-10-22"`

- `:"computer-use-2025-01-24"`

- `:"pdfs-2024-09-25"`

- `:"token-counting-2024-11-01"`

- `:"token-efficient-tools-2025-02-19"`

- `:"output-128k-2025-02-19"`

- `:"files-api-2025-04-14"`

- `:"mcp-client-2025-04-04"`

- `:"mcp-client-2025-11-20"`

- `:"dev-full-thinking-2025-05-14"`

- `:"interleaved-thinking-2025-05-14"`

- `:"code-execution-2025-05-22"`

- `:"extended-cache-ttl-2025-04-11"`

- `:"context-1m-2025-08-07"`

- `:"context-management-2025-06-27"`

- `:"model-context-window-exceeded-2025-08-26"`

- `:"skills-2025-10-02"`

- `class VersionListResponse`

Unique identifier for the skill version.

The format and length of IDs may change over time.

- `created_at: String`

ISO 8601 timestamp of when the skill version was created.

- `description: String`

Description of the skill version.

This is extracted from the SKILL.md file in the skill upload.

- `directory: String`

Directory name of the skill version.

This is the top-level directory name that was extracted from the uploaded files.

Human-readable name of the skill version.

This is extracted from the SKILL.md file in the skill upload.

Identifier for the skill that this version belongs to.

For Skill Versions, this is always `"skill_version"`.

Version identifier for the skill.

Each version is identified by a Unix epoch timestamp (e.g., "1759178010641129").

**Examples:**

Example 1 (ruby):
```ruby
require "anthropic"

anthropic = Anthropic::Client.new(api_key: "my-anthropic-api-key")

page = anthropic.beta.skills.versions.list("skill_id")

puts(page)
```

---

## Wrong - Skills won't be loaded

**URL:** llms-txt#wrong---skills-won't-be-loaded

options = ClaudeAgentOptions(
    allowed_tools=["Skill"]
)

---

## Delete Skill Version (Beta) (Ruby)

**URL:** llms-txt#delete-skill-version-(beta)-(ruby)

**Contents:**
- Delete
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/ruby/beta/skills/versions/delete

`beta.skills.versions.delete(version, **kwargs) -> VersionDeleteResponse`

**delete** `/v1/skills/{skill_id}/versions/{version}`

Unique identifier for the skill.

The format and length of IDs may change over time.

Version identifier for the skill.

Each version is identified by a Unix epoch timestamp (e.g., "1759178010641129").

- `anthropic_beta: Array[AnthropicBeta]`

Optional header to specify the beta version(s) you want to use.

- `:"message-batches-2024-09-24" | :"prompt-caching-2024-07-31" | :"computer-use-2024-10-22" | 16 more`

- `:"message-batches-2024-09-24"`

- `:"prompt-caching-2024-07-31"`

- `:"computer-use-2024-10-22"`

- `:"computer-use-2025-01-24"`

- `:"pdfs-2024-09-25"`

- `:"token-counting-2024-11-01"`

- `:"token-efficient-tools-2025-02-19"`

- `:"output-128k-2025-02-19"`

- `:"files-api-2025-04-14"`

- `:"mcp-client-2025-04-04"`

- `:"mcp-client-2025-11-20"`

- `:"dev-full-thinking-2025-05-14"`

- `:"interleaved-thinking-2025-05-14"`

- `:"code-execution-2025-05-22"`

- `:"extended-cache-ttl-2025-04-11"`

- `:"context-1m-2025-08-07"`

- `:"context-management-2025-06-27"`

- `:"model-context-window-exceeded-2025-08-26"`

- `:"skills-2025-10-02"`

- `class VersionDeleteResponse`

Version identifier for the skill.

Each version is identified by a Unix epoch timestamp (e.g., "1759178010641129").

For Skill Versions, this is always `"skill_version_deleted"`.

**Examples:**

Example 1 (ruby):
```ruby
require "anthropic"

anthropic = Anthropic::Client.new(api_key: "my-anthropic-api-key")

version = anthropic.beta.skills.versions.delete("version", skill_id: "skill_id")

puts(version)
```

---

## Delete Skill Version (Beta) (Python)

**URL:** llms-txt#delete-skill-version-(beta)-(python)

**Contents:**
- Delete
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/python/beta/skills/versions/delete

`beta.skills.versions.delete(strversion, VersionDeleteParams**kwargs)  -> VersionDeleteResponse`

**delete** `/v1/skills/{skill_id}/versions/{version}`

Unique identifier for the skill.

The format and length of IDs may change over time.

Version identifier for the skill.

Each version is identified by a Unix epoch timestamp (e.g., "1759178010641129").

- `betas: Optional[List[AnthropicBetaParam]]`

Optional header to specify the beta version(s) you want to use.

- `UnionMember0 = str`

- `UnionMember1 = Literal["message-batches-2024-09-24", "prompt-caching-2024-07-31", "computer-use-2024-10-22", 16 more]`

- `"message-batches-2024-09-24"`

- `"prompt-caching-2024-07-31"`

- `"computer-use-2024-10-22"`

- `"computer-use-2025-01-24"`

- `"pdfs-2024-09-25"`

- `"token-counting-2024-11-01"`

- `"token-efficient-tools-2025-02-19"`

- `"output-128k-2025-02-19"`

- `"files-api-2025-04-14"`

- `"mcp-client-2025-04-04"`

- `"mcp-client-2025-11-20"`

- `"dev-full-thinking-2025-05-14"`

- `"interleaved-thinking-2025-05-14"`

- `"code-execution-2025-05-22"`

- `"extended-cache-ttl-2025-04-11"`

- `"context-1m-2025-08-07"`

- `"context-management-2025-06-27"`

- `"model-context-window-exceeded-2025-08-26"`

- `"skills-2025-10-02"`

- `class VersionDeleteResponse: …`

Version identifier for the skill.

Each version is identified by a Unix epoch timestamp (e.g., "1759178010641129").

For Skill Versions, this is always `"skill_version_deleted"`.

**Examples:**

Example 1 (python):
```python
import os
from anthropic import Anthropic

client = Anthropic(
    api_key=os.environ.get("ANTHROPIC_API_KEY"),  # This is the default and can be omitted
)
version = client.beta.skills.versions.delete(
    version="version",
    skill_id="skill_id",
)
print(version.id)
```

---

## Create Skill Version (Beta)

**URL:** llms-txt#create-skill-version-(beta)

**Contents:**
- Create
  - Path Parameters
  - Header Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/beta/skills/versions/create

**post** `/v1/skills/{skill_id}/versions`

Unique identifier for the skill.

The format and length of IDs may change over time.

### Header Parameters

- `"anthropic-beta": optional array of AnthropicBeta`

Optional header to specify the beta version(s) you want to use.

- `UnionMember0 = string`

- `UnionMember1 = "message-batches-2024-09-24" or "prompt-caching-2024-07-31" or "computer-use-2024-10-22" or 16 more`

- `"message-batches-2024-09-24"`

- `"prompt-caching-2024-07-31"`

- `"computer-use-2024-10-22"`

- `"computer-use-2025-01-24"`

- `"pdfs-2024-09-25"`

- `"token-counting-2024-11-01"`

- `"token-efficient-tools-2025-02-19"`

- `"output-128k-2025-02-19"`

- `"files-api-2025-04-14"`

- `"mcp-client-2025-04-04"`

- `"mcp-client-2025-11-20"`

- `"dev-full-thinking-2025-05-14"`

- `"interleaved-thinking-2025-05-14"`

- `"code-execution-2025-05-22"`

- `"extended-cache-ttl-2025-04-11"`

- `"context-1m-2025-08-07"`

- `"context-management-2025-06-27"`

- `"model-context-window-exceeded-2025-08-26"`

- `"skills-2025-10-02"`

Unique identifier for the skill version.

The format and length of IDs may change over time.

- `created_at: string`

ISO 8601 timestamp of when the skill version was created.

- `description: string`

Description of the skill version.

This is extracted from the SKILL.md file in the skill upload.

- `directory: string`

Directory name of the skill version.

This is the top-level directory name that was extracted from the uploaded files.

Human-readable name of the skill version.

This is extracted from the SKILL.md file in the skill upload.

Identifier for the skill that this version belongs to.

For Skill Versions, this is always `"skill_version"`.

Version identifier for the skill.

Each version is identified by a Unix epoch timestamp (e.g., "1759178010641129").

**Examples:**

Example 1 (http):
```http
curl https://api.anthropic.com/v1/skills/$SKILL_ID/versions \
    -X POST \
    -H 'anthropic-version: 2023-06-01' \
    -H 'anthropic-beta: skills-2025-10-02' \
    -H "X-Api-Key: $ANTHROPIC_API_KEY"
```

---

## Your Skill Name

**URL:** llms-txt#your-skill-name

**Contents:**
- Instructions
- Examples
- Security considerations
- Available Skills
  - Pre-built Agent Skills
  - Custom Skills examples
- Limitations and constraints
  - Cross-surface availability
  - Sharing scope
  - Runtime environment constraints

## Instructions
[Clear, step-by-step guidance for Claude to follow]

## Examples
[Concrete examples of using this Skill]
```

**Required fields**: `name` and `description`

**Field requirements**:

`name`:
- Maximum 64 characters
- Must contain only lowercase letters, numbers, and hyphens
- Cannot contain XML tags
- Cannot contain reserved words: "anthropic", "claude"

`description`:
- Must be non-empty
- Maximum 1024 characters
- Cannot contain XML tags

The `description` should include both what the Skill does and when Claude should use it. For complete authoring guidance, see the [best practices guide](/docs/en/agents-and-tools/agent-skills/best-practices).

## Security considerations

We strongly recommend using Skills only from trusted sources: those you created yourself or obtained from Anthropic. Skills provide Claude with new capabilities through instructions and code, and while this makes them powerful, it also means a malicious Skill can direct Claude to invoke tools or execute code in ways that don't match the Skill's stated purpose.

<Warning>
If you must use a Skill from an untrusted or unknown source, exercise extreme caution and thoroughly audit it before use. Depending on what access Claude has when executing the Skill, malicious Skills could lead to data exfiltration, unauthorized system access, or other security risks.
</Warning>

**Key security considerations**:
- **Audit thoroughly**: Review all files bundled in the Skill: SKILL.md, scripts, images, and other resources. Look for unusual patterns like unexpected network calls, file access patterns, or operations that don't match the Skill's stated purpose
- **External sources are risky**: Skills that fetch data from external URLs pose particular risk, as fetched content may contain malicious instructions. Even trustworthy Skills can be compromised if their external dependencies change over time
- **Tool misuse**: Malicious Skills can invoke tools (file operations, bash commands, code execution) in harmful ways
- **Data exposure**: Skills with access to sensitive data could be designed to leak information to external systems
- **Treat like installing software**: Only use Skills from trusted sources. Be especially careful when integrating Skills into production systems with access to sensitive data or critical operations

### Pre-built Agent Skills

The following pre-built Agent Skills are available for immediate use:

- **PowerPoint (pptx)**: Create presentations, edit slides, analyze presentation content
- **Excel (xlsx)**: Create spreadsheets, analyze data, generate reports with charts
- **Word (docx)**: Create documents, edit content, format text
- **PDF (pdf)**: Generate formatted PDF documents and reports

These Skills are available on the Claude API and claude.ai. See the [quickstart tutorial](/docs/en/agents-and-tools/agent-skills/quickstart) to start using them in the API.

### Custom Skills examples

For complete examples of custom Skills, see the [Skills cookbook](https://platform.claude.com/cookbook/skills-notebooks-01-skills-introduction).

## Limitations and constraints

Understanding these limitations helps you plan your Skills deployment effectively.

### Cross-surface availability

**Custom Skills do not sync across surfaces**. Skills uploaded to one surface are not automatically available on others:

- Skills uploaded to Claude.ai must be separately uploaded to the API
- Skills uploaded via the API are not available on Claude.ai
- Claude Code Skills are filesystem-based and separate from both Claude.ai and API

You'll need to manage and upload Skills separately for each surface where you want to use them.

Skills have different sharing models depending on where you use them:
- **Claude.ai**: Individual user only; each team member must upload separately
- **Claude API**: Workspace-wide; all workspace members can access uploaded Skills
- **Claude Code**: Personal (`~/.claude/skills/`) or project-based (`.claude/skills/`); can also be shared via Claude Code Plugins

Claude.ai does not currently support centralized admin management or org-wide distribution of custom Skills.

### Runtime environment constraints

The exact runtime environment available to your skill depends on the product surface where you use it.

- **Claude.ai**:
    - **Varying network access**: Depending on user/admin settings, Skills may have full, partial, or no network access. For more details, see the [Create and Edit Files](https://support.claude.com/en/articles/12111783-create-and-edit-files-with-claude#h_6b7e833898) support article.
- **Claude API**:
    - **No network access**: Skills cannot make external API calls or access the internet
    - **No runtime package installation**: Only pre-installed packages are available. You cannot install new packages during execution.
    - **Pre-configured dependencies only**: Check the [code execution tool documentation](/docs/en/agents-and-tools/tool-use/code-execution-tool) for the list of available packages
- **Claude Code**:
    - **Full network access**: Skills have the same network access as any other program on the user's computer
    - **Global package installation discouraged**: Skills should only install packages locally in order to avoid interfering with the user's computer

Plan your Skills to work within these constraints.

<CardGroup cols={2}>
  <Card
    title="Get started with Agent Skills"
    icon="graduation-cap"
    href="/docs/en/agents-and-tools/agent-skills/quickstart"
  >
    Create your first Skill
  </Card>
  <Card
    title="API Guide"
    icon="code"
    href="/docs/en/build-with-claude/skills-guide"
  >
    Use Skills with the Claude API
  </Card>
  <Card
    title="Use Skills in Claude Code"
    icon="terminal"
    href="https://code.claude.com/docs/en/skills"
  >
    Create and manage custom Skills in Claude Code
  </Card>
  <Card
    title="Use Skills in the Agent SDK"
    icon="cube"
    href="/docs/en/agent-sdk/skills"
  >
    Use Skills programmatically in TypeScript and Python
  </Card>
  <Card
    title="Authoring best practices"
    icon="lightbulb"
    href="/docs/en/agents-and-tools/agent-skills/best-practices"
  >
    Write Skills that Claude can use effectively
  </Card>
</CardGroup>

---

## List Skills (Beta) (Python)

**URL:** llms-txt#list-skills-(beta)-(python)

**Contents:**
- List
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/python/beta/skills/list

`beta.skills.list(SkillListParams**kwargs)  -> SyncPageCursor[SkillListResponse]`

- `limit: Optional[int]`

Number of results to return per page.

Maximum value is 100. Defaults to 20.

- `page: Optional[str]`

Pagination token for fetching a specific page of results.

Pass the value from a previous response's `next_page` field to get the next page of results.

- `source: Optional[str]`

Filter skills by source.

If provided, only skills from the specified source will be returned:

* `"custom"`: only return user-created skills
  * `"anthropic"`: only return Anthropic-created skills

- `betas: Optional[List[AnthropicBetaParam]]`

Optional header to specify the beta version(s) you want to use.

- `UnionMember0 = str`

- `UnionMember1 = Literal["message-batches-2024-09-24", "prompt-caching-2024-07-31", "computer-use-2024-10-22", 16 more]`

- `"message-batches-2024-09-24"`

- `"prompt-caching-2024-07-31"`

- `"computer-use-2024-10-22"`

- `"computer-use-2025-01-24"`

- `"pdfs-2024-09-25"`

- `"token-counting-2024-11-01"`

- `"token-efficient-tools-2025-02-19"`

- `"output-128k-2025-02-19"`

- `"files-api-2025-04-14"`

- `"mcp-client-2025-04-04"`

- `"mcp-client-2025-11-20"`

- `"dev-full-thinking-2025-05-14"`

- `"interleaved-thinking-2025-05-14"`

- `"code-execution-2025-05-22"`

- `"extended-cache-ttl-2025-04-11"`

- `"context-1m-2025-08-07"`

- `"context-management-2025-06-27"`

- `"model-context-window-exceeded-2025-08-26"`

- `"skills-2025-10-02"`

- `class SkillListResponse: …`

Unique identifier for the skill.

The format and length of IDs may change over time.

ISO 8601 timestamp of when the skill was created.

- `display_title: Optional[str]`

Display title for the skill.

This is a human-readable label that is not included in the prompt sent to the model.

- `latest_version: Optional[str]`

The latest version identifier for the skill.

This represents the most recent version of the skill that has been created.

This may be one of the following values:

* `"custom"`: the skill was created by a user
    * `"anthropic"`: the skill was created by Anthropic

For Skills, this is always `"skill"`.

ISO 8601 timestamp of when the skill was last updated.

**Examples:**

Example 1 (python):
```python
import os
from anthropic import Anthropic

client = Anthropic(
    api_key=os.environ.get("ANTHROPIC_API_KEY"),  # This is the default and can be omitted
)
page = client.beta.skills.list()
page = page.data[0]
print(page.id)
```

---

## Delete Skill (Beta) (Go)

**URL:** llms-txt#delete-skill-(beta)-(go)

**Contents:**
- Delete
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/go/beta/skills/delete

`client.Beta.Skills.Delete(ctx, skillID, body) (*BetaSkillDeleteResponse, error)`

**delete** `/v1/skills/{skill_id}`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `body BetaSkillDeleteParams`

- `Betas param.Field[[]AnthropicBeta]`

Optional header to specify the beta version(s) you want to use.

- `type AnthropicBeta string`

- `const AnthropicBetaMessageBatches2024_09_24 AnthropicBeta = "message-batches-2024-09-24"`

- `const AnthropicBetaPromptCaching2024_07_31 AnthropicBeta = "prompt-caching-2024-07-31"`

- `const AnthropicBetaComputerUse2024_10_22 AnthropicBeta = "computer-use-2024-10-22"`

- `const AnthropicBetaComputerUse2025_01_24 AnthropicBeta = "computer-use-2025-01-24"`

- `const AnthropicBetaPDFs2024_09_25 AnthropicBeta = "pdfs-2024-09-25"`

- `const AnthropicBetaTokenCounting2024_11_01 AnthropicBeta = "token-counting-2024-11-01"`

- `const AnthropicBetaTokenEfficientTools2025_02_19 AnthropicBeta = "token-efficient-tools-2025-02-19"`

- `const AnthropicBetaOutput128k2025_02_19 AnthropicBeta = "output-128k-2025-02-19"`

- `const AnthropicBetaFilesAPI2025_04_14 AnthropicBeta = "files-api-2025-04-14"`

- `const AnthropicBetaMCPClient2025_04_04 AnthropicBeta = "mcp-client-2025-04-04"`

- `const AnthropicBetaMCPClient2025_11_20 AnthropicBeta = "mcp-client-2025-11-20"`

- `const AnthropicBetaDevFullThinking2025_05_14 AnthropicBeta = "dev-full-thinking-2025-05-14"`

- `const AnthropicBetaInterleavedThinking2025_05_14 AnthropicBeta = "interleaved-thinking-2025-05-14"`

- `const AnthropicBetaCodeExecution2025_05_22 AnthropicBeta = "code-execution-2025-05-22"`

- `const AnthropicBetaExtendedCacheTTL2025_04_11 AnthropicBeta = "extended-cache-ttl-2025-04-11"`

- `const AnthropicBetaContext1m2025_08_07 AnthropicBeta = "context-1m-2025-08-07"`

- `const AnthropicBetaContextManagement2025_06_27 AnthropicBeta = "context-management-2025-06-27"`

- `const AnthropicBetaModelContextWindowExceeded2025_08_26 AnthropicBeta = "model-context-window-exceeded-2025-08-26"`

- `const AnthropicBetaSkills2025_10_02 AnthropicBeta = "skills-2025-10-02"`

- `type BetaSkillDeleteResponse struct{…}`

Unique identifier for the skill.

The format and length of IDs may change over time.

For Skills, this is always `"skill_deleted"`.

**Examples:**

Example 1 (go):
```go
package main

import (
  "context"
  "fmt"

  "github.com/anthropics/anthropic-sdk-go"
  "github.com/anthropics/anthropic-sdk-go/option"
)

func main() {
  client := anthropic.NewClient(
    option.WithAPIKey("my-anthropic-api-key"),
  )
  skill, err := client.Beta.Skills.Delete(
    context.TODO(),
    "skill_id",
    anthropic.BetaSkillDeleteParams{

    },
  )
  if err != nil {
    panic(err.Error())
  }
  fmt.Printf("%+v\n", skill.ID)
}
```

---

## Skills (Beta) (Ruby)

**URL:** llms-txt#skills-(beta)-(ruby)

URL: https://platform.claude.com/docs/en/api/ruby/beta/skills

---

## Skills (Beta)

**URL:** llms-txt#skills-(beta)

URL: https://platform.claude.com/docs/en/api/beta/skills

---

## Skills (Beta) (Kotlin)

**URL:** llms-txt#skills-(beta)-(kotlin)

URL: https://platform.claude.com/docs/en/api/kotlin/beta/skills

---

## Delete Skill Version (Beta)

**URL:** llms-txt#delete-skill-version-(beta)

**Contents:**
- Delete
  - Path Parameters
  - Header Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/beta/skills/versions/delete

**delete** `/v1/skills/{skill_id}/versions/{version}`

Unique identifier for the skill.

The format and length of IDs may change over time.

Version identifier for the skill.

Each version is identified by a Unix epoch timestamp (e.g., "1759178010641129").

### Header Parameters

- `"anthropic-beta": optional array of AnthropicBeta`

Optional header to specify the beta version(s) you want to use.

- `UnionMember0 = string`

- `UnionMember1 = "message-batches-2024-09-24" or "prompt-caching-2024-07-31" or "computer-use-2024-10-22" or 16 more`

- `"message-batches-2024-09-24"`

- `"prompt-caching-2024-07-31"`

- `"computer-use-2024-10-22"`

- `"computer-use-2025-01-24"`

- `"pdfs-2024-09-25"`

- `"token-counting-2024-11-01"`

- `"token-efficient-tools-2025-02-19"`

- `"output-128k-2025-02-19"`

- `"files-api-2025-04-14"`

- `"mcp-client-2025-04-04"`

- `"mcp-client-2025-11-20"`

- `"dev-full-thinking-2025-05-14"`

- `"interleaved-thinking-2025-05-14"`

- `"code-execution-2025-05-22"`

- `"extended-cache-ttl-2025-04-11"`

- `"context-1m-2025-08-07"`

- `"context-management-2025-06-27"`

- `"model-context-window-exceeded-2025-08-26"`

- `"skills-2025-10-02"`

Version identifier for the skill.

Each version is identified by a Unix epoch timestamp (e.g., "1759178010641129").

For Skill Versions, this is always `"skill_version_deleted"`.

**Examples:**

Example 1 (http):
```http
curl https://api.anthropic.com/v1/skills/$SKILL_ID/versions/$VERSION \
    -X DELETE \
    -H 'anthropic-version: 2023-06-01' \
    -H 'anthropic-beta: skills-2025-10-02' \
    -H "X-Api-Key: $ANTHROPIC_API_KEY"
```

---

## Delete Skill Version (Beta) (Java)

**URL:** llms-txt#delete-skill-version-(beta)-(java)

**Contents:**
- Delete
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/java/beta/skills/versions/delete

`VersionDeleteResponse beta().skills().versions().delete(VersionDeleteParamsparams, RequestOptionsrequestOptions = RequestOptions.none())`

**delete** `/v1/skills/{skill_id}/versions/{version}`

- `VersionDeleteParams params`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `Optional<String> version`

Version identifier for the skill.

Each version is identified by a Unix epoch timestamp (e.g., "1759178010641129").

- `Optional<List<AnthropicBeta>> betas`

Optional header to specify the beta version(s) you want to use.

- `MESSAGE_BATCHES_2024_09_24("message-batches-2024-09-24")`

- `PROMPT_CACHING_2024_07_31("prompt-caching-2024-07-31")`

- `COMPUTER_USE_2024_10_22("computer-use-2024-10-22")`

- `COMPUTER_USE_2025_01_24("computer-use-2025-01-24")`

- `PDFS_2024_09_25("pdfs-2024-09-25")`

- `TOKEN_COUNTING_2024_11_01("token-counting-2024-11-01")`

- `TOKEN_EFFICIENT_TOOLS_2025_02_19("token-efficient-tools-2025-02-19")`

- `OUTPUT_128K_2025_02_19("output-128k-2025-02-19")`

- `FILES_API_2025_04_14("files-api-2025-04-14")`

- `MCP_CLIENT_2025_04_04("mcp-client-2025-04-04")`

- `MCP_CLIENT_2025_11_20("mcp-client-2025-11-20")`

- `DEV_FULL_THINKING_2025_05_14("dev-full-thinking-2025-05-14")`

- `INTERLEAVED_THINKING_2025_05_14("interleaved-thinking-2025-05-14")`

- `CODE_EXECUTION_2025_05_22("code-execution-2025-05-22")`

- `EXTENDED_CACHE_TTL_2025_04_11("extended-cache-ttl-2025-04-11")`

- `CONTEXT_1M_2025_08_07("context-1m-2025-08-07")`

- `CONTEXT_MANAGEMENT_2025_06_27("context-management-2025-06-27")`

- `MODEL_CONTEXT_WINDOW_EXCEEDED_2025_08_26("model-context-window-exceeded-2025-08-26")`

- `SKILLS_2025_10_02("skills-2025-10-02")`

- `class VersionDeleteResponse:`

Version identifier for the skill.

Each version is identified by a Unix epoch timestamp (e.g., "1759178010641129").

For Skill Versions, this is always `"skill_version_deleted"`.

**Examples:**

Example 1 (java):
```java
package com.anthropic.example;

import com.anthropic.client.AnthropicClient;
import com.anthropic.client.okhttp.AnthropicOkHttpClient;
import com.anthropic.models.beta.skills.versions.VersionDeleteParams;
import com.anthropic.models.beta.skills.versions.VersionDeleteResponse;

public final class Main {
    private Main() {}

    public static void main(String[] args) {
        AnthropicClient client = AnthropicOkHttpClient.fromEnv();

        VersionDeleteParams params = VersionDeleteParams.builder()
            .skillId("skill_id")
            .version("version")
            .build();
        VersionDeleteResponse version = client.beta().skills().versions().delete(params);
    }
}
```

---

## Delete all versions first, then delete the Skill

**URL:** llms-txt#delete-all-versions-first,-then-delete-the-skill

**Contents:**
  - Versioning

curl -X DELETE "https://api.anthropic.com/v1/skills/skill_01AbCdEfGhIjKlMnOpQrStUv" \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -H "anthropic-beta: skills-2025-10-02"
python Python

**Examples:**

Example 1 (unknown):
```unknown
</CodeGroup>

Attempting to delete a Skill with existing versions will return a 400 error.

### Versioning

Skills support versioning to manage updates safely:

**Anthropic-Managed Skills**:
- Versions use date format: `20251013`
- New versions released as updates are made
- Specify exact versions for stability

**Custom Skills**:
- Auto-generated epoch timestamps: `1759178010641129`
- Use `"latest"` to always get the most recent version
- Create new versions when updating Skill files

<CodeGroup>
```

---

## Skills (Beta) (TypeScript)

**URL:** llms-txt#skills-(beta)-(typescript)

URL: https://platform.claude.com/docs/en/api/typescript/beta/skills

---

## Create a message with the PowerPoint Skill

**URL:** llms-txt#create-a-message-with-the-powerpoint-skill

**Contents:**
- Step 3: Download the created file

response = client.beta.messages.create(
    model="claude-sonnet-4-5-20250929",
    max_tokens=4096,
    betas=["code-execution-2025-08-25", "skills-2025-10-02"],
    container={
        "skills": [
            {
                "type": "anthropic",
                "skill_id": "pptx",
                "version": "latest"
            }
        ]
    },
    messages=[{
        "role": "user",
        "content": "Create a presentation about renewable energy with 5 slides"
    }],
    tools=[{
        "type": "code_execution_20250825",
        "name": "code_execution"
    }]
)

print(response.content)
typescript TypeScript
import Anthropic from '@anthropic-ai/sdk';

const client = new Anthropic();

// Create a message with the PowerPoint Skill
const response = await client.beta.messages.create({
  model: 'claude-sonnet-4-5-20250929',
  max_tokens: 4096,
  betas: ['code-execution-2025-08-25', 'skills-2025-10-02'],
  container: {
    skills: [
      {
        type: 'anthropic',
        skill_id: 'pptx',
        version: 'latest'
      }
    ]
  },
  messages: [{
    role: 'user',
    content: 'Create a presentation about renewable energy with 5 slides'
  }],
  tools: [{
    type: 'code_execution_20250825',
    name: 'code_execution'
  }]
});

console.log(response.content);
bash Shell
curl https://api.anthropic.com/v1/messages \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -H "anthropic-beta: code-execution-2025-08-25,skills-2025-10-02" \
  -H "content-type: application/json" \
  -d '{
    "model": "claude-sonnet-4-5-20250929",
    "max_tokens": 4096,
    "container": {
      "skills": [
        {
          "type": "anthropic",
          "skill_id": "pptx",
          "version": "latest"
        }
      ]
    },
    "messages": [{
      "role": "user",
      "content": "Create a presentation about renewable energy with 5 slides"
    }],
    "tools": [{
      "type": "code_execution_20250825",
      "name": "code_execution"
    }]
  }'
python Python

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

Let's break down what each part does:

- **`container.skills`**: Specifies which Skills Claude can use
- **`type: "anthropic"`**: Indicates this is an Anthropic-managed Skill
- **`skill_id: "pptx"`**: The PowerPoint Skill identifier
- **`version: "latest"`**: The Skill version set to the most recently published
- **`tools`**: Enables code execution (required for Skills)
- **Beta headers**: `code-execution-2025-08-25` and `skills-2025-10-02`

When you make this request, Claude automatically matches your task to the relevant Skill. Since you asked for a presentation, Claude determines the PowerPoint Skill is relevant and loads its full instructions: the second level of progressive disclosure. Then Claude executes the Skill's code to create your presentation.

## Step 3: Download the created file

The presentation was created in the code execution container and saved as a file. The response includes a file reference with a file ID. Extract the file ID and download it using the Files API:

<CodeGroup>
```

---

## List Skill Versions (Beta) (Java)

**URL:** llms-txt#list-skill-versions-(beta)-(java)

**Contents:**
- List
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/java/beta/skills/versions/list

`VersionListPage beta().skills().versions().list(VersionListParamsparams = VersionListParams.none(), RequestOptionsrequestOptions = RequestOptions.none())`

**get** `/v1/skills/{skill_id}/versions`

- `VersionListParams params`

- `Optional<String> skillId`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `Optional<Long> limit`

Number of items to return per page.

Defaults to `20`. Ranges from `1` to `1000`.

- `Optional<String> page`

Optionally set to the `next_page` token from the previous response.

- `Optional<List<AnthropicBeta>> betas`

Optional header to specify the beta version(s) you want to use.

- `MESSAGE_BATCHES_2024_09_24("message-batches-2024-09-24")`

- `PROMPT_CACHING_2024_07_31("prompt-caching-2024-07-31")`

- `COMPUTER_USE_2024_10_22("computer-use-2024-10-22")`

- `COMPUTER_USE_2025_01_24("computer-use-2025-01-24")`

- `PDFS_2024_09_25("pdfs-2024-09-25")`

- `TOKEN_COUNTING_2024_11_01("token-counting-2024-11-01")`

- `TOKEN_EFFICIENT_TOOLS_2025_02_19("token-efficient-tools-2025-02-19")`

- `OUTPUT_128K_2025_02_19("output-128k-2025-02-19")`

- `FILES_API_2025_04_14("files-api-2025-04-14")`

- `MCP_CLIENT_2025_04_04("mcp-client-2025-04-04")`

- `MCP_CLIENT_2025_11_20("mcp-client-2025-11-20")`

- `DEV_FULL_THINKING_2025_05_14("dev-full-thinking-2025-05-14")`

- `INTERLEAVED_THINKING_2025_05_14("interleaved-thinking-2025-05-14")`

- `CODE_EXECUTION_2025_05_22("code-execution-2025-05-22")`

- `EXTENDED_CACHE_TTL_2025_04_11("extended-cache-ttl-2025-04-11")`

- `CONTEXT_1M_2025_08_07("context-1m-2025-08-07")`

- `CONTEXT_MANAGEMENT_2025_06_27("context-management-2025-06-27")`

- `MODEL_CONTEXT_WINDOW_EXCEEDED_2025_08_26("model-context-window-exceeded-2025-08-26")`

- `SKILLS_2025_10_02("skills-2025-10-02")`

- `class VersionListResponse:`

Unique identifier for the skill version.

The format and length of IDs may change over time.

ISO 8601 timestamp of when the skill version was created.

- `String description`

Description of the skill version.

This is extracted from the SKILL.md file in the skill upload.

Directory name of the skill version.

This is the top-level directory name that was extracted from the uploaded files.

Human-readable name of the skill version.

This is extracted from the SKILL.md file in the skill upload.

Identifier for the skill that this version belongs to.

For Skill Versions, this is always `"skill_version"`.

Version identifier for the skill.

Each version is identified by a Unix epoch timestamp (e.g., "1759178010641129").

**Examples:**

Example 1 (java):
```java
package com.anthropic.example;

import com.anthropic.client.AnthropicClient;
import com.anthropic.client.okhttp.AnthropicOkHttpClient;
import com.anthropic.models.beta.skills.versions.VersionListPage;
import com.anthropic.models.beta.skills.versions.VersionListParams;

public final class Main {
    private Main() {}

    public static void main(String[] args) {
        AnthropicClient client = AnthropicOkHttpClient.fromEnv();

        VersionListPage page = client.beta().skills().versions().list("skill_id");
    }
}
```

---

## List Skill Versions (Beta) (Go)

**URL:** llms-txt#list-skill-versions-(beta)-(go)

**Contents:**
- List
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/go/beta/skills/versions/list

`client.Beta.Skills.Versions.List(ctx, skillID, params) (*PageCursor[BetaSkillVersionListResponse], error)`

**get** `/v1/skills/{skill_id}/versions`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `params BetaSkillVersionListParams`

- `Limit param.Field[int64]`

Query param: Number of items to return per page.

Defaults to `20`. Ranges from `1` to `1000`.

- `Page param.Field[string]`

Query param: Optionally set to the `next_page` token from the previous response.

- `Betas param.Field[[]AnthropicBeta]`

Header param: Optional header to specify the beta version(s) you want to use.

- `type AnthropicBeta string`

- `const AnthropicBetaMessageBatches2024_09_24 AnthropicBeta = "message-batches-2024-09-24"`

- `const AnthropicBetaPromptCaching2024_07_31 AnthropicBeta = "prompt-caching-2024-07-31"`

- `const AnthropicBetaComputerUse2024_10_22 AnthropicBeta = "computer-use-2024-10-22"`

- `const AnthropicBetaComputerUse2025_01_24 AnthropicBeta = "computer-use-2025-01-24"`

- `const AnthropicBetaPDFs2024_09_25 AnthropicBeta = "pdfs-2024-09-25"`

- `const AnthropicBetaTokenCounting2024_11_01 AnthropicBeta = "token-counting-2024-11-01"`

- `const AnthropicBetaTokenEfficientTools2025_02_19 AnthropicBeta = "token-efficient-tools-2025-02-19"`

- `const AnthropicBetaOutput128k2025_02_19 AnthropicBeta = "output-128k-2025-02-19"`

- `const AnthropicBetaFilesAPI2025_04_14 AnthropicBeta = "files-api-2025-04-14"`

- `const AnthropicBetaMCPClient2025_04_04 AnthropicBeta = "mcp-client-2025-04-04"`

- `const AnthropicBetaMCPClient2025_11_20 AnthropicBeta = "mcp-client-2025-11-20"`

- `const AnthropicBetaDevFullThinking2025_05_14 AnthropicBeta = "dev-full-thinking-2025-05-14"`

- `const AnthropicBetaInterleavedThinking2025_05_14 AnthropicBeta = "interleaved-thinking-2025-05-14"`

- `const AnthropicBetaCodeExecution2025_05_22 AnthropicBeta = "code-execution-2025-05-22"`

- `const AnthropicBetaExtendedCacheTTL2025_04_11 AnthropicBeta = "extended-cache-ttl-2025-04-11"`

- `const AnthropicBetaContext1m2025_08_07 AnthropicBeta = "context-1m-2025-08-07"`

- `const AnthropicBetaContextManagement2025_06_27 AnthropicBeta = "context-management-2025-06-27"`

- `const AnthropicBetaModelContextWindowExceeded2025_08_26 AnthropicBeta = "model-context-window-exceeded-2025-08-26"`

- `const AnthropicBetaSkills2025_10_02 AnthropicBeta = "skills-2025-10-02"`

- `type BetaSkillVersionListResponse struct{…}`

Unique identifier for the skill version.

The format and length of IDs may change over time.

ISO 8601 timestamp of when the skill version was created.

- `Description string`

Description of the skill version.

This is extracted from the SKILL.md file in the skill upload.

Directory name of the skill version.

This is the top-level directory name that was extracted from the uploaded files.

Human-readable name of the skill version.

This is extracted from the SKILL.md file in the skill upload.

Identifier for the skill that this version belongs to.

For Skill Versions, this is always `"skill_version"`.

Version identifier for the skill.

Each version is identified by a Unix epoch timestamp (e.g., "1759178010641129").

**Examples:**

Example 1 (go):
```go
package main

import (
  "context"
  "fmt"

  "github.com/anthropics/anthropic-sdk-go"
  "github.com/anthropics/anthropic-sdk-go/option"
)

func main() {
  client := anthropic.NewClient(
    option.WithAPIKey("my-anthropic-api-key"),
  )
  page, err := client.Beta.Skills.Versions.List(
    context.TODO(),
    "skill_id",
    anthropic.BetaSkillVersionListParams{

    },
  )
  if err != nil {
    panic(err.Error())
  }
  fmt.Printf("%+v\n", page)
}
```

---

## Delete Skill (Beta) (TypeScript)

**URL:** llms-txt#delete-skill-(beta)-(typescript)

**Contents:**
- Delete
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/typescript/beta/skills/delete

`client.beta.skills.delete(stringskillID, SkillDeleteParamsparams?, RequestOptionsoptions?): SkillDeleteResponse`

**delete** `/v1/skills/{skill_id}`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `params: SkillDeleteParams`

- `betas?: Array<AnthropicBeta>`

Optional header to specify the beta version(s) you want to use.

- `"message-batches-2024-09-24" | "prompt-caching-2024-07-31" | "computer-use-2024-10-22" | 16 more`

- `"message-batches-2024-09-24"`

- `"prompt-caching-2024-07-31"`

- `"computer-use-2024-10-22"`

- `"computer-use-2025-01-24"`

- `"pdfs-2024-09-25"`

- `"token-counting-2024-11-01"`

- `"token-efficient-tools-2025-02-19"`

- `"output-128k-2025-02-19"`

- `"files-api-2025-04-14"`

- `"mcp-client-2025-04-04"`

- `"mcp-client-2025-11-20"`

- `"dev-full-thinking-2025-05-14"`

- `"interleaved-thinking-2025-05-14"`

- `"code-execution-2025-05-22"`

- `"extended-cache-ttl-2025-04-11"`

- `"context-1m-2025-08-07"`

- `"context-management-2025-06-27"`

- `"model-context-window-exceeded-2025-08-26"`

- `"skills-2025-10-02"`

- `SkillDeleteResponse`

Unique identifier for the skill.

The format and length of IDs may change over time.

For Skills, this is always `"skill_deleted"`.

**Examples:**

Example 1 (typescript):
```typescript
import Anthropic from '@anthropic-ai/sdk';

const client = new Anthropic({
  apiKey: process.env['ANTHROPIC_API_KEY'], // This is the default and can be omitted
});

const skill = await client.beta.skills.delete('skill_id');

console.log(skill.id);
```

---

## List Skills (Beta) (Java)

**URL:** llms-txt#list-skills-(beta)-(java)

**Contents:**
- List
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/java/beta/skills/list

`SkillListPage beta().skills().list(SkillListParamsparams = SkillListParams.none(), RequestOptionsrequestOptions = RequestOptions.none())`

- `SkillListParams params`

- `Optional<Long> limit`

Number of results to return per page.

Maximum value is 100. Defaults to 20.

- `Optional<String> page`

Pagination token for fetching a specific page of results.

Pass the value from a previous response's `next_page` field to get the next page of results.

- `Optional<String> source`

Filter skills by source.

If provided, only skills from the specified source will be returned:

* `"custom"`: only return user-created skills
    * `"anthropic"`: only return Anthropic-created skills

- `Optional<List<AnthropicBeta>> betas`

Optional header to specify the beta version(s) you want to use.

- `MESSAGE_BATCHES_2024_09_24("message-batches-2024-09-24")`

- `PROMPT_CACHING_2024_07_31("prompt-caching-2024-07-31")`

- `COMPUTER_USE_2024_10_22("computer-use-2024-10-22")`

- `COMPUTER_USE_2025_01_24("computer-use-2025-01-24")`

- `PDFS_2024_09_25("pdfs-2024-09-25")`

- `TOKEN_COUNTING_2024_11_01("token-counting-2024-11-01")`

- `TOKEN_EFFICIENT_TOOLS_2025_02_19("token-efficient-tools-2025-02-19")`

- `OUTPUT_128K_2025_02_19("output-128k-2025-02-19")`

- `FILES_API_2025_04_14("files-api-2025-04-14")`

- `MCP_CLIENT_2025_04_04("mcp-client-2025-04-04")`

- `MCP_CLIENT_2025_11_20("mcp-client-2025-11-20")`

- `DEV_FULL_THINKING_2025_05_14("dev-full-thinking-2025-05-14")`

- `INTERLEAVED_THINKING_2025_05_14("interleaved-thinking-2025-05-14")`

- `CODE_EXECUTION_2025_05_22("code-execution-2025-05-22")`

- `EXTENDED_CACHE_TTL_2025_04_11("extended-cache-ttl-2025-04-11")`

- `CONTEXT_1M_2025_08_07("context-1m-2025-08-07")`

- `CONTEXT_MANAGEMENT_2025_06_27("context-management-2025-06-27")`

- `MODEL_CONTEXT_WINDOW_EXCEEDED_2025_08_26("model-context-window-exceeded-2025-08-26")`

- `SKILLS_2025_10_02("skills-2025-10-02")`

- `class SkillListResponse:`

Unique identifier for the skill.

The format and length of IDs may change over time.

ISO 8601 timestamp of when the skill was created.

- `Optional<String> displayTitle`

Display title for the skill.

This is a human-readable label that is not included in the prompt sent to the model.

- `Optional<String> latestVersion`

The latest version identifier for the skill.

This represents the most recent version of the skill that has been created.

This may be one of the following values:

* `"custom"`: the skill was created by a user
    * `"anthropic"`: the skill was created by Anthropic

For Skills, this is always `"skill"`.

ISO 8601 timestamp of when the skill was last updated.

**Examples:**

Example 1 (java):
```java
package com.anthropic.example;

import com.anthropic.client.AnthropicClient;
import com.anthropic.client.okhttp.AnthropicOkHttpClient;
import com.anthropic.models.beta.skills.SkillListPage;
import com.anthropic.models.beta.skills.SkillListParams;

public final class Main {
    private Main() {}

    public static void main(String[] args) {
        AnthropicClient client = AnthropicOkHttpClient.fromEnv();

        SkillListPage page = client.beta().skills().list();
    }
}
```

---

## Get Skill (Beta) (Kotlin)

**URL:** llms-txt#get-skill-(beta)-(kotlin)

**Contents:**
- Retrieve
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/kotlin/beta/skills/retrieve

`beta().skills().retrieve(SkillRetrieveParamsparams = SkillRetrieveParams.none(), RequestOptionsrequestOptions = RequestOptions.none()) : SkillRetrieveResponse`

**get** `/v1/skills/{skill_id}`

- `params: SkillRetrieveParams`

- `skillId: Optional<String>`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `betas: Optional<List<AnthropicBeta>>`

Optional header to specify the beta version(s) you want to use.

- `MESSAGE_BATCHES_2024_09_24("message-batches-2024-09-24")`

- `PROMPT_CACHING_2024_07_31("prompt-caching-2024-07-31")`

- `COMPUTER_USE_2024_10_22("computer-use-2024-10-22")`

- `COMPUTER_USE_2025_01_24("computer-use-2025-01-24")`

- `PDFS_2024_09_25("pdfs-2024-09-25")`

- `TOKEN_COUNTING_2024_11_01("token-counting-2024-11-01")`

- `TOKEN_EFFICIENT_TOOLS_2025_02_19("token-efficient-tools-2025-02-19")`

- `OUTPUT_128K_2025_02_19("output-128k-2025-02-19")`

- `FILES_API_2025_04_14("files-api-2025-04-14")`

- `MCP_CLIENT_2025_04_04("mcp-client-2025-04-04")`

- `MCP_CLIENT_2025_11_20("mcp-client-2025-11-20")`

- `DEV_FULL_THINKING_2025_05_14("dev-full-thinking-2025-05-14")`

- `INTERLEAVED_THINKING_2025_05_14("interleaved-thinking-2025-05-14")`

- `CODE_EXECUTION_2025_05_22("code-execution-2025-05-22")`

- `EXTENDED_CACHE_TTL_2025_04_11("extended-cache-ttl-2025-04-11")`

- `CONTEXT_1M_2025_08_07("context-1m-2025-08-07")`

- `CONTEXT_MANAGEMENT_2025_06_27("context-management-2025-06-27")`

- `MODEL_CONTEXT_WINDOW_EXCEEDED_2025_08_26("model-context-window-exceeded-2025-08-26")`

- `SKILLS_2025_10_02("skills-2025-10-02")`

- `class SkillRetrieveResponse:`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `createdAt: String`

ISO 8601 timestamp of when the skill was created.

- `displayTitle: Optional<String>`

Display title for the skill.

This is a human-readable label that is not included in the prompt sent to the model.

- `latestVersion: Optional<String>`

The latest version identifier for the skill.

This represents the most recent version of the skill that has been created.

This may be one of the following values:

* `"custom"`: the skill was created by a user
    * `"anthropic"`: the skill was created by Anthropic

For Skills, this is always `"skill"`.

- `updatedAt: String`

ISO 8601 timestamp of when the skill was last updated.

**Examples:**

Example 1 (kotlin):
```kotlin
package com.anthropic.example

import com.anthropic.client.AnthropicClient
import com.anthropic.client.okhttp.AnthropicOkHttpClient
import com.anthropic.models.beta.skills.SkillRetrieveParams
import com.anthropic.models.beta.skills.SkillRetrieveResponse

fun main() {
    val client: AnthropicClient = AnthropicOkHttpClient.fromEnv()

    val skill: SkillRetrieveResponse = client.beta().skills().retrieve("skill_id")
}
```

---

## Skills (Beta) (Go)

**URL:** llms-txt#skills-(beta)-(go)

URL: https://platform.claude.com/docs/en/api/go/beta/skills

---

## Adding/removing Skills breaks cache

**URL:** llms-txt#adding/removing-skills-breaks-cache

**Contents:**
  - Error Handling
- Next Steps
  - Agent SDK

curl https://api.anthropic.com/v1/messages \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -H "anthropic-beta: code-execution-2025-08-25,skills-2025-10-02,prompt-caching-2024-07-31" \
  -H "content-type: application/json" \
  -d '{
    "model": "claude-sonnet-4-5-20250929",
    "max_tokens": 4096,
    "container": {
      "skills": [
        {"type": "anthropic", "skill_id": "xlsx", "version": "latest"},
        {"type": "anthropic", "skill_id": "pptx", "version": "latest"}
      ]
    },
    "messages": [{"role": "user", "content": "Create a presentation"}],
    "tools": [{"type": "code_execution_20250825", "name": "code_execution"}]
  }'
python Python
try:
    response = client.beta.messages.create(
        model="claude-sonnet-4-5-20250929",
        max_tokens=4096,
        betas=["code-execution-2025-08-25", "skills-2025-10-02"],
        container={
            "skills": [
                {"type": "custom", "skill_id": "skill_01AbCdEfGhIjKlMnOpQrStUv", "version": "latest"}
            ]
        },
        messages=[{"role": "user", "content": "Process data"}],
        tools=[{"type": "code_execution_20250825", "name": "code_execution"}]
    )
except anthropic.BadRequestError as e:
    if "skill" in str(e):
        print(f"Skill error: {e}")
        # Handle skill-specific errors
    else:
        raise
typescript TypeScript
try {
  const response = await client.beta.messages.create({
    model: 'claude-sonnet-4-5-20250929',
    max_tokens: 4096,
    betas: ['code-execution-2025-08-25', 'skills-2025-10-02'],
    container: {
      skills: [
        {type: 'custom', skill_id: 'skill_01AbCdEfGhIjKlMnOpQrStUv', version: 'latest'}
      ]
    },
    messages: [{role: 'user', content: 'Process data'}],
    tools: [{type: 'code_execution_20250825', name: 'code_execution'}]
  });
} catch (error) {
  if (error instanceof Anthropic.BadRequestError && error.message.includes('skill')) {
    console.error(`Skill error: ${error.message}`);
    // Handle skill-specific errors
  } else {
    throw error;
  }
}
```
</CodeGroup>

<CardGroup cols={2}>
  <Card
    title="API Reference"
    icon="book"
    href="/docs/en/api/skills/create-skill"
  >
    Complete API reference with all endpoints
  </Card>
  <Card
    title="Authoring Guide"
    icon="edit"
    href="/docs/en/agents-and-tools/agent-skills/best-practices"
  >
    Best practices for writing effective Skills
  </Card>
  <Card
    title="Code Execution Tool"
    icon="terminal"
    href="/docs/en/agents-and-tools/tool-use/code-execution-tool"
  >
    Learn about the code execution environment
  </Card>
</CardGroup>

**Examples:**

Example 1 (unknown):
```unknown
</CodeGroup>

For best caching performance, keep your Skills list consistent across requests.

### Error Handling

Handle Skill-related errors gracefully:

<CodeGroup>
```

Example 2 (unknown):
```unknown

```

---

## Get Skill Version (Beta) (Kotlin)

**URL:** llms-txt#get-skill-version-(beta)-(kotlin)

**Contents:**
- Retrieve
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/kotlin/beta/skills/versions/retrieve

`beta().skills().versions().retrieve(VersionRetrieveParamsparams, RequestOptionsrequestOptions = RequestOptions.none()) : VersionRetrieveResponse`

**get** `/v1/skills/{skill_id}/versions/{version}`

- `params: VersionRetrieveParams`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `version: Optional<String>`

Version identifier for the skill.

Each version is identified by a Unix epoch timestamp (e.g., "1759178010641129").

- `betas: Optional<List<AnthropicBeta>>`

Optional header to specify the beta version(s) you want to use.

- `MESSAGE_BATCHES_2024_09_24("message-batches-2024-09-24")`

- `PROMPT_CACHING_2024_07_31("prompt-caching-2024-07-31")`

- `COMPUTER_USE_2024_10_22("computer-use-2024-10-22")`

- `COMPUTER_USE_2025_01_24("computer-use-2025-01-24")`

- `PDFS_2024_09_25("pdfs-2024-09-25")`

- `TOKEN_COUNTING_2024_11_01("token-counting-2024-11-01")`

- `TOKEN_EFFICIENT_TOOLS_2025_02_19("token-efficient-tools-2025-02-19")`

- `OUTPUT_128K_2025_02_19("output-128k-2025-02-19")`

- `FILES_API_2025_04_14("files-api-2025-04-14")`

- `MCP_CLIENT_2025_04_04("mcp-client-2025-04-04")`

- `MCP_CLIENT_2025_11_20("mcp-client-2025-11-20")`

- `DEV_FULL_THINKING_2025_05_14("dev-full-thinking-2025-05-14")`

- `INTERLEAVED_THINKING_2025_05_14("interleaved-thinking-2025-05-14")`

- `CODE_EXECUTION_2025_05_22("code-execution-2025-05-22")`

- `EXTENDED_CACHE_TTL_2025_04_11("extended-cache-ttl-2025-04-11")`

- `CONTEXT_1M_2025_08_07("context-1m-2025-08-07")`

- `CONTEXT_MANAGEMENT_2025_06_27("context-management-2025-06-27")`

- `MODEL_CONTEXT_WINDOW_EXCEEDED_2025_08_26("model-context-window-exceeded-2025-08-26")`

- `SKILLS_2025_10_02("skills-2025-10-02")`

- `class VersionRetrieveResponse:`

Unique identifier for the skill version.

The format and length of IDs may change over time.

- `createdAt: String`

ISO 8601 timestamp of when the skill version was created.

- `description: String`

Description of the skill version.

This is extracted from the SKILL.md file in the skill upload.

- `directory: String`

Directory name of the skill version.

This is the top-level directory name that was extracted from the uploaded files.

Human-readable name of the skill version.

This is extracted from the SKILL.md file in the skill upload.

Identifier for the skill that this version belongs to.

For Skill Versions, this is always `"skill_version"`.

Version identifier for the skill.

Each version is identified by a Unix epoch timestamp (e.g., "1759178010641129").

**Examples:**

Example 1 (kotlin):
```kotlin
package com.anthropic.example

import com.anthropic.client.AnthropicClient
import com.anthropic.client.okhttp.AnthropicOkHttpClient
import com.anthropic.models.beta.skills.versions.VersionRetrieveParams
import com.anthropic.models.beta.skills.versions.VersionRetrieveResponse

fun main() {
    val client: AnthropicClient = AnthropicOkHttpClient.fromEnv()

    val params: VersionRetrieveParams = VersionRetrieveParams.builder()
        .skillId("skill_id")
        .version("version")
        .build()
    val version: VersionRetrieveResponse = client.beta().skills().versions().retrieve(params)
}
```

---

## Step 1: Use a Skill to create a file

**URL:** llms-txt#step-1:-use-a-skill-to-create-a-file

RESPONSE=$(curl https://api.anthropic.com/v1/messages \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -H "anthropic-beta: code-execution-2025-08-25,skills-2025-10-02" \
  -H "content-type: application/json" \
  -d '{
    "model": "claude-sonnet-4-5-20250929",
    "max_tokens": 4096,
    "container": {
      "skills": [
        {"type": "anthropic", "skill_id": "xlsx", "version": "latest"}
      ]
    },
    "messages": [{
      "role": "user",
      "content": "Create an Excel file with a simple budget spreadsheet"
    }],
    "tools": [{
      "type": "code_execution_20250825",
      "name": "code_execution"
    }]
  }')

---

## Create Skill (Beta) (Go)

**URL:** llms-txt#create-skill-(beta)-(go)

**Contents:**
- Create
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/go/beta/skills/create

`client.Beta.Skills.New(ctx, params) (*BetaSkillNewResponse, error)`

**post** `/v1/skills`

- `params BetaSkillNewParams`

- `DisplayTitle param.Field[string]`

Body param: Display title for the skill.

This is a human-readable label that is not included in the prompt sent to the model.

- `Files param.Field[[]Reader]`

Body param: Files to upload for the skill.

All files must be in the same top-level directory and must include a SKILL.md file at the root of that directory.

- `Betas param.Field[[]AnthropicBeta]`

Header param: Optional header to specify the beta version(s) you want to use.

- `type AnthropicBeta string`

- `const AnthropicBetaMessageBatches2024_09_24 AnthropicBeta = "message-batches-2024-09-24"`

- `const AnthropicBetaPromptCaching2024_07_31 AnthropicBeta = "prompt-caching-2024-07-31"`

- `const AnthropicBetaComputerUse2024_10_22 AnthropicBeta = "computer-use-2024-10-22"`

- `const AnthropicBetaComputerUse2025_01_24 AnthropicBeta = "computer-use-2025-01-24"`

- `const AnthropicBetaPDFs2024_09_25 AnthropicBeta = "pdfs-2024-09-25"`

- `const AnthropicBetaTokenCounting2024_11_01 AnthropicBeta = "token-counting-2024-11-01"`

- `const AnthropicBetaTokenEfficientTools2025_02_19 AnthropicBeta = "token-efficient-tools-2025-02-19"`

- `const AnthropicBetaOutput128k2025_02_19 AnthropicBeta = "output-128k-2025-02-19"`

- `const AnthropicBetaFilesAPI2025_04_14 AnthropicBeta = "files-api-2025-04-14"`

- `const AnthropicBetaMCPClient2025_04_04 AnthropicBeta = "mcp-client-2025-04-04"`

- `const AnthropicBetaMCPClient2025_11_20 AnthropicBeta = "mcp-client-2025-11-20"`

- `const AnthropicBetaDevFullThinking2025_05_14 AnthropicBeta = "dev-full-thinking-2025-05-14"`

- `const AnthropicBetaInterleavedThinking2025_05_14 AnthropicBeta = "interleaved-thinking-2025-05-14"`

- `const AnthropicBetaCodeExecution2025_05_22 AnthropicBeta = "code-execution-2025-05-22"`

- `const AnthropicBetaExtendedCacheTTL2025_04_11 AnthropicBeta = "extended-cache-ttl-2025-04-11"`

- `const AnthropicBetaContext1m2025_08_07 AnthropicBeta = "context-1m-2025-08-07"`

- `const AnthropicBetaContextManagement2025_06_27 AnthropicBeta = "context-management-2025-06-27"`

- `const AnthropicBetaModelContextWindowExceeded2025_08_26 AnthropicBeta = "model-context-window-exceeded-2025-08-26"`

- `const AnthropicBetaSkills2025_10_02 AnthropicBeta = "skills-2025-10-02"`

- `type BetaSkillNewResponse struct{…}`

Unique identifier for the skill.

The format and length of IDs may change over time.

ISO 8601 timestamp of when the skill was created.

- `DisplayTitle string`

Display title for the skill.

This is a human-readable label that is not included in the prompt sent to the model.

- `LatestVersion string`

The latest version identifier for the skill.

This represents the most recent version of the skill that has been created.

This may be one of the following values:

* `"custom"`: the skill was created by a user
    * `"anthropic"`: the skill was created by Anthropic

For Skills, this is always `"skill"`.

ISO 8601 timestamp of when the skill was last updated.

**Examples:**

Example 1 (go):
```go
package main

import (
  "context"
  "fmt"

  "github.com/anthropics/anthropic-sdk-go"
  "github.com/anthropics/anthropic-sdk-go/option"
)

func main() {
  client := anthropic.NewClient(
    option.WithAPIKey("my-anthropic-api-key"),
  )
  skill, err := client.Beta.Skills.New(context.TODO(), anthropic.BetaSkillNewParams{

  })
  if err != nil {
    panic(err.Error())
  }
  fmt.Printf("%+v\n", skill.ID)
}
```

---

## Get Skill Version (Beta) (Ruby)

**URL:** llms-txt#get-skill-version-(beta)-(ruby)

**Contents:**
- Retrieve
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/ruby/beta/skills/versions/retrieve

`beta.skills.versions.retrieve(version, **kwargs) -> VersionRetrieveResponse`

**get** `/v1/skills/{skill_id}/versions/{version}`

Unique identifier for the skill.

The format and length of IDs may change over time.

Version identifier for the skill.

Each version is identified by a Unix epoch timestamp (e.g., "1759178010641129").

- `anthropic_beta: Array[AnthropicBeta]`

Optional header to specify the beta version(s) you want to use.

- `:"message-batches-2024-09-24" | :"prompt-caching-2024-07-31" | :"computer-use-2024-10-22" | 16 more`

- `:"message-batches-2024-09-24"`

- `:"prompt-caching-2024-07-31"`

- `:"computer-use-2024-10-22"`

- `:"computer-use-2025-01-24"`

- `:"pdfs-2024-09-25"`

- `:"token-counting-2024-11-01"`

- `:"token-efficient-tools-2025-02-19"`

- `:"output-128k-2025-02-19"`

- `:"files-api-2025-04-14"`

- `:"mcp-client-2025-04-04"`

- `:"mcp-client-2025-11-20"`

- `:"dev-full-thinking-2025-05-14"`

- `:"interleaved-thinking-2025-05-14"`

- `:"code-execution-2025-05-22"`

- `:"extended-cache-ttl-2025-04-11"`

- `:"context-1m-2025-08-07"`

- `:"context-management-2025-06-27"`

- `:"model-context-window-exceeded-2025-08-26"`

- `:"skills-2025-10-02"`

- `class VersionRetrieveResponse`

Unique identifier for the skill version.

The format and length of IDs may change over time.

- `created_at: String`

ISO 8601 timestamp of when the skill version was created.

- `description: String`

Description of the skill version.

This is extracted from the SKILL.md file in the skill upload.

- `directory: String`

Directory name of the skill version.

This is the top-level directory name that was extracted from the uploaded files.

Human-readable name of the skill version.

This is extracted from the SKILL.md file in the skill upload.

Identifier for the skill that this version belongs to.

For Skill Versions, this is always `"skill_version"`.

Version identifier for the skill.

Each version is identified by a Unix epoch timestamp (e.g., "1759178010641129").

**Examples:**

Example 1 (ruby):
```ruby
require "anthropic"

anthropic = Anthropic::Client.new(api_key: "my-anthropic-api-key")

version = anthropic.beta.skills.versions.retrieve("version", skill_id: "skill_id")

puts(version)
```

---

## Create Skill Version (Beta) (Ruby)

**URL:** llms-txt#create-skill-version-(beta)-(ruby)

**Contents:**
- Create
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/ruby/beta/skills/versions/create

`beta.skills.versions.create(skill_id, **kwargs) -> VersionCreateResponse`

**post** `/v1/skills/{skill_id}/versions`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `files: Array[String]`

Files to upload for the skill.

All files must be in the same top-level directory and must include a SKILL.md file at the root of that directory.

- `anthropic_beta: Array[AnthropicBeta]`

Optional header to specify the beta version(s) you want to use.

- `:"message-batches-2024-09-24" | :"prompt-caching-2024-07-31" | :"computer-use-2024-10-22" | 16 more`

- `:"message-batches-2024-09-24"`

- `:"prompt-caching-2024-07-31"`

- `:"computer-use-2024-10-22"`

- `:"computer-use-2025-01-24"`

- `:"pdfs-2024-09-25"`

- `:"token-counting-2024-11-01"`

- `:"token-efficient-tools-2025-02-19"`

- `:"output-128k-2025-02-19"`

- `:"files-api-2025-04-14"`

- `:"mcp-client-2025-04-04"`

- `:"mcp-client-2025-11-20"`

- `:"dev-full-thinking-2025-05-14"`

- `:"interleaved-thinking-2025-05-14"`

- `:"code-execution-2025-05-22"`

- `:"extended-cache-ttl-2025-04-11"`

- `:"context-1m-2025-08-07"`

- `:"context-management-2025-06-27"`

- `:"model-context-window-exceeded-2025-08-26"`

- `:"skills-2025-10-02"`

- `class VersionCreateResponse`

Unique identifier for the skill version.

The format and length of IDs may change over time.

- `created_at: String`

ISO 8601 timestamp of when the skill version was created.

- `description: String`

Description of the skill version.

This is extracted from the SKILL.md file in the skill upload.

- `directory: String`

Directory name of the skill version.

This is the top-level directory name that was extracted from the uploaded files.

Human-readable name of the skill version.

This is extracted from the SKILL.md file in the skill upload.

Identifier for the skill that this version belongs to.

For Skill Versions, this is always `"skill_version"`.

Version identifier for the skill.

Each version is identified by a Unix epoch timestamp (e.g., "1759178010641129").

**Examples:**

Example 1 (ruby):
```ruby
require "anthropic"

anthropic = Anthropic::Client.new(api_key: "my-anthropic-api-key")

version = anthropic.beta.skills.versions.create("skill_id")

puts(version)
```

---

## List Skills (Beta)

**URL:** llms-txt#list-skills-(beta)

**Contents:**
- List
  - Query Parameters
  - Header Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/beta/skills/list

- `limit: optional number`

Number of results to return per page.

Maximum value is 100. Defaults to 20.

- `page: optional string`

Pagination token for fetching a specific page of results.

Pass the value from a previous response's `next_page` field to get the next page of results.

- `source: optional string`

Filter skills by source.

If provided, only skills from the specified source will be returned:

* `"custom"`: only return user-created skills
  * `"anthropic"`: only return Anthropic-created skills

### Header Parameters

- `"anthropic-beta": optional array of AnthropicBeta`

Optional header to specify the beta version(s) you want to use.

- `UnionMember0 = string`

- `UnionMember1 = "message-batches-2024-09-24" or "prompt-caching-2024-07-31" or "computer-use-2024-10-22" or 16 more`

- `"message-batches-2024-09-24"`

- `"prompt-caching-2024-07-31"`

- `"computer-use-2024-10-22"`

- `"computer-use-2025-01-24"`

- `"pdfs-2024-09-25"`

- `"token-counting-2024-11-01"`

- `"token-efficient-tools-2025-02-19"`

- `"output-128k-2025-02-19"`

- `"files-api-2025-04-14"`

- `"mcp-client-2025-04-04"`

- `"mcp-client-2025-11-20"`

- `"dev-full-thinking-2025-05-14"`

- `"interleaved-thinking-2025-05-14"`

- `"code-execution-2025-05-22"`

- `"extended-cache-ttl-2025-04-11"`

- `"context-1m-2025-08-07"`

- `"context-management-2025-06-27"`

- `"model-context-window-exceeded-2025-08-26"`

- `"skills-2025-10-02"`

- `data: array of object { id, created_at, display_title, 4 more }`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `created_at: string`

ISO 8601 timestamp of when the skill was created.

- `display_title: string`

Display title for the skill.

This is a human-readable label that is not included in the prompt sent to the model.

- `latest_version: string`

The latest version identifier for the skill.

This represents the most recent version of the skill that has been created.

This may be one of the following values:

* `"custom"`: the skill was created by a user
    * `"anthropic"`: the skill was created by Anthropic

For Skills, this is always `"skill"`.

- `updated_at: string`

ISO 8601 timestamp of when the skill was last updated.

- `has_more: boolean`

Whether there are more results available.

If `true`, there are additional results that can be fetched using the `next_page` token.

- `next_page: string`

Token for fetching the next page of results.

If `null`, there are no more results available. Pass this value to the `page_token` parameter in the next request to get the next page.

**Examples:**

Example 1 (http):
```http
curl https://api.anthropic.com/v1/skills \
    -H 'anthropic-version: 2023-06-01' \
    -H 'anthropic-beta: skills-2025-10-02' \
    -H "X-Api-Key: $ANTHROPIC_API_KEY"
```

---

## Create Skill Version (Beta) (Go)

**URL:** llms-txt#create-skill-version-(beta)-(go)

**Contents:**
- Create
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/go/beta/skills/versions/create

`client.Beta.Skills.Versions.New(ctx, skillID, params) (*BetaSkillVersionNewResponse, error)`

**post** `/v1/skills/{skill_id}/versions`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `params BetaSkillVersionNewParams`

- `Files param.Field[[]Reader]`

Body param: Files to upload for the skill.

All files must be in the same top-level directory and must include a SKILL.md file at the root of that directory.

- `Betas param.Field[[]AnthropicBeta]`

Header param: Optional header to specify the beta version(s) you want to use.

- `type AnthropicBeta string`

- `const AnthropicBetaMessageBatches2024_09_24 AnthropicBeta = "message-batches-2024-09-24"`

- `const AnthropicBetaPromptCaching2024_07_31 AnthropicBeta = "prompt-caching-2024-07-31"`

- `const AnthropicBetaComputerUse2024_10_22 AnthropicBeta = "computer-use-2024-10-22"`

- `const AnthropicBetaComputerUse2025_01_24 AnthropicBeta = "computer-use-2025-01-24"`

- `const AnthropicBetaPDFs2024_09_25 AnthropicBeta = "pdfs-2024-09-25"`

- `const AnthropicBetaTokenCounting2024_11_01 AnthropicBeta = "token-counting-2024-11-01"`

- `const AnthropicBetaTokenEfficientTools2025_02_19 AnthropicBeta = "token-efficient-tools-2025-02-19"`

- `const AnthropicBetaOutput128k2025_02_19 AnthropicBeta = "output-128k-2025-02-19"`

- `const AnthropicBetaFilesAPI2025_04_14 AnthropicBeta = "files-api-2025-04-14"`

- `const AnthropicBetaMCPClient2025_04_04 AnthropicBeta = "mcp-client-2025-04-04"`

- `const AnthropicBetaMCPClient2025_11_20 AnthropicBeta = "mcp-client-2025-11-20"`

- `const AnthropicBetaDevFullThinking2025_05_14 AnthropicBeta = "dev-full-thinking-2025-05-14"`

- `const AnthropicBetaInterleavedThinking2025_05_14 AnthropicBeta = "interleaved-thinking-2025-05-14"`

- `const AnthropicBetaCodeExecution2025_05_22 AnthropicBeta = "code-execution-2025-05-22"`

- `const AnthropicBetaExtendedCacheTTL2025_04_11 AnthropicBeta = "extended-cache-ttl-2025-04-11"`

- `const AnthropicBetaContext1m2025_08_07 AnthropicBeta = "context-1m-2025-08-07"`

- `const AnthropicBetaContextManagement2025_06_27 AnthropicBeta = "context-management-2025-06-27"`

- `const AnthropicBetaModelContextWindowExceeded2025_08_26 AnthropicBeta = "model-context-window-exceeded-2025-08-26"`

- `const AnthropicBetaSkills2025_10_02 AnthropicBeta = "skills-2025-10-02"`

- `type BetaSkillVersionNewResponse struct{…}`

Unique identifier for the skill version.

The format and length of IDs may change over time.

ISO 8601 timestamp of when the skill version was created.

- `Description string`

Description of the skill version.

This is extracted from the SKILL.md file in the skill upload.

Directory name of the skill version.

This is the top-level directory name that was extracted from the uploaded files.

Human-readable name of the skill version.

This is extracted from the SKILL.md file in the skill upload.

Identifier for the skill that this version belongs to.

For Skill Versions, this is always `"skill_version"`.

Version identifier for the skill.

Each version is identified by a Unix epoch timestamp (e.g., "1759178010641129").

**Examples:**

Example 1 (go):
```go
package main

import (
  "context"
  "fmt"

  "github.com/anthropics/anthropic-sdk-go"
  "github.com/anthropics/anthropic-sdk-go/option"
)

func main() {
  client := anthropic.NewClient(
    option.WithAPIKey("my-anthropic-api-key"),
  )
  version, err := client.Beta.Skills.Versions.New(
    context.TODO(),
    "skill_id",
    anthropic.BetaSkillVersionNewParams{

    },
  )
  if err != nil {
    panic(err.Error())
  }
  fmt.Printf("%+v\n", version.ID)
}
```

---

## SKILL.md

**URL:** llms-txt#skill.md

**Contents:**
  - Structure longer reference files with table of contents

**Basic usage**: [instructions in SKILL.md]
**Advanced features**: See [advanced.md](advanced.md)
**API reference**: See [reference.md](reference.md)
**Examples**: See [examples.md](examples.md)
markdown

**Examples:**

Example 1 (unknown):
```unknown
### Structure longer reference files with table of contents

For reference files longer than 100 lines, include a table of contents at the top. This ensures Claude can see the full scope of available information even when previewing with partial reads.

**Example**:
```

---

## Delete Skill Version (Beta) (Go)

**URL:** llms-txt#delete-skill-version-(beta)-(go)

**Contents:**
- Delete
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/go/beta/skills/versions/delete

`client.Beta.Skills.Versions.Delete(ctx, version, params) (*BetaSkillVersionDeleteResponse, error)`

**delete** `/v1/skills/{skill_id}/versions/{version}`

Version identifier for the skill.

Each version is identified by a Unix epoch timestamp (e.g., "1759178010641129").

- `params BetaSkillVersionDeleteParams`

- `SkillID param.Field[string]`

Path param: Unique identifier for the skill.

The format and length of IDs may change over time.

- `Betas param.Field[[]AnthropicBeta]`

Header param: Optional header to specify the beta version(s) you want to use.

- `type AnthropicBeta string`

- `const AnthropicBetaMessageBatches2024_09_24 AnthropicBeta = "message-batches-2024-09-24"`

- `const AnthropicBetaPromptCaching2024_07_31 AnthropicBeta = "prompt-caching-2024-07-31"`

- `const AnthropicBetaComputerUse2024_10_22 AnthropicBeta = "computer-use-2024-10-22"`

- `const AnthropicBetaComputerUse2025_01_24 AnthropicBeta = "computer-use-2025-01-24"`

- `const AnthropicBetaPDFs2024_09_25 AnthropicBeta = "pdfs-2024-09-25"`

- `const AnthropicBetaTokenCounting2024_11_01 AnthropicBeta = "token-counting-2024-11-01"`

- `const AnthropicBetaTokenEfficientTools2025_02_19 AnthropicBeta = "token-efficient-tools-2025-02-19"`

- `const AnthropicBetaOutput128k2025_02_19 AnthropicBeta = "output-128k-2025-02-19"`

- `const AnthropicBetaFilesAPI2025_04_14 AnthropicBeta = "files-api-2025-04-14"`

- `const AnthropicBetaMCPClient2025_04_04 AnthropicBeta = "mcp-client-2025-04-04"`

- `const AnthropicBetaMCPClient2025_11_20 AnthropicBeta = "mcp-client-2025-11-20"`

- `const AnthropicBetaDevFullThinking2025_05_14 AnthropicBeta = "dev-full-thinking-2025-05-14"`

- `const AnthropicBetaInterleavedThinking2025_05_14 AnthropicBeta = "interleaved-thinking-2025-05-14"`

- `const AnthropicBetaCodeExecution2025_05_22 AnthropicBeta = "code-execution-2025-05-22"`

- `const AnthropicBetaExtendedCacheTTL2025_04_11 AnthropicBeta = "extended-cache-ttl-2025-04-11"`

- `const AnthropicBetaContext1m2025_08_07 AnthropicBeta = "context-1m-2025-08-07"`

- `const AnthropicBetaContextManagement2025_06_27 AnthropicBeta = "context-management-2025-06-27"`

- `const AnthropicBetaModelContextWindowExceeded2025_08_26 AnthropicBeta = "model-context-window-exceeded-2025-08-26"`

- `const AnthropicBetaSkills2025_10_02 AnthropicBeta = "skills-2025-10-02"`

- `type BetaSkillVersionDeleteResponse struct{…}`

Version identifier for the skill.

Each version is identified by a Unix epoch timestamp (e.g., "1759178010641129").

For Skill Versions, this is always `"skill_version_deleted"`.

**Examples:**

Example 1 (go):
```go
package main

import (
  "context"
  "fmt"

  "github.com/anthropics/anthropic-sdk-go"
  "github.com/anthropics/anthropic-sdk-go/option"
)

func main() {
  client := anthropic.NewClient(
    option.WithAPIKey("my-anthropic-api-key"),
  )
  version, err := client.Beta.Skills.Versions.Delete(
    context.TODO(),
    "version",
    anthropic.BetaSkillVersionDeleteParams{
      SkillID: "skill_id",
    },
  )
  if err != nil {
    panic(err.Error())
  }
  fmt.Printf("%+v\n", version.ID)
}
```

---

## Get Skill Version (Beta) (TypeScript)

**URL:** llms-txt#get-skill-version-(beta)-(typescript)

**Contents:**
- Retrieve
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/typescript/beta/skills/versions/retrieve

`client.beta.skills.versions.retrieve(stringversion, VersionRetrieveParamsparams, RequestOptionsoptions?): VersionRetrieveResponse`

**get** `/v1/skills/{skill_id}/versions/{version}`

Version identifier for the skill.

Each version is identified by a Unix epoch timestamp (e.g., "1759178010641129").

- `params: VersionRetrieveParams`

Path param: Unique identifier for the skill.

The format and length of IDs may change over time.

- `betas?: Array<AnthropicBeta>`

Header param: Optional header to specify the beta version(s) you want to use.

- `"message-batches-2024-09-24" | "prompt-caching-2024-07-31" | "computer-use-2024-10-22" | 16 more`

- `"message-batches-2024-09-24"`

- `"prompt-caching-2024-07-31"`

- `"computer-use-2024-10-22"`

- `"computer-use-2025-01-24"`

- `"pdfs-2024-09-25"`

- `"token-counting-2024-11-01"`

- `"token-efficient-tools-2025-02-19"`

- `"output-128k-2025-02-19"`

- `"files-api-2025-04-14"`

- `"mcp-client-2025-04-04"`

- `"mcp-client-2025-11-20"`

- `"dev-full-thinking-2025-05-14"`

- `"interleaved-thinking-2025-05-14"`

- `"code-execution-2025-05-22"`

- `"extended-cache-ttl-2025-04-11"`

- `"context-1m-2025-08-07"`

- `"context-management-2025-06-27"`

- `"model-context-window-exceeded-2025-08-26"`

- `"skills-2025-10-02"`

- `VersionRetrieveResponse`

Unique identifier for the skill version.

The format and length of IDs may change over time.

- `created_at: string`

ISO 8601 timestamp of when the skill version was created.

- `description: string`

Description of the skill version.

This is extracted from the SKILL.md file in the skill upload.

- `directory: string`

Directory name of the skill version.

This is the top-level directory name that was extracted from the uploaded files.

Human-readable name of the skill version.

This is extracted from the SKILL.md file in the skill upload.

Identifier for the skill that this version belongs to.

For Skill Versions, this is always `"skill_version"`.

Version identifier for the skill.

Each version is identified by a Unix epoch timestamp (e.g., "1759178010641129").

**Examples:**

Example 1 (typescript):
```typescript
import Anthropic from '@anthropic-ai/sdk';

const client = new Anthropic({
  apiKey: process.env['ANTHROPIC_API_KEY'], // This is the default and can be omitted
});

const version = await client.beta.skills.versions.retrieve('version', { skill_id: 'skill_id' });

console.log(version.id);
```

---

## List Skill Versions (Beta)

**URL:** llms-txt#list-skill-versions-(beta)

**Contents:**
- List
  - Path Parameters
  - Query Parameters
  - Header Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/beta/skills/versions/list

**get** `/v1/skills/{skill_id}/versions`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `limit: optional number`

Number of items to return per page.

Defaults to `20`. Ranges from `1` to `1000`.

- `page: optional string`

Optionally set to the `next_page` token from the previous response.

### Header Parameters

- `"anthropic-beta": optional array of AnthropicBeta`

Optional header to specify the beta version(s) you want to use.

- `UnionMember0 = string`

- `UnionMember1 = "message-batches-2024-09-24" or "prompt-caching-2024-07-31" or "computer-use-2024-10-22" or 16 more`

- `"message-batches-2024-09-24"`

- `"prompt-caching-2024-07-31"`

- `"computer-use-2024-10-22"`

- `"computer-use-2025-01-24"`

- `"pdfs-2024-09-25"`

- `"token-counting-2024-11-01"`

- `"token-efficient-tools-2025-02-19"`

- `"output-128k-2025-02-19"`

- `"files-api-2025-04-14"`

- `"mcp-client-2025-04-04"`

- `"mcp-client-2025-11-20"`

- `"dev-full-thinking-2025-05-14"`

- `"interleaved-thinking-2025-05-14"`

- `"code-execution-2025-05-22"`

- `"extended-cache-ttl-2025-04-11"`

- `"context-1m-2025-08-07"`

- `"context-management-2025-06-27"`

- `"model-context-window-exceeded-2025-08-26"`

- `"skills-2025-10-02"`

- `data: array of object { id, created_at, description, 5 more }`

List of skill versions.

Unique identifier for the skill version.

The format and length of IDs may change over time.

- `created_at: string`

ISO 8601 timestamp of when the skill version was created.

- `description: string`

Description of the skill version.

This is extracted from the SKILL.md file in the skill upload.

- `directory: string`

Directory name of the skill version.

This is the top-level directory name that was extracted from the uploaded files.

Human-readable name of the skill version.

This is extracted from the SKILL.md file in the skill upload.

Identifier for the skill that this version belongs to.

For Skill Versions, this is always `"skill_version"`.

Version identifier for the skill.

Each version is identified by a Unix epoch timestamp (e.g., "1759178010641129").

- `has_more: boolean`

Indicates if there are more results in the requested page direction.

- `next_page: string`

Token to provide in as `page` in the subsequent request to retrieve the next page of data.

**Examples:**

Example 1 (http):
```http
curl https://api.anthropic.com/v1/skills/$SKILL_ID/versions \
    -H 'anthropic-version: 2023-06-01' \
    -H 'anthropic-beta: skills-2025-10-02' \
    -H "X-Api-Key: $ANTHROPIC_API_KEY"
```

---

## List Skill Versions (Beta) (TypeScript)

**URL:** llms-txt#list-skill-versions-(beta)-(typescript)

**Contents:**
- List
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/typescript/beta/skills/versions/list

`client.beta.skills.versions.list(stringskillID, VersionListParamsparams?, RequestOptionsoptions?): PageCursor<VersionListResponse>`

**get** `/v1/skills/{skill_id}/versions`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `params: VersionListParams`

- `limit?: number | null`

Query param: Number of items to return per page.

Defaults to `20`. Ranges from `1` to `1000`.

- `page?: string | null`

Query param: Optionally set to the `next_page` token from the previous response.

- `betas?: Array<AnthropicBeta>`

Header param: Optional header to specify the beta version(s) you want to use.

- `"message-batches-2024-09-24" | "prompt-caching-2024-07-31" | "computer-use-2024-10-22" | 16 more`

- `"message-batches-2024-09-24"`

- `"prompt-caching-2024-07-31"`

- `"computer-use-2024-10-22"`

- `"computer-use-2025-01-24"`

- `"pdfs-2024-09-25"`

- `"token-counting-2024-11-01"`

- `"token-efficient-tools-2025-02-19"`

- `"output-128k-2025-02-19"`

- `"files-api-2025-04-14"`

- `"mcp-client-2025-04-04"`

- `"mcp-client-2025-11-20"`

- `"dev-full-thinking-2025-05-14"`

- `"interleaved-thinking-2025-05-14"`

- `"code-execution-2025-05-22"`

- `"extended-cache-ttl-2025-04-11"`

- `"context-1m-2025-08-07"`

- `"context-management-2025-06-27"`

- `"model-context-window-exceeded-2025-08-26"`

- `"skills-2025-10-02"`

- `VersionListResponse`

Unique identifier for the skill version.

The format and length of IDs may change over time.

- `created_at: string`

ISO 8601 timestamp of when the skill version was created.

- `description: string`

Description of the skill version.

This is extracted from the SKILL.md file in the skill upload.

- `directory: string`

Directory name of the skill version.

This is the top-level directory name that was extracted from the uploaded files.

Human-readable name of the skill version.

This is extracted from the SKILL.md file in the skill upload.

Identifier for the skill that this version belongs to.

For Skill Versions, this is always `"skill_version"`.

Version identifier for the skill.

Each version is identified by a Unix epoch timestamp (e.g., "1759178010641129").

**Examples:**

Example 1 (typescript):
```typescript
import Anthropic from '@anthropic-ai/sdk';

const client = new Anthropic({
  apiKey: process.env['ANTHROPIC_API_KEY'], // This is the default and can be omitted
});

// Automatically fetches more pages as needed.
for await (const versionListResponse of client.beta.skills.versions.list('skill_id')) {
  console.log(versionListResponse.id);
}
```

---

## Get Skill (Beta) (Python)

**URL:** llms-txt#get-skill-(beta)-(python)

**Contents:**
- Retrieve
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/python/beta/skills/retrieve

`beta.skills.retrieve(strskill_id, SkillRetrieveParams**kwargs)  -> SkillRetrieveResponse`

**get** `/v1/skills/{skill_id}`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `betas: Optional[List[AnthropicBetaParam]]`

Optional header to specify the beta version(s) you want to use.

- `UnionMember0 = str`

- `UnionMember1 = Literal["message-batches-2024-09-24", "prompt-caching-2024-07-31", "computer-use-2024-10-22", 16 more]`

- `"message-batches-2024-09-24"`

- `"prompt-caching-2024-07-31"`

- `"computer-use-2024-10-22"`

- `"computer-use-2025-01-24"`

- `"pdfs-2024-09-25"`

- `"token-counting-2024-11-01"`

- `"token-efficient-tools-2025-02-19"`

- `"output-128k-2025-02-19"`

- `"files-api-2025-04-14"`

- `"mcp-client-2025-04-04"`

- `"mcp-client-2025-11-20"`

- `"dev-full-thinking-2025-05-14"`

- `"interleaved-thinking-2025-05-14"`

- `"code-execution-2025-05-22"`

- `"extended-cache-ttl-2025-04-11"`

- `"context-1m-2025-08-07"`

- `"context-management-2025-06-27"`

- `"model-context-window-exceeded-2025-08-26"`

- `"skills-2025-10-02"`

- `class SkillRetrieveResponse: …`

Unique identifier for the skill.

The format and length of IDs may change over time.

ISO 8601 timestamp of when the skill was created.

- `display_title: Optional[str]`

Display title for the skill.

This is a human-readable label that is not included in the prompt sent to the model.

- `latest_version: Optional[str]`

The latest version identifier for the skill.

This represents the most recent version of the skill that has been created.

This may be one of the following values:

* `"custom"`: the skill was created by a user
    * `"anthropic"`: the skill was created by Anthropic

For Skills, this is always `"skill"`.

ISO 8601 timestamp of when the skill was last updated.

**Examples:**

Example 1 (python):
```python
import os
from anthropic import Anthropic

client = Anthropic(
    api_key=os.environ.get("ANTHROPIC_API_KEY"),  # This is the default and can be omitted
)
skill = client.beta.skills.retrieve(
    skill_id="skill_id",
)
print(skill.id)
```

---

## Get Skill Version (Beta)

**URL:** llms-txt#get-skill-version-(beta)

**Contents:**
- Retrieve
  - Path Parameters
  - Header Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/beta/skills/versions/retrieve

**get** `/v1/skills/{skill_id}/versions/{version}`

Unique identifier for the skill.

The format and length of IDs may change over time.

Version identifier for the skill.

Each version is identified by a Unix epoch timestamp (e.g., "1759178010641129").

### Header Parameters

- `"anthropic-beta": optional array of AnthropicBeta`

Optional header to specify the beta version(s) you want to use.

- `UnionMember0 = string`

- `UnionMember1 = "message-batches-2024-09-24" or "prompt-caching-2024-07-31" or "computer-use-2024-10-22" or 16 more`

- `"message-batches-2024-09-24"`

- `"prompt-caching-2024-07-31"`

- `"computer-use-2024-10-22"`

- `"computer-use-2025-01-24"`

- `"pdfs-2024-09-25"`

- `"token-counting-2024-11-01"`

- `"token-efficient-tools-2025-02-19"`

- `"output-128k-2025-02-19"`

- `"files-api-2025-04-14"`

- `"mcp-client-2025-04-04"`

- `"mcp-client-2025-11-20"`

- `"dev-full-thinking-2025-05-14"`

- `"interleaved-thinking-2025-05-14"`

- `"code-execution-2025-05-22"`

- `"extended-cache-ttl-2025-04-11"`

- `"context-1m-2025-08-07"`

- `"context-management-2025-06-27"`

- `"model-context-window-exceeded-2025-08-26"`

- `"skills-2025-10-02"`

Unique identifier for the skill version.

The format and length of IDs may change over time.

- `created_at: string`

ISO 8601 timestamp of when the skill version was created.

- `description: string`

Description of the skill version.

This is extracted from the SKILL.md file in the skill upload.

- `directory: string`

Directory name of the skill version.

This is the top-level directory name that was extracted from the uploaded files.

Human-readable name of the skill version.

This is extracted from the SKILL.md file in the skill upload.

Identifier for the skill that this version belongs to.

For Skill Versions, this is always `"skill_version"`.

Version identifier for the skill.

Each version is identified by a Unix epoch timestamp (e.g., "1759178010641129").

**Examples:**

Example 1 (http):
```http
curl https://api.anthropic.com/v1/skills/$SKILL_ID/versions/$VERSION \
    -H 'anthropic-version: 2023-06-01' \
    -H 'anthropic-beta: skills-2025-10-02' \
    -H "X-Api-Key: $ANTHROPIC_API_KEY"
```

---

## Get Skill (Beta) (Go)

**URL:** llms-txt#get-skill-(beta)-(go)

**Contents:**
- Retrieve
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/go/beta/skills/retrieve

`client.Beta.Skills.Get(ctx, skillID, query) (*BetaSkillGetResponse, error)`

**get** `/v1/skills/{skill_id}`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `query BetaSkillGetParams`

- `Betas param.Field[[]AnthropicBeta]`

Optional header to specify the beta version(s) you want to use.

- `type AnthropicBeta string`

- `const AnthropicBetaMessageBatches2024_09_24 AnthropicBeta = "message-batches-2024-09-24"`

- `const AnthropicBetaPromptCaching2024_07_31 AnthropicBeta = "prompt-caching-2024-07-31"`

- `const AnthropicBetaComputerUse2024_10_22 AnthropicBeta = "computer-use-2024-10-22"`

- `const AnthropicBetaComputerUse2025_01_24 AnthropicBeta = "computer-use-2025-01-24"`

- `const AnthropicBetaPDFs2024_09_25 AnthropicBeta = "pdfs-2024-09-25"`

- `const AnthropicBetaTokenCounting2024_11_01 AnthropicBeta = "token-counting-2024-11-01"`

- `const AnthropicBetaTokenEfficientTools2025_02_19 AnthropicBeta = "token-efficient-tools-2025-02-19"`

- `const AnthropicBetaOutput128k2025_02_19 AnthropicBeta = "output-128k-2025-02-19"`

- `const AnthropicBetaFilesAPI2025_04_14 AnthropicBeta = "files-api-2025-04-14"`

- `const AnthropicBetaMCPClient2025_04_04 AnthropicBeta = "mcp-client-2025-04-04"`

- `const AnthropicBetaMCPClient2025_11_20 AnthropicBeta = "mcp-client-2025-11-20"`

- `const AnthropicBetaDevFullThinking2025_05_14 AnthropicBeta = "dev-full-thinking-2025-05-14"`

- `const AnthropicBetaInterleavedThinking2025_05_14 AnthropicBeta = "interleaved-thinking-2025-05-14"`

- `const AnthropicBetaCodeExecution2025_05_22 AnthropicBeta = "code-execution-2025-05-22"`

- `const AnthropicBetaExtendedCacheTTL2025_04_11 AnthropicBeta = "extended-cache-ttl-2025-04-11"`

- `const AnthropicBetaContext1m2025_08_07 AnthropicBeta = "context-1m-2025-08-07"`

- `const AnthropicBetaContextManagement2025_06_27 AnthropicBeta = "context-management-2025-06-27"`

- `const AnthropicBetaModelContextWindowExceeded2025_08_26 AnthropicBeta = "model-context-window-exceeded-2025-08-26"`

- `const AnthropicBetaSkills2025_10_02 AnthropicBeta = "skills-2025-10-02"`

- `type BetaSkillGetResponse struct{…}`

Unique identifier for the skill.

The format and length of IDs may change over time.

ISO 8601 timestamp of when the skill was created.

- `DisplayTitle string`

Display title for the skill.

This is a human-readable label that is not included in the prompt sent to the model.

- `LatestVersion string`

The latest version identifier for the skill.

This represents the most recent version of the skill that has been created.

This may be one of the following values:

* `"custom"`: the skill was created by a user
    * `"anthropic"`: the skill was created by Anthropic

For Skills, this is always `"skill"`.

ISO 8601 timestamp of when the skill was last updated.

**Examples:**

Example 1 (go):
```go
package main

import (
  "context"
  "fmt"

  "github.com/anthropics/anthropic-sdk-go"
  "github.com/anthropics/anthropic-sdk-go/option"
)

func main() {
  client := anthropic.NewClient(
    option.WithAPIKey("my-anthropic-api-key"),
  )
  skill, err := client.Beta.Skills.Get(
    context.TODO(),
    "skill_id",
    anthropic.BetaSkillGetParams{

    },
  )
  if err != nil {
    panic(err.Error())
  }
  fmt.Printf("%+v\n", skill.ID)
}
```

---

## Delete Skill (Beta) (Ruby)

**URL:** llms-txt#delete-skill-(beta)-(ruby)

**Contents:**
- Delete
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/ruby/beta/skills/delete

`beta.skills.delete(skill_id, **kwargs) -> SkillDeleteResponse`

**delete** `/v1/skills/{skill_id}`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `anthropic_beta: Array[AnthropicBeta]`

Optional header to specify the beta version(s) you want to use.

- `:"message-batches-2024-09-24" | :"prompt-caching-2024-07-31" | :"computer-use-2024-10-22" | 16 more`

- `:"message-batches-2024-09-24"`

- `:"prompt-caching-2024-07-31"`

- `:"computer-use-2024-10-22"`

- `:"computer-use-2025-01-24"`

- `:"pdfs-2024-09-25"`

- `:"token-counting-2024-11-01"`

- `:"token-efficient-tools-2025-02-19"`

- `:"output-128k-2025-02-19"`

- `:"files-api-2025-04-14"`

- `:"mcp-client-2025-04-04"`

- `:"mcp-client-2025-11-20"`

- `:"dev-full-thinking-2025-05-14"`

- `:"interleaved-thinking-2025-05-14"`

- `:"code-execution-2025-05-22"`

- `:"extended-cache-ttl-2025-04-11"`

- `:"context-1m-2025-08-07"`

- `:"context-management-2025-06-27"`

- `:"model-context-window-exceeded-2025-08-26"`

- `:"skills-2025-10-02"`

- `class SkillDeleteResponse`

Unique identifier for the skill.

The format and length of IDs may change over time.

For Skills, this is always `"skill_deleted"`.

**Examples:**

Example 1 (ruby):
```ruby
require "anthropic"

anthropic = Anthropic::Client.new(api_key: "my-anthropic-api-key")

skill = anthropic.beta.skills.delete("skill_id")

puts(skill)
```

---

## Delete Skill Version (Beta) (Kotlin)

**URL:** llms-txt#delete-skill-version-(beta)-(kotlin)

**Contents:**
- Delete
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/kotlin/beta/skills/versions/delete

`beta().skills().versions().delete(VersionDeleteParamsparams, RequestOptionsrequestOptions = RequestOptions.none()) : VersionDeleteResponse`

**delete** `/v1/skills/{skill_id}/versions/{version}`

- `params: VersionDeleteParams`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `version: Optional<String>`

Version identifier for the skill.

Each version is identified by a Unix epoch timestamp (e.g., "1759178010641129").

- `betas: Optional<List<AnthropicBeta>>`

Optional header to specify the beta version(s) you want to use.

- `MESSAGE_BATCHES_2024_09_24("message-batches-2024-09-24")`

- `PROMPT_CACHING_2024_07_31("prompt-caching-2024-07-31")`

- `COMPUTER_USE_2024_10_22("computer-use-2024-10-22")`

- `COMPUTER_USE_2025_01_24("computer-use-2025-01-24")`

- `PDFS_2024_09_25("pdfs-2024-09-25")`

- `TOKEN_COUNTING_2024_11_01("token-counting-2024-11-01")`

- `TOKEN_EFFICIENT_TOOLS_2025_02_19("token-efficient-tools-2025-02-19")`

- `OUTPUT_128K_2025_02_19("output-128k-2025-02-19")`

- `FILES_API_2025_04_14("files-api-2025-04-14")`

- `MCP_CLIENT_2025_04_04("mcp-client-2025-04-04")`

- `MCP_CLIENT_2025_11_20("mcp-client-2025-11-20")`

- `DEV_FULL_THINKING_2025_05_14("dev-full-thinking-2025-05-14")`

- `INTERLEAVED_THINKING_2025_05_14("interleaved-thinking-2025-05-14")`

- `CODE_EXECUTION_2025_05_22("code-execution-2025-05-22")`

- `EXTENDED_CACHE_TTL_2025_04_11("extended-cache-ttl-2025-04-11")`

- `CONTEXT_1M_2025_08_07("context-1m-2025-08-07")`

- `CONTEXT_MANAGEMENT_2025_06_27("context-management-2025-06-27")`

- `MODEL_CONTEXT_WINDOW_EXCEEDED_2025_08_26("model-context-window-exceeded-2025-08-26")`

- `SKILLS_2025_10_02("skills-2025-10-02")`

- `class VersionDeleteResponse:`

Version identifier for the skill.

Each version is identified by a Unix epoch timestamp (e.g., "1759178010641129").

For Skill Versions, this is always `"skill_version_deleted"`.

**Examples:**

Example 1 (kotlin):
```kotlin
package com.anthropic.example

import com.anthropic.client.AnthropicClient
import com.anthropic.client.okhttp.AnthropicOkHttpClient
import com.anthropic.models.beta.skills.versions.VersionDeleteParams
import com.anthropic.models.beta.skills.versions.VersionDeleteResponse

fun main() {
    val client: AnthropicClient = AnthropicOkHttpClient.fromEnv()

    val params: VersionDeleteParams = VersionDeleteParams.builder()
        .skillId("skill_id")
        .version("version")
        .build()
    val version: VersionDeleteResponse = client.beta().skills().versions().delete(params)
}
```

---

## Skills (Beta) (Python)

**URL:** llms-txt#skills-(beta)-(python)

URL: https://platform.claude.com/docs/en/api/python/beta/skills

---

## List Skill Versions (Beta) (Kotlin)

**URL:** llms-txt#list-skill-versions-(beta)-(kotlin)

**Contents:**
- List
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/kotlin/beta/skills/versions/list

`beta().skills().versions().list(VersionListParamsparams = VersionListParams.none(), RequestOptionsrequestOptions = RequestOptions.none()) : VersionListPage`

**get** `/v1/skills/{skill_id}/versions`

- `params: VersionListParams`

- `skillId: Optional<String>`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `limit: Optional<Long>`

Number of items to return per page.

Defaults to `20`. Ranges from `1` to `1000`.

- `page: Optional<String>`

Optionally set to the `next_page` token from the previous response.

- `betas: Optional<List<AnthropicBeta>>`

Optional header to specify the beta version(s) you want to use.

- `MESSAGE_BATCHES_2024_09_24("message-batches-2024-09-24")`

- `PROMPT_CACHING_2024_07_31("prompt-caching-2024-07-31")`

- `COMPUTER_USE_2024_10_22("computer-use-2024-10-22")`

- `COMPUTER_USE_2025_01_24("computer-use-2025-01-24")`

- `PDFS_2024_09_25("pdfs-2024-09-25")`

- `TOKEN_COUNTING_2024_11_01("token-counting-2024-11-01")`

- `TOKEN_EFFICIENT_TOOLS_2025_02_19("token-efficient-tools-2025-02-19")`

- `OUTPUT_128K_2025_02_19("output-128k-2025-02-19")`

- `FILES_API_2025_04_14("files-api-2025-04-14")`

- `MCP_CLIENT_2025_04_04("mcp-client-2025-04-04")`

- `MCP_CLIENT_2025_11_20("mcp-client-2025-11-20")`

- `DEV_FULL_THINKING_2025_05_14("dev-full-thinking-2025-05-14")`

- `INTERLEAVED_THINKING_2025_05_14("interleaved-thinking-2025-05-14")`

- `CODE_EXECUTION_2025_05_22("code-execution-2025-05-22")`

- `EXTENDED_CACHE_TTL_2025_04_11("extended-cache-ttl-2025-04-11")`

- `CONTEXT_1M_2025_08_07("context-1m-2025-08-07")`

- `CONTEXT_MANAGEMENT_2025_06_27("context-management-2025-06-27")`

- `MODEL_CONTEXT_WINDOW_EXCEEDED_2025_08_26("model-context-window-exceeded-2025-08-26")`

- `SKILLS_2025_10_02("skills-2025-10-02")`

- `class VersionListResponse:`

Unique identifier for the skill version.

The format and length of IDs may change over time.

- `createdAt: String`

ISO 8601 timestamp of when the skill version was created.

- `description: String`

Description of the skill version.

This is extracted from the SKILL.md file in the skill upload.

- `directory: String`

Directory name of the skill version.

This is the top-level directory name that was extracted from the uploaded files.

Human-readable name of the skill version.

This is extracted from the SKILL.md file in the skill upload.

Identifier for the skill that this version belongs to.

For Skill Versions, this is always `"skill_version"`.

Version identifier for the skill.

Each version is identified by a Unix epoch timestamp (e.g., "1759178010641129").

**Examples:**

Example 1 (kotlin):
```kotlin
package com.anthropic.example

import com.anthropic.client.AnthropicClient
import com.anthropic.client.okhttp.AnthropicOkHttpClient
import com.anthropic.models.beta.skills.versions.VersionListPage
import com.anthropic.models.beta.skills.versions.VersionListParams

fun main() {
    val client: AnthropicClient = AnthropicOkHttpClient.fromEnv()

    val page: VersionListPage = client.beta().skills().versions().list("skill_id")
}
```

---

## List Skills (Beta) (Go)

**URL:** llms-txt#list-skills-(beta)-(go)

**Contents:**
- List
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/go/beta/skills/list

`client.Beta.Skills.List(ctx, params) (*PageCursor[BetaSkillListResponse], error)`

- `params BetaSkillListParams`

- `Limit param.Field[int64]`

Query param: Number of results to return per page.

Maximum value is 100. Defaults to 20.

- `Page param.Field[string]`

Query param: Pagination token for fetching a specific page of results.

Pass the value from a previous response's `next_page` field to get the next page of results.

- `Source param.Field[string]`

Query param: Filter skills by source.

If provided, only skills from the specified source will be returned:

* `"custom"`: only return user-created skills
    * `"anthropic"`: only return Anthropic-created skills

- `Betas param.Field[[]AnthropicBeta]`

Header param: Optional header to specify the beta version(s) you want to use.

- `type AnthropicBeta string`

- `const AnthropicBetaMessageBatches2024_09_24 AnthropicBeta = "message-batches-2024-09-24"`

- `const AnthropicBetaPromptCaching2024_07_31 AnthropicBeta = "prompt-caching-2024-07-31"`

- `const AnthropicBetaComputerUse2024_10_22 AnthropicBeta = "computer-use-2024-10-22"`

- `const AnthropicBetaComputerUse2025_01_24 AnthropicBeta = "computer-use-2025-01-24"`

- `const AnthropicBetaPDFs2024_09_25 AnthropicBeta = "pdfs-2024-09-25"`

- `const AnthropicBetaTokenCounting2024_11_01 AnthropicBeta = "token-counting-2024-11-01"`

- `const AnthropicBetaTokenEfficientTools2025_02_19 AnthropicBeta = "token-efficient-tools-2025-02-19"`

- `const AnthropicBetaOutput128k2025_02_19 AnthropicBeta = "output-128k-2025-02-19"`

- `const AnthropicBetaFilesAPI2025_04_14 AnthropicBeta = "files-api-2025-04-14"`

- `const AnthropicBetaMCPClient2025_04_04 AnthropicBeta = "mcp-client-2025-04-04"`

- `const AnthropicBetaMCPClient2025_11_20 AnthropicBeta = "mcp-client-2025-11-20"`

- `const AnthropicBetaDevFullThinking2025_05_14 AnthropicBeta = "dev-full-thinking-2025-05-14"`

- `const AnthropicBetaInterleavedThinking2025_05_14 AnthropicBeta = "interleaved-thinking-2025-05-14"`

- `const AnthropicBetaCodeExecution2025_05_22 AnthropicBeta = "code-execution-2025-05-22"`

- `const AnthropicBetaExtendedCacheTTL2025_04_11 AnthropicBeta = "extended-cache-ttl-2025-04-11"`

- `const AnthropicBetaContext1m2025_08_07 AnthropicBeta = "context-1m-2025-08-07"`

- `const AnthropicBetaContextManagement2025_06_27 AnthropicBeta = "context-management-2025-06-27"`

- `const AnthropicBetaModelContextWindowExceeded2025_08_26 AnthropicBeta = "model-context-window-exceeded-2025-08-26"`

- `const AnthropicBetaSkills2025_10_02 AnthropicBeta = "skills-2025-10-02"`

- `type BetaSkillListResponse struct{…}`

Unique identifier for the skill.

The format and length of IDs may change over time.

ISO 8601 timestamp of when the skill was created.

- `DisplayTitle string`

Display title for the skill.

This is a human-readable label that is not included in the prompt sent to the model.

- `LatestVersion string`

The latest version identifier for the skill.

This represents the most recent version of the skill that has been created.

This may be one of the following values:

* `"custom"`: the skill was created by a user
    * `"anthropic"`: the skill was created by Anthropic

For Skills, this is always `"skill"`.

ISO 8601 timestamp of when the skill was last updated.

**Examples:**

Example 1 (go):
```go
package main

import (
  "context"
  "fmt"

  "github.com/anthropics/anthropic-sdk-go"
  "github.com/anthropics/anthropic-sdk-go/option"
)

func main() {
  client := anthropic.NewClient(
    option.WithAPIKey("my-anthropic-api-key"),
  )
  page, err := client.Beta.Skills.List(context.TODO(), anthropic.BetaSkillListParams{

  })
  if err != nil {
    panic(err.Error())
  }
  fmt.Printf("%+v\n", page)
}
```

---

## Create Skill Version (Beta) (TypeScript)

**URL:** llms-txt#create-skill-version-(beta)-(typescript)

**Contents:**
- Create
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/typescript/beta/skills/versions/create

`client.beta.skills.versions.create(stringskillID, VersionCreateParamsparams?, RequestOptionsoptions?): VersionCreateResponse`

**post** `/v1/skills/{skill_id}/versions`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `params: VersionCreateParams`

- `files?: Array<Uploadable> | null`

Body param: Files to upload for the skill.

All files must be in the same top-level directory and must include a SKILL.md file at the root of that directory.

- `betas?: Array<AnthropicBeta>`

Header param: Optional header to specify the beta version(s) you want to use.

- `"message-batches-2024-09-24" | "prompt-caching-2024-07-31" | "computer-use-2024-10-22" | 16 more`

- `"message-batches-2024-09-24"`

- `"prompt-caching-2024-07-31"`

- `"computer-use-2024-10-22"`

- `"computer-use-2025-01-24"`

- `"pdfs-2024-09-25"`

- `"token-counting-2024-11-01"`

- `"token-efficient-tools-2025-02-19"`

- `"output-128k-2025-02-19"`

- `"files-api-2025-04-14"`

- `"mcp-client-2025-04-04"`

- `"mcp-client-2025-11-20"`

- `"dev-full-thinking-2025-05-14"`

- `"interleaved-thinking-2025-05-14"`

- `"code-execution-2025-05-22"`

- `"extended-cache-ttl-2025-04-11"`

- `"context-1m-2025-08-07"`

- `"context-management-2025-06-27"`

- `"model-context-window-exceeded-2025-08-26"`

- `"skills-2025-10-02"`

- `VersionCreateResponse`

Unique identifier for the skill version.

The format and length of IDs may change over time.

- `created_at: string`

ISO 8601 timestamp of when the skill version was created.

- `description: string`

Description of the skill version.

This is extracted from the SKILL.md file in the skill upload.

- `directory: string`

Directory name of the skill version.

This is the top-level directory name that was extracted from the uploaded files.

Human-readable name of the skill version.

This is extracted from the SKILL.md file in the skill upload.

Identifier for the skill that this version belongs to.

For Skill Versions, this is always `"skill_version"`.

Version identifier for the skill.

Each version is identified by a Unix epoch timestamp (e.g., "1759178010641129").

**Examples:**

Example 1 (typescript):
```typescript
import Anthropic from '@anthropic-ai/sdk';

const client = new Anthropic({
  apiKey: process.env['ANTHROPIC_API_KEY'], // This is the default and can be omitted
});

const version = await client.beta.skills.versions.create('skill_id');

console.log(version.id);
```

---

## Get Skill (Beta) (TypeScript)

**URL:** llms-txt#get-skill-(beta)-(typescript)

**Contents:**
- Retrieve
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/typescript/beta/skills/retrieve

`client.beta.skills.retrieve(stringskillID, SkillRetrieveParamsparams?, RequestOptionsoptions?): SkillRetrieveResponse`

**get** `/v1/skills/{skill_id}`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `params: SkillRetrieveParams`

- `betas?: Array<AnthropicBeta>`

Optional header to specify the beta version(s) you want to use.

- `"message-batches-2024-09-24" | "prompt-caching-2024-07-31" | "computer-use-2024-10-22" | 16 more`

- `"message-batches-2024-09-24"`

- `"prompt-caching-2024-07-31"`

- `"computer-use-2024-10-22"`

- `"computer-use-2025-01-24"`

- `"pdfs-2024-09-25"`

- `"token-counting-2024-11-01"`

- `"token-efficient-tools-2025-02-19"`

- `"output-128k-2025-02-19"`

- `"files-api-2025-04-14"`

- `"mcp-client-2025-04-04"`

- `"mcp-client-2025-11-20"`

- `"dev-full-thinking-2025-05-14"`

- `"interleaved-thinking-2025-05-14"`

- `"code-execution-2025-05-22"`

- `"extended-cache-ttl-2025-04-11"`

- `"context-1m-2025-08-07"`

- `"context-management-2025-06-27"`

- `"model-context-window-exceeded-2025-08-26"`

- `"skills-2025-10-02"`

- `SkillRetrieveResponse`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `created_at: string`

ISO 8601 timestamp of when the skill was created.

- `display_title: string | null`

Display title for the skill.

This is a human-readable label that is not included in the prompt sent to the model.

- `latest_version: string | null`

The latest version identifier for the skill.

This represents the most recent version of the skill that has been created.

This may be one of the following values:

* `"custom"`: the skill was created by a user
    * `"anthropic"`: the skill was created by Anthropic

For Skills, this is always `"skill"`.

- `updated_at: string`

ISO 8601 timestamp of when the skill was last updated.

**Examples:**

Example 1 (typescript):
```typescript
import Anthropic from '@anthropic-ai/sdk';

const client = new Anthropic({
  apiKey: process.env['ANTHROPIC_API_KEY'], // This is the default and can be omitted
});

const skill = await client.beta.skills.retrieve('skill_id');

console.log(skill.id);
```

---

## Create Skill Version (Beta) (Java)

**URL:** llms-txt#create-skill-version-(beta)-(java)

**Contents:**
- Create
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/java/beta/skills/versions/create

`VersionCreateResponse beta().skills().versions().create(VersionCreateParamsparams = VersionCreateParams.none(), RequestOptionsrequestOptions = RequestOptions.none())`

**post** `/v1/skills/{skill_id}/versions`

- `VersionCreateParams params`

- `Optional<String> skillId`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `Optional<List<AnthropicBeta>> betas`

Optional header to specify the beta version(s) you want to use.

- `MESSAGE_BATCHES_2024_09_24("message-batches-2024-09-24")`

- `PROMPT_CACHING_2024_07_31("prompt-caching-2024-07-31")`

- `COMPUTER_USE_2024_10_22("computer-use-2024-10-22")`

- `COMPUTER_USE_2025_01_24("computer-use-2025-01-24")`

- `PDFS_2024_09_25("pdfs-2024-09-25")`

- `TOKEN_COUNTING_2024_11_01("token-counting-2024-11-01")`

- `TOKEN_EFFICIENT_TOOLS_2025_02_19("token-efficient-tools-2025-02-19")`

- `OUTPUT_128K_2025_02_19("output-128k-2025-02-19")`

- `FILES_API_2025_04_14("files-api-2025-04-14")`

- `MCP_CLIENT_2025_04_04("mcp-client-2025-04-04")`

- `MCP_CLIENT_2025_11_20("mcp-client-2025-11-20")`

- `DEV_FULL_THINKING_2025_05_14("dev-full-thinking-2025-05-14")`

- `INTERLEAVED_THINKING_2025_05_14("interleaved-thinking-2025-05-14")`

- `CODE_EXECUTION_2025_05_22("code-execution-2025-05-22")`

- `EXTENDED_CACHE_TTL_2025_04_11("extended-cache-ttl-2025-04-11")`

- `CONTEXT_1M_2025_08_07("context-1m-2025-08-07")`

- `CONTEXT_MANAGEMENT_2025_06_27("context-management-2025-06-27")`

- `MODEL_CONTEXT_WINDOW_EXCEEDED_2025_08_26("model-context-window-exceeded-2025-08-26")`

- `SKILLS_2025_10_02("skills-2025-10-02")`

- `Optional<List<String>> files`

Files to upload for the skill.

All files must be in the same top-level directory and must include a SKILL.md file at the root of that directory.

- `class VersionCreateResponse:`

Unique identifier for the skill version.

The format and length of IDs may change over time.

ISO 8601 timestamp of when the skill version was created.

- `String description`

Description of the skill version.

This is extracted from the SKILL.md file in the skill upload.

Directory name of the skill version.

This is the top-level directory name that was extracted from the uploaded files.

Human-readable name of the skill version.

This is extracted from the SKILL.md file in the skill upload.

Identifier for the skill that this version belongs to.

For Skill Versions, this is always `"skill_version"`.

Version identifier for the skill.

Each version is identified by a Unix epoch timestamp (e.g., "1759178010641129").

**Examples:**

Example 1 (java):
```java
package com.anthropic.example;

import com.anthropic.client.AnthropicClient;
import com.anthropic.client.okhttp.AnthropicOkHttpClient;
import com.anthropic.models.beta.skills.versions.VersionCreateParams;
import com.anthropic.models.beta.skills.versions.VersionCreateResponse;

public final class Main {
    private Main() {}

    public static void main(String[] args) {
        AnthropicClient client = AnthropicOkHttpClient.fromEnv();

        VersionCreateResponse version = client.beta().skills().versions().create("skill_id");
    }
}
```

---

## Get Skill (Beta) (Java)

**URL:** llms-txt#get-skill-(beta)-(java)

**Contents:**
- Retrieve
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/java/beta/skills/retrieve

`SkillRetrieveResponse beta().skills().retrieve(SkillRetrieveParamsparams = SkillRetrieveParams.none(), RequestOptionsrequestOptions = RequestOptions.none())`

**get** `/v1/skills/{skill_id}`

- `SkillRetrieveParams params`

- `Optional<String> skillId`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `Optional<List<AnthropicBeta>> betas`

Optional header to specify the beta version(s) you want to use.

- `MESSAGE_BATCHES_2024_09_24("message-batches-2024-09-24")`

- `PROMPT_CACHING_2024_07_31("prompt-caching-2024-07-31")`

- `COMPUTER_USE_2024_10_22("computer-use-2024-10-22")`

- `COMPUTER_USE_2025_01_24("computer-use-2025-01-24")`

- `PDFS_2024_09_25("pdfs-2024-09-25")`

- `TOKEN_COUNTING_2024_11_01("token-counting-2024-11-01")`

- `TOKEN_EFFICIENT_TOOLS_2025_02_19("token-efficient-tools-2025-02-19")`

- `OUTPUT_128K_2025_02_19("output-128k-2025-02-19")`

- `FILES_API_2025_04_14("files-api-2025-04-14")`

- `MCP_CLIENT_2025_04_04("mcp-client-2025-04-04")`

- `MCP_CLIENT_2025_11_20("mcp-client-2025-11-20")`

- `DEV_FULL_THINKING_2025_05_14("dev-full-thinking-2025-05-14")`

- `INTERLEAVED_THINKING_2025_05_14("interleaved-thinking-2025-05-14")`

- `CODE_EXECUTION_2025_05_22("code-execution-2025-05-22")`

- `EXTENDED_CACHE_TTL_2025_04_11("extended-cache-ttl-2025-04-11")`

- `CONTEXT_1M_2025_08_07("context-1m-2025-08-07")`

- `CONTEXT_MANAGEMENT_2025_06_27("context-management-2025-06-27")`

- `MODEL_CONTEXT_WINDOW_EXCEEDED_2025_08_26("model-context-window-exceeded-2025-08-26")`

- `SKILLS_2025_10_02("skills-2025-10-02")`

- `class SkillRetrieveResponse:`

Unique identifier for the skill.

The format and length of IDs may change over time.

ISO 8601 timestamp of when the skill was created.

- `Optional<String> displayTitle`

Display title for the skill.

This is a human-readable label that is not included in the prompt sent to the model.

- `Optional<String> latestVersion`

The latest version identifier for the skill.

This represents the most recent version of the skill that has been created.

This may be one of the following values:

* `"custom"`: the skill was created by a user
    * `"anthropic"`: the skill was created by Anthropic

For Skills, this is always `"skill"`.

ISO 8601 timestamp of when the skill was last updated.

**Examples:**

Example 1 (java):
```java
package com.anthropic.example;

import com.anthropic.client.AnthropicClient;
import com.anthropic.client.okhttp.AnthropicOkHttpClient;
import com.anthropic.models.beta.skills.SkillRetrieveParams;
import com.anthropic.models.beta.skills.SkillRetrieveResponse;

public final class Main {
    private Main() {}

    public static void main(String[] args) {
        AnthropicClient client = AnthropicOkHttpClient.fromEnv();

        SkillRetrieveResponse skill = client.beta().skills().retrieve("skill_id");
    }
}
```

---

## List only custom Skills

**URL:** llms-txt#list-only-custom-skills

**Contents:**
  - Retrieving a Skill
  - Deleting a Skill

curl "https://api.anthropic.com/v1/skills?source=custom" \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -H "anthropic-beta: skills-2025-10-02"
python Python
skill = client.beta.skills.retrieve(
    skill_id="skill_01AbCdEfGhIjKlMnOpQrStUv",
    betas=["skills-2025-10-02"]
)

print(f"Skill: {skill.display_title}")
print(f"Latest version: {skill.latest_version}")
print(f"Created: {skill.created_at}")
typescript TypeScript
const skill = await client.beta.skills.retrieve(
  'skill_01AbCdEfGhIjKlMnOpQrStUv',
  { betas: ['skills-2025-10-02'] }
);

console.log(`Skill: ${skill.display_title}`);
console.log(`Latest version: ${skill.latest_version}`);
console.log(`Created: ${skill.created_at}`);
bash Shell
curl "https://api.anthropic.com/v1/skills/skill_01AbCdEfGhIjKlMnOpQrStUv" \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -H "anthropic-beta: skills-2025-10-02"
python Python

**Examples:**

Example 1 (unknown):
```unknown
</CodeGroup>

See the [List Skills API reference](/docs/en/api/skills/list-skills) for pagination and filtering options.

### Retrieving a Skill

Get details about a specific Skill:

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

### Deleting a Skill

To delete a Skill, you must first delete all its versions:

<CodeGroup>
```

---

## Create Skill (Beta) (Ruby)

**URL:** llms-txt#create-skill-(beta)-(ruby)

**Contents:**
- Create
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/ruby/beta/skills/create

`beta.skills.create(**kwargs) -> SkillCreateResponse`

**post** `/v1/skills`

- `display_title: String`

Display title for the skill.

This is a human-readable label that is not included in the prompt sent to the model.

- `files: Array[String]`

Files to upload for the skill.

All files must be in the same top-level directory and must include a SKILL.md file at the root of that directory.

- `anthropic_beta: Array[AnthropicBeta]`

Optional header to specify the beta version(s) you want to use.

- `:"message-batches-2024-09-24" | :"prompt-caching-2024-07-31" | :"computer-use-2024-10-22" | 16 more`

- `:"message-batches-2024-09-24"`

- `:"prompt-caching-2024-07-31"`

- `:"computer-use-2024-10-22"`

- `:"computer-use-2025-01-24"`

- `:"pdfs-2024-09-25"`

- `:"token-counting-2024-11-01"`

- `:"token-efficient-tools-2025-02-19"`

- `:"output-128k-2025-02-19"`

- `:"files-api-2025-04-14"`

- `:"mcp-client-2025-04-04"`

- `:"mcp-client-2025-11-20"`

- `:"dev-full-thinking-2025-05-14"`

- `:"interleaved-thinking-2025-05-14"`

- `:"code-execution-2025-05-22"`

- `:"extended-cache-ttl-2025-04-11"`

- `:"context-1m-2025-08-07"`

- `:"context-management-2025-06-27"`

- `:"model-context-window-exceeded-2025-08-26"`

- `:"skills-2025-10-02"`

- `class SkillCreateResponse`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `created_at: String`

ISO 8601 timestamp of when the skill was created.

- `display_title: String`

Display title for the skill.

This is a human-readable label that is not included in the prompt sent to the model.

- `latest_version: String`

The latest version identifier for the skill.

This represents the most recent version of the skill that has been created.

This may be one of the following values:

* `"custom"`: the skill was created by a user
    * `"anthropic"`: the skill was created by Anthropic

For Skills, this is always `"skill"`.

- `updated_at: String`

ISO 8601 timestamp of when the skill was last updated.

**Examples:**

Example 1 (ruby):
```ruby
require "anthropic"

anthropic = Anthropic::Client.new(api_key: "my-anthropic-api-key")

skill = anthropic.beta.skills.create

puts(skill)
```

---

## Get Skill Version (Beta) (Go)

**URL:** llms-txt#get-skill-version-(beta)-(go)

**Contents:**
- Retrieve
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/go/beta/skills/versions/retrieve

`client.Beta.Skills.Versions.Get(ctx, version, params) (*BetaSkillVersionGetResponse, error)`

**get** `/v1/skills/{skill_id}/versions/{version}`

Version identifier for the skill.

Each version is identified by a Unix epoch timestamp (e.g., "1759178010641129").

- `params BetaSkillVersionGetParams`

- `SkillID param.Field[string]`

Path param: Unique identifier for the skill.

The format and length of IDs may change over time.

- `Betas param.Field[[]AnthropicBeta]`

Header param: Optional header to specify the beta version(s) you want to use.

- `type AnthropicBeta string`

- `const AnthropicBetaMessageBatches2024_09_24 AnthropicBeta = "message-batches-2024-09-24"`

- `const AnthropicBetaPromptCaching2024_07_31 AnthropicBeta = "prompt-caching-2024-07-31"`

- `const AnthropicBetaComputerUse2024_10_22 AnthropicBeta = "computer-use-2024-10-22"`

- `const AnthropicBetaComputerUse2025_01_24 AnthropicBeta = "computer-use-2025-01-24"`

- `const AnthropicBetaPDFs2024_09_25 AnthropicBeta = "pdfs-2024-09-25"`

- `const AnthropicBetaTokenCounting2024_11_01 AnthropicBeta = "token-counting-2024-11-01"`

- `const AnthropicBetaTokenEfficientTools2025_02_19 AnthropicBeta = "token-efficient-tools-2025-02-19"`

- `const AnthropicBetaOutput128k2025_02_19 AnthropicBeta = "output-128k-2025-02-19"`

- `const AnthropicBetaFilesAPI2025_04_14 AnthropicBeta = "files-api-2025-04-14"`

- `const AnthropicBetaMCPClient2025_04_04 AnthropicBeta = "mcp-client-2025-04-04"`

- `const AnthropicBetaMCPClient2025_11_20 AnthropicBeta = "mcp-client-2025-11-20"`

- `const AnthropicBetaDevFullThinking2025_05_14 AnthropicBeta = "dev-full-thinking-2025-05-14"`

- `const AnthropicBetaInterleavedThinking2025_05_14 AnthropicBeta = "interleaved-thinking-2025-05-14"`

- `const AnthropicBetaCodeExecution2025_05_22 AnthropicBeta = "code-execution-2025-05-22"`

- `const AnthropicBetaExtendedCacheTTL2025_04_11 AnthropicBeta = "extended-cache-ttl-2025-04-11"`

- `const AnthropicBetaContext1m2025_08_07 AnthropicBeta = "context-1m-2025-08-07"`

- `const AnthropicBetaContextManagement2025_06_27 AnthropicBeta = "context-management-2025-06-27"`

- `const AnthropicBetaModelContextWindowExceeded2025_08_26 AnthropicBeta = "model-context-window-exceeded-2025-08-26"`

- `const AnthropicBetaSkills2025_10_02 AnthropicBeta = "skills-2025-10-02"`

- `type BetaSkillVersionGetResponse struct{…}`

Unique identifier for the skill version.

The format and length of IDs may change over time.

ISO 8601 timestamp of when the skill version was created.

- `Description string`

Description of the skill version.

This is extracted from the SKILL.md file in the skill upload.

Directory name of the skill version.

This is the top-level directory name that was extracted from the uploaded files.

Human-readable name of the skill version.

This is extracted from the SKILL.md file in the skill upload.

Identifier for the skill that this version belongs to.

For Skill Versions, this is always `"skill_version"`.

Version identifier for the skill.

Each version is identified by a Unix epoch timestamp (e.g., "1759178010641129").

**Examples:**

Example 1 (go):
```go
package main

import (
  "context"
  "fmt"

  "github.com/anthropics/anthropic-sdk-go"
  "github.com/anthropics/anthropic-sdk-go/option"
)

func main() {
  client := anthropic.NewClient(
    option.WithAPIKey("my-anthropic-api-key"),
  )
  version, err := client.Beta.Skills.Versions.Get(
    context.TODO(),
    "version",
    anthropic.BetaSkillVersionGetParams{
      SkillID: "skill_id",
    },
  )
  if err != nil {
    panic(err.Error())
  }
  fmt.Printf("%+v\n", version.ID)
}
```

---

## Create Skill (Beta) (TypeScript)

**URL:** llms-txt#create-skill-(beta)-(typescript)

**Contents:**
- Create
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/typescript/beta/skills/create

`client.beta.skills.create(SkillCreateParamsparams?, RequestOptionsoptions?): SkillCreateResponse`

**post** `/v1/skills`

- `params: SkillCreateParams`

- `display_title?: string | null`

Body param: Display title for the skill.

This is a human-readable label that is not included in the prompt sent to the model.

- `files?: Array<Uploadable> | null`

Body param: Files to upload for the skill.

All files must be in the same top-level directory and must include a SKILL.md file at the root of that directory.

- `betas?: Array<AnthropicBeta>`

Header param: Optional header to specify the beta version(s) you want to use.

- `"message-batches-2024-09-24" | "prompt-caching-2024-07-31" | "computer-use-2024-10-22" | 16 more`

- `"message-batches-2024-09-24"`

- `"prompt-caching-2024-07-31"`

- `"computer-use-2024-10-22"`

- `"computer-use-2025-01-24"`

- `"pdfs-2024-09-25"`

- `"token-counting-2024-11-01"`

- `"token-efficient-tools-2025-02-19"`

- `"output-128k-2025-02-19"`

- `"files-api-2025-04-14"`

- `"mcp-client-2025-04-04"`

- `"mcp-client-2025-11-20"`

- `"dev-full-thinking-2025-05-14"`

- `"interleaved-thinking-2025-05-14"`

- `"code-execution-2025-05-22"`

- `"extended-cache-ttl-2025-04-11"`

- `"context-1m-2025-08-07"`

- `"context-management-2025-06-27"`

- `"model-context-window-exceeded-2025-08-26"`

- `"skills-2025-10-02"`

- `SkillCreateResponse`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `created_at: string`

ISO 8601 timestamp of when the skill was created.

- `display_title: string | null`

Display title for the skill.

This is a human-readable label that is not included in the prompt sent to the model.

- `latest_version: string | null`

The latest version identifier for the skill.

This represents the most recent version of the skill that has been created.

This may be one of the following values:

* `"custom"`: the skill was created by a user
    * `"anthropic"`: the skill was created by Anthropic

For Skills, this is always `"skill"`.

- `updated_at: string`

ISO 8601 timestamp of when the skill was last updated.

**Examples:**

Example 1 (typescript):
```typescript
import Anthropic from '@anthropic-ai/sdk';

const client = new Anthropic({
  apiKey: process.env['ANTHROPIC_API_KEY'], // This is the default and can be omitted
});

const skill = await client.beta.skills.create();

console.log(skill.id);
```

---

## Skills (Beta) (Java)

**URL:** llms-txt#skills-(beta)-(java)

URL: https://platform.claude.com/docs/en/api/java/beta/skills

---

## Create Skill (Beta)

**URL:** llms-txt#create-skill-(beta)

**Contents:**
- Create
  - Header Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/beta/skills/create

**post** `/v1/skills`

### Header Parameters

- `"anthropic-beta": optional array of AnthropicBeta`

Optional header to specify the beta version(s) you want to use.

- `UnionMember0 = string`

- `UnionMember1 = "message-batches-2024-09-24" or "prompt-caching-2024-07-31" or "computer-use-2024-10-22" or 16 more`

- `"message-batches-2024-09-24"`

- `"prompt-caching-2024-07-31"`

- `"computer-use-2024-10-22"`

- `"computer-use-2025-01-24"`

- `"pdfs-2024-09-25"`

- `"token-counting-2024-11-01"`

- `"token-efficient-tools-2025-02-19"`

- `"output-128k-2025-02-19"`

- `"files-api-2025-04-14"`

- `"mcp-client-2025-04-04"`

- `"mcp-client-2025-11-20"`

- `"dev-full-thinking-2025-05-14"`

- `"interleaved-thinking-2025-05-14"`

- `"code-execution-2025-05-22"`

- `"extended-cache-ttl-2025-04-11"`

- `"context-1m-2025-08-07"`

- `"context-management-2025-06-27"`

- `"model-context-window-exceeded-2025-08-26"`

- `"skills-2025-10-02"`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `created_at: string`

ISO 8601 timestamp of when the skill was created.

- `display_title: string`

Display title for the skill.

This is a human-readable label that is not included in the prompt sent to the model.

- `latest_version: string`

The latest version identifier for the skill.

This represents the most recent version of the skill that has been created.

This may be one of the following values:

* `"custom"`: the skill was created by a user
  * `"anthropic"`: the skill was created by Anthropic

For Skills, this is always `"skill"`.

- `updated_at: string`

ISO 8601 timestamp of when the skill was last updated.

**Examples:**

Example 1 (http):
```http
curl https://api.anthropic.com/v1/skills \
    -X POST \
    -H 'anthropic-version: 2023-06-01' \
    -H 'anthropic-beta: skills-2025-10-02' \
    -H "X-Api-Key: $ANTHROPIC_API_KEY"
```

---

## Skills

**URL:** llms-txt#skills

**Contents:**
- Create
  - Parameters
  - Returns
  - Example
- List
  - Parameters
  - Returns
  - Example
- Retrieve
  - Parameters

`client.beta.skills.create(SkillCreateParamsparams?, RequestOptionsoptions?): SkillCreateResponse`

**post** `/v1/skills`

- `params: SkillCreateParams`

- `display_title?: string | null`

Body param: Display title for the skill.

This is a human-readable label that is not included in the prompt sent to the model.

- `files?: Array<Uploadable> | null`

Body param: Files to upload for the skill.

All files must be in the same top-level directory and must include a SKILL.md file at the root of that directory.

- `betas?: Array<AnthropicBeta>`

Header param: Optional header to specify the beta version(s) you want to use.

- `"message-batches-2024-09-24" | "prompt-caching-2024-07-31" | "computer-use-2024-10-22" | 16 more`

- `"message-batches-2024-09-24"`

- `"prompt-caching-2024-07-31"`

- `"computer-use-2024-10-22"`

- `"computer-use-2025-01-24"`

- `"pdfs-2024-09-25"`

- `"token-counting-2024-11-01"`

- `"token-efficient-tools-2025-02-19"`

- `"output-128k-2025-02-19"`

- `"files-api-2025-04-14"`

- `"mcp-client-2025-04-04"`

- `"mcp-client-2025-11-20"`

- `"dev-full-thinking-2025-05-14"`

- `"interleaved-thinking-2025-05-14"`

- `"code-execution-2025-05-22"`

- `"extended-cache-ttl-2025-04-11"`

- `"context-1m-2025-08-07"`

- `"context-management-2025-06-27"`

- `"model-context-window-exceeded-2025-08-26"`

- `"skills-2025-10-02"`

- `SkillCreateResponse`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `created_at: string`

ISO 8601 timestamp of when the skill was created.

- `display_title: string | null`

Display title for the skill.

This is a human-readable label that is not included in the prompt sent to the model.

- `latest_version: string | null`

The latest version identifier for the skill.

This represents the most recent version of the skill that has been created.

This may be one of the following values:

* `"custom"`: the skill was created by a user
    * `"anthropic"`: the skill was created by Anthropic

For Skills, this is always `"skill"`.

- `updated_at: string`

ISO 8601 timestamp of when the skill was last updated.

`client.beta.skills.list(SkillListParamsparams?, RequestOptionsoptions?): PageCursor<SkillListResponse>`

- `params: SkillListParams`

Query param: Number of results to return per page.

Maximum value is 100. Defaults to 20.

- `page?: string | null`

Query param: Pagination token for fetching a specific page of results.

Pass the value from a previous response's `next_page` field to get the next page of results.

- `source?: string | null`

Query param: Filter skills by source.

If provided, only skills from the specified source will be returned:

* `"custom"`: only return user-created skills
    * `"anthropic"`: only return Anthropic-created skills

- `betas?: Array<AnthropicBeta>`

Header param: Optional header to specify the beta version(s) you want to use.

- `"message-batches-2024-09-24" | "prompt-caching-2024-07-31" | "computer-use-2024-10-22" | 16 more`

- `"message-batches-2024-09-24"`

- `"prompt-caching-2024-07-31"`

- `"computer-use-2024-10-22"`

- `"computer-use-2025-01-24"`

- `"pdfs-2024-09-25"`

- `"token-counting-2024-11-01"`

- `"token-efficient-tools-2025-02-19"`

- `"output-128k-2025-02-19"`

- `"files-api-2025-04-14"`

- `"mcp-client-2025-04-04"`

- `"mcp-client-2025-11-20"`

- `"dev-full-thinking-2025-05-14"`

- `"interleaved-thinking-2025-05-14"`

- `"code-execution-2025-05-22"`

- `"extended-cache-ttl-2025-04-11"`

- `"context-1m-2025-08-07"`

- `"context-management-2025-06-27"`

- `"model-context-window-exceeded-2025-08-26"`

- `"skills-2025-10-02"`

- `SkillListResponse`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `created_at: string`

ISO 8601 timestamp of when the skill was created.

- `display_title: string | null`

Display title for the skill.

This is a human-readable label that is not included in the prompt sent to the model.

- `latest_version: string | null`

The latest version identifier for the skill.

This represents the most recent version of the skill that has been created.

This may be one of the following values:

* `"custom"`: the skill was created by a user
    * `"anthropic"`: the skill was created by Anthropic

For Skills, this is always `"skill"`.

- `updated_at: string`

ISO 8601 timestamp of when the skill was last updated.

`client.beta.skills.retrieve(stringskillID, SkillRetrieveParamsparams?, RequestOptionsoptions?): SkillRetrieveResponse`

**get** `/v1/skills/{skill_id}`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `params: SkillRetrieveParams`

- `betas?: Array<AnthropicBeta>`

Optional header to specify the beta version(s) you want to use.

- `"message-batches-2024-09-24" | "prompt-caching-2024-07-31" | "computer-use-2024-10-22" | 16 more`

- `"message-batches-2024-09-24"`

- `"prompt-caching-2024-07-31"`

- `"computer-use-2024-10-22"`

- `"computer-use-2025-01-24"`

- `"pdfs-2024-09-25"`

- `"token-counting-2024-11-01"`

- `"token-efficient-tools-2025-02-19"`

- `"output-128k-2025-02-19"`

- `"files-api-2025-04-14"`

- `"mcp-client-2025-04-04"`

- `"mcp-client-2025-11-20"`

- `"dev-full-thinking-2025-05-14"`

- `"interleaved-thinking-2025-05-14"`

- `"code-execution-2025-05-22"`

- `"extended-cache-ttl-2025-04-11"`

- `"context-1m-2025-08-07"`

- `"context-management-2025-06-27"`

- `"model-context-window-exceeded-2025-08-26"`

- `"skills-2025-10-02"`

- `SkillRetrieveResponse`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `created_at: string`

ISO 8601 timestamp of when the skill was created.

- `display_title: string | null`

Display title for the skill.

This is a human-readable label that is not included in the prompt sent to the model.

- `latest_version: string | null`

The latest version identifier for the skill.

This represents the most recent version of the skill that has been created.

This may be one of the following values:

* `"custom"`: the skill was created by a user
    * `"anthropic"`: the skill was created by Anthropic

For Skills, this is always `"skill"`.

- `updated_at: string`

ISO 8601 timestamp of when the skill was last updated.

`client.beta.skills.delete(stringskillID, SkillDeleteParamsparams?, RequestOptionsoptions?): SkillDeleteResponse`

**delete** `/v1/skills/{skill_id}`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `params: SkillDeleteParams`

- `betas?: Array<AnthropicBeta>`

Optional header to specify the beta version(s) you want to use.

- `"message-batches-2024-09-24" | "prompt-caching-2024-07-31" | "computer-use-2024-10-22" | 16 more`

- `"message-batches-2024-09-24"`

- `"prompt-caching-2024-07-31"`

- `"computer-use-2024-10-22"`

- `"computer-use-2025-01-24"`

- `"pdfs-2024-09-25"`

- `"token-counting-2024-11-01"`

- `"token-efficient-tools-2025-02-19"`

- `"output-128k-2025-02-19"`

- `"files-api-2025-04-14"`

- `"mcp-client-2025-04-04"`

- `"mcp-client-2025-11-20"`

- `"dev-full-thinking-2025-05-14"`

- `"interleaved-thinking-2025-05-14"`

- `"code-execution-2025-05-22"`

- `"extended-cache-ttl-2025-04-11"`

- `"context-1m-2025-08-07"`

- `"context-management-2025-06-27"`

- `"model-context-window-exceeded-2025-08-26"`

- `"skills-2025-10-02"`

- `SkillDeleteResponse`

Unique identifier for the skill.

The format and length of IDs may change over time.

For Skills, this is always `"skill_deleted"`.

**Examples:**

Example 1 (typescript):
```typescript
import Anthropic from '@anthropic-ai/sdk';

const client = new Anthropic({
  apiKey: process.env['ANTHROPIC_API_KEY'], // This is the default and can be omitted
});

const skill = await client.beta.skills.create();

console.log(skill.id);
```

Example 2 (typescript):
```typescript
import Anthropic from '@anthropic-ai/sdk';

const client = new Anthropic({
  apiKey: process.env['ANTHROPIC_API_KEY'], // This is the default and can be omitted
});

// Automatically fetches more pages as needed.
for await (const skillListResponse of client.beta.skills.list()) {
  console.log(skillListResponse.id);
}
```

Example 3 (typescript):
```typescript
import Anthropic from '@anthropic-ai/sdk';

const client = new Anthropic({
  apiKey: process.env['ANTHROPIC_API_KEY'], // This is the default and can be omitted
});

const skill = await client.beta.skills.retrieve('skill_id');

console.log(skill.id);
```

Example 4 (typescript):
```typescript
import Anthropic from '@anthropic-ai/sdk';

const client = new Anthropic({
  apiKey: process.env['ANTHROPIC_API_KEY'], // This is the default and can be omitted
});

const skill = await client.beta.skills.delete('skill_id');

console.log(skill.id);
```

---

## Get Skill (Beta)

**URL:** llms-txt#get-skill-(beta)

**Contents:**
- Retrieve
  - Path Parameters
  - Header Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/beta/skills/retrieve

**get** `/v1/skills/{skill_id}`

Unique identifier for the skill.

The format and length of IDs may change over time.

### Header Parameters

- `"anthropic-beta": optional array of AnthropicBeta`

Optional header to specify the beta version(s) you want to use.

- `UnionMember0 = string`

- `UnionMember1 = "message-batches-2024-09-24" or "prompt-caching-2024-07-31" or "computer-use-2024-10-22" or 16 more`

- `"message-batches-2024-09-24"`

- `"prompt-caching-2024-07-31"`

- `"computer-use-2024-10-22"`

- `"computer-use-2025-01-24"`

- `"pdfs-2024-09-25"`

- `"token-counting-2024-11-01"`

- `"token-efficient-tools-2025-02-19"`

- `"output-128k-2025-02-19"`

- `"files-api-2025-04-14"`

- `"mcp-client-2025-04-04"`

- `"mcp-client-2025-11-20"`

- `"dev-full-thinking-2025-05-14"`

- `"interleaved-thinking-2025-05-14"`

- `"code-execution-2025-05-22"`

- `"extended-cache-ttl-2025-04-11"`

- `"context-1m-2025-08-07"`

- `"context-management-2025-06-27"`

- `"model-context-window-exceeded-2025-08-26"`

- `"skills-2025-10-02"`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `created_at: string`

ISO 8601 timestamp of when the skill was created.

- `display_title: string`

Display title for the skill.

This is a human-readable label that is not included in the prompt sent to the model.

- `latest_version: string`

The latest version identifier for the skill.

This represents the most recent version of the skill that has been created.

This may be one of the following values:

* `"custom"`: the skill was created by a user
  * `"anthropic"`: the skill was created by Anthropic

For Skills, this is always `"skill"`.

- `updated_at: string`

ISO 8601 timestamp of when the skill was last updated.

**Examples:**

Example 1 (http):
```http
curl https://api.anthropic.com/v1/skills/$SKILL_ID \
    -H 'anthropic-version: 2023-06-01' \
    -H 'anthropic-beta: skills-2025-10-02' \
    -H "X-Api-Key: $ANTHROPIC_API_KEY"
```

---

## List Anthropic-managed Skills

**URL:** llms-txt#list-anthropic-managed-skills

**Contents:**
- Step 2: Create a presentation

skills = client.beta.skills.list(
    source="anthropic",
    betas=["skills-2025-10-02"]
)

for skill in skills.data:
    print(f"{skill.id}: {skill.display_title}")
typescript TypeScript
import Anthropic from '@anthropic-ai/sdk';

const client = new Anthropic();

// List Anthropic-managed Skills
const skills = await client.beta.skills.list({
  source: 'anthropic',
  betas: ['skills-2025-10-02']
});

for (const skill of skills.data) {
  console.log(`${skill.id}: ${skill.display_title}`);
}
bash Shell
curl "https://api.anthropic.com/v1/skills?source=anthropic" \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -H "anthropic-beta: skills-2025-10-02"
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
</CodeGroup>

You see the following Skills: `pptx`, `xlsx`, `docx`, and `pdf`.

This API returns each Skill's metadata: its name and description. Claude loads this metadata at startup to know what Skills are available. This is the first level of **progressive disclosure**, where Claude discovers Skills without loading their full instructions yet.

## Step 2: Create a presentation

Now we'll use the PowerPoint Skill to create a presentation about renewable energy. We specify Skills using the `container` parameter in the Messages API:

<CodeGroup>
```

---

## Get Skill (Beta) (Ruby)

**URL:** llms-txt#get-skill-(beta)-(ruby)

**Contents:**
- Retrieve
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/ruby/beta/skills/retrieve

`beta.skills.retrieve(skill_id, **kwargs) -> SkillRetrieveResponse`

**get** `/v1/skills/{skill_id}`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `anthropic_beta: Array[AnthropicBeta]`

Optional header to specify the beta version(s) you want to use.

- `:"message-batches-2024-09-24" | :"prompt-caching-2024-07-31" | :"computer-use-2024-10-22" | 16 more`

- `:"message-batches-2024-09-24"`

- `:"prompt-caching-2024-07-31"`

- `:"computer-use-2024-10-22"`

- `:"computer-use-2025-01-24"`

- `:"pdfs-2024-09-25"`

- `:"token-counting-2024-11-01"`

- `:"token-efficient-tools-2025-02-19"`

- `:"output-128k-2025-02-19"`

- `:"files-api-2025-04-14"`

- `:"mcp-client-2025-04-04"`

- `:"mcp-client-2025-11-20"`

- `:"dev-full-thinking-2025-05-14"`

- `:"interleaved-thinking-2025-05-14"`

- `:"code-execution-2025-05-22"`

- `:"extended-cache-ttl-2025-04-11"`

- `:"context-1m-2025-08-07"`

- `:"context-management-2025-06-27"`

- `:"model-context-window-exceeded-2025-08-26"`

- `:"skills-2025-10-02"`

- `class SkillRetrieveResponse`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `created_at: String`

ISO 8601 timestamp of when the skill was created.

- `display_title: String`

Display title for the skill.

This is a human-readable label that is not included in the prompt sent to the model.

- `latest_version: String`

The latest version identifier for the skill.

This represents the most recent version of the skill that has been created.

This may be one of the following values:

* `"custom"`: the skill was created by a user
    * `"anthropic"`: the skill was created by Anthropic

For Skills, this is always `"skill"`.

- `updated_at: String`

ISO 8601 timestamp of when the skill was last updated.

**Examples:**

Example 1 (ruby):
```ruby
require "anthropic"

anthropic = Anthropic::Client.new(api_key: "my-anthropic-api-key")

skill = anthropic.beta.skills.retrieve("skill_id")

puts(skill)
```

---

## Check personal Skills

**URL:** llms-txt#check-personal-skills

**Contents:**
  - Skill Not Being Used
  - Additional Troubleshooting
- Related Documentation
  - Skills Guides
  - SDK Resources

ls ~/.claude/skills/*/SKILL.md
```

### Skill Not Being Used

**Check the Skill tool is enabled**: Confirm `"Skill"` is in your `allowedTools`.

**Check the description**: Ensure it's specific and includes relevant keywords. See [Agent Skills Best Practices](/docs/en/agents-and-tools/agent-skills/best-practices#writing-effective-descriptions) for guidance on writing effective descriptions.

### Additional Troubleshooting

For general Skills troubleshooting (YAML syntax, debugging, etc.), see the [Claude Code Skills troubleshooting section](https://code.claude.com/docs/en/skills#troubleshooting).

## Related Documentation

### Skills Guides
- [Agent Skills in Claude Code](https://code.claude.com/docs/en/skills): Complete Skills guide with creation, examples, and troubleshooting
- [Agent Skills Overview](/docs/en/agents-and-tools/agent-skills/overview): Conceptual overview, benefits, and architecture
- [Agent Skills Best Practices](/docs/en/agents-and-tools/agent-skills/best-practices): Authoring guidelines for effective Skills
- [Agent Skills Cookbook](https://platform.claude.com/cookbook/skills-notebooks-01-skills-introduction): Example Skills and templates

### SDK Resources
- [Subagents in the SDK](/docs/en/agent-sdk/subagents): Similar filesystem-based agents with programmatic options
- [Slash Commands in the SDK](/docs/en/agent-sdk/slash-commands): User-invoked commands
- [SDK Overview](/docs/en/agent-sdk/overview): General SDK concepts
- [TypeScript SDK Reference](/docs/en/agent-sdk/typescript): Complete API documentation
- [Python SDK Reference](/docs/en/agent-sdk/python): Complete API documentation

---

## List Skill Versions (Beta) (Python)

**URL:** llms-txt#list-skill-versions-(beta)-(python)

**Contents:**
- List
  - Parameters
  - Returns
  - Example

URL: https://platform.claude.com/docs/en/api/python/beta/skills/versions/list

`beta.skills.versions.list(strskill_id, VersionListParams**kwargs)  -> SyncPageCursor[VersionListResponse]`

**get** `/v1/skills/{skill_id}/versions`

Unique identifier for the skill.

The format and length of IDs may change over time.

- `limit: Optional[int]`

Number of items to return per page.

Defaults to `20`. Ranges from `1` to `1000`.

- `page: Optional[str]`

Optionally set to the `next_page` token from the previous response.

- `betas: Optional[List[AnthropicBetaParam]]`

Optional header to specify the beta version(s) you want to use.

- `UnionMember0 = str`

- `UnionMember1 = Literal["message-batches-2024-09-24", "prompt-caching-2024-07-31", "computer-use-2024-10-22", 16 more]`

- `"message-batches-2024-09-24"`

- `"prompt-caching-2024-07-31"`

- `"computer-use-2024-10-22"`

- `"computer-use-2025-01-24"`

- `"pdfs-2024-09-25"`

- `"token-counting-2024-11-01"`

- `"token-efficient-tools-2025-02-19"`

- `"output-128k-2025-02-19"`

- `"files-api-2025-04-14"`

- `"mcp-client-2025-04-04"`

- `"mcp-client-2025-11-20"`

- `"dev-full-thinking-2025-05-14"`

- `"interleaved-thinking-2025-05-14"`

- `"code-execution-2025-05-22"`

- `"extended-cache-ttl-2025-04-11"`

- `"context-1m-2025-08-07"`

- `"context-management-2025-06-27"`

- `"model-context-window-exceeded-2025-08-26"`

- `"skills-2025-10-02"`

- `class VersionListResponse: …`

Unique identifier for the skill version.

The format and length of IDs may change over time.

ISO 8601 timestamp of when the skill version was created.

Description of the skill version.

This is extracted from the SKILL.md file in the skill upload.

Directory name of the skill version.

This is the top-level directory name that was extracted from the uploaded files.

Human-readable name of the skill version.

This is extracted from the SKILL.md file in the skill upload.

Identifier for the skill that this version belongs to.

For Skill Versions, this is always `"skill_version"`.

Version identifier for the skill.

Each version is identified by a Unix epoch timestamp (e.g., "1759178010641129").

**Examples:**

Example 1 (python):
```python
import os
from anthropic import Anthropic

client = Anthropic(
    api_key=os.environ.get("ANTHROPIC_API_KEY"),  # This is the default and can be omitted
)
page = client.beta.skills.versions.list(
    skill_id="skill_id",
)
page = page.data[0]
print(page.id)
```

---

## List all Skills

**URL:** llms-txt#list-all-skills

curl "https://api.anthropic.com/v1/skills" \
  -H "x-api-key: $ANTHROPIC_API_KEY" \
  -H "anthropic-version: 2023-06-01" \
  -H "anthropic-beta: skills-2025-10-02"

---

## Step 2: Delete the Skill

**URL:** llms-txt#step-2:-delete-the-skill

client.beta.skills.delete(
    skill_id="skill_01AbCdEfGhIjKlMnOpQrStUv",
    betas=["skills-2025-10-02"]
)
typescript TypeScript
// Step 1: Delete all versions
const versions = await client.beta.skills.versions.list(
  'skill_01AbCdEfGhIjKlMnOpQrStUv',
  { betas: ['skills-2025-10-02'] }
);

for (const version of versions.data) {
  await client.beta.skills.versions.delete(
    'skill_01AbCdEfGhIjKlMnOpQrStUv',
    version.version,
    { betas: ['skills-2025-10-02'] }
  );
}

// Step 2: Delete the Skill
await client.beta.skills.delete(
  'skill_01AbCdEfGhIjKlMnOpQrStUv',
  { betas: ['skills-2025-10-02'] }
);
bash Shell

**Examples:**

Example 1 (unknown):
```unknown

```

Example 2 (unknown):
```unknown

```

---
