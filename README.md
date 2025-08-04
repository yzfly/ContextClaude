# ContextX

<div align="center">
  <img src="https://files.mdnice.com/user/43439/1668dfff-b9b9-4b58-a228-0009bcb8e47d.jpg" alt="ContextX Logo" width="400"/>
  <br/><br/>
  
![GitHub stars](https://img.shields.io/github/stars/yzfly/ContextX?style=social)
![GitHub forks](https://img.shields.io/github/forks/yzfly/ContextX?style=social)
![GitHub issues](https://img.shields.io/github/issues/yzfly/ContextX)
![GitHub license](https://img.shields.io/github/license/yzfly/ContextX)
![Version](https://img.shields.io/badge/version-1.0-blue)

**From Documents to Code - Let AI Understand Your Complete Intent**

Context engineering framework based on Claude Code that intelligently analyzes project complexity and automatically selects the most suitable development workflow.

**Core Advantages:** One-Click Project Generation • Context-Aware • Zero-Config Startup

English | [简体中文](./README_CN.md)

</div>

## 📖 Project Overview

### What is this?
ContextX is a context engineering framework powered by Claude Code that automatically generates complete code projects from your documents and requirement descriptions.

### What problems does it solve?
- **Requirement Communication Difficulties**: AI struggles to understand complete context of complex projects
- **Low Development Efficiency**: Repetitive project setup and configuration work
- **Unstable Quality**: Lack of standardized AI-assisted development processes

### Use Cases
- 🚀 Rapid prototype development
- 📚 Document-based project implementation
- 🔄 Automated generation of repetitive projects
- 🧠 AI-assisted development of complex business logic

## ⚡ Quick Start

### Prerequisites
- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) > 1.0.61
- Git

### 3-Step Setup

**Step 1: Get the Framework**
```bash
git clone https://github.com/yzfly/ContextX.git
cd ContextX
```

**Step 2: Prepare Project Context**
```bash
# Prepare your project materials in the .contextx directory
.contextx/
├── TASK_PROMPT.md          # Project requirements description
├── data/                   # Related documentation
└── examples/               # Reference code (optional)
```

**Step 3: Generate Project**
```bash
# Claude Code will automatically analyze complexity and choose the right workflow
claude "/create Your project requirements description"
```

### First Project Example

Create a simple Todo application:

```bash
# 1. Prepare requirements description
echo "Create a React Todo app with add, delete, and mark complete functionality" > .contextx/TASK_PROMPT.md

# 2. Run generation command
claude "/create React-based Todo application"

# 3. View generated results
ls -la  # Check generated project files
```

## 💡 Core Concepts

### Context-Driven Development
AI programming paradigm driven by Markdown documents, where documents serve as the project's knowledge base and context source.

**Philosophy:** *We used to program with code, now we program with documents*

### Intelligent Complexity Analysis
System automatically analyzes task complexity and selects the most suitable agent workflow:

```
Simple Tasks  →  Builder Agent (Direct Build)
Medium Tasks  →  Designer → Builder (Design then Build)  
Complex Tasks →  Learner → Designer → Builder (Learn-Design-Build)
```

### Agent Collaboration Model

| Agent | Responsibility | Trigger Condition |
|-------|----------------|-------------------|
| **Learner** | Learn and organize external docs, analyze technical specs | Need to understand complex docs or new technologies |
| **Designer** | Requirements analysis, architecture design, solution planning | Need system design and technology selection |
| **Builder** | Code implementation, documentation, deployment config | Required for all projects |

## 📚 Usage Guide

### Project Preparation

#### 1. Context Directory Structure
```
.contextx/
├── TASK_PROMPT.md          # [Required] Project requirements description
├── data/                   # [Optional] Related documentation
│   ├── api_docs.md         # API documentation
│   ├── business_rules.md   # Business rules
│   └── web_docs.md         # Web links (auto-fetch)
├── examples/               # [Optional] Reference code
│   ├── template.js         # Code templates
│   └── reference.py        # Reference implementations
├── knowledge/              # [Auto-generated] Structured knowledge base
└── PRC.md                  # [Auto-generated] Project requirements document
```

#### 2. TASK_PROMPT.md Writing Guide

**Basic Template:**
```markdown
# Project Requirements

## Project Overview
[One-sentence description of project goal]

## Functional Requirements
- [ ] Feature 1
- [ ] Feature 2

## Technical Requirements
- Programming Language:
- Framework Choice:
- Database: (if needed)

## Special Requirements
[Any special implementation requirements or constraints]
```

**Advanced Template (Complex Projects):**
```markdown
# Project Requirements

## Background
[Project background and business value]

## User Stories
- As a [user role], I want [feature description], so that [business value]

## Functional Requirements
### Core Features
- [ ] Detailed feature description 1
- [ ] Detailed feature description 2

### Extended Features
- [ ] Optional feature 1

## Non-Functional Requirements
- Performance Requirements:
- Security Requirements:
- Maintainability:

## Technical Architecture
- Frontend Tech Stack:
- Backend Tech Stack:
- Data Storage:
- Deployment Method:

## Constraints
[Technical constraints, time constraints, etc.]
```

### Configuration Instructions

#### Tool Integration (Optional)
To enhance network search capabilities, install:

```bash
# 1. Install context-mcp-server
claude mcp add context-mcp-server -e CONTEXT_DIR=$(pwd)/context/knowledge -- uvx context-mcp-server

# 2. Verify installation
claude mcp list
```

#### Web Document Retrieval
Add web links to fetch in `data/web_docs.md`:

```markdown
# Web Document Links

## API Documentation
- https://docs.example.com/api/v1
- https://developer.example.com/guides

## Technical References
- https://framework.example.com/docs
```

### Best Practices

#### ✅ Recommended Practices
- Be specific and clear in requirement descriptions, avoid vague statements
- Provide relevant technical documentation and API specifications
- Include specific functional examples or use cases
- Clearly specify tech stack and architecture preferences

#### ❌ Things to Avoid
- Requirements that are too simple or too complex
- Lack of key technical constraint explanations
- Insufficient contextual information
- Frequent requirement changes causing context inconsistency

## 🏗️ Architecture Design

### System Architecture Diagram

```
                    ┌───────────────────────────────┐
                    │        /create Entry          │
                    │   Intelligent Complexity      │
                    │        Analysis               │
                    └──────────────┬────────────────┘
                                   │
            ┌──────────────────────┼──────────────────────┐
            │                      │                      │
            ▼                      ▼                      ▼
    ┌───────────────┐      ┌───────────────┐      ┌───────────────┐
    │ Simple Tasks  │      │ Medium Tasks  │      │ Complex Tasks │
    │               │      │               │      │               │
    │   Builder     │      │  Designer     │      │   Learner     │
    │               │      │     ↓         │      │     ↓         │
    │               │      │  Builder      │      │  Designer     │
    │               │      │               │      │     ↓         │
    │               │      │               │      │  Builder      │
    └───────┬───────┘      └───────┬───────┘      └───────┬───────┘
            │                      │                      │
            └──────────────────────┼──────────────────────┘
                                   │
                                   ▼
                    ┌───────────────────────────────┐
                    │    Complete Project Delivery  │
                    │ • Fully functional source code│
                    │ • Detailed project docs       │
                    │ • Deployment configuration    │
                    │ • Usage instructions          │
                    └───────────────────────────────┘
```

### Detailed Agent Design

#### Learner Agent
**Responsibility:** Knowledge organization and learning
- Parse external documents and technical specifications
- Process web resources and API documentation
- Generate structured knowledge base
- Provide accurate technical background for subsequent agents

**Input:** Raw documents, web links, technical materials
**Output:** Structured documents in `.contextx/knowledge/` directory

#### Designer Agent  
**Responsibility:** Requirements analysis and architecture design
- Analyze project requirements and technical specifications
- Design system architecture and module structure
- Formulate technology selection and implementation plans
- Generate detailed project requirements document (PRC.md)

**Input:** Task requirements, knowledge base, technical constraints
**Output:** `.contextx/PRC.md` project requirements document

#### Builder Agent
**Responsibility:** Code implementation and project delivery
- Write complete code based on requirements and design
- Implement all functional modules and interfaces
- Generate configuration files and deployment scripts
- Write project documentation and usage instructions

**Input:** Project requirements document, design plan, code examples
**Output:** Complete project code and documentation

### Decision Flow

```
User Input Requirements
    │
    ▼
┌─────────────────┐
│ Requirements    │ ── Generate/optimize TASK_PROMPT.md
│ Preprocessing   │
└─────┬───────────┘
      │
      ▼
┌─────────────────┐
│ Context Resource│ ── Check data/, examples/ directories
│ Scanning        │
└─────┬───────────┘
      │
      ▼
┌─────────────────┐     ┌─────────────────┐
│ Intelligent     │ ──→ │ Simple: Direct  │
│ Complexity      │     │ Build           │
│ Assessment      │     └─────────────────┘
│                 │     ┌─────────────────┐
│ Assessment      │ ──→ │ Medium: Design  │
│ Dimensions:     │     │ + Build         │
│ • Requirement   │     └─────────────────┘
│   Clarity       │     ┌─────────────────┐
│ • Technical     │ ──→ │ Complex: Full   │
│   Difficulty    │     │ Process         │
│ • External      │     └─────────────────┘
│   Dependencies  │
│ • Material      │
│   Completeness  │
└─────────────────┘
```

## ⚙️ Advanced Configuration

### Custom Agents

Create custom agents in the `.claude/agents/` directory:

```markdown
# custom_agent.md

## Role Definition
[Description of agent's responsibilities and capabilities]

## Workflow
[Detailed work steps]

## Input/Output
- Input: [Expected input format]
- Output: [Output format and content]
```

### Extension Commands

Add custom commands in the `.claude/commands/` directory:

```markdown
# custom_command.md

## Command Description
/custom - Custom functionality description

## Usage
[Specific usage of the command]

## Parameter Description
[Detailed parameter explanations]
```

### Configuration File

Edit `.claude/settings.local.json` for personalized configuration:

```json
{
  "default_model": "claude-3-5-sonnet-20241022",
  "context_window": 200000,
  "temperature": 0.7,
  "custom_settings": {
    "preferred_language": "en-US",
    "code_style": "standard",
    "documentation_level": "detailed"
  }
}
```

## 📖 Usage Examples

### Example 1: Web Application Development

```bash
# 1. Prepare requirements
cat > .contextx/TASK_PROMPT.md << EOF
# Blog Management System

## Project Overview
Create a simple blog management system supporting CRUD operations for articles.

## Functional Requirements
- [ ] User registration and login
- [ ] Article list display
- [ ] Article detail view
- [ ] Article editing and publishing
- [ ] Article deletion

## Technical Requirements
- Frontend: React + TypeScript
- Backend: Node.js + Express
- Database: SQLite
- Styling: Tailwind CSS
EOF

# 2. Run generation
claude "/create Blog management system development"
```

### Example 2: API Integration Project

```bash
# 1. Prepare API documentation
mkdir -p .contextx/data
cat > .contextx/data/api_docs.md << EOF
# Third-party API Documentation

## User API
- GET /api/users - Get user list
- POST /api/users - Create user
- PUT /api/users/:id - Update user
- DELETE /api/users/:id - Delete user

## Authentication
Bearer Token authentication
EOF

# 2. Prepare requirements
cat > .contextx/TASK_PROMPT.md << EOF
# User Management Client

Based on the provided API documentation, create a user management frontend application.
Need to implement complete CRUD operations with user-friendly interface.
EOF

# 3. Run generation
claude "/create User management client development"
```

## ❓ FAQ

### Q: How to improve the quality of generated code?
A: 
- Provide detailed requirement descriptions and technical constraints
- Include relevant API documentation and business rules
- Provide code examples and reference implementations
- Clearly specify tech stack and architecture preferences

### Q: What if the generated project doesn't meet expectations?
A: 
- Check if `.contextx/TASK_PROMPT.md` is clearly described
- Add necessary technical documentation to the `data/` directory
- You can iteratively optimize by appending requirements
- Review the generated `PRC.md` to see if the system understands correctly

### Q: How to handle complex business logic?
A: 
- Break complex logic into multiple clear functional points
- Provide business flow charts or state diagrams
- Include specific business rule documentation
- Provide code examples of similar business scenarios

## 🤝 Contributing

We welcome all forms of contributions!

### How to Contribute

1. **Fork the project** and create a feature branch
2. **Write code** and necessary tests
3. **Update documentation** to keep it in sync with code
4. **Submit PR** and describe your changes

### Development Workflow

```bash
# 1. Clone your fork
git clone https://github.com/your-username/ContextX.git
cd ContextX

# 2. Create feature branch
git checkout -b feature/your-feature-name

# 3. Develop
# ... your code changes

# 4. Commit changes
git add .
git commit -m "feat: add your feature description"

# 5. Push branch
git push origin feature/your-feature-name

# 6. Create Pull Request
```

### Contribution Types

- 🐛 **Bug Fixes**: Fix known issues
- ✨ **New Features**: Add new functionality
- 📚 **Documentation Improvements**: Enhance docs and examples
- 🔧 **Tool Optimization**: Improve development tools and processes
- 🌐 **Internationalization**: Add multi-language support

## 📜 License

This project is open source under the [Apache 2.0 License](./LICENSE).

## 🙏 Acknowledgments

Thanks to all developers and users who contributed to the project!

**Core Contributors:**
- [yzfly](https://github.com/yzfly) - Project creator and main maintainer

**Special Thanks:**
- [Anthropic](https://www.anthropic.com/) for Claude Code technical support
- All community members who provided feedback and suggestions

## 🔗 Related Links

- **Official Documentation**: [Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- **Issue Reports**: [GitHub Issues](https://github.com/yzfly/ContextX/issues)
- **Feature Requests**: [Feature Request](https://github.com/yzfly/ContextX/issues/new?template=feature_request.md)
- **Project Discussions**: [GitHub Discussions](https://github.com/yzfly/ContextX/discussions)

## 💬 Community Support

- **WeChat Group**: Add WeChat (1796060717) to join LangGPT exchange group
- **WeChat Official Account**: 云中江树 (Cloud Tree)
- **GitHub**: [@yzfly](https://github.com/yzfly)

---

<div align="center">

**If this project helps you, please give it a ⭐️**

[![Star History Chart](https://www.star-history.com/#yzfly/ContextX&Date)](https://www.star-history.com/#yzfly/ContextX&Date)

Made with ❤️ by [yzfly](https://github.com/yzfly)

</div>