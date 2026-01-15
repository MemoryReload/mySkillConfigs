# Claude Code

Claude Code is an agentic coding tool that lives in your terminal. It helps you with software engineering tasks by understanding your codebase, running commands, and editing files.

## Overview

Claude Code is an interactive CLI tool that brings the power of Claude 3.5 Sonnet directly into your development workflow. It can:
- Explore and understand complex codebases
- Edit files and fix bugs
- Run tests and execute commands
- Manage git operations
- Plan and implement new features

## Key Features

### Agentic Capabilities
- **Deep Context**: Claude Code can read thousands of lines of code to understand the context of your project.
- **Tool Use**: It has access to a suite of tools including `Bash`, `Glob`, `Grep`, `Read`, `Edit`, `Write`, and specialized agents.
- **Task Management**: Uses a todo list to track progress on complex tasks.
- **Planning**: Can enter "Plan Mode" to design implementation strategies before writing code.

### CLI & TUI
- **Terminal Interface**: Runs directly in your shell, integrating with your existing tools.
- **Slash Commands**: Quick access to common actions like `/help`, `/clear`, `/compact`, `/cost`, `/doctor`.
- **Keyboard Shortcuts**:
  - `Esc`: Enter/Exit different modes
  - `Ctrl+C`: Interrupt current action
  - `Ctrl+D`: Exit Claude Code

### Tools & Integration
- **File Operations**: Read, write, edit, and search files efficiently.
- **Command Execution**: Run shell commands (git, npm, cargo, etc.) securely.
- **MCP Support**: Connects to Model Context Protocol servers to extend capabilities (e.g., database access, browser automation).
- **VS Code Integration**: Can open files in VS Code.

## Usage

### Basic Interaction
Simply type your request in natural language.
```bash
> Explain the authentication flow in src/auth.ts
> Fix the bug in the login component
> Add a new endpoint for user profile
```

### Slash Commands
- `/help`: Show available commands and help text.
- `/clear`: Clear the conversation history.
- `/compact`: Compact the conversation history to save tokens.
- `/cost`: Show token usage and cost for the current session.
- `/doctor`: Run health checks on the Claude Code environment.
- `/bug`: Report a bug to Anthropic.
- `/init`: Initialize Claude Code in a new project.
- `/login`: Log in to your Anthropic account.
- `/logout`: Log out.
- `/settings`: Open the settings configuration.

### Configuration
Claude Code can be configured via `config.toml` (usually in `~/.claude/config.toml` or project-specific `.claude/config.toml`) or through the interactive `/settings` command.

Common settings:
- `theme`: UI color theme.
- `editor`: Preferred text editor.
- `auto_context`: Whether to automatically add context from open files.

### Ignore Files
Claude Code respects `.gitignore` files. You can also use `.claudeignore` to specifically exclude files from Claude's context without affecting git.

## Architecture

### Agents
Claude Code uses specialized agents for different tasks:
- **Planner**: Designs high-level plans.
- **Explorer**: Searches and analyzes the codebase.
- **Coder**: Writes and edits code.
- **Terminal**: Executes shell commands.

### Tool Security
- **Bash Tool**: Executes commands in a persistent shell. Dangerous commands may require user confirmation.
- **File Edits**: Edits are applied directly to the filesystem. It's recommended to work in a git repository to easily revert changes.

## Common Workflows

### Bug Fixing
1. User reports a bug.
2. Claude explores the codebase to locate the issue.
3. Claude creates a reproduction test case (optional but recommended).
4. Claude implements the fix.
5. Claude runs tests to verify the fix.

### Feature Implementation
1. User requests a new feature.
2. Claude enters Plan Mode to design the solution.
3. User approves the plan.
4. Claude implements the feature step-by-step, updating the todo list.
5. Claude verifies the implementation.

### Code Explanation
1. User asks to explain a complex module.
2. Claude reads the relevant files.
3. Claude provides a high-level summary and detailed explanation of the logic.

## Troubleshooting
- **Permission Denied**: Ensure Claude Code has read/write access to the directory.
- **Context Limit**: Use `/compact` or start a new session if the conversation gets too long.
- **Network Issues**: Check your internet connection and proxy settings if Claude cannot connect to the API.

## Resources
- [Claude Code Documentation](https://docs.anthropic.com/en/docs/agents-and-tools/claude-code)
- [Anthropic Console](https://console.anthropic.com)
