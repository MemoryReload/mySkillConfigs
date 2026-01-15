# Claude-Code - Workflows

**Pages:** 3

---

## Create the full URL from the GitHub repository

**URL:** llms-txt#create-the-full-url-from-the-github-repository

url = "https://raw.githubusercontent.com/anthropics/anthropic-cookbook/main/skills/summarization/data/Sample Sublease Agreement.pdf"
url = url.replace(" ", "%20")

---

## Git gud

**URL:** llms-txt#git-gud

**Contents:**
- Example Output
- API Request

Generate appropriate Git commands based on user-described version control actions.

> Copy this prompt into our developer [Console](/dashboard) to try it for yourself!

| Role | Content                                                                                                                       |
| ---- | ----------------------------------------------------------------------------------------------------------------------------- |
| User | I have made some changes to my local files and I want to save them in my local Git repository. What Git command should I use? |

To save the changes you've made to your local files in your local Git repository, you should use the following Git commands:

1.  `git add .` or `git add <file>`

- This command stages the changes you've made, preparing them to be committed. The `.` adds all changes in the current directory and its subdirectories. If you only want to stage specific files, replace `.` with the file name or path.

2.  `git commit -m "Your commit message"`

- This command commits the staged changes to your local Git repository. The `-m` flag allows you to provide a commit message describing the changes you've made. Replace `"Your commit message"` with a brief, descriptive summary of your changes.

For example, if you've made changes to a file named `example.txt`, you would run:

These commands will save your changes in your local Git repository, creating a new commit with the provided message. You can later push these changes to a remote repository or continue working on your local branch.

Remember to run these commands from within your Git repository directory in your terminal or command prompt.

<Tabs>
<Tab title="Python">
`
</Tab>
<Tab title="TypeScript">

</Tab>
<Tab title="AWS Bedrock Python">

```python
from anthropic import AnthropicBedrock

**Examples:**

Example 1 (unknown):
```unknown
git add example.txt
git commit -m "Update example.txt with new content"
```

Example 2 (python):
```python
import anthropic

client = anthropic.Anthropic(  # defaults to os.environ.get("ANTHROPIC_API_KEY")
    api_key="my_api_key",
)
message = client.messages.create(
    model="claude-sonnet-4-5",
    max_tokens=1000,
    temperature=0,
    messages=[
        {
            "role": "user",
            "content": [
                {
                    "type": "text",
                    "text": "I have made some changes to my local files and I want to save them in my local Git repository. What Git command should I use?",
                }
            ],
        }
    ],
)
print(message.content)
```

Example 3 (typescript):
```typescript
import Anthropic from "@anthropic-ai/sdk";

const anthropic = new Anthropic({
  apiKey: "my_api_key", // defaults to process.env["ANTHROPIC_API_KEY"]
});

const msg = await anthropic.messages.create({
  model: "claude-sonnet-4-5",
  max_tokens: 1000,
  temperature: 0,
  messages: [
    {
      "role": "user",
      "content": [
        {
          "type": "text",
          "text": "I have made some changes to my local files and I want to save them in my local Git repository. What Git command should I use?"
        }
      ]
    }
  ]
});
console.log(msg);
```

---

## Agent uses Grep to find TODOs, Bash to get git blame info

**URL:** llms-txt#agent-uses-grep-to-find-todos,-bash-to-get-git-blame-info

**Contents:**
- Error handling
- Related resources

async for message in query(
    prompt="Find all TODO comments in src/ and identify who added them",
    options={
        "output_format": {
            "type": "json_schema",
            "schema": todo_schema
        }
    }
):
    if hasattr(message, 'structured_output'):
        data = message.structured_output
        print(f"Found {data['total_count']} TODOs")
        for todo in data['todos']:
            print(f"{todo['file']}:{todo['line']} - {todo['text']}")
            if 'author' in todo:
                print(f"  Added by {todo['author']} on {todo['date']}")
typescript
for await (const msg of query({
  prompt: 'Analyze the data',
  options: {
    outputFormat: {
      type: 'json_schema',
      schema: mySchema
    }
  }
})) {
  if (msg.type === 'result') {
    if (msg.subtype === 'success' && msg.structured_output) {
      console.log(msg.structured_output)
    } else if (msg.subtype === 'error_max_structured_output_retries') {
      console.error('Could not produce valid output')
    }
  }
}
```

- [JSON Schema documentation](https://json-schema.org/)
- [API Structured Outputs](/docs/en/build-with-claude/structured-outputs) - For single API calls
- [Custom tools](/docs/en/agent-sdk/custom-tools) - Define tools for your agents
- [TypeScript SDK reference](/docs/en/agent-sdk/typescript) - Full TypeScript API
- [Python SDK reference](/docs/en/agent-sdk/python) - Full Python API

**Examples:**

Example 1 (unknown):
```unknown
</CodeGroup>

The agent autonomously uses the right tools (Grep, Bash) to gather information and returns validated data.

## Error handling

If the agent cannot produce valid output matching your schema, you'll receive an error result:
```

---
