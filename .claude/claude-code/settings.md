# Claude Code Settings

> **来源**: [Anthropic Docs - Claude Code Settings](https://docs.anthropic.com/en/docs/claude-code/settings)
> 
> **整理**: [云中江树](https://github.com/yzfly)

Configure Claude Code with global and project-level settings, and environment variables. Claude Code offers a variety of settings to configure its behavior to meet your needs. You can configure Claude Code by running the `/config` command when using the interactive REPL.

## Settings Files

The `settings.json` file is our official mechanism for configuring Claude Code through hierarchical settings:

### Settings Hierarchy

- **User settings** are defined in `~/.claude/settings.json` and apply to all projects
- **Project settings** are saved in your project directory:
  - `.claude/settings.json` for settings that are checked into source control and shared with your team
  - `.claude/settings.local.json` for settings that are not checked in, useful for personal preferences and experimentation. Claude Code will configure git to ignore `.claude/settings.local.json` when it is created
- **Enterprise managed policy settings** (for enterprise deployments):
  - macOS: `/Library/Application Support/ClaudeCode/managed-settings.json`
  - Linux and WSL: `/etc/claude-code/managed-settings.json`
  - Windows: `C:\ProgramData\ClaudeCode\managed-settings.json`

### Example settings.json

```json
{
  "permissions": {
    "allow": [
      "Bash(npm run lint)",
      "Bash(npm run test:*)",
      "Read(~/.zshrc)"
    ],
    "deny": [
      "Bash(curl:*)"
    ]
  },
  "env": {
    "CLAUDE_CODE_ENABLE_TELEMETRY": "1",
    "OTEL_METRICS_EXPORTER": "otlp"
  }
}
```

### Available Settings

`settings.json` supports the following options:

| Key                          | Description                                                                                                                                                                                                    | Example                                                 |
| :--------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------ |
| `apiKeyHelper`               | Custom script, to be executed in `/bin/sh`, to generate an auth value. This value will generally be sent as `X-Api-Key`, `Authorization: Bearer`, and `Proxy-Authorization: Bearer` headers for model requests | `/bin/generate_temp_api_key.sh`                         |
| `cleanupPeriodDays`          | How long to locally retain chat transcripts (default: 30 days)                                                                                                                                                 | `20`                                                    |
| `env`                        | Environment variables that will be applied to every session                                                                                                                                                    | `{"FOO": "bar"}`                                        |
| `includeCoAuthoredBy`        | Whether to include the `co-authored-by Claude` byline in git commits and pull requests (default: `true`)                                                                                                       | `false`                                                 |
| `permissions`                | Configure permission rules for tool access                                                                                                                                                                      |                                                         |
| `hooks`                      | Configure custom commands to run before or after tool executions                                                                                                                                               | `{"PreToolUse": {"Bash": "echo 'Running command...'"}}` |
| `model`                      | Override the default model to use for Claude Code                                                                                                                                                              | `"claude-3-5-sonnet-20241022"`                          |
| `forceLoginMethod`           | Use `claudeai` to restrict login to Claude.ai accounts, `console` to restrict login to Anthropic Console (API usage billing) accounts                                                                          | `claudeai`                                              |
| `enableAllProjectMcpServers` | Automatically approve all MCP servers defined in project `.mcp.json` files                                                                                                                                     | `true`                                                  |
| `enabledMcpjsonServers`      | List of specific MCP servers from `.mcp.json` files to approve                                                                                                                                                 | `["memory", "github"]`                                  |
| `disabledMcpjsonServers`     | List of specific MCP servers from `.mcp.json` files to reject                                                                                                                                                  | `["filesystem"]`                                        |

### Permission Settings

| Keys                           | Description                                                                                                                                        | Example                          |
| :----------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------- |
| `allow`                        | Array of permission rules to allow tool use                                                                                                       | `[ "Bash(git diff:*)" ]`         |
| `deny`                         | Array of permission rules to deny tool use                                                                                                        | `[ "WebFetch", "Bash(curl:*)" ]` |
| `additionalDirectories`        | Additional working directories that Claude has access to                                                                                          | `[ "../docs/" ]`                 |
| `defaultMode`                  | Default permission mode when opening Claude Code                                                                                                  | `"acceptEdits"`                  |
| `disableBypassPermissionsMode` | Set to `"disable"` to prevent `bypassPermissions` mode from being activated                                                                       | `"disable"`                      |

### Settings Precedence

Settings are applied in order of precedence:

1. Enterprise policies
2. Command line arguments
3. Local project settings
4. Shared project settings
5. User settings

## Environment Variables

Claude Code supports the following environment variables to control its behavior:

**Note**: All environment variables can also be configured in `settings.json`. This is useful as a way to automatically set environment variables for each session, or to roll out a set of environment variables for your whole team or organization.

### Authentication and API Configuration

| Variable                                   | Purpose                                                                                                                                                            |
| :----------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `ANTHROPIC_API_KEY`                        | API key sent as `X-Api-Key` header, typically for the Claude SDK (for interactive usage, run `/login`)                                                             |
| `ANTHROPIC_AUTH_TOKEN`                     | Custom value for the `Authorization` and `Proxy-Authorization` headers (the value you set here will be prefixed with `Bearer `)                                    |
| `ANTHROPIC_CUSTOM_HEADERS`                 | Custom headers you want to add to the request (in `Name: Value` format)                                                                                            |
| `ANTHROPIC_MODEL`                          | Name of custom model to use                                                                                                                                        |
| `ANTHROPIC_SMALL_FAST_MODEL`               | Name of Haiku-class model for background tasks                                                                                                                     |
| `ANTHROPIC_SMALL_FAST_MODEL_AWS_REGION`    | Override AWS region for the small/fast model when using Bedrock                                                                                                    |
| `AWS_BEARER_TOKEN_BEDROCK`                 | Bedrock API key for authentication                                                                                                                                 |

### Cloud Provider Configuration

| Variable                                   | Purpose                                                                                                                                                            |
| :----------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `CLAUDE_CODE_USE_BEDROCK`                  | Use Amazon Bedrock                                                                                                                                                |
| `CLAUDE_CODE_USE_VERTEX`                   | Use Google Vertex AI                                                                                                                                              |
| `CLAUDE_CODE_SKIP_BEDROCK_AUTH`            | Skip AWS authentication for Bedrock (e.g. when using an LLM gateway)                                                                                             |
| `CLAUDE_CODE_SKIP_VERTEX_AUTH`             | Skip Google authentication for Vertex (e.g. when using an LLM gateway)                                                                                           |
| `VERTEX_REGION_CLAUDE_3_5_HAIKU`           | Override region for Claude 3.5 Haiku when using Vertex AI                                                                                                        |
| `VERTEX_REGION_CLAUDE_3_5_SONNET`          | Override region for Claude 3.5 Sonnet when using Vertex AI                                                                                                       |
| `VERTEX_REGION_CLAUDE_3_7_SONNET`          | Override region for Claude 3.7 Sonnet when using Vertex AI                                                                                                       |
| `VERTEX_REGION_CLAUDE_4_0_OPUS`            | Override region for Claude 4.0 Opus when using Vertex AI                                                                                                         |
| `VERTEX_REGION_CLAUDE_4_0_SONNET`          | Override region for Claude 4.0 Sonnet when using Vertex AI                                                                                                       |

### Tool and Execution Configuration

| Variable                                   | Purpose                                                                                                                                                            |
| :----------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `BASH_DEFAULT_TIMEOUT_MS`                  | Default timeout for long-running bash commands                                                                                                                     |
| `BASH_MAX_TIMEOUT_MS`                      | Maximum timeout the model can set for long-running bash commands                                                                                                   |
| `BASH_MAX_OUTPUT_LENGTH`                   | Maximum number of characters in bash outputs before they are middle-truncated                                                                                      |
| `CLAUDE_BASH_MAINTAIN_PROJECT_WORKING_DIR` | Return to the original working directory after each Bash command                                                                                                   |
| `CLAUDE_CODE_MAX_OUTPUT_TOKENS`            | Set the maximum number of output tokens for most requests                                                                                                          |
| `MAX_THINKING_TOKENS`                      | Force a thinking for the model budget                                                                                                                              |

### MCP Configuration

| Variable                                   | Purpose                                                                                                                                                            |
| :----------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `MCP_TIMEOUT`                              | Timeout in milliseconds for MCP server startup                                                                                                                     |
| `MCP_TOOL_TIMEOUT`                         | Timeout in milliseconds for MCP tool execution                                                                                                                     |
| `MAX_MCP_OUTPUT_TOKENS`                    | Maximum number of tokens allowed in MCP tool responses (default: 25000)                                                                                            |

### Privacy and Telemetry

| Variable                                   | Purpose                                                                                                                                                            |
| :----------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `CLAUDE_CODE_DISABLE_NONESSENTIAL_TRAFFIC` | Equivalent of setting `DISABLE_AUTOUPDATER`, `DISABLE_BUG_COMMAND`, `DISABLE_ERROR_REPORTING`, and `DISABLE_TELEMETRY`                                           |
| `DISABLE_AUTOUPDATER`                      | Set to `1` to disable automatic updates. This takes precedence over the `autoUpdates` configuration setting                                                       |
| `DISABLE_BUG_COMMAND`                      | Set to `1` to disable the `/bug` command                                                                                                                           |
| `DISABLE_COST_WARNINGS`                    | Set to `1` to disable cost warning messages                                                                                                                        |
| `DISABLE_ERROR_REPORTING`                  | Set to `1` to opt out of Sentry error reporting                                                                                                                    |
| `DISABLE_NON_ESSENTIAL_MODEL_CALLS`        | Set to `1` to disable model calls for non-critical paths like flavor text                                                                                          |
| `DISABLE_TELEMETRY`                        | Set to `1` to opt out of Statsig telemetry (note that Statsig events do not include user data like code, file paths, or bash commands)                           |

### Network and Interface

| Variable                                   | Purpose                                                                                                                                                            |
| :----------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `HTTP_PROXY`                               | Specify HTTP proxy server for network connections                                                                                                                  |
| `HTTPS_PROXY`                              | Specify HTTPS proxy server for network connections                                                                                                                 |
| `CLAUDE_CODE_DISABLE_TERMINAL_TITLE`       | Set to `1` to disable automatic terminal title updates based on conversation context                                                                               |
| `CLAUDE_CODE_IDE_SKIP_AUTO_INSTALL`        | Skip auto-installation of IDE extensions                                                                                                                           |

### Authentication Helper

| Variable                                   | Purpose                                                                                                                                                            |
| :----------------------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `CLAUDE_CODE_API_KEY_HELPER_TTL_MS`        | Interval in milliseconds at which credentials should be refreshed (when using `apiKeyHelper`)                                                                      |

## Configuration Options

To manage your configurations, use the following commands:

### Basic Configuration Commands

- **List settings**: `claude config list`
- **See a setting**: `claude config get <key>`
- **Change a setting**: `claude config set <key> <value>`
- **Push to a setting** (for lists): `claude config add <key> <value>`
- **Remove from a setting** (for lists): `claude config remove <key> <value>`

By default `config` changes your project configuration. To manage your global configuration, use the `--global` (or `-g`) flag.

### Global Configuration

To set a global configuration, use `claude config set -g <key> <value>`:

| Key                     | Description                                                                                                                                                                                        | Example                                                                    |
| :---------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------- |
| `autoUpdates`           | Whether to enable automatic updates (default: `true`). When enabled, Claude Code automatically downloads and installs updates in the background. Updates are applied when you restart Claude Code. | `false`                                                                    |
| `preferredNotifChannel` | Where you want to receive notifications (default: `iterm2`)                                                                                                                                        | `iterm2`, `iterm2_with_bell`, `terminal_bell`, or `notifications_disabled` |
| `theme`                 | Color theme                                                                                                                                                                                        | `dark`, `light`, `light-daltonized`, or `dark-daltonized`                  |
| `verbose`               | Whether to show full bash and command outputs (default: `false`)                                                                                                                                   | `true`                                                                     |

## Tools Available to Claude

Claude Code has access to a set of powerful tools that help it understand and modify your codebase:

| Tool             | Description                                          | Permission Required |
| :--------------- | :--------------------------------------------------- | :------------------ |
| **Bash**         | Executes shell commands in your environment          | Yes                 |
| **Edit**         | Makes targeted edits to specific files               | Yes                 |
| **Glob**         | Finds files based on pattern matching                | No                  |
| **Grep**         | Searches for patterns in file contents               | No                  |
| **LS**           | Lists files and directories                          | No                  |
| **MultiEdit**    | Performs multiple edits on a single file atomically  | Yes                 |
| **NotebookEdit** | Modifies Jupyter notebook cells                      | Yes                 |
| **NotebookRead** | Reads and displays Jupyter notebook contents         | No                  |
| **Read**         | Reads the contents of files                          | No                  |
| **Task**         | Runs a sub-agent to handle complex, multi-step tasks | No                  |
| **TodoWrite**    | Creates and manages structured task lists            | No                  |
| **WebFetch**     | Fetches content from a specified URL                 | Yes                 |
| **WebSearch**    | Performs web searches with domain filtering          | Yes                 |
| **Write**        | Creates or overwrites files                          | Yes                 |

Permission rules can be configured using `/allowed-tools` or in permission settings.

### Extending Tools with Hooks

You can run custom commands before or after any tool executes using Claude Code hooks.

For example, you could automatically run a Python formatter after Claude modifies Python files, or prevent modifications to production configuration files by blocking Write operations to certain paths.

## Related Resources

- [Identity and Access Management](https://docs.anthropic.com/en/docs/claude-code/iam#configuring-permissions) - Learn about Claude Code's permission system
- [IAM and access control](https://docs.anthropic.com/en/docs/claude-code/iam#enterprise-managed-policy-settings) - Enterprise policy management
- [Troubleshooting](https://docs.anthropic.com/en/docs/claude-code/troubleshooting#auto-updater-issues) - Solutions for common configuration issues
- [Hooks Guide](https://docs.anthropic.com/en/docs/claude-code/hooks-guide) - Custom command execution