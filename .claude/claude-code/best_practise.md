# Claude Code Best Practices

> **来源**: [Anthropic Engineering - Claude Code Best Practices](https://www.anthropic.com/engineering/claude-code-best-practices)
> 
> **整理**: [云中江树](https://github.com/yzfly)

Claude Code is a command line tool for agentic coding that provides close to raw model access without forcing specific workflows. This creates a flexible, customizable, scriptable, and safe power tool.

## 1. Customize Your Setup

### Create `CLAUDE.md` Files

`CLAUDE.md` is a special file that Claude automatically pulls into context when starting a conversation. Document:

- Common bash commands
- Core files and utility functions  
- Code style guidelines
- Testing instructions
- Repository etiquette (branch naming, merge vs. rebase)
- Developer environment setup
- Any unexpected behaviors or warnings
- Other information you want Claude to remember

**Example `CLAUDE.md`:**
```markdown
# Bash commands
- npm run build: Build the project
- npm run typecheck: Run the typechecker

# Code style
- Use ES modules (import/export) syntax, not CommonJS (require)
- Destructure imports when possible (eg. import { foo } from 'bar')

# Workflow
- Be sure to typecheck when you're done making changes
- Prefer running single tests, not the whole test suite, for performance
```

**File Locations:**
- **Root of repo**: `CLAUDE.md` (check into git) or `CLAUDE.local.md` (gitignored)
- **Parent directories**: For monorepos, both `root/CLAUDE.md` and `root/foo/CLAUDE.md`
- **Child directories**: Auto-pulled when working with files in child directories
- **Home folder**: `~/.claude/CLAUDE.md` (applies to all sessions)

**Tips:**
- Use `/init` command to auto-generate initial `CLAUDE.md`
- Press `#` key to have Claude add instructions to relevant `CLAUDE.md`
- Run through [prompt improver](https://docs.anthropic.com/en/docs/build-with-claude/prompt-engineering/prompt-improver)
- Add emphasis with "IMPORTANT" or "YOU MUST" for better adherence

### Curate Allowed Tools

Manage allowed tools through:
- **Select "Always allow"** when prompted
- **Use `/permissions` command** to add/remove tools
- **Manually edit** `.claude/settings.json` or `~/.claude.json` 
- **Use `--allowedTools` CLI flag** for session-specific permissions

### Install GitHub CLI

Install `gh` CLI for Claude to interact with GitHub (create issues, open PRs, read comments, etc.).

## 2. Give Claude More Tools

### Use Bash Tools

Claude inherits your bash environment. For custom tools:
1. Tell Claude the tool name with usage examples
2. Tell Claude to run `--help` for documentation  
3. Document frequently used tools in `CLAUDE.md`

### Use MCP (Model Context Protocol)

Claude Code functions as both MCP server and client. Configure MCP servers:
- **In project config** (available when running in that directory)
- **In global config** (available in all projects)
- **In checked-in `.mcp.json`** (available to team)

Use `--mcp-debug` flag to identify configuration issues.

### Use Custom Slash Commands

Store prompt templates in `.claude/commands/` folder as Markdown files. Use `$ARGUMENTS` for parameters.

**Example** (`.claude/commands/fix-github-issue.md`):
```markdown
Please analyze and fix the GitHub issue: $ARGUMENTS.

Follow these steps:
1. Use `gh issue view` to get issue details
2. Understand the problem
3. Search codebase for relevant files
4. Implement necessary changes
5. Write and run tests
6. Ensure code passes linting
7. Create descriptive commit message
8. Push and create PR
```

Usage: `/project:fix-github-issue 1234`

## 3. Common Workflows

### Explore, Plan, Code, Commit

1. **Ask Claude to read relevant files** - explicitly tell it not to write code yet
   - Consider using subagents for complex problems
2. **Ask Claude to make a plan** - use "think" to trigger extended thinking mode
   - Thinking levels: "think" < "think hard" < "think harder" < "ultrathink"
3. **Ask Claude to implement solution** - have it verify reasonableness
4. **Ask Claude to commit and create PR** - update READMEs/changelogs

### Write Tests, Commit; Code, Iterate, Commit

Test-driven development workflow:
1. **Ask Claude to write tests** - be explicit about TDD to avoid mock implementations
2. **Tell Claude to run tests and confirm they fail** - no implementation yet
3. **Ask Claude to commit tests** when satisfied
4. **Ask Claude to write code that passes tests** - don't modify tests
5. **Ask Claude to commit code** once satisfied

### Write Code, Screenshot Result, Iterate

Visual development workflow:
1. **Give Claude screenshot capability** (Puppeteer MCP, iOS simulator MCP, manual screenshots)
2. **Give Claude visual mock** (copy/paste, drag-drop, file path)
3. **Ask Claude to implement design** and iterate until matching mock
4. **Ask Claude to commit** when satisfied

### Safe YOLO Mode

Use `claude --dangerously-skip-permissions` to bypass permission checks for:
- Fixing lint errors
- Generating boilerplate code

**Safety**: Use in container without internet access. See [reference implementation](https://github.com/anthropics/claude-code/tree/main/.devcontainer).

### Codebase Q&A

Use Claude for learning and exploration:
- How does logging work?
- How do I make a new API endpoint?
- What does this code do?
- What edge cases does this handle?
- Why use this approach instead of that?

### Git Operations

Claude can handle 90%+ of git interactions:
- **Searching git history** - "What changes made it into v1.2.3?"
- **Writing commit messages** - based on changes and recent history
- **Complex git operations** - reverting files, resolving conflicts, comparing patches

### GitHub Operations

- **Creating pull requests** - understands "pr" shorthand
- **Implementing code review fixes** - tell it to fix PR comments
- **Fixing failing builds** or linter warnings
- **Categorizing and triaging issues** - loop over open GitHub issues

### Jupyter Notebooks

- Read and write notebooks side-by-side in VS Code
- Interpret outputs including images
- Ask Claude to make notebooks "aesthetically pleasing" for sharing

## 4. Optimize Your Workflow

### Be Specific in Instructions

| Poor | Good |
|------|------|
| add tests for foo.py | write a new test case for foo.py, covering the edge case where the user is logged out. avoid mocks |
| why does ExecutionFactory have such a weird api? | look through ExecutionFactory's git history and summarize how its api came to be |

### Give Claude Images

Methods:
- **Paste screenshots** (macOS: cmd+ctrl+shift+4, then ctrl+v)
- **Drag and drop** images into prompt
- **Provide file paths** for images

### Mention Files

Use tab-completion to reference files or folders anywhere in repository.

### Give Claude URLs

Paste URLs for Claude to fetch and read. Use `/permissions` to allowlist domains.

### Course Correct Early and Often

Tools for course correction:
- **Ask Claude to make a plan** before coding
- **Press Escape** to interrupt Claude during any phase
- **Double-tap Escape** to jump back in history and edit previous prompt
- **Ask Claude to undo changes** to take different approach

### Use `/clear` to Keep Context Focused

Reset context window between tasks to maintain performance and focus.

### Use Checklists and Scratchpads

For complex workflows:
1. **Tell Claude to create Markdown checklist** of all tasks
2. **Instruct Claude to address each issue one by one** and check off completed items

### Pass Data into Claude

Methods:
- **Copy and paste** directly into prompt
- **Pipe into Claude Code** (`cat foo.txt | claude`)
- **Tell Claude to pull data** via bash/MCP/slash commands
- **Ask Claude to read files** or fetch URLs

## 5. Headless Mode for Automation

Use `-p` flag with prompt for non-interactive contexts (CI, pre-commit hooks, build scripts).

### Issue Triage

Automate GitHub issue labeling when new issues are created.

### Subjective Code Reviews

Provide reviews beyond traditional linting (typos, stale comments, misleading names).

## 6. Multi-Claude Workflows

### Verification Pattern

1. Use Claude to write code
2. Run `/clear` or start second Claude
3. Have second Claude review first Claude's work
4. Third Claude edits based on feedback

### Multiple Checkouts

1. **Create 3-4 git checkouts** in separate folders
2. **Open each in separate terminal tabs**
3. **Start Claude in each** with different tasks
4. **Cycle through** to check progress

### Git Worktrees

Lighter-weight alternative to multiple checkouts:
1. **Create worktrees**: `git worktree add ../project-feature-a feature-a`
2. **Launch Claude in each**: `cd ../project-feature-a && claude`
3. **Clean up**: `git worktree remove ../project-feature-a`

### Headless Mode with Custom Harness

**Fanning Out Pattern:**
1. Generate task list with Claude
2. Loop through tasks with `claude -p "<task>" --allowedTools <tools>`
3. Refine prompt based on results

**Pipelining Pattern:**
```bash
claude -p "<your prompt>" --json | your_command
```

Use `--verbose` flag for debugging.

---

## Key Takeaways

- **Setup is crucial** - invest time in `CLAUDE.md` files and tool configuration
- **Be specific** - clear instructions reduce course corrections
- **Iterate frequently** - Claude improves with feedback loops
- **Use visual feedback** - screenshots and tests provide clear targets
- **Leverage multiple instances** - parallel workflows increase productivity
- **Course correct early** - guide Claude's approach for better results

> Claude Code doesn't impose workflows but enables flexible, powerful agentic coding through these proven patterns.