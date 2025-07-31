# ContextX 架构文档

> 本文档详细介绍 ContextX 的系统架构、设计理念和技术实现。

## 📋 目录

- [设计理念](#-设计理念)
- [核心架构](#-核心架构)
- [智能体设计](#-智能体设计)
- [决策流程](#-决策流程)
- [目录结构](#-目录结构)
- [上下文管理](#-上下文管理)
- [工作流程](#-工作流程)
- [技术特性](#-技术特性)

## 💡 设计理念

### 核心思想
> 以前我们用代码编程，现在我们用文档编程。

ContextX 以 markdown 文档驱动的 AI 编程为核心，文档作为 Claude Code 的上下文驱动生成代码，文档作为项目的知识库，文档的内容作为项目的上下文。

### 系统特性

🧠 **智能化**: 自动分析任务复杂度，选择最适合的工作流  
📚 **上下文感知**: 充分利用用户提供的文档、示例和历史信息  
🔄 **渐进式**: 根据复杂度递增的处理流程，确保质量  
📋 **结构化**: 标准化的上下文管理和文档生成  
🎯 **专业化**: 每个智能体专注特定职责，提高效率和质量  

## 🏗️ 核心架构

### 总体架构图

```
                        ┌───────────────────────────────┐
                        │           /create             │
                        │         Entry Point           │
                        └──────────────┬────────────────┘
                                       │
                                       ▼
                        ┌───────────────────────────────┐
                        │   Smart Complexity Analysis   │
                        │ (Auto-invoke /think if needed)│
                        └──────────────┬────────────────┘
                                       │
                ┌──────────────────────┼──────────────────────┐
                │                      │                      │
                ▼                      ▼                      ▼
        ┌───────────────┐      ┌───────────────┐      ┌───────────────┐
        │  Simple Task  │      │  Medium Task  │      │ Complex Task  │
        │               │      │               │      │               │
        │     build     │      │design -> build│      │learn->design->│
        │               │      │               │      │     build     │
        └───────┬───────┘      └───────┬───────┘      └───────┬───────┘
                │                      │                      │
                ▼                      ▼                      ▼
        ┌───────────────┐      ┌───────────────┐      ┌───────────────┐
        │ builder agent │      │designer agent │      │learner agent  │
        └───────┬───────┘      └───────┬───────┘      └───────┬───────┘
                │                      │                      │
                │                      ▼                      ▼
                │              ┌───────────────┐      ┌───────────────┐
                │              │ builder agent │      │designer agent │
                │              └───────┬───────┘      └───────┬───────┘
                │                      │                      │
                │                      │                      ▼
                │                      │              ┌───────────────┐
                │                      │              │ builder agent │
                │                      │              └───────┬───────┘
                │                      │                      │
                └──────────────────────┼──────────────────────┘
                                       │
                                       ▼
                        ┌───────────────────────────────┐
                        │      Complete Code Project    │
                        │ ┌───────────────────────────┐ │
                        │ │ • Functional Code         │ │
                        │ │ • Project Documentation   │ │
                        │ │ • Configuration Files     │ │
                        │ │ • Deployment Instructions │ │
                        │ └───────────────────────────┘ │
                        └───────────────────────────────┘
```

### 架构分层说明

| 层级 | 组件 | 职责 |
|------|------|------|
| **入口层** | /create Command | 接收用户需求，启动智能分析 |
| **决策层** | Complexity Analysis | 分析任务复杂度，选择执行路径 |
| **执行层** | Agent Workflow | 根据复杂度调用相应的智能体组合 |
| **输出层** | Code Generation | 生成完整的项目代码和文档 |

## 🤖 智能体设计

### 智能体职责分工

```
┌─────────────┬──────────────────────────────────────────────────┐
│ 智能体名称   │                主要职责                             │
├─────────────┼──────────────────────────────────────────────────┤
│ learner     │ • 学习和整理外部文档资料                             │
│             │ • 处理 API 规范和技术文档                           │
│             │ • 分析代码示例和最佳实践                             │
│             │ • 生成结构化的知识库                                │
├─────────────┼──────────────────────────────────────────────────┤
│ designer    │ • 分析项目需求和技术要求                             │
│             │ • 设计系统架构和模块结构                             │
│             │ • 制定技术选型和实现方案                             │
│             │ • 生成详细的项目需求文档 (PRC.md)                    │
├─────────────┼──────────────────────────────────────────────────┤
│ builder     │ • 根据需求和设计编写完整代码                         │
│             │ • 实现所有功能模块和接口                            │
│             │ • 生成配置文件和部署脚本                            │
│             │ • 编写项目文档和使用说明                            │
└─────────────┴──────────────────────────────────────────────────┘
```

### 智能体协作模式

#### 简单任务模式
```
User Input → Builder Agent → Complete Project
```

#### 中等任务模式
```
User Input → Designer Agent → Builder Agent → Complete Project
```

#### 复杂任务模式
```
User Input → Learner Agent → Designer Agent → Builder Agent → Complete Project
```

## 🔄 决策流程

### 复杂度评估决策图

```
用户输入
    │
    ▼
┌─────────────────┐
│ 需求增强与存储    │ ── 生成 TASK_PROMPT.md
└─────┬───────────┘
      │
      ▼
┌─────────────────┐
│ 上下文资源检查    │ ── 检查 data/, examples/ 目录
└─────┬───────────┘
      │
      ▼
┌─────────────────┐     ┌─────────────────┐
│ 复杂度评估决策    │ ──→ │ 简单: 直接构建    │
│                 │     └─────────────────┘
│ • 需求明确性      │     ┌──────────────────┐
│ • 技术难度        │ ──→ │ 中等: 设计+构建    │
│ • 外部依赖        │     └──────────────────┘
│ • 资料完整性      │     ┌──────────────────┐
│                 │ ──→ │ 复杂: 学习+设计+构建│
└─────────────────┘     └──────────────────┘
```

### 评估维度详解

| 评估维度 | 简单任务 | 中等任务 | 复杂任务 |
|----------|----------|----------|----------|
| **需求明确性** | 需求清晰具体 | 需求基本明确，需要细化 | 需求模糊，需要深入分析 |
| **技术难度** | 标准技术栈 | 需要技术选型 | 涉及新技术或复杂集成 |
| **外部依赖** | 无或少量依赖 | 中等外部依赖 | 大量外部API/服务依赖 |
| **资料完整性** | 无需额外资料 | 需要部分技术文档 | 需要学习大量外部文档 |

## 📁 目录结构

### 框架目录结构

```
ContextX/
├── .claude/                     ← Claude Code Framework
│   ├── agents/                  ← Claude Code Agents
│   │   ├── builder.md           ← Builder Agent Definition
│   │   ├── designer.md          ← Designer Agent Definition
│   │   ├── learner.md           ← Learner Agent Definition
│   │   └── prompter.md          ← Prompter Agent Definition
│   │
│   ├── claude-code/             ← Claude Code Documentation
│   │   ├── best_practise.md     ← Best Practices Guide
│   │   ├── cli.md               ← CLI Usage Guide
│   │   ├── common_workflow.md   ← Common Workflow Guide
│   │   ├── sdk.md               ← SDK Documentation
│   │   └── settings.md          ← Settings Configuration
│   │
│   ├── commands/                ← Claude Code Commands
│   │   ├── commit.md            ← Commit Command
│   │   ├── create.md            ← Create Command
│   │   └── think.md             ← Think Command
│   │
│   └── settings.local.json      ← Local Settings
│
├── .contextx/                    ← User Configuration Directory
│   ├── data/                    ← Raw Data & Documents
│   │   ├── api_specs.md         ← API Documentation
│   │   ├── xxx.md               ← Other Documents
│   │   └── web_docs.md          ← Web Links to Fetch
│   │
│   ├── examples/                ← Reference Code Samples
│   │
│   ├── knowledge/               ← Generated Knowledge Base
│   │   ├── xxx.md               ← Processed Documentation
│   │   ├── task_requirements.md ← Task Requirements
│   │   └── web_links.md         ← Downloaded Web Content
│   │
│   ├── TASK_PROMPT.md           ← Task Requirements (User Input)
│   └── PRC.md                   ← Generated Project Requirements
│
├── README.md                    ← Project Documentation
│
└── [Generated Project Files]    ← Final Output
    ├── src/                     ← Source Code
    ├── docs/                    ← Project Documentation  
    ├── config/                  ← Configuration Files
    └── deploy/                  ← Deployment Scripts
```

## 🗂️ 上下文管理

### 上下文管理架构

```
项目根目录/
├── .contextx/                   ← 上下文管理目录
│   ├── TASK_PROMPT.md          ← 任务需求描述提示词文档
│   ├── data/                   ← 用户提供的文档资料
│   │   ├── api_docs.md         ← API 文档
│   │   ├── xxx.md              ← 技术规范
│   │   └── xxxx.md             ← 业务规则
│   ├── examples/               ← 参考代码和模板
│   │   ├── xxx                 ← 代码模板
│   │   └── reference.py        ← 参考实现
│   ├── knowledge/              ← learner 生成的知识库
│   │   ├── processed_apis.md   ← 处理后的 API 文档
│   │   └── xxx.md              ← 处理后的其他文档
│   └── PRC.md                  ← designer 生成的项目需求
└── [生成的项目文件]
    ├── src/                    ← 源代码目录
    ├── config/                 ← 配置文件
    ├── docs/                   ← 项目文档
    └── deploy/                 ← 部署脚本
```

### 上下文目录说明

| 目录/文件 | 类型 | 说明 | 维护方式 |
|-----------|------|------|----------|
| **TASK_PROMPT.md** | 必需 | 用户任务需求描述 | 用户创建/维护 |
| **data/** | 可选 | 原始文档资料 | 用户提供 |
| **examples/** | 可选 | 参考代码和模板 | 用户提供 |
| **knowledge/** | 自动 | 结构化知识库 | Learner Agent 生成 |
| **PRC.md** | 自动 | 项目需求文档 | Designer Agent 生成 |

## 🔄 工作流程

### 逐步使用流程

```
Step 1: Get Framework
┌─────────────────────────────────────────────────────────┐
│ git clone https://github.com/yzfly/ContextX.git    │
│ cd ContextX                                        │
└─────────────────────────────────────────────────────────┘
                               │
                               ▼
Step 2: Context Preparation
┌─────────────────────────────────────────────────────────┐
│ User prepares .contextx/ directory with:                 │
│ • Raw documents in data/                                │
│ • Web links in data/web_docs.md                         │
│ • Example code in examples/                             │
│ • Task description in TASK_PROMPT.md                    │
└─────────────────────────────────────────────────────────┘
                               │
                               ▼
Step 3: Claude Code Processing
┌─────────────────────────────────────────────────────────┐
│ Claude Code automatically:                              │
│ • Processes all context materials                       │
│ • Generates structured knowledge/ directory (optional)  │
│ • Creates detailed PRC.md requirements (optional)       │
│ • Builds complete project code based on available ctx   │
└─────────────────────────────────────────────────────────┘
                               │
                               ▼
Step 4: Final Output
┌─────────────────────────────────────────────────────────┐
│ Complete project with:                                  │
│ • Functional source code                                │
│ • Comprehensive documentation                           │
│ • Ready-to-deploy configuration                         │
└─────────────────────────────────────────────────────────┘
```

### 项目设置和使用架构

```
                        ┌───────────────────────────────┐
                        │       Get Framework           │
                        │         git clone             │
                        │  github.com/yzfly/            │
                        │     ContextX.git         │
                        └──────────────┬────────────────┘
                                       │
                                       ▼
                        ┌───────────────────────────────┐
                        │    User Context Setup         │
                        │     (.contextx directory)      │
                        └──────────────┬────────────────┘
                                       │
                ┌──────────────────────┼──────────────────────┐
                │                      │                      │
                ▼                      ▼                      ▼
        ┌───────────────┐      ┌───────────────┐      ┌───────────────┐
        │ Raw Data &    │      │ Example Code  │      │ Task Prompt   │
        │ Documents     │      │               │      │               │
        │               │      │               │      │               │
        │.contextx/data/ │      │.contextx/      │      │.contextx/      │
        │               │      │examples/      │      │TASK_PROMPT.md │
        └───────┬───────┘      └───────┬───────┘      └───────┬───────┘
                │                      │                      │
                ▼                      │                      │
        ┌───────────────┐              │                      │
        │ Web Documents │              │                      │
        │     Links     │              │                      │
        │               │              │                      │
        │data/web_docs  │              │                      │
        │    .md        │              │                      │
        └───────┬───────┘              │                      │
                │                      │                      │
                └──────────────────────┼──────────────────────┘
                                       │
                                       ▼
                        ┌───────────────────────────────┐
                        │     Claude Code Processing    │
                        │   (Auto-generates based on    │
                        │       user context)           │
                        └──────────────┬────────────────┘
                                       │
                ┌──────────────────────┼──────────────────────┐
                │                      │                      │
                ▼                      ▼                      ▼
        ┌───────────────┐      ┌───────────────┐      ┌───────────────┐
        │ Structured    │      │ Project       │      │ Final Code    │
        │ Knowledge     │      │ Requirements  │      │ Generation    │
        │ Base          │      │ Document      │      │ (Based on     │
        │(Optional)     │      │(Optional)     │      │ available     │
        │.contextx/      │      │.contextx/      │      │ context)      │
        │knowledge/     │      │PRC.md         │      └───────┬───────┘
        └───────┬───────┘      └───────┬───────┘              │
                │                      │                      │
                └──────────────────────┼──────────────────────┘
                                       │
                                       ▼
                        ┌───────────────────────────────┐
                        │      Project Deliverables     │
                        │ ┌───────────────────────────┐ │
                        │ │ • Complete Source Code    │ │
                        │ │ • Documentation Files     │ │
                        │ │ • Configuration Setup     │ │
                        │ │ • Deployment Scripts      │ │
                        │ └───────────────────────────┘ │
                        └───────────────────────────────┘
```

## ⚙️ 技术特性

### 关键特性

- **📁 Context-Driven**: 基于用户提供的上下文进行所有开发
- **🤖 Auto-Processing**: Claude Code 处理所有文档处理工作
- **📚 Knowledge Extraction**: 自动生成知识库
- **🏗️ Complete Output**: 包含代码、文档和部署的完整项目
- **🔄 Iterative**: 可以更新上下文来优化项目

### 智能网络搜索策略

#### 搜索触发条件
当本地知识库无法满足信息需求时，自动启用网络搜索功能检索最新相关资料

#### URL访问规范
- 使用 context-mcp-server 插件进行网页访问，WebSearch 插件进行搜索操作
- **访问前缀**: 所有 URL 链接添加 `https://r.jina.ai/` 前缀。如访问失败，则去掉前缀重试
- **格式示例**: 
  ```
  原始 URL: https://docs.anthropic.com/en/docs/claude-code/mcp
  访问 URL: https://r.jina.ai/https://docs.anthropic.com/en/docs/claude-code/mcp
  ```
- **适用场景**: 特别适用于需要获取完整网页内容的场景

### 文件编码标准
- **统一编码标准**: 所有文件必须使用 UTF-8 编码格式
- **编码一致性**: 确保文件编码正确，防止字符乱码问题

### Claude Code 知识库结构

为确保 Claude Code 按照最佳实践运行，内置文档在 `.claude/claude-code` 目录中，包含全面的 Claude Code 相关文档和配置。

```
.claude/
└── claude-code/
    ├── best_practise.md      # 最佳实践指南
    ├── cli.md                # 命令行界面文档
    ├── common_workflow.md    # 常用工作流程
    ├── sdk.md                # SDK 开发文档
    └── settings.md           # 配置文档
```

---

**本架构文档持续更新中，如有疑问请参考 [项目主页](https://github.com/yzfly/ContextX) 或提交 Issue。**