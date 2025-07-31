# ContextX

<div align="center">
  <img src="https://files.mdnice.com/user/43439/1668dfff-b9b9-4b58-a228-0009bcb8e47d.jpg" alt="ContextX Logo" width="400"/>
  <br/><br/>
  
![GitHub stars](https://img.shields.io/github/stars/yzfly/ContextX?style=social)
![GitHub forks](https://img.shields.io/github/forks/yzfly/ContextX?style=social)
![GitHub issues](https://img.shields.io/github/issues/yzfly/ContextX)
![GitHub license](https://img.shields.io/github/license/yzfly/ContextX)
![Version](https://img.shields.io/badge/version-1.0-blue)

**从文档到代码，让 AI 理解你的完整意图**

基于 Claude Code 的上下文工程框架，通过智能分析项目复杂度，自动选择最适合的开发工作流。

**核心优势：** 一键式项目生成 • 上下文感知 • 零配置启动

[English](./README.md) | 简体中文

</div>

## 📖 项目概览

### 这是什么？
ContextX 是一个以 Claude Code 为核心引擎的上下文工程框架，让你通过文档和需求描述就能自动生成完整的代码项目。

### 解决什么问题？
- **需求传达困难**：AI 难以理解复杂项目的完整上下文
- **开发效率低下**：重复性的项目搭建和配置工作
- **质量不稳定**：缺乏标准化的 AI 辅助开发流程

### 适用场景
- 🚀 快速原型开发
- 📚 基于文档的项目实现
- 🔄 重复性项目的自动化生成
- 🧠 复杂业务逻辑的 AI 辅助开发

## ⚡ 快速开始

### 环境要求
- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) >= 1.0.61
- Git

### 3 步上手

**第 1 步：获取框架**
```bash
git clone https://github.com/yzfly/ContextX.git
cd ContextX
```

**第 2 步：准备项目上下文**
```bash
# 在 .context 目录中准备你的项目材料
.context/
├── TASK_PROMPT.md          # 项目需求描述
├── data/                   # 相关文档资料
└── examples/               # 参考代码（可选）
```

**第 3 步：生成项目**
```bash
# Claude Code 会自动分析复杂度并选择合适的工作流
claude "/create 你的项目需求描述"
```

### 第一个项目示例

创建一个简单的 Todo 应用：

```bash
# 1. 准备需求描述
echo "创建一个 React Todo 应用，支持添加、删除、标记完成功能" > .context/TASK_PROMPT.md

# 2. 运行生成命令
claude "/create 基于 React 的 Todo 应用"

# 3. 查看生成结果
ls -la  # 查看生成的项目文件
```

## 💡 核心概念

### 上下文驱动开发
以 Markdown 文档为驱动的 AI 编程模式，文档作为项目的知识库和上下文源。

**理念：** *以前我们用代码编程，现在我们用文档编程*

### 智能复杂度分析
系统自动分析任务复杂度，选择最适合的智能体工作流：

```
简单任务  →  Builder Agent (直接构建)
中等任务  →  Designer → Builder (设计后构建)  
复杂任务  →  Learner → Designer → Builder (学习-设计-构建)
```

### 智能体协作模式

| 智能体 | 职责 | 触发条件 |
|-------|------|----------|
| **Learner** | 学习整理外部文档、分析技术规范 | 需要理解复杂文档或新技术 |
| **Designer** | 需求分析、架构设计、方案制定 | 需要系统设计和技术选型 |
| **Builder** | 代码实现、文档编写、部署配置 | 所有项目都需要 |

## 📚 使用指南

### 项目准备

#### 1. 上下文目录结构
```
.context/
├── TASK_PROMPT.md          # 【必需】项目需求描述
├── data/                   # 【可选】相关文档资料
│   ├── api_docs.md         # API 文档
│   ├── business_rules.md   # 业务规则
│   └── web_docs.md         # 网页链接（自动抓取）
├── examples/               # 【可选】参考代码
│   ├── template.js         # 代码模板
│   └── reference.py        # 参考实现
├── knowledge/              # 【自动生成】结构化知识库
└── PRC.md                  # 【自动生成】项目需求文档
```

#### 2. TASK_PROMPT.md 编写指南

**基础模板：**
```markdown
# 项目需求

## 项目概述
[一句话描述项目目标]

## 功能需求
- [ ] 功能点 1
- [ ] 功能点 2

## 技术要求
- 编程语言：
- 框架选择：
- 数据库：（如需要）

## 特殊要求
[任何特殊的实现要求或约束]
```

**高级模板（复杂项目）：**
```markdown
# 项目需求

## 背景说明
[项目背景和业务价值]

## 用户故事
- 作为 [用户角色]，我希望 [功能描述]，以便 [业务价值]

## 功能需求
### 核心功能
- [ ] 详细功能描述 1
- [ ] 详细功能描述 2

### 扩展功能
- [ ] 可选功能 1

## 非功能需求
- 性能要求：
- 安全要求：
- 可维护性：

## 技术架构
- 前端技术栈：
- 后端技术栈：
- 数据存储：
- 部署方式：

## 约束条件
[技术约束、时间约束等]
```

### 配置说明

#### 工具集成（可选）
为增强网络搜索能力，建议安装：

```bash
# 1. 安装 context-mcp-server
claude mcp add context-mcp-server -e CONTEXT_DIR=$(pwd)/context/knowledge -- uvx context-mcp-server

# 2. 验证安装
claude mcp list
```

#### 网络文档获取
在 `data/web_docs.md` 中添加需要获取的网页链接：

```markdown
# 网络文档链接

## API 文档
- https://docs.example.com/api/v1
- https://developer.example.com/guides

## 技术参考
- https://framework.example.com/docs
```

### 最佳实践

#### ✅ 推荐做法
- 需求描述要具体明确，避免模糊表述
- 提供相关的技术文档和 API 规范
- 包含具体的功能示例或用例
- 明确技术栈和架构偏好

#### ❌ 避免事项
- 需求过于简单或过于复杂
- 缺少关键的技术约束说明
- 没有提供足够的上下文信息
- 频繁修改需求导致上下文不一致

## 🏗️ 架构设计

### 系统架构图

```
                    ┌───────────────────────────────┐
                    │        /create 入口           │
                    │     智能复杂度分析             │
                    └──────────────┬────────────────┘
                                   │
            ┌──────────────────────┼──────────────────────┐
            │                      │                      │
            ▼                      ▼                      ▼
    ┌───────────────┐      ┌───────────────┐      ┌───────────────┐
    │   简单任务     │      │   中等任务     │      │   复杂任务     │
    │               │      │               │      │               │
    │   Builder     │      │ Designer      │      │  Learner      │
    │               │      │    ↓          │      │    ↓          │
    │               │      │ Builder       │      │ Designer      │
    │               │      │               │      │    ↓          │
    │               │      │               │      │ Builder       │
    └───────┬───────┘      └───────┬───────┘      └───────┬───────┘
            │                      │                      │
            └──────────────────────┼──────────────────────┘
                                   │
                                   ▼
                    ┌───────────────────────────────┐
                    │       完整项目交付             │
                    │ • 功能完整的源代码             │
                    │ • 详细的项目文档               │
                    │ • 部署配置文件                │
                    │ • 使用说明                   │
                    └───────────────────────────────┘
```

### 智能体详细设计

#### Learner Agent
**职责：** 知识整理与学习
- 解析外部文档和技术规范
- 处理网络资源和 API 文档
- 生成结构化知识库
- 为后续智能体提供准确的技术背景

**输入：** 原始文档、网页链接、技术资料
**输出：** `.context/knowledge/` 目录下的结构化文档

#### Designer Agent  
**职责：** 需求分析与架构设计
- 分析项目需求和技术要求
- 设计系统架构和模块结构
- 制定技术选型和实现方案
- 生成详细的项目需求文档 (PRC.md)

**输入：** 任务需求、知识库、技术约束
**输出：** `.context/PRC.md` 项目需求文档

#### Builder Agent
**职责：** 代码实现与项目交付
- 根据需求和设计编写完整代码
- 实现所有功能模块和接口
- 生成配置文件和部署脚本
- 编写项目文档和使用说明

**输入：** 项目需求文档、设计方案、代码示例
**输出：** 完整的项目代码和文档

### 决策流程

```
用户输入需求
    │
    ▼
┌─────────────────┐
│  需求预处理      │ ── 生成/优化 TASK_PROMPT.md
└─────┬───────────┘
      │
      ▼
┌─────────────────┐
│ 上下文资源扫描   │ ── 检查 data/, examples/ 目录
└─────┬───────────┘
      │
      ▼
┌─────────────────┐     ┌─────────────────┐
│ 复杂度智能评估   │ ──→ │ 简单：直接构建   │
│                │     └─────────────────┘
│ 评估维度：       │     ┌─────────────────┐
│ • 需求明确性     │ ──→ │ 中等：设计+构建  │
│ • 技术难度       │     └─────────────────┘
│ • 外部依赖       │     ┌─────────────────┐
│ • 资料完整性     │ ──→ │ 复杂：全流程     │
└─────────────────┘     └─────────────────┘
```

## ⚙️ 高级配置

### 自定义智能体

可以在 `.claude/agents/` 目录下创建自定义智能体：

```markdown
# custom_agent.md

## 角色定义
[智能体的职责和能力描述]

## 工作流程
[详细的工作步骤]

## 输入输出
- 输入：[期望的输入格式]
- 输出：[输出的格式和内容]
```

### 扩展命令

在 `.claude/commands/` 目录下可以添加自定义命令：

```markdown
# custom_command.md

## 命令说明
/custom - 自定义功能描述

## 使用方法
[命令的具体使用方式]

## 参数说明
[参数的详细说明]
```

### 配置文件

编辑 `.claude/settings.local.json` 进行个性化配置：

```json
{
  "default_model": "claude-3-5-sonnet-20241022",
  "context_window": 200000,
  "temperature": 0.7,
  "custom_settings": {
    "preferred_language": "zh-CN",
    "code_style": "standard",
    "documentation_level": "detailed"
  }
}
```

## 📖 使用示例

### 示例 1：Web 应用开发

```bash
# 1. 准备需求
cat > .context/TASK_PROMPT.md << EOF
# 博客管理系统

## 项目概述
创建一个简单的博客管理系统，支持文章的增删改查。

## 功能需求
- [ ] 用户注册和登录
- [ ] 文章列表展示
- [ ] 文章详情查看
- [ ] 文章编辑和发布
- [ ] 文章删除

## 技术要求
- 前端：React + TypeScript
- 后端：Node.js + Express
- 数据库：SQLite
- 样式：Tailwind CSS
EOF

# 2. 运行生成
claude "/create 博客管理系统开发"
```

### 示例 2：API 集成项目

```bash
# 1. 准备 API 文档
mkdir -p .context/data
cat > .context/data/api_docs.md << EOF
# 第三方 API 文档

## 用户 API
- GET /api/users - 获取用户列表
- POST /api/users - 创建用户
- PUT /api/users/:id - 更新用户
- DELETE /api/users/:id - 删除用户

## 认证方式
Bearer Token 认证
EOF

# 2. 准备需求
cat > .context/TASK_PROMPT.md << EOF
# 用户管理客户端

基于提供的 API 文档，创建一个用户管理的前端应用。
需要实现完整的 CRUD 操作和用户友好的界面。
EOF

# 3. 运行生成
claude "/create 用户管理客户端开发"
```

## ❓ 常见问题

### Q: 如何提高生成代码的质量？
A: 
- 提供详细的需求描述和技术约束
- 包含相关的 API 文档和业务规则
- 提供代码示例和参考实现
- 明确指定技术栈和架构偏好

### Q: 生成的项目不符合预期怎么办？
A: 
- 检查 `.context/TASK_PROMPT.md` 是否描述清晰
- 补充必要的技术文档到 `data/` 目录
- 可以通过追加需求进行迭代优化
- 查看生成的 `PRC.md` 了解系统理解是否正确

### Q: 如何处理复杂的业务逻辑？
A: 
- 将复杂逻辑拆分为多个清晰的功能点
- 提供业务流程图或状态图
- 包含具体的业务规则文档
- 提供相似业务的代码示例

## 🤝 贡献指南

我们欢迎所有形式的贡献！

### 如何贡献

1. **Fork 项目**并创建功能分支
2. **编写代码**和必要的测试
3. **更新文档**确保与代码同步
4. **提交 PR** 并描述你的改动

### 开发流程

```bash
# 1. 克隆你的 Fork
git clone https://github.com/yzfly/ContextX.git
cd ContextX

# 2. 创建功能分支
git checkout -b feature/your-feature-name

# 3. 进行开发
# ... 你的代码改动

# 4. 提交更改
git add .
git commit -m "feat: 添加你的功能描述"

# 5. 推送分支
git push origin feature/your-feature-name

# 6. 创建 Pull Request
```

### 贡献类型

- 🐛 **Bug 修复**：修复已知问题
- ✨ **新功能**：添加新的功能特性
- 📚 **文档改进**：完善文档和示例
- 🔧 **工具优化**：改进开发工具和流程
- 🌐 **国际化**：添加多语言支持

## 📜 许可证

本项目采用 [Apache 2.0 许可证](./LICENSE) 开源。

## 🙏 致谢

感谢所有为项目做出贡献的开发者和用户！

**核心贡献者：**
- [yzfly](https://github.com/yzfly) - 项目创建者和主要维护者

**特别感谢：**
- [Anthropic](https://www.anthropic.com/) 提供的 Claude Code 技术支持
- 所有提供反馈和建议的社区成员

## 🔗 相关链接

- **官方文档**: [Claude Code Documentation](https://docs.anthropic.com/en/docs/claude-code)
- **问题反馈**: [GitHub Issues](https://github.com/yzfly/ContextX/issues)
- **功能请求**: [Feature Request](https://github.com/yzfly/ContextX/issues/new?template=feature_request.md)
- **项目讨论**: [GitHub Discussions](https://github.com/yzfly/ContextX/discussions)

## 💬 社区支持

- **微信群**: 添加云中江树微信（1796060717）加入 LangGPT 交流群
- **微信公众号**: 云中江树
- **GitHub**: [@yzfly](https://github.com/yzfly)

---

<div align="center">

**如果这个项目对你有帮助，请给它一个 ⭐️**

[![Star History Chart](https://api.star-history.com/svg?repos=yzfly/ContextX&type=Date)](https://star-history.com/#yzfly/ContextX&Date)

Made with ❤️ by [yzfly](https://github.com/yzfly)

</div>