# Project Context Directory

The project context directory is designed to store project-related contextual information, including task descriptions, data, examples, knowledge base, and more.

## Project Context Directory Structure
```
.context/
├── data/
├── examples/
├── knowledge/
├── TASK_PROMPT.md
├── PRC.md    # Product Requirements Context Document
└── README.md
```

## Tool Configuration

To enhance context retrieval through internet connectivity, it is recommended to install the following tools:

1. Install context-mcp-server
```bash
claude mcp add context-mcp-server -e CONTEXT_DIR=$(pwd)/context/knowledge -- uvx context-mcp-server
```
After installation, verify successful setup by running `claude mcp list`

2. Install Git and gh

## 📄 File Encoding Standards
- **Unified Encoding Standard**: All files must use UTF-8 encoding format
- **Encoding Consistency**: Ensure proper file encoding to prevent character corruption issues

## 🌐 Intelligent Web Search Strategy

### Search Trigger Conditions
When the local knowledge base cannot satisfy information requirements, automatically enable web search functionality to retrieve the latest relevant materials

### URL Access Specifications
- Use context-mcp-server plugin for web page access, and WebSearch plugin for search operations.
- **Access Prefix**: Add `https://r.jina.ai/` prefix to all URL links. If access fails, remove the prefix and retry.
- **Format Example**: 
  ```
  Original URL: https://docs.anthropic.com/en/docs/claude-code/mcp
  Access URL: https://r.jina.ai/https://docs.anthropic.com/en/docs/claude-code/mcp
  ```
- **Use Cases**: Particularly suitable for scenarios requiring complete web page content retrieval.

## 🛠️ Claude Code Knowledge Base Structure

To ensure Claude Code operates according to best practices with built-in documentation, the documentation is internalized within the `.claude/claude-code` directory, containing comprehensive Claude Code-related documentation and configurations.

### Directory Structure Overview
```
.claude/
└── claude-code/
    ├── best_practise.md      # Best practices guide
    ├── cli.md                # Command line interface documentation
    ├── common_workflow.md    # Common workflow procedures
    ├── sdk.md                # SDK development documentation
    └── settings.md           # Configuration documentation
```