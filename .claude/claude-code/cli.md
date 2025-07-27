# Claude Code CLI Reference

> **来源**: [Anthropic Docs - CLI Reference](https://docs.anthropic.com/en/docs/claude-code/cli-reference)
> 
> **整理**: [云中江树](https://github.com/yzfly)

Complete reference for Claude Code command-line interface, including commands and flags.

## CLI Commands

| Command                            | Description                                    | Example                                                            |
| :--------------------------------- | :--------------------------------------------- | :----------------------------------------------------------------- |
| `claude`                           | Start interactive REPL                         | `claude`                                                           |
| `claude "query"`                   | Start REPL with initial prompt                 | `claude "explain this project"`                                    |
| `claude -p "query"`                | Query via SDK, then exit                       | `claude -p "explain this function"`                                |
| `cat file \| claude -p "query"`    | Process piped content                          | `cat logs.txt \| claude -p "explain"`                              |
| `claude -c`                        | Continue most recent conversation              | `claude -c`                                                        |
| `claude -c -p "query"`             | Continue via SDK                               | `claude -c -p "Check for type errors"`                             |
| `claude -r "<session-id>" "query"` | Resume session by ID                           | `claude -r "abc123" "Finish this PR"`                              |
| `claude update`                    | Update to latest version                       | `claude update`                                                    |
| `claude mcp`                       | Configure Model Context Protocol (MCP) servers | Configure MCP servers for extended functionality                   |

## CLI Flags

Customize Claude Code's behavior with these command-line flags:

### Session Management

| Flag                             | Description                                                                                                                                              | Example                                                     |
| :------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------- | :---------------------------------------------------------- |
| `--resume`                       | Resume a specific session by ID, or by choosing in interactive mode                                                                                      | `claude --resume abc123 "query"`                            |
| `--continue`                     | Load the most recent conversation in the current directory                                                                                               | `claude --continue`                                         |

### Execution Mode

| Flag                             | Description                                                                                                                                              | Example                                                     |
| :------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------- | :---------------------------------------------------------- |
| `--print`, `-p`                  | Print response without interactive mode (see SDK documentation for programmatic usage details)                                                          | `claude -p "query"`                                         |
| `--output-format`                | Specify output format for print mode (options: `text`, `json`, `stream-json`)                                                                            | `claude -p "query" --output-format json`                    |
| `--input-format`                 | Specify input format for print mode (options: `text`, `stream-json`)                                                                                     | `claude -p --output-format json --input-format stream-json` |
| `--max-turns`                    | Limit the number of agentic turns in non-interactive mode                                                                                                | `claude -p --max-turns 3 "query"`                           |
| `--verbose`                      | Enable verbose logging, shows full turn-by-turn output (helpful for debugging in both print and interactive modes)                                       | `claude --verbose`                                          |

### Model Configuration

| Flag                             | Description                                                                                                                                              | Example                                                     |
| :------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------- | :---------------------------------------------------------- |
| `--model`                        | Sets the model for the current session with an alias for the latest model (`sonnet` or `opus`) or a model's full name                                    | `claude --model claude-sonnet-4-20250514`                   |

### Permission Management

| Flag                             | Description                                                                                                                                              | Example                                                     |
| :------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------- | :---------------------------------------------------------- |
| `--permission-mode`              | Begin in a specified permission mode                                                                                                                     | `claude --permission-mode plan`                             |
| `--permission-prompt-tool`       | Specify an MCP tool to handle permission prompts in non-interactive mode                                                                                 | `claude -p --permission-prompt-tool mcp_auth_tool "query"`  |
| `--allowedTools`                 | A list of tools that should be allowed without prompting the user for permission, in addition to settings.json files                                    | `"Bash(git log:*)" "Bash(git diff:*)" "Read"`               |
| `--disallowedTools`              | A list of tools that should be disallowed without prompting the user for permission, in addition to settings.json files                                 | `"Bash(git log:*)" "Bash(git diff:*)" "Edit"`               |
| `--dangerously-skip-permissions` | Skip permission prompts (use with caution)                                                                                                               | `claude --dangerously-skip-permissions`                     |

### Directory Access

| Flag                             | Description                                                                                                                                              | Example                                                     |
| :------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------- | :---------------------------------------------------------- |
| `--add-dir`                      | Add additional working directories for Claude to access (validates each path exists as a directory)                                                      | `claude --add-dir ../apps ../lib`                           |

## Output Formats

### Text Format (Default)

Returns just the response text:

```bash
$ claude -p "Explain this code" --output-format text
```

### JSON Format

Returns structured data including metadata:

```bash
$ claude -p "Analyze this function" --output-format json
```

**JSON Response Structure:**
```json
{
  "type": "result",
  "subtype": "success",
  "total_cost_usd": 0.003,
  "is_error": false,
  "duration_ms": 1234,
  "duration_api_ms": 800,
  "num_turns": 6,
  "result": "The response text here...",
  "session_id": "abc123"
}
```

### Streaming JSON Format

Streams each message as it is received:

```bash
$ claude -p "Build an application" --output-format stream-json
```

**Tip**: The `--output-format json` flag is particularly useful for scripting and automation, allowing you to parse Claude's responses programmatically.

## Common Usage Patterns

### Interactive Mode

```bash
# Start interactive session
claude

# Start with initial prompt
claude "Review this codebase"

# Continue previous conversation
claude --continue

# Resume specific session
claude --resume abc123
```

### Non-Interactive Mode (SDK)

```bash
# Simple query
claude -p "What does this function do?"

# With JSON output for scripting
claude -p "Analyze code quality" --output-format json

# Process file content
cat myfile.py | claude -p "Review this code"

# Limit execution turns
claude -p "Fix all bugs" --max-turns 5
```

### Permission Control

```bash
# Allow specific tools
claude --allowedTools "Bash(git:*)" "Read" "Edit"

# Deny dangerous operations
claude --disallowedTools "Bash(rm:*)" "Bash(curl:*)"

# Start in specific permission mode
claude --permission-mode acceptEdits
```

### Session Management

```bash
# Continue most recent conversation
claude --continue

# Continue with new prompt
claude --continue "Add unit tests"

# Resume specific session
claude --resume 550e8400-e29b-41d4-a716-446655440000

# Resume in non-interactive mode
claude -p --resume abc123 "Update documentation"
```

### Advanced Usage

```bash
# Verbose output for debugging
claude --verbose -p "Complex analysis"

# Custom model selection
claude --model claude-sonnet-4-20250514

# Additional working directories
claude --add-dir ../shared-lib ../config

# MCP tool for permission handling
claude -p --permission-prompt-tool mcp__auth__approve "Deploy code"
```

## Integration Examples

### Shell Scripting

```bash
#!/bin/bash

# Use Claude in shell script
result=$(claude -p "Summarize git log" --output-format json)
echo "$result" | jq -r '.result'

# Process multiple files
for file in *.py; do
    claude -p "Add type hints to this file:" < "$file" > "${file}.typed"
done
```

### CI/CD Pipeline

```bash
# Automated code review
claude -p "Review changes vs main branch" \
  --output-format json \
  --allowedTools "Bash(git:*)" "Read" \
  > review_results.json
```

### Data Processing

```bash
# Analyze log files
cat error.log | claude -p "Find root cause of errors" --output-format json

# Process CSV data
claude -p "Analyze sales trends in this data:" < sales_data.csv
```

## Related Resources

- [Interactive Mode](https://docs.anthropic.com/en/docs/claude-code/interactive-mode) - Shortcuts, input modes, and interactive features
- [Slash Commands](https://docs.anthropic.com/en/docs/claude-code/slash-commands) - Interactive session commands
- [SDK Documentation](claude-code-sdk.md) - Programmatic usage and integrations
- [Common Workflows](claude-common-workflow.md) - Advanced workflows and patterns
- [Settings](claude-settings.md) - Configuration options
- [Quickstart Guide](https://docs.anthropic.com/en/docs/claude-code/quickstart) - Getting started with Claude Code