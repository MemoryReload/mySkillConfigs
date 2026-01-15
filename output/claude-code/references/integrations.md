# Claude-Code - Integrations

**Pages:** 1

---

## Plugins in the SDK

**URL:** llms-txt#plugins-in-the-sdk

**Contents:**
- What are plugins?
- Loading plugins
  - Path specifications
- Verifying plugin installation
- Using plugin commands
- Complete example
- Plugin structure reference
- Common use cases
  - Development and testing
  - Project-specific extensions

Load custom plugins to extend Claude Code with commands, agents, skills, and hooks through the Agent SDK

Plugins allow you to extend Claude Code with custom functionality that can be shared across projects. Through the Agent SDK, you can programmatically load plugins from local directories to add custom slash commands, agents, skills, hooks, and MCP servers to your agent sessions.

Plugins are packages of Claude Code extensions that can include:
- **Commands**: Custom slash commands
- **Agents**: Specialized subagents for specific tasks
- **Skills**: Model-invoked capabilities that Claude uses autonomously
- **Hooks**: Event handlers that respond to tool use and other events
- **MCP servers**: External tool integrations via Model Context Protocol

For complete information on plugin structure and how to create plugins, see [Plugins](https://code.claude.com/docs/en/plugins).

Load plugins by providing their local file system paths in your options configuration. The SDK supports loading multiple plugins from different locations.

### Path specifications

Plugin paths can be:
- **Relative paths**: Resolved relative to your current working directory (e.g., `"./plugins/my-plugin"`)
- **Absolute paths**: Full file system paths (e.g., `"/home/user/plugins/my-plugin"`)

<Note>
The path should point to the plugin's root directory (the directory containing `.claude-plugin/plugin.json`).
</Note>

## Verifying plugin installation

When plugins load successfully, they appear in the system initialization message. You can verify that your plugins are available:

## Using plugin commands

Commands from plugins are automatically namespaced with the plugin name to avoid conflicts. The format is `plugin-name:command-name`.

<Note>
If you installed a plugin via the CLI (e.g., `/plugin install my-plugin@marketplace`), you can still use it in the SDK by providing its installation path. Check `~/.claude/plugins/` for CLI-installed plugins.
</Note>

Here's a full example demonstrating plugin loading and usage:

## Plugin structure reference

A plugin directory must contain a `.claude-plugin/plugin.json` manifest file. It can optionally include:

For detailed information on creating plugins, see:
- [Plugins](https://code.claude.com/docs/en/plugins) - Complete plugin development guide
- [Plugins reference](https://code.claude.com/docs/en/plugins-reference) - Technical specifications and schemas

### Development and testing

Load plugins during development without installing them globally:

### Project-specific extensions

Include plugins in your project repository for team-wide consistency:

### Multiple plugin sources

Combine plugins from different locations:

### Plugin not loading

If your plugin doesn't appear in the init message:

1. **Check the path**: Ensure the path points to the plugin root directory (containing `.claude-plugin/`)
2. **Validate plugin.json**: Ensure your manifest file has valid JSON syntax
3. **Check file permissions**: Ensure the plugin directory is readable

### Commands not available

If plugin commands don't work:

1. **Use the namespace**: Plugin commands require the `plugin-name:command-name` format
2. **Check init message**: Verify the command appears in `slash_commands` with the correct namespace
3. **Validate command files**: Ensure command markdown files are in the `commands/` directory

### Path resolution issues

If relative paths don't work:

1. **Check working directory**: Relative paths are resolved from your current working directory
2. **Use absolute paths**: For reliability, consider using absolute paths
3. **Normalize paths**: Use path utilities to construct paths correctly

- [Plugins](https://code.claude.com/docs/en/plugins) - Complete plugin development guide
- [Plugins reference](https://code.claude.com/docs/en/plugins-reference) - Technical specifications
- [Slash Commands](/docs/en/agent-sdk/slash-commands) - Using slash commands in the SDK
- [Subagents](/docs/en/agent-sdk/subagents) - Working with specialized agents
- [Skills](/docs/en/agent-sdk/skills) - Using Agent Skills

**Examples:**

Example 1 (unknown):
```unknown

```

Example 2 (unknown):
```unknown
</CodeGroup>

### Path specifications

Plugin paths can be:
- **Relative paths**: Resolved relative to your current working directory (e.g., `"./plugins/my-plugin"`)
- **Absolute paths**: Full file system paths (e.g., `"/home/user/plugins/my-plugin"`)

<Note>
The path should point to the plugin's root directory (the directory containing `.claude-plugin/plugin.json`).
</Note>

## Verifying plugin installation

When plugins load successfully, they appear in the system initialization message. You can verify that your plugins are available:

<CodeGroup>
```

Example 3 (unknown):
```unknown

```

Example 4 (unknown):
```unknown
</CodeGroup>

## Using plugin commands

Commands from plugins are automatically namespaced with the plugin name to avoid conflicts. The format is `plugin-name:command-name`.

<CodeGroup>
```

---
