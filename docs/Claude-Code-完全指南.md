# Claude Code 完全指南

> **版本：** 7.0.0
> **更新日期：** 2026-04-29
> **适用版本：** Claude Code CLI v1.x
> **基于参考资源：** refer.md

---

## 目录

- [序言](#序言)
  - [关于本书](#关于本书)
  - [阅读建议](#阅读建议)
- [第一部分：快速上手](#第一部分快速上手)
  - [第1章 认识 Claude Code](#第1章-认识-claude-code)
  - [第2章 安装与配置](#第2章-安装与配置)
  - [第3章 核心概念](#第3章-核心概念)
  - [第4章 目录结构](#第4章-目录结构)
  - [第5章 第一个项目](#第5章-第一个项目)
- [第二部分：功能详解](#第二部分功能详解)
  - [第6章 记忆系统](#第6章-记忆系统)
  - [第7章 钩子系统](#第7章-钩子系统)
  - [第8章 命令行工具](#第8章-命令行工具)
  - [第9章 技能系统](#第9章-技能系统)
  - [第10章 插件系统](#第10章-插件系统)
  - [第11章 提示词系统](#第11章-提示词系统)
  - [第12章 工具集](#第12章-工具集)
  - [第13章 MCP 协议](#第13章-mcp-协议)
  - [第14章 子代理系统](#第14章-子代理系统)
  - [第15章 新功能](#第15章-新功能)
- [第三部分：编写指南](#第三部分编写指南)
  - [第16章 如何写好 CLAUDE.md](#第16章-如何写好-claudemd)
  - [第17章 如何写好 Skills](#第17章-如何写好-skills)
- [第四部分：交互与工作流](#第四部分交互与工作流)
  - [第18章 交互模式](#第18章-交互模式)
  - [第19章 工作流程](#第19章-工作流程)
  - [第20章 CLI 命令大全](#第20章-cli-命令大全)
- [第五部分：实战场景](#第五部分实战场景)
  - [第21章 代码重构场景](#第21章-代码重构场景)
  - [第22章 调试与修复场景](#第22章-调试与修复场景)
  - [第23章 项目初始化场景](#第23章-项目初始化场景)
- [第六部分：深入原理](#第六部分深入原理)
  - [第24章 架构设计总览](#第24章-架构设计总览)
  - [第25章 上下文压缩机制](#第25章-上下文压缩机制)
  - [第26章 源码分析：核心引擎](#第26章-源码分析核心引擎)
  - [第27章 源码分析：扩展系统](#第27章-源码分析扩展系统)
- [第七部分：生态融合](#第七部分生态融合)
  - [第28章 CLI-Anything 集成](#第28章-cli-anything-集成)
  - [第29章 OpenCLI 集成](#第29章-opencli-集成)
- [第八部分：对比分析](#第八部分对比分析)
  - [第30章 与 Codex 智能体对比](#第30章-与-codex-智能体对比)
- [附录](#附录)
  - [A. 配置文件参考](#a-配置文件参考)
  - [B. 命令速查表](#b-命令速查表)
  - [C. 常见问题解答](#c-常见问题解答)
  - [D. 参考资源索引](#d-参考资源索引)

---

## 序言

### 关于本书

Claude Code 是 Anthropic 官方推出的 AI 编程助手命令行工具，它将 Claude AI 的强大能力直接带入您的终端。与传统的 IDE 插件不同，Claude Code 深度集成了文件系统、Git 工作流和命令行工具，能够真正理解您的项目并进行智能化的代码操作。

本书旨在为开发者提供一份全面、深入的 Claude Code 学习指南，从快速上手到源码分析，帮助您充分利用这一强大的 AI 编程工具。

### 阅读建议

- **初学者**：建议从第一部分开始，按顺序阅读
- **有经验用户**：可直接跳转到功能详解或实战场景部分
- **架构师/高级开发者**：重点关注深入原理和生态融合部分

---

## 第一部分：快速上手

### 第1章 认识 Claude Code

#### 1.1 什么是 Claude Code

Claude Code 是 Anthropic 官方推出的命令行 AI 编程助手，它具有以下核心特点：

**终端原生**
- 直接在命令行中运行，无需 IDE 插件
- 与 Shell 环境深度集成
- 支持管道操作和脚本化

**上下文感知**
- 自动理解项目结构
- 读取相关文件内容
- 保持对话历史

**工具链集成**
- 内置文件操作工具
- Git 工作流支持
- 命令执行能力

#### 1.2 与其他工具对比

| 特性    | Claude Code | Cursor | GitHub Copilot | Claude.ai |
|-------|-------------|--------|----------------|-----------|
| 运行环境  | 终端          | IDE    | IDE            | Web       |
| 项目级理解 | ✅           | ✅      | ❌              | ❌         |
| 命令执行  | ✅           | ❌      | ❌              | ❌         |
| 自定义工具 | ✅           | ❌      | ❌              | ❌         |
| 离线使用  | ❌           | ❌      | ❌              | ❌         |

#### 1.3 核心优势

1. **真正的代码理解**：能够阅读整个项目，理解代码结构和依赖关系
2. **安全的命令执行**：在用户确认后执行 Shell 命令
3. **灵活的扩展性**：支持自定义工具、技能和插件
4. **强大的记忆系统**：跨会话保持上下文和项目知识

---

### 第2章 安装与配置

#### 2.1 系统要求

- **操作系统**：macOS、Linux、Windows (WSL2)
- **Node.js**：v18.0.0 或更高版本
- **网络**：需要访问 Anthropic API

#### 2.2 安装步骤

**macOS / Linux**

```bash
# 使用 npm 全局安装
npm install -g @anthropic-ai/claude-code

# 或使用官方安装脚本
curl -fsSL https://claude.ai/install.sh | sh
```

**Windows (WSL2)**

```bash
# 在 WSL2 环境中安装
npm install -g @anthropic-ai/claude-code
```

**验证安装**

```bash
claude --version
# 输出: claude-code v1.x.x
```

#### 2.3 认证配置

```bash
# 登录 Anthropic 账户
claude login

# 或设置 API Key
export ANTHROPIC_API_KEY=your_api_key_here
```

#### 2.4 基础配置

Claude Code 的配置文件位于 `~/.claude/config.json`：

```json
{
  "model": "claude-sonnet-4-6",
  "maxTokens": 4096,
  "temperature": 0.7,
  "autoApprove": false,
  "permissions": {
    "allow": ["Read", "Glob", "Grep"],
    "deny": ["Bash"]
  }
}
```

---

### 第3章 核心概念

#### 3.1 会话（Session）

会话是 Claude Code 工作的基本单元，包含：

- **对话历史**：用户与 AI 的交互记录
- **上下文窗口**：当前可用的 Token 空间
- **工作目录**：项目根目录

```
┌─────────────────────────────────────┐
│           Session                    │
├─────────────────────────────────────┤
│  ┌─────────────────────────────┐    │
│  │     Conversation History     │    │
│  │  - User Message 1            │    │
│  │  - Assistant Response 1      │    │
│  │  - User Message 2            │    │
│  │  - Assistant Response 2      │    │
│  └─────────────────────────────┘    │
│                                      │
│  ┌─────────────────────────────┐    │
│  │     Context Window           │    │
│  │  - System Prompt             │    │
│  │  - CLAUDE.md Content         │    │
│  │  - File Contents             │    │
│  │  - Tool Definitions          │    │
│  └─────────────────────────────┘    │
│                                      │
│  Working Directory: /project/root    │
└─────────────────────────────────────┘
```

#### 3.2 工具系统

Claude Code 内置多种工具，AI 可根据需要调用：

| 工具    | 功能          | 权限级别 |
|-------|-------------|------|
| Read  | 读取文件内容      | 低风险  |
| Write | 写入文件内容      | 高风险  |
| Edit  | 编辑文件        | 高风险  |
| Bash  | 执行 Shell 命令 | 高风险  |
| Grep  | 搜索文件内容      | 低风险  |
| Glob  | 匹配文件路径      | 低风险  |

#### 3.3 工作目录

Claude Code 会自动识别项目根目录，通常基于：

1. `.git` 目录位置
2. `package.json` 或其他项目配置文件
3. 用户指定的目录

**`.claude` 目录结构**

```
.claude/
├── config.json        # 项目级配置
├── CLAUDE.md          # 项目提示词
├── memory/            # 记忆存储
│   ├── user/
│   ├── feedback/
│   └── project/
├── sessions/          # 会话历史
└── rules/             # 规则系统
    └── common/
```

#### 3.4 模型选择

Claude Code 支持多种 Claude 模型：

| 模型 | 特点 | 适用场景 |
|------|------|----------|
| **Haiku 4.5** | 快速、经济 | 简单查询、代码补全 |
| **Sonnet 4.6** | 平衡性能与成本 | 日常开发、代码重构 |
| **Opus 4.6** | 最强推理能力 | 复杂架构设计、难题分析 |

---

### 第4章 目录结构

#### 4.1 项目目录结构概述

Claude Code 的项目目录结构是其工作的基础，理解并正确配置目录结构对于高效使用 Claude Code 至关重要。

**标准项目目录结构**

```
project-root/
├── .claude/                    # Claude Code 配置目录
│   ├── CLAUDE.md              # 项目级提示词（优先级最高）
│   ├── config.json            # 项目级配置
│   ├── settings.json          # 设置文件
│   ├── memory/                # 记忆存储目录
│   │   ├── user/              # 用户偏好记忆
│   │   ├── feedback/          # 反馈记忆
│   │   ├── project/           # 项目知识记忆
│   │   └── reference/         # 外部引用记忆
│   ├── sessions/              # 会话历史存储
│   ├── skills/                # 项目级技能
│   ├── plugins/               # 项目级插件
│   └── rules/                 # 规则系统
│       └── common/            # 通用规则
├── CLAUDE.md                   # 项目根目录提示词（推荐位置）
├── .claudeignore              # 忽略文件配置
└── [项目文件...]
```

#### 4.2 .claude 目录详解

**配置文件层级**

```
┌─────────────────────────────────────────────────────────────┐
│                   配置文件优先级                              │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  优先级 1: ./.claude/CLAUDE.md    (项目级，最高)             │
│     │                                                        │
│     ▼                                                        │
│  优先级 2: ./CLAUDE.md            (根目录)                   │
│     │                                                        │
│     ▼                                                        │
│  优先级 3: ~/.claude/CLAUDE.md    (全局，最低)               │
│                                                              │
│  加载规则:                                                   │
│  - 从当前目录向上查找                                        │
│  - 多个文件按优先级合并                                      │
│  - 项目级覆盖全局配置                                        │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

**memory 目录结构**

```
memory/
├── user/                       # 用户偏好记忆
│   ├── coding-style.md        # 编码风格偏好
│   ├── tool-preferences.md    # 工具偏好
│   └── communication.md       # 沟通偏好
├── feedback/                   # 反馈记忆
│   ├── corrections.md         # 纠正记录
│   ├── validations.md         # 验证记录
│   └── learnings.md           # 学习记录
├── project/                    # 项目知识记忆
│   ├── architecture.md        # 架构知识
│   ├── conventions.md         # 项目约定
│   └── decisions.md           # 技术决策
└── reference/                  # 外部引用记忆
    ├── apis.md                # API 参考
    ├── libraries.md           # 库参考
    └── resources.md           # 资源链接
```

**sessions 目录结构**

```
sessions/
├── session-2026-04-07-001.json    # 会话记录
├── session-2026-04-07-002.json
└── session-2026-04-07-003.json

# 会话文件格式
{
  "id": "session-2026-04-07-001",
  "created": "2026-04-07T10:30:00Z",
  "workingDir": "/Users/user/project",
  "messages": [...],
  "summary": "代码重构讨论"
}
```

#### 4.3 CLAUDE.md 文件位置

**推荐位置对比**

| 位置 | 优点 | 缺点 | 推荐场景 |
|------|------|------|----------|
| `./CLAUDE.md` | 显眼、易发现 | 根目录文件多 | 新项目、小型项目 |
| `./.claude/CLAUDE.md` | 整洁、集中 | 需要进入目录查看 | 大型项目、团队项目 |
| `~/.claude/CLAUDE.md` | 全局生效 | 项目特定配置需覆盖 | 个人通用配置 |

**最佳实践**

```markdown
推荐配置方案:

project-root/
├── .claude/
│   └── CLAUDE.md          # 项目特定配置（详细）
└── CLAUDE.md              # 快速参考（简要，可选）

# .claude/CLAUDE.md 内容示例
---
name: project-config
priority: high
---

# 项目详细配置

## 技术栈
...

## 编码规范
...

# 根目录 CLAUDE.md 内容示例（可选）
> 详细配置请查看 .claude/CLAUDE.md

# 项目简介
这是一个 Vue 3 + TypeScript 项目。
```

#### 4.4 .claudeignore 文件

**作用**

`.claudeignore` 文件用于指定 Claude Code 应该忽略的文件和目录，类似于 `.gitignore`。

**语法规则**

```bash
# .claudeignore 示例

# 忽略依赖目录
node_modules/
vendor/

# 忽略构建输出
dist/
build/
out/

# 忽略临时文件
*.tmp
*.log
*.cache

# 忽略敏感文件
.env
.env.local
*.pem
*.key

# 忽略大型文件
*.min.js
*.min.css
*.map

# 忽略特定目录
coverage/
.nyc_output/

# 但不忽略重要配置
!.claude/
!CLAUDE.md
```

**与 .gitignore 的区别**

| 特性 | .claudeignore | .gitignore |
|------|---------------|------------|
| 影响范围 | Claude Code 上下文 | Git 版本控制 |
| 默认行为 | 继承 .gitignore 规则 | 独立规则 |
| 优先级 | 高于 .gitignore | - |
| 用途 | 减少上下文噪音 | 控制版本追踪 |

#### 4.5 项目初始化最佳实践

**标准初始化流程**

```bash
# 1. 创建项目目录
mkdir my-project && cd my-project

# 2. 初始化 Git（推荐）
git init

# 3. 创建 Claude Code 配置目录
mkdir -p .claude/memory .claude/sessions .claude/skills

# 4. 创建基础配置文件
cat > CLAUDE.md << 'EOF'
# 项目名称

> 一句话项目描述

## 技术栈
- 前端: Vue 3 + TypeScript
- 后端: Node.js + Express

## 编码规范
- 使用 2 空格缩进
- 函数必须有注释
- 测试覆盖率 >= 80%
EOF

# 5. 创建 .claudeignore
cat > .claudeignore << 'EOF'
node_modules/
dist/
*.log
.env
EOF

# 6. 启动 Claude Code
claude
```

**目录结构模板**

```bash
# 快速创建标准目录结构
mkdir -p .claude/{memory/{user,feedback,project,reference},sessions,skills,rules/common}
```

#### 4.6 多项目管理

**Monorepo 结构**

```
monorepo/
├── .claude/                    # 根级配置
│   └── CLAUDE.md              # 通用配置
├── packages/
│   ├── frontend/
│   │   ├── .claude/
│   │   │   └── CLAUDE.md      # 前端特定配置
│   │   └── src/
│   ├── backend/
│   │   ├── .claude/
│   │   │   └── CLAUDE.md      # 后端特定配置
│   │   └── src/
│   └── shared/
│       └── .claude/
│           └── CLAUDE.md      # 共享配置
└── CLAUDE.md                   # 根目录配置
```

**配置继承规则**

```markdown
# monorepo/.claude/CLAUDE.md
---
name: monorepo-base
scope: all
---

# Monorepo 通用配置

## 通用规范
- 所有包使用 TypeScript
- 统一的代码风格
- 共享的工具函数

# packages/frontend/.claude/CLAUDE.md
---
name: frontend-config
extends: monorepo-base
---

# 前端特定配置

## 技术栈
- Vue 3 + Vite
- Naive UI

## 继承的通用规范
[自动继承 monorepo-base 的配置]
```

#### 4.7 目录结构检查清单

```markdown
## Claude Code 项目目录检查清单

### 必需文件
- [ ] CLAUDE.md 或 .claude/CLAUDE.md
- [ ] .gitignore（如果是 Git 项目）

### 推荐文件
- [ ] .claudeignore
- [ ] .claude/config.json
- [ ] .claude/settings.json

### 目录结构
- [ ] .claude/memory/ 目录存在
- [ ] .claude/sessions/ 目录存在
- [ ] .claude/skills/ 目录存在（如需自定义技能）

### 配置内容
- [ ] CLAUDE.md 包含项目描述
- [ ] CLAUDE.md 包含技术栈说明
- [ ] CLAUDE.md 包含编码规范
- [ ] .claudeignore 排除敏感文件

### 版本控制
- [ ] .claude/ 目录已加入 .gitignore（可选）
- [ ] CLAUDE.md 已加入版本控制
- [ ] 敏感配置未加入版本控制
```

---

### 第5章 第一个项目

#### 5.1 创建示例项目

```bash
# 创建项目目录
mkdir my-first-claude-project
cd my-first-claude-project

# 初始化 Git
git init

# 创建 CLAUDE.md
cat > CLAUDE.md << 'EOF'
# 项目说明

这是一个示例项目，用于演示 Claude Code 的基本功能。

## 技术栈
- Node.js
- TypeScript

## 编码规范
- 使用 2 空格缩进
- 函数必须有注释
EOF
```

#### 5.2 基本对话操作

**启动 Claude Code**

```bash
claude
```

**代码查询**

```
用户: 请查看当前目录下有哪些文件？

Claude: 我来查看当前目录的文件结构。
[使用 Glob 工具]

当前目录下有以下文件：
- CLAUDE.md (项目配置文件)
- .git/ (Git 仓库目录)
```

**代码生成**

```
用户: 请创建一个简单的 TypeScript 函数，实现两个数字相加

Claude: 我来创建一个 TypeScript 文件，包含加法函数。
[使用 Write 工具创建 math.ts]

已创建 src/math.ts 文件：

```typescript
/**
 * 计算两个数字的和
 * @param a - 第一个数字
 * @param b - 第二个数字
 * @returns 两个数字的和
 */
export function add(a: number, b: number): number {
  return a + b;
}
```
```

#### 5.3 完成一个小功能

让我们实现一个完整的计算器模块：

```
用户: 请帮我创建一个简单的计算器模块，支持加减乘除

Claude: 我来创建一个计算器模块，包含基本的四则运算功能。

[创建 src/calculator.ts]

```typescript
/**
 * 简单计算器模块
 * 提供基本的四则运算功能
 */

/**
 * 加法运算
 */
export function add(a: number, b: number): number {
  return a + b;
}

/**
 * 减法运算
 */
export function subtract(a: number, b: number): number {
  return a - b;
}

/**
 * 乘法运算
 */
export function multiply(a: number, b: number): number {
  return a * b;
}

/**
 * 除法运算
 * @throws 当除数为 0 时抛出错误
 */
export function divide(a: number, b: number): number {
  if (b === 0) {
    throw new Error('除数不能为零');
  }
  return a / b;
}
```

同时创建测试文件：

[创建 tests/calculator.test.ts]

```typescript
import { add, subtract, multiply, divide } from '../src/calculator';

describe('Calculator', () => {
  test('加法', () => {
    expect(add(1, 2)).toBe(3);
  });

  test('减法', () => {
    expect(subtract(5, 3)).toBe(2);
  });

  test('乘法', () => {
    expect(multiply(2, 3)).toBe(6);
  });

  test('除法', () => {
    expect(divide(6, 2)).toBe(3);
  });

  test('除数为零应抛出错误', () => {
    expect(() => divide(1, 0)).toThrow('除数不能为零');
  });
});
```
```

---

## 第二部分：功能详解

### 第6章 记忆系统

#### 6.1 记忆系统概述

Claude Code 的记忆系统是其核心特性之一，它允许 AI 跨会话保持和利用信息。记忆系统分为三个层次：

```
┌────────────────────────────────────────────────┐
│                  记忆系统架构                    │
├────────────────────────────────────────────────┤
│                                                 │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────┐│
│  │  短期记忆    │  │  工作记忆    │  │ 长期记忆 ││
│  │  (Session)  │  │ (Context)   │  │(Memory) ││
│  └─────────────┘  └─────────────┘  └─────────┘│
│        │                │               │      │
│        ▼                ▼               ▼      │
│  当前会话内容      CLAUDE.md        磁盘存储    │
│  对话历史         项目配置         跨会话知识   │
│                                                 │
└────────────────────────────────────────────────┘
```

**记忆类型**

| 类型 | 存储位置 | 生命周期 | 用途 |
|------|----------|----------|------|
| 短期记忆 | 内存 | 单次会话 | 当前对话上下文 |
| 工作记忆 | CLAUDE.md | 项目级 | 项目特定知识 |
| 长期记忆 | ~/.claude/memory/ | 永久 | 跨项目知识 |

#### 6.2 CLAUDE.md 配置文件

CLAUDE.md 是 Claude Code 的核心配置文件，用于定义项目级的提示词和规则。

**文件位置优先级**

1. `./CLAUDE.md` - 当前目录
2. `./.claude/CLAUDE.md` - .claude 目录
3. `~/.claude/CLAUDE.md` - 全局配置

**配置示例**

```markdown
# 项目说明

## 技术栈
- 前端：Vue 3 + TypeScript + Naive UI
- 样式：Uno CSS
- 构建工具：Vite

## 编码规范
- 使用 2 空格缩进
- 组件命名使用 PascalCase
- 函数必须有 JSDoc 注释
- 测试覆盖率不低于 80%

## 禁止事项
- 不要使用 any 类型
- 不要在组件中直接调用 API
- 不要跳过 TypeScript 类型检查

## 工作流程
1. 新功能必须先写测试
2. 代码提交前运行 lint
3. 所有 PR 需要代码审查
```

#### 6.3 长期记忆机制

长期记忆存储在 `~/.claude/memory/` 目录下，按类型分类：

```
memory/
├── user/           # 用户偏好
│   └── preferences.md
├── feedback/       # 反馈记忆
│   └── corrections.md
├── project/        # 项目知识
│   └── architecture.md
└── reference/      # 外部引用
    └── resources.md
```

**记忆文件格式**

```markdown
---
name: 用户偏好
description: 用户的编码偏好和习惯
type: user
---

## 语言偏好
- 所有回复使用中文
- 代码注释使用英文

## 技术偏好
- 优先使用函数式编程
- 避免使用 class 组件

## Why:
用户是中文母语开发者，习惯函数式编程风格

## How to apply:
在生成代码时遵循这些偏好，除非用户明确要求其他方式
```

#### 6.4 记忆管理实践

**查看当前记忆**

```bash
# 查看所有记忆文件
ls ~/.claude/memory/

# 查看特定记忆
cat ~/.claude/memory/user/preferences.md
```

**添加记忆**

```
用户: 请记住我偏好使用 Vue 3 的 Composition API

Claude: 我已将您的偏好记录到记忆系统中。

[创建/更新 memory/user/preferences.md]
```

**清除记忆**

```bash
# 清除特定类型的记忆
rm ~/.claude/memory/user/preferences.md

# 清除所有记忆
rm -rf ~/.claude/memory/*
```

#### 6.5 高级记忆技巧

**层级化记忆组织**

```
memory/
├── user/
│   ├── coding-style.md      # 编码风格
│   ├── tool-preferences.md  # 工具偏好
│   └── communication.md     # 沟通偏好
├── project/
│   ├── frontend/
│   │   ├── components.md
│   │   └── state-management.md
│   └── backend/
│       ├── api-design.md
│       └── database.md
└── team/
    ├── conventions.md
    └── review-checklist.md
```

**团队共享记忆**

```bash
# 将记忆文件加入版本控制
git add .claude/memory/team/

# 团队成员同步记忆
git pull
```

---

### 第7章 钩子系统

#### 7.1 Hook 机制概述

钩子系统允许您在特定事件发生时执行自定义脚本，实现自动化工作流。

```
┌─────────────────────────────────────────┐
│           Hook 执行流程                  │
├─────────────────────────────────────────┤
│                                          │
│  事件触发                                 │
│     │                                    │
│     ▼                                    │
│  ┌─────────────┐                        │
│  │ PreToolUse  │ → 验证/修改参数         │
│  └─────────────┘                        │
│     │                                    │
│     ▼                                    │
│  ┌─────────────┐                        │
│  │ Tool 执行    │                        │
│  └─────────────┘                        │
│     │                                    │
│     ▼                                    │
│  ┌─────────────┐                        │
│  │ PostToolUse │ → 格式化/检查结果       │
│  └─────────────┘                        │
│     │                                    │
│     ▼                                    │
│  会话结束                                 │
│     │                                    │
│     ▼                                    │
│  ┌─────────────┐                        │
│  │ Stop        │ → 清理/报告             │
│  └─────────────┘                        │
│                                          │
└─────────────────────────────────────────┘
```

#### 7.2 Hook 类型详解

**PreToolUse**

在工具执行前触发，可用于：
- 验证参数有效性
- 修改工具参数
- 阻止危险操作

**PostToolUse**

在工具执行后触发，可用于：
- 自动格式化代码
- 运行测试
- 记录操作日志

**Stop**

在会话结束时触发，可用于：
- 清理临时文件
- 生成工作报告
- 同步状态

#### 7.3 Hook 配置

钩子配置位于 `~/.claude/settings.json`：

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write",
        "hooks": [
          {
            "type": "command",
            "command": "prettier --write \"$CLAUDE_FILE_PATH\""
          }
        ]
      },
      {
        "matcher": "Edit",
        "hooks": [
          {
            "type": "command",
            "command": "eslint --fix \"$CLAUDE_FILE_PATH\""
          }
        ]
      }
    ],
    "PreToolUse": [
      {
        "matcher": "Bash",
        "hooks": [
          {
            "type": "command",
            "command": "echo '即将执行命令: $CLAUDE_COMMAND'"
          }
        ]
      }
    ]
  }
}
```

#### 7.4 Hook 实践案例

**自动代码格式化**

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "prettier --write \"$CLAUDE_FILE_PATH\""
          }
        ]
      }
    ]
  }
}
```

**敏感信息检查**

```bash
#!/bin/bash
# ~/.claude/hooks/check-secrets.sh

FILE_PATH="$CLAUDE_FILE_PATH"

# 检查是否包含敏感信息
if grep -qE "(password|secret|api_key|token)" "$FILE_PATH"; then
  echo "警告: 文件可能包含敏感信息"
  exit 1
fi

exit 0
```

**自动运行测试**

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write|Edit",
        "hooks": [
          {
            "type": "command",
            "command": "npm test -- --findRelatedTests \"$CLAUDE_FILE_PATH\""
          }
        ]
      }
    ]
  }
}
```

---

### 第8章 命令行工具

#### 8.1 命令行概览

Claude Code 提供了丰富的命令行选项：

```bash
claude [选项] [文件...]
```

#### 8.2 核心命令详解

**启动交互会话**

```bash
# 在当前目录启动
claude

# 指定工作目录
claude --cwd /path/to/project

# 使用特定模型
claude --model claude-opus-4-6
```

**单次查询**

```bash
# 执行单个提示词
claude -p "解释这个函数的作用"

# 带文件上下文
claude -p "优化这段代码" src/utils.ts

# 从标准输入读取
cat error.log | claude -p "分析这个错误"
```

**会话管理**

```bash
# 继续上次会话
claude --continue

# 恢复特定会话
claude --resume session-2026-04-07-001

# 列出所有会话
claude --list-sessions
```

#### 8.3 文件操作命令

```bash
# 指定文件上下文
claude --file src/main.ts --file src/utils.ts

# Git 相关操作
claude --git diff HEAD~1
claude --git log --oneline -10

# 差异对比
claude --diff main..feature-branch
```

#### 8.4 配置与诊断命令

```bash
# 查看配置
claude --config show

# 设置配置项
claude --config set model claude-sonnet-4-6

# 诊断工具
claude --diagnostics

# 查看版本
claude --version
```

#### 8.5 高级命令选项

```bash
# 模型选择
claude --model claude-opus-4-6

# Token 限制
claude --max-tokens 8192

# 温度参数
claude --temperature 0.5

# 禁用工具
claude --no-tools

# 允许特定工具
claude --allow-tools Read,Write,Grep
```

#### 8.6 命令组合技巧

**管道操作**

```bash
# 分析 Git diff
git diff | claude -p "审查这些更改"

# 处理命令输出
npm test 2>&1 | claude -p "修复测试失败"
```

**Shell 脚本集成**

```bash
#!/bin/bash
# auto-review.sh

# 获取修改的文件
FILES=$(git diff --name-only HEAD~1)

# 对每个文件进行代码审查
for FILE in $FILES; do
  claude -p "审查这个文件的更改" "$FILE"
done
```

**别名配置**

```bash
# 添加到 ~/.bashrc 或 ~/.zshrc
alias cc='claude'
alias ccr='claude --continue'
alias ccp='claude -p'
alias ccd='claude --diagnostics'
```

---

### 第9章 技能系统

#### 9.1 技能系统概述

技能（Skills）是 Claude Code 的扩展机制，允许您定义可复用的提示词模板和工作流程。

**技能 vs 插件**

| 特性 | 技能 | 插件 |
|------|------|------|
| 定义方式 | Markdown 文件 | 代码包 |
| 复杂度 | 简单 | 复杂 |
| 用途 | 提示词模板 | 功能扩展 |
| 分发 | 文件复制 | npm 包 |

#### 9.2 内置技能

Claude Code 提供了丰富的内置技能：

**开发类技能**
- `tdd` - 测试驱动开发
- `code-review` - 代码审查
- `refactor-clean` - 代码重构

**文档类技能**
- `docs` - 文档生成
- `update-docs` - 文档更新
- `update-codemaps` - 代码地图更新

**构建类技能**
- `build-fix` - 构建错误修复
- `rust-build` - Rust 构建支持
- `go-build` - Go 构建支持

**调用技能**

```
用户: /tdd 实现一个用户认证功能

Claude: [加载 tdd 技能]
我将按照 TDD 流程实现用户认证功能...

1. 首先编写测试用例
2. 运行测试（应该失败）
3. 实现最小代码
4. 运行测试（应该通过）
5. 重构优化
```

#### 9.3 自定义技能开发

**技能文件结构**

```
skills/
└── my-skill.md
```

**技能定义语法**

```markdown
---
name: my-skill
description: 技能描述
trigger: 触发条件（可选）
---

# 技能名称

技能的详细说明和使用方法。

## 参数
- param1: 参数1说明
- param2: 参数2说明

## 执行步骤

1. 第一步
2. 第二步
3. 第三步

## 示例

\`\`\`
/my-skill param1 value1
\`\`\`
```

**完整示例**

```markdown
---
name: api-design
description: 设计 RESTful API 接口
---

# API 设计技能

帮助您设计符合 RESTful 规范的 API 接口。

## 参数
- resource: 资源名称
- actions: 操作列表（可选，默认为 CRUD）

## 执行步骤

1. 分析资源属性和关系
2. 设计 URL 路径
3. 定义 HTTP 方法
4. 设计请求/响应格式
5. 添加错误处理

## 输出格式

\`\`\`markdown
## 资源: {resource}

### 端点列表

| 方法 | 路径 | 描述 |
|------|------|------|
| GET | /{resource} | 列表查询 |
| GET | /{resource}/:id | 详情查询 |
| POST | /{resource} | 创建 |
| PUT | /{resource}/:id | 更新 |
| DELETE | /{resource}/:id | 删除 |

### 请求/响应示例

...
\`\`\`
```

#### 9.4 技能管理

**安装技能**

```bash
# 从本地文件安装
cp my-skill.md ~/.claude/skills/

# 从 Git 仓库安装
git clone https://github.com/user/claude-skills.git ~/.claude/skills/
```

**查看已安装技能**

```bash
claude --list-skills
```

---

### 第10章 插件系统

#### 10.1 插件系统概述

插件系统提供了比技能更强大的扩展能力，允许您：
- 添加自定义工具
- 扩展 CLI 命令
- 集成外部服务

#### 10.2 安装与管理插件

**安装插件**

```bash
# 从 npm 安装
npm install -g @anthropic-ai/claude-plugin-example

# 从 Git 仓库安装
git clone https://github.com/user/claude-plugin.git
cd claude-plugin
npm link
```

**配置插件**

在 `~/.claude/settings.json` 中配置：

```json
{
  "plugins": [
    "@anthropic-ai/claude-plugin-example",
    "./local-plugins/my-plugin"
  ]
}
```

#### 10.3 插件开发指南

**插件项目结构**

```
my-plugin/
├── package.json
├── src/
│   ├── index.ts
│   ├── tools/
│   │   └── my-tool.ts
│   └── commands/
│       └── my-command.ts
└── README.md
```

**package.json**

```json
{
  "name": "claude-plugin-my-plugin",
  "version": "1.0.0",
  "main": "dist/index.js",
  "claudePlugin": {
    "name": "my-plugin",
    "description": "我的自定义插件",
    "tools": ["my-tool"],
    "commands": ["my-command"]
  }
}
```

**工具定义**

```typescript
// src/tools/my-tool.ts
import { Tool } from '@anthropic-ai/claude-code';

export const myTool: Tool = {
  name: 'my_tool',
  description: '我的自定义工具',
  parameters: {
    type: 'object',
    properties: {
      input: {
        type: 'string',
        description: '输入参数'
      }
    },
    required: ['input']
  },
  async execute(params: { input: string }) {
    // 工具实现
    return {
      success: true,
      result: `处理结果: ${params.input}`
    };
  }
};
```

---

### 第11章 提示词系统

#### 11.1 提示词基础

好的提示词应该：
- **明确**：清楚表达意图
- **具体**：提供足够上下文
- **可执行**：AI 能够理解和执行

**常见问题**

| 问题 | 示例 | 改进 |
|------|------|------|
| 模糊 | "优化代码" | "优化 src/utils.ts 中的 sort 函数，减少时间复杂度" |
| 缺乏上下文 | "修复这个 bug" | "修复用户登录时的 401 错误，错误日志在 error.log 中" |
| 过于复杂 | "重构整个项目" | 分解为多个小任务 |

#### 11.2 CLAUDE.md 提示词

**系统指令**

```markdown
# 角色定义

你是一个资深前端工程师，专注于 Vue 3 和 TypeScript 开发。

## 专业领域
- 组件设计
- 状态管理
- 性能优化

## 工作方式
- 先理解需求，再动手实现
- 编写可测试的代码
- 注重代码可读性
```

**约束条件**

```markdown
# 编码规范

## 必须遵守
- 使用 TypeScript，禁止 any
- 组件必须有 PropTypes
- 函数必须有返回类型

## 禁止事项
- 不要使用 var
- 不要在循环中调用异步函数
- 不要忽略 ESLint 警告
```

#### 11.3 高级提示词技术

**链式提示**

```
用户: 分析 src/api/ 目录下的所有 API 调用，找出可能存在性能问题的地方

Claude: 我来分析 API 调用...

[分析完成]

发现以下潜在问题：
1. src/api/user.ts 中的 getUserList 没有分页
2. src/api/product.ts 中的 getProductDetails 在循环中调用

用户: 针对第一个问题，提供优化方案

Claude: 针对 getUserList 没有分页的问题，建议...

用户: 实现这个优化方案

Claude: 我来实现分页功能...
```

**思维链**

```markdown
在解决复杂问题时，请按照以下步骤思考：

1. **理解问题**：重述问题的核心要点
2. **分析现状**：检查相关代码和配置
3. **设计方案**：提出多个可行方案
4. **评估选择**：比较各方案的优缺点
5. **实施计划**：详细的实施步骤
6. **验证方法**：如何验证解决方案有效
```

---

### 第12章 工具集

#### 12.1 工具系统架构

```
┌─────────────────────────────────────────┐
│            工具调用流程                   │
├─────────────────────────────────────────┤
│                                          │
│  AI 决策                                 │
│     │                                    │
│     ▼                                    │
│  ┌─────────────┐                        │
│  │ 工具选择     │                        │
│  └─────────────┘                        │
│     │                                    │
│     ▼                                    │
│  ┌─────────────┐                        │
│  │ 权限检查     │                        │
│  └─────────────┘                        │
│     │                                    │
│     ▼                                    │
│  ┌─────────────┐                        │
│  │ 参数验证     │                        │
│  └─────────────┘                        │
│     │                                    │
│     ▼                                    │
│  ┌─────────────┐                        │
│  │ 执行工具     │                        │
│  └─────────────┘                        │
│     │                                    │
│     ▼                                    │
│  ┌─────────────┐                        │
│  │ 返回结果     │                        │
│  └─────────────┘                        │
│                                          │
└─────────────────────────────────────────┘
```

#### 12.2 内置工具详解

**Read - 文件读取**

```typescript
// 工具定义
{
  name: 'Read',
  description: '读取文件内容',
  parameters: {
    file_path: string,  // 文件路径
    offset?: number,    // 起始行
    limit?: number      // 行数限制
  }
}
```

**Write - 文件写入**

```typescript
// 工具定义
{
  name: 'Write',
  description: '创建或覆盖文件',
  parameters: {
    file_path: string,  // 文件路径
    content: string     // 文件内容
  }
}
```

**Edit - 文件编辑**

```typescript
// 工具定义
{
  name: 'Edit',
  description: '编辑文件内容',
  parameters: {
    file_path: string,   // 文件路径
    old_string: string,  // 要替换的内容
    new_string: string,  // 新内容
    replace_all?: boolean // 是否替换所有
  }
}
```

**Bash - 命令执行**

```typescript
// 工具定义
{
  name: 'Bash',
  description: '执行 Shell 命令',
  parameters: {
    command: string,     // 命令
    timeout?: number,    // 超时时间
    run_in_background?: boolean // 后台运行
  }
}
```

**Grep - 内容搜索**

```typescript
// 工具定义
{
  name: 'Grep',
  description: '搜索文件内容',
  parameters: {
    pattern: string,     // 正则表达式
    path?: string,       // 搜索路径
    output_mode?: string // 输出模式
  }
}
```

**Glob - 文件匹配**

```typescript
// 工具定义
{
  name: 'Glob',
  description: '匹配文件路径',
  parameters: {
    pattern: string,  // glob 模式
    path?: string     // 搜索路径
  }
}
```

#### 12.3 工具使用实践

**工具组合**

```
用户: 查找项目中所有使用了 deprecated API 的地方

Claude: 我来搜索 deprecated API 的使用情况。

[使用 Grep 工具]
grep -r "@deprecated" src/

[使用 Glob 工具]
glob "**/*.ts" src/

[使用 Read 工具]
读取相关文件内容...

找到以下位置使用了 deprecated API：
1. src/utils/old-helper.ts:15 - formatDate 函数
2. src/api/legacy.ts:42 - oldRequest 函数
```

---

### 第13章 MCP 协议

#### 13.1 MCP 协议概述

MCP（Model Context Protocol）是 Anthropic 提出的模型上下文协议，用于连接 AI 模型与外部数据源和工具。

```
┌─────────────────────────────────────────┐
│            MCP 架构                      │
├─────────────────────────────────────────┤
│                                          │
│  ┌─────────────┐     ┌─────────────┐   │
│  │ MCP Client  │ ←→  │ MCP Server  │   │
│  │ (Claude)    │     │ (外部服务)   │   │
│  └─────────────┘     └─────────────┘   │
│        │                    │           │
│        │                    │           │
│        ▼                    ▼           │
│  ┌─────────────┐     ┌─────────────┐   │
│  │ 模型调用     │     │ 资源/工具    │   │
│  └─────────────┘     └─────────────┘   │
│                                          │
└─────────────────────────────────────────┘
```

**核心概念**

- **Resources**：可读取的数据源（文件、数据库等）
- **Tools**：可调用的函数
- **Prompts**：预定义的提示词模板

#### 13.2 MCP Server 开发

**简单 MCP Server 示例**

```typescript
import { Server } from '@modelcontextprotocol/sdk';

const server = new Server({
  name: 'my-mcp-server',
  version: '1.0.0'
});

// 定义资源
server.resource('file', async (uri: string) => {
  const content = await fs.readFile(uri, 'utf-8');
  return { content };
});

// 定义工具
server.tool('read_file', {
  description: '读取文件内容',
  parameters: {
    type: 'object',
    properties: {
      path: { type: 'string' }
    },
    required: ['path']
  }
}, async (params: { path: string }) => {
  const content = await fs.readFile(params.path, 'utf-8');
  return { content };
});

// 启动服务器
server.start();
```

#### 13.3 MCP Client 集成

**配置 MCP Server**

在 `~/.claude/settings.json` 中配置：

```json
{
  "mcpServers": {
    "my-server": {
      "command": "node",
      "args": ["/path/to/mcp-server/index.js"],
      "env": {
        "API_KEY": "your-api-key"
      }
    }
  }
}
```

#### 13.4 实战案例

**数据库查询 MCP**

```typescript
import { Server } from '@modelcontextprotocol/sdk';
import { Pool } from 'pg';

const pool = new Pool({
  connectionString: process.env.DATABASE_URL
});

const server = new Server({
  name: 'postgres-mcp',
  version: '1.0.0'
});

server.tool('query', {
  description: '执行 SQL 查询',
  parameters: {
    type: 'object',
    properties: {
      sql: { type: 'string', description: 'SQL 查询语句' }
    },
    required: ['sql']
  }
}, async (params: { sql: string }) => {
  const result = await pool.query(params.sql);
  return { rows: result.rows };
});

server.start();
```

---

### 第14章 子代理系统（Subagent）

#### 14.1 子代理概述

子代理（Subagent）是 Claude Code 的核心特性之一，它允许主代理派发独立的子代理来并行处理复杂任务。这种设计模式极大地提升了 Claude Code 处理大型、复杂项目的能力。

**核心优势**

| 优势 | 说明 |
|------|------|
| **并行处理** | 多个子代理同时执行独立任务，大幅提升效率 |
| **上下文隔离** | 每个子代理拥有独立的上下文窗口，避免信息干扰 |
| **专业分工** | 不同类型的子代理专注于特定领域，提高处理质量 |
| **结果聚合** | 主代理汇总各子代理结果，形成完整解决方案 |

#### 14.2 子代理架构

```
┌─────────────────────────────────────────────────────────────┐
│                    子代理系统架构                             │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │                    主代理 (Main Agent)                │    │
│  │  ┌─────────┐  ┌─────────┐  ┌─────────┐             │    │
│  │  │ 任务    │  │ 调度器  │  │ 结果    │             │    │
│  │  │ 分析    │  │         │  │ 聚合器  │             │    │
│  │  └─────────┘  └─────────┘  └─────────┘             │    │
│  └─────────────────────────────────────────────────────┘    │
│                          │                                   │
│              ┌───────────┼───────────┐                      │
│              │           │           │                       │
│              ▼           ▼           ▼                       │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐           │
│  │ 子代理 A    │ │ 子代理 B    │ │ 子代理 C    │           │
│  │             │ │             │ │             │           │
│  │ Explore     │ │ Planner     │ │ Code Review │           │
│  │ 代码探索    │ │ 计划制定    │ │ 代码审查    │           │
│  └─────────────┘ └─────────────┘ └─────────────┘           │
│        │               │               │                     │
│        ▼               ▼               ▼                     │
│  ┌─────────────────────────────────────────────────┐       │
│  │              结果队列 (Result Queue)              │       │
│  └─────────────────────────────────────────────────┘       │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

#### 14.3 内置子代理类型

Claude Code 提供了多种内置子代理类型，每种专注于特定任务：

**通用型子代理**

| 类型 | 说明 | 典型用途 |
|------|------|----------|
| `general-purpose` | 通用代理，处理各类任务 | 复杂查询、代码搜索、多步骤任务 |
| `Explore` | 快速代码探索代理 | 查找文件、搜索代码、理解代码库结构 |

**规划型子代理**

| 类型 | 说明 | 典型用途 |
|------|------|----------|
| `Plan` | 软件架构规划代理 | 设计实现方案、识别关键文件、架构权衡 |
| `planner` | 专家规划代理 | 复杂功能实现、架构变更、重构规划 |

**代码审查型子代理**

| 类型 | 说明 | 典型用途 |
|------|------|----------|
| `code-reviewer` | 代码审查专家 | 代码质量、安全性、可维护性审查 |
| `superpowers:code-reviewer` | 增强版代码审查 | 对比原始计划、检查编码标准 |

**语言专用审查子代理**

| 类型 | 说明 | 专注领域 |
|------|------|----------|
| `python-reviewer` | Python 代码审查 | PEP 8、Pythonic 惯用语、类型提示 |
| `rust-reviewer` | Rust 代码审查 | 所有权、生命周期、unsafe 使用 |
| `go-reviewer` | Go 代码审查 | 惯用模式、并发、错误处理 |
| `java-reviewer` | Java 代码审查 | 分层架构、JPA 模式、安全性 |
| `kotlin-reviewer` | Kotlin 代码审查 | 协程安全、Compose 最佳实践 |
| `cpp-reviewer` | C++ 代码审查 | 内存安全、现代 C++ 惯用语 |

**构建解决型子代理**

| 类型 | 说明 | 典型用途 |
|------|------|----------|
| `build-error-resolver` | 构建错误解决专家 | TypeScript 错误、构建失败 |
| `rust-build-resolver` | Rust 构建专家 | Cargo 错误、借用检查器问题 |
| `go-build-resolver` | Go 构建专家 | Go vet 问题、编译警告 |
| `java-build-resolver` | Java 构建专家 | Maven/Gradle 问题 |
| `cpp-build-resolver` | C++ 构建专家 | CMake 错误、链接器问题 |

**测试型子代理**

| 类型 | 说明 | 典型用途 |
|------|------|----------|
| `tdd-guide` | TDD 专家 | 测试驱动开发、80%+ 覆盖率 |
| `e2e-runner` | E2E 测试专家 | 端到端测试、用户流程验证 |

**安全型子代理**

| 类型 | 说明 | 典型用途 |
|------|------|----------|
| `security-reviewer` | 安全漏洞检测专家 | OWASP Top 10、敏感数据、注入攻击 |

**文档型子代理**

| 类型 | 说明 | 典型用途 |
|------|------|----------|
| `doc-updater` | 文档更新专家 | 代码地图、README 更新 |
| `docs-lookup` | 文档查询代理 | 库文档、API 使用示例 |

**架构型子代理**

| 类型 | 说明 | 典型用途 |
|------|------|----------|
| `architect` | 软件架构专家 | 系统设计、可扩展性、技术决策 |

**其他专用子代理**

| 类型 | 说明 | 典型用途 |
|------|------|----------|
| `refactor-cleaner` | 代码清理专家 | 死代码删除、重复代码合并 |
| `database-reviewer` | 数据库专家 | 查询优化、Schema 设计 |
| `loop-operator` | 循环操作代理 | 自主循环、进度监控 |

#### 14.4 子代理调用方式

**通过 Agent 工具调用**

```
用户: 分析整个项目的代码质量，找出需要改进的地方

Claude: 我将派发多个子代理并行分析项目的不同方面。

[调用 code-reviewer 子代理]
[调用 security-reviewer 子代理]
[调用 database-reviewer 子代理]

正在汇总各子代理的分析结果...

## 代码质量分析报告

### 1. 代码审查结果 (code-reviewer)
- 发现 15 处代码风格问题
- 3 处潜在的性能问题
- 建议重构 2 个复杂函数

### 2. 安全审查结果 (security-reviewer)
- 发现 2 处 SQL 注入风险
- 1 处 XSS 漏洞
- 建议更新 3 个过期的依赖

### 3. 数据库审查结果 (database-reviewer)
- 发现 5 个慢查询
- 建议添加 3 个索引
- 优化 2 个 N+1 查询问题

是否需要我针对这些问题提供修复方案？
```

**并行调用多个子代理**

```typescript
// 主代理并行派发多个子代理
const results = await Promise.all([
  agent.dispatch('code-reviewer', {
    task: '审查 src/api/ 目录的代码质量'
  }),
  agent.dispatch('security-reviewer', {
    task: '检查 src/auth/ 的安全性'
  }),
  agent.dispatch('python-reviewer', {
    task: '审查 src/utils/ 的 Python 代码规范'
  })
]);

// 汇总结果
const summary = agent.aggregateResults(results);
```

#### 14.5 子代理工作流程

```
┌─────────────────────────────────────────────────────────────┐
│                    子代理工作流程                            │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  1. 任务分析                                                 │
│     │                                                        │
│     ▼                                                        │
│  ┌─────────────────────────────────────────────────┐       │
│  │ 主代理分析用户请求，判断是否需要派发子代理         │       │
│  │ - 任务复杂度评估                                  │       │
│  │ - 可并行性分析                                    │       │
│  │ - 子代理类型选择                                  │       │
│  └─────────────────────────────────────────────────┘       │
│     │                                                        │
│     ▼                                                        │
│  2. 子代理派发                                               │
│     │                                                        │
│     ▼                                                        │
│  ┌─────────────────────────────────────────────────┐       │
│  │ 创建子代理实例，分配任务                          │       │
│  │ - 设置独立上下文                                  │       │
│  │ - 传递必要参数                                    │       │
│  │ - 配置工具权限                                    │       │
│  └─────────────────────────────────────────────────┘       │
│     │                                                        │
│     ▼                                                        │
│  3. 并行执行                                                 │
│     │                                                        │
│     ▼                                                        │
│  ┌─────────────────────────────────────────────────┐       │
│  │ 各子代理独立执行任务                              │       │
│  │ - 使用专属工具集                                  │       │
│  │ - 访问文件系统                                    │       │
│  │ - 生成中间结果                                    │       │
│  └─────────────────────────────────────────────────┘       │
│     │                                                        │
│     ▼                                                        │
│  4. 结果聚合                                                 │
│     │                                                        │
│     ▼                                                        │
│  ┌─────────────────────────────────────────────────┐       │
│  │ 主代理收集并整合所有子代理的结果                   │       │
│  │ - 去重和排序                                      │       │
│  │ - 冲突解决                                        │       │
│  │ - 生成最终报告                                    │       │
│  └─────────────────────────────────────────────────┘       │
│     │                                                        │
│     ▼                                                        │
│  5. 响应用户                                                 │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

#### 14.6 子代理配置与定制

**子代理配置文件**

```json
// ~/.claude/agents/custom-reviewer.json
{
  "name": "custom-reviewer",
  "description": "自定义代码审查代理",
  "model": "claude-sonnet-4-6",
  "tools": ["Read", "Grep", "Glob"],
  "systemPrompt": "你是一个专注于代码质量的审查专家...",
  "maxTokens": 4096,
  "temperature": 0.3
}
```

**自定义子代理开发**

```typescript
// custom-agent.ts
import { Agent, AgentConfig } from '@anthropic-ai/claude-code';

const customAgentConfig: AgentConfig = {
  name: 'performance-analyzer',
  description: '性能分析专家代理',
  model: 'claude-sonnet-4-6',
  tools: ['Read', 'Grep', 'Bash'],
  systemPrompt: `
    你是一个性能分析专家，专注于：
    - 识别性能瓶颈
    - 分析算法复杂度
    - 建议优化方案

    请按照以下格式输出分析结果：
    1. 问题定位
    2. 性能影响评估
    3. 优化建议
    4. 预期收益
  `,
  maxTokens: 4096,
  temperature: 0.2
};

// 注册自定义代理
Agent.register(customAgentConfig);
```

#### 14.7 子代理最佳实践

**何时使用子代理**

| 场景 | 是否使用子代理 | 说明 |
|------|----------------|------|
| 简单代码修改 | ❌ | 主代理直接处理更高效 |
| 单文件审查 | ❌ | 不需要并行处理 |
| 多模块并行分析 | ✅ | 充分利用并行能力 |
| 安全审计 | ✅ | 使用专用安全代理 |
| 大型重构规划 | ✅ | 使用 Plan 代理规划 |
| 代码质量全面检查 | ✅ | 多代理协同工作 |

**性能优化建议**

```markdown
1. **合理选择子代理类型**
   - 简单任务使用通用代理
   - 专业任务使用专用代理
   - 避免过度派发

2. **控制并发数量**
   - 建议 3-5 个子代理并行
   - 过多会增加协调开销
   - 根据任务复杂度调整

3. **优化上下文传递**
   - 只传递必要信息
   - 避免重复传递大文件
   - 使用引用而非复制

4. **结果聚合策略**
   - 设置超时机制
   - 处理失败情况
   - 合并相似结果
```

#### 14.8 子代理源码实现（Rust）

**子代理调度器**

```rust
// src/agent/dispatcher.rs

use std::collections::HashMap;
use std::sync::Arc;
use tokio::sync::RwLock;
use async_trait::async_trait;

/// 子代理调度器
pub struct SubagentDispatcher {
    /// 已注册的子代理
    agents: Arc<RwLock<HashMap<String, Box<dyn Subagent>>>>,

    /// 执行队列
    execution_queue: Arc<RwLock<Vec<AgentTask>>>,

    /// 结果存储
    results: Arc<RwLock<HashMap<String, AgentResult>>>,

    /// 配置
    config: DispatcherConfig,
}

/// 子代理特征
#[async_trait]
pub trait Subagent: Send + Sync {
    /// 代理名称
    fn name(&self) -> &str;

    /// 代理描述
    fn description(&self) -> &str;

    /// 可用工具
    fn tools(&self) -> Vec<String>;

    /// 执行任务
    async fn execute(&self, task: AgentTask) -> Result<AgentResult, AgentError>;
}

/// 代理任务
#[derive(Debug, Clone)]
pub struct AgentTask {
    /// 任务 ID
    pub id: String,

    /// 任务描述
    pub prompt: String,

    /// 工作目录
    pub working_dir: PathBuf,

    /// 上下文文件
    pub context_files: Vec<PathBuf>,

    /// 超时时间
    pub timeout: Duration,

    /// 优先级
    pub priority: Priority,
}

/// 代理结果
#[derive(Debug, Clone)]
pub struct AgentResult {
    /// 任务 ID
    pub task_id: String,

    /// 执行状态
    pub status: AgentStatus,

    /// 输出内容
    pub output: String,

    /// 发现的问题
    pub findings: Vec<Finding>,

    /// 执行时间
    pub duration: Duration,

    /// Token 使用量
    pub token_usage: TokenUsage,
}

/// 执行状态
#[derive(Debug, Clone, PartialEq)]
pub enum AgentStatus {
    Success,
    PartialSuccess,
    Failed(String),
    Timeout,
}

/// 发现项
#[derive(Debug, Clone, Serialize)]
pub struct Finding {
    /// 严重程度
    pub severity: Severity,

    /// 文件路径
    pub file: String,

    /// 行号
    pub line: Option<usize>,

    /// 问题描述
    pub message: String,

    /// 修复建议
    pub suggestion: Option<String>,
}

/// 严重程度
#[derive(Debug, Clone, Serialize)]
pub enum Severity {
    Critical,
    High,
    Medium,
    Low,
    Info,
}

impl SubagentDispatcher {
    /// 创建新的调度器
    pub fn new(config: DispatcherConfig) -> Self {
        let mut dispatcher = Self {
            agents: Arc::new(RwLock::new(HashMap::new())),
            execution_queue: Arc::new(RwLock::new(Vec::new())),
            results: Arc::new(RwLock::new(HashMap::new())),
            config,
        };

        // 注册内置子代理
        dispatcher.register_builtin_agents();

        dispatcher
    }

    /// 注册内置子代理
    fn register_builtin_agents(&mut self) {
        self.register(Box::new(ExploreAgent::new()));
        self.register(Box::new(PlanAgent::new()));
        self.register(Box::new(CodeReviewerAgent::new()));
        self.register(Box::new(SecurityReviewerAgent::new()));
        self.register(Box::new(BuildErrorResolver::new()));
        self.register(Box::new(TddGuideAgent::new()));
        // ... 其他内置代理
    }

    /// 注册子代理
    pub fn register(&self, agent: Box<dyn Subagent>) {
        let mut agents = self.agents.blocking_write();
        agents.insert(agent.name().to_string(), agent);
    }

    /// 派发任务到子代理
    pub async fn dispatch(
        &self,
        agent_name: &str,
        task: AgentTask,
    ) -> Result<AgentResult, AgentError> {
        let agents = self.agents.read().await;

        let agent = agents.get(agent_name)
            .ok_or_else(|| AgentError::NotFound(agent_name.to_string()))?;

        // 执行任务
        let result = tokio::time::timeout(
            task.timeout,
            agent.execute(task.clone())
        ).await;

        match result {
            Ok(Ok(result)) => {
                // 存储结果
                let mut results = self.results.write().await;
                results.insert(task.id.clone(), result.clone());
                Ok(result)
            }
            Ok(Err(e)) => Err(e),
            Err(_) => Err(AgentError::Timeout(task.id)),
        }
    }

    /// 并行派发多个任务
    pub async fn dispatch_parallel(
        &self,
        tasks: Vec<(String, AgentTask)>,
    ) -> Vec<Result<AgentResult, AgentError>> {
        let futures: Vec<_> = tasks
            .into_iter()
            .map(|(agent_name, task)| self.dispatch(&agent_name, task))
            .collect();

        futures::future::join_all(futures).await
    }

    /// 获取所有可用的子代理
    pub async fn list_agents(&self) -> Vec<AgentInfo> {
        let agents = self.agents.read().await;

        agents.values().map(|agent| AgentInfo {
            name: agent.name().to_string(),
            description: agent.description().to_string(),
            tools: agent.tools(),
        }).collect()
    }
}
```

**代码审查子代理实现**

```rust
// src/agents/code_reviewer.rs

use async_trait::async_trait;
use crate::agent::{Subagent, AgentTask, AgentResult, AgentError, Finding, Severity};

/// 代码审查子代理
pub struct CodeReviewerAgent {
    model: String,
    max_tokens: usize,
}

impl CodeReviewerAgent {
    pub fn new() -> Self {
        Self {
            model: "claude-sonnet-4-6".to_string(),
            max_tokens: 4096,
        }
    }
}

#[async_trait]
impl Subagent for CodeReviewerAgent {
    fn name(&self) -> &str {
        "code-reviewer"
    }

    fn description(&self) -> &str {
        "专家代码审查代理，主动审查代码的质量、安全性和可维护性"
    }

    fn tools(&self) -> Vec<String> {
        vec![
            "Read".to_string(),
            "Grep".to_string(),
            "Glob".to_string(),
            "Bash".to_string(),
        ]
    }

    async fn execute(&self, task: AgentTask) -> Result<AgentResult, AgentError> {
        let start = std::time::Instant::now();

        // 1. 读取上下文文件
        let mut context = String::new();
        for file in &task.context_files {
            let content = tokio::fs::read_to_string(file).await?;
            context.push_str(&format!("// File: {}\n{}\n\n", file.display(), content));
        }

        // 2. 构建审查提示词
        let prompt = format!(
            r#"请审查以下代码，关注以下方面：

1. 代码质量
   - 可读性和命名规范
   - 函数复杂度
   - 代码重复

2. 安全性
   - 输入验证
   - 敏感数据处理
   - 潜在漏洞

3. 可维护性
   - 文档和注释
   - 测试覆盖
   - 依赖管理

代码内容：
{}

请以 JSON 格式输出审查结果，包含 findings 数组。"#,
            context
        );

        // 3. 调用 API
        let api_client = ApiClient::new(self.model.clone());
        let response = api_client.send_message(prompt).await?;

        // 4. 解析结果
        let findings = self.parse_findings(&response)?;

        // 5. 构建结果
        Ok(AgentResult {
            task_id: task.id,
            status: AgentStatus::Success,
            output: response,
            findings,
            duration: start.elapsed(),
            token_usage: TokenUsage {
                input: 0,  // 从 API 响应中获取
                output: 0,
            },
        })
    }
}

impl CodeReviewerAgent {
    /// 解析审查发现
    fn parse_findings(&self, response: &str) -> Result<Vec<Finding>, AgentError> {
        // 尝试从响应中提取 JSON
        let json_start = response.find('[').unwrap_or(0);
        let json_end = response.rfind(']').map(|i| i + 1).unwrap_or(response.len());

        let json_str = &response[json_start..json_end];

        let raw_findings: Vec<RawFinding> = serde_json::from_str(json_str)
            .unwrap_or_default();

        Ok(raw_findings.into_iter().map(|f| Finding {
            severity: match f.severity.as_str() {
                "critical" => Severity::Critical,
                "high" => Severity::High,
                "medium" => Severity::Medium,
                "low" => Severity::Low,
                _ => Severity::Info,
            },
            file: f.file,
            line: f.line,
            message: f.message,
            suggestion: f.suggestion,
        }).collect())
    }
}
```

**结果聚合器**

```rust
// src/agent/aggregator.rs

use std::collections::HashMap;
use crate::agent::{AgentResult, Finding, Severity};

/// 结果聚合器
pub struct ResultAggregator {
    /// 聚合策略
    strategy: AggregationStrategy,
}

/// 聚合策略
#[derive(Debug, Clone)]
pub enum AggregationStrategy {
    /// 合并所有结果
    MergeAll,

    /// 按严重程度过滤
    FilterBySeverity(Severity),

    /// 按文件分组
    GroupByFile,

    /// 去重合并
    DeduplicateMerge,
}

impl ResultAggregator {
    /// 创建新的聚合器
    pub fn new(strategy: AggregationStrategy) -> Self {
        Self { strategy }
    }

    /// 聚合多个代理结果
    pub fn aggregate(&self, results: Vec<AgentResult>) -> AggregatedResult {
        match &self.strategy {
            AggregationStrategy::MergeAll => self.merge_all(results),
            AggregationStrategy::FilterBySeverity(min_severity) => {
                self.filter_by_severity(results, min_severity)
            }
            AggregationStrategy::GroupByFile => self.group_by_file(results),
            AggregationStrategy::DeduplicateMerge => self.deduplicate_merge(results),
        }
    }

    /// 合并所有结果
    fn merge_all(&self, results: Vec<AgentResult>) -> AggregatedResult {
        let mut all_findings = Vec::new();
        let mut total_duration = Duration::ZERO;
        let mut total_tokens = TokenUsage::default();

        for result in results {
            all_findings.extend(result.findings);
            total_duration += result.duration;
            total_tokens.input += result.token_usage.input;
            total_tokens.output += result.token_usage.output;
        }

        // 按严重程度排序
        all_findings.sort_by(|a, b| {
            self.severity_order(&a.severity)
                .cmp(&self.severity_order(&b.severity))
        });

        AggregatedResult {
            findings: all_findings,
            summary: self.generate_summary(&all_findings),
            total_duration,
            total_tokens,
        }
    }

    /// 按严重程度过滤
    fn filter_by_severity(
        &self,
        results: Vec<AgentResult>,
        min_severity: &Severity,
    ) -> AggregatedResult {
        let min_level = self.severity_level(min_severity);

        let filtered_findings: Vec<Finding> = results
            .into_iter()
            .flat_map(|r| r.findings)
            .filter(|f| self.severity_level(&f.severity) >= min_level)
            .collect();

        AggregatedResult {
            findings: filtered_findings.clone(),
            summary: self.generate_summary(&filtered_findings),
            total_duration: Duration::ZERO,
            total_tokens: TokenUsage::default(),
        }
    }

    /// 按文件分组
    fn group_by_file(&self, results: Vec<AgentResult>) -> AggregatedResult {
        let mut file_groups: HashMap<String, Vec<Finding>> = HashMap::new();

        for result in results {
            for finding in result.findings {
                file_groups
                    .entry(finding.file.clone())
                    .or_insert_with(Vec::new)
                    .push(finding);
            }
        }

        let all_findings: Vec<Finding> = file_groups
            .into_values()
            .flat_map(|mut findings| {
                findings.sort_by(|a, b| {
                    self.severity_order(&a.severity)
                        .cmp(&self.severity_order(&b.severity))
                });
                findings
            })
            .collect();

        AggregatedResult {
            findings: all_findings.clone(),
            summary: self.generate_summary(&all_findings),
            total_duration: Duration::ZERO,
            total_tokens: TokenUsage::default(),
        }
    }

    /// 去重合并
    fn deduplicate_merge(&self, results: Vec<AgentResult>) -> AggregatedResult {
        let mut seen: HashSet<(String, Option<usize>, String)> = HashSet::new();
        let mut unique_findings = Vec::new();

        for result in results {
            for finding in result.findings {
                let key = (
                    finding.file.clone(),
                    finding.line,
                    finding.message.clone(),
                );

                if seen.insert(key) {
                    unique_findings.push(finding);
                }
            }
        }

        AggregatedResult {
            findings: unique_findings.clone(),
            summary: self.generate_summary(&unique_findings),
            total_duration: Duration::ZERO,
            total_tokens: TokenUsage::default(),
        }
    }

    /// 生成摘要
    fn generate_summary(&self, findings: &[Finding]) -> String {
        let mut counts = HashMap::new();

        for finding in findings {
            let severity_str = match &finding.severity {
                Severity::Critical => "Critical",
                Severity::High => "High",
                Severity::Medium => "Medium",
                Severity::Low => "Low",
                Severity::Info => "Info",
            };
            *counts.entry(severity_str).or_insert(0) += 1;
        }

        format!(
            "Total: {} findings (Critical: {}, High: {}, Medium: {}, Low: {}, Info: {})",
            findings.len(),
            counts.get("Critical").unwrap_or(&0),
            counts.get("High").unwrap_or(&0),
            counts.get("Medium").unwrap_or(&0),
            counts.get("Low").unwrap_or(&0),
            counts.get("Info").unwrap_or(&0)
        )
    }

    /// 严重程度排序值
    fn severity_order(&self, severity: &Severity) -> u8 {
        match severity {
            Severity::Critical => 0,
            Severity::High => 1,
            Severity::Medium => 2,
            Severity::Low => 3,
            Severity::Info => 4,
        }
    }

    /// 严重程度级别
    fn severity_level(&self, severity: &Severity) -> u8 {
        self.severity_order(severity)
    }
}

/// 聚合结果
#[derive(Debug)]
pub struct AggregatedResult {
    /// 所有发现
    pub findings: Vec<Finding>,

    /// 摘要
    pub summary: String,

    /// 总执行时间
    pub total_duration: Duration,

    /// 总 Token 使用量
    pub total_tokens: TokenUsage,
}
```

#### 14.9 子代理使用场景实例

**场景一：全面代码质量检查**

```
用户: 对 src/ 目录进行全面的代码质量检查

Claude: 我将派发多个专业子代理并行检查代码的不同方面。

[并行派发 5 个子代理]

1. code-reviewer: 审查代码质量和可维护性
2. security-reviewer: 检查安全漏洞
3. python-reviewer: 检查 Python 代码规范
4. database-reviewer: 检查数据库相关代码
5. refactor-cleaner: 查找死代码和重复代码

[等待所有子代理完成...]

## 综合质量报告

### 严重问题 (Critical) - 2 个
1. [security-reviewer] SQL 注入风险: src/api/users.ts:45
2. [security-reviewer] 敏感数据泄露: src/config.ts:12

### 高优先级问题 (High) - 5 个
1. [code-reviewer] 复杂度过高: src/utils/parser.ts:120
2. [database-reviewer] N+1 查询: src/services/user.ts:78
...

### 中优先级问题 (Medium) - 12 个
...

### 低优先级问题 (Low) - 8 个
...

### 信息 (Info) - 15 个
...

**总计: 42 个发现**
**建议优先处理 2 个严重问题和 5 个高优先级问题**

是否需要我针对这些问题提供详细的修复方案？
```

**场景二：大型重构规划**

```
用户: 帮我规划将这个单体应用拆分为微服务的方案

Claude: 我将使用 Plan 代理进行详细的架构分析和规划。

[派发 Plan 子代理]

Plan 代理正在分析项目结构...

[Plan 代理输出]

## 微服务拆分方案

### 阶段 1: 服务边界识别
根据代码依赖关系和业务领域，建议拆分为以下服务：

1. **用户服务 (User Service)**
   - 文件: src/users/, src/auth/
   - 数据库表: users, sessions, permissions
   - API 端点: /api/users/*, /api/auth/*

2. **订单服务 (Order Service)**
   - 文件: src/orders/, src/cart/
   - 数据库表: orders, order_items, carts
   - API 端点: /api/orders/*, /api/cart/*

3. **商品服务 (Product Service)**
   - 文件: src/products/, src/inventory/
   - 数据库表: products, categories, inventory
   - API 端点: /api/products/*

4. **支付服务 (Payment Service)**
   - 文件: src/payments/, src/invoices/
   - 数据库表: payments, invoices
   - API 端点: /api/payments/*

### 阶段 2: 迁移步骤
...

### 阶段 3: 风险评估
...

是否需要我详细展开某个阶段的具体实施步骤？
```

---

### 第15章 新功能

#### 15.1 新功能概述

Claude Code 持续迭代更新，不断引入新功能以提升开发体验。本章介绍最新的重要功能特性。

**最新功能列表**

```
┌─────────────────────────────────────────────────────────────┐
│                   Claude Code 新功能                         │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐         │
│  │ Computer    │  │ 增强型      │  │ 多模态      │         │
│  │ Use         │  │ 子代理      │  │ 支持        │         │
│  └─────────────┘  └─────────────┘  └─────────────┘         │
│        │                │               │                    │
│        ▼                ▼               ▼                    │
│  桌面操作自动化    更智能的任务派发    图像理解能力           │
│  GUI 交互         专业领域代理        截图分析               │
│                                                              │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐         │
│  │ 改进的      │  │ 增强的      │  │ 新的        │         │
│  │ 上下文管理  │  │ 安全特性    │  │ 集成能力    │         │
│  └─────────────┘  └─────────────┘  └─────────────┘         │
│        │                │               │                    │
│        ▼                ▼               ▼                    │
│  更智能的压缩      权限精细化        更多 MCP 服务器          │
│  更大的窗口        敏感数据保护      第三方集成               │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

#### 15.2 Computer Use（计算机使用）

**功能概述**

Computer Use 是 Claude Code 的革命性功能，允许 AI 直接与计算机桌面环境交互，实现真正的 GUI 自动化操作。

**核心能力**

```
┌─────────────────────────────────────────────────────────────┐
│                   Computer Use 架构                          │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │                    Claude AI                         │    │
│  │  ┌─────────┐  ┌─────────┐  ┌─────────┐             │    │
│  │  │ 理解    │  │ 规划    │  │ 执行    │             │    │
│  │  │ 屏幕内容 │  │ 操作步骤 │  │ 鼠标键盘 │             │    │
│  │  └─────────┘  └─────────┘  └─────────┘             │    │
│  └─────────────────────────────────────────────────────┘    │
│                          │                                   │
│                          ▼                                   │
│  ┌─────────────────────────────────────────────────────┐    │
│  │                   桌面环境                           │    │
│  │  ┌─────────┐  ┌─────────┐  ┌─────────┐             │    │
│  │  │ 浏览器   │  │ 应用程序 │  │ 文件系统 │             │    │
│  │  └─────────┘  └─────────┘  └─────────┘             │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

**启用 Computer Use**

```bash
# 启用 Computer Use 模式
claude --computer-use

# 或在配置中启用
# ~/.claude/settings.json
{
  "computerUse": {
    "enabled": true,
    "permissions": {
      "allow": ["browser", "files", "applications"],
      "deny": ["system-settings"]
    }
  }
}
```

**使用场景**

| 场景 | 描述 | 示例 |
|------|------|------|
| **浏览器自动化** | 自动操作网页 | 填写表单、截图、数据采集 |
| **应用程序操作** | 控制 GUI 应用 | 打开文件、编辑文档、导出数据 |
| **文件管理** | 图形化文件操作 | 整理文件夹、批量重命名 |
| **测试自动化** | GUI 测试 | 点击测试、表单验证 |

**安全配置**

```json
// ~/.claude/settings.json
{
  "computerUse": {
    "enabled": true,
    "sandbox": true,
    "permissions": {
      "allow": [
        "browser.read",
        "browser.click",
        "browser.type",
        "files.read",
        "files.write"
      ],
      "deny": [
        "system.settings",
        "system.network",
        "files.delete"
      ]
    },
    "confirmBeforeAction": true,
    "recordSession": true
  }
}
```

**使用示例**

```
用户: 打开浏览器，访问 GitHub，搜索 "claude-code" 项目

Claude: 我来执行这个任务。

[Computer Use 模式激活]

步骤 1: 打开浏览器
[执行: 打开默认浏览器]

步骤 2: 导航到 GitHub
[执行: 在地址栏输入 github.com]

步骤 3: 输入搜索关键词
[执行: 在搜索框输入 "claude-code"]

步骤 4: 执行搜索
[执行: 按 Enter 键]

搜索结果已显示。找到以下相关项目：
1. anthropics/claude-code - 官方仓库
2. shareAI-lab/learn-claude-code - 学习资源
...

是否需要我打开某个项目页面？
```

**最佳实践**

```markdown
1. **明确指令**
   - 提供清晰的操作步骤
   - 指明具体的目标元素
   - 设定预期结果

2. **安全考量**
   - 启用沙箱模式
   - 设置权限白名单
   - 记录操作日志

3. **错误处理**
   - 设置超时时间
   - 定义重试策略
   - 人工干预机制

4. **性能优化**
   - 减少不必要的操作
   - 使用快捷键替代点击
   - 批量处理任务
```

#### 15.3 增强型子代理

**新特性**

Claude Code 的子代理系统得到了显著增强：

| 特性 | 说明 |
|------|------|
| **动态子代理创建** | 根据任务自动创建专用子代理 |
| **子代理协作** | 多个子代理可以协同工作 |
| **结果缓存** | 相似任务复用之前的结果 |
| **进度追踪** | 实时查看子代理执行进度 |

**动态子代理示例**

```
用户: 分析这个项目的安全性，包括代码审计、依赖检查和配置审查

Claude: 这是一个复杂的安全分析任务，我将创建多个专用子代理并行处理。

[创建动态子代理组]

┌─────────────────────────────────────────────────────────────┐
│                  安全分析子代理组                            │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  主代理 (协调者)                                             │
│     │                                                        │
│     ├─→ security-reviewer (代码审计)                        │
│     │      └─→ 分析源代码安全漏洞                           │
│     │                                                        │
│     ├─→ dependency-auditor (依赖检查)                       │
│     │      └─→ 检查第三方依赖安全性                          │
│     │                                                        │
│     └─→ config-reviewer (配置审查)                          │
│            └─→ 审查配置文件安全性                            │
│                                                              │
│  [并行执行中...]                                             │
│  ████████████████████░░░░░░░░░░ 67%                         │
│                                                              │
└─────────────────────────────────────────────────────────────┘

[等待所有子代理完成...]

## 安全分析报告

### 代码审计结果 (security-reviewer)
发现 3 个潜在安全问题：
- SQL 注入风险: src/api/users.ts:45
- XSS 漏洞: src/components/Comment.vue:28
- 敏感数据泄露: src/utils/logger.ts:12

### 依赖检查结果 (dependency-auditor)
扫描了 156 个依赖：
- 2 个已知漏洞 (lodash < 4.17.21)
- 5 个过期依赖
- 0 个恶意包

### 配置审查结果 (config-reviewer)
发现 2 个配置问题：
- CORS 配置过于宽松
- 缺少 CSP 头配置

### 综合评估
安全评分: 72/100
建议优先处理 SQL 注入和 XSS 漏洞。
```

#### 15.4 多模态支持

**图像理解能力**

Claude Code 现在支持图像输入，可以分析截图、图表等视觉内容。

```bash
# 分析图像文件
claude -p "分析这个 UI 截图" screenshot.png

# 分析多张图像
claude -p "比较这两个设计稿的差异" design-v1.png design-v2.png
```

**使用场景**

```
用户: [上传错误截图] 这个错误怎么解决？

Claude: 我来分析这个错误截图。

[图像分析中...]

这是 TypeScript 编译错误：

错误位置: src/utils/parser.ts:45
错误类型: Type 'string | undefined' is not assignable to type 'string'

问题原因:
变量 `result` 可能是 undefined，但被赋值给需要 string 类型的变量。

修复方案:
```typescript
// 原代码
const name: string = result;

// 修复后
const name: string = result ?? '';
// 或
const name: string = result!; // 如果你确定 result 不为空
```

是否需要我直接修复这个文件？
```

**支持的图像格式**

| 格式 | 支持程度 |
|------|----------|
| PNG | ✅ 完全支持 |
| JPEG | ✅ 完全支持 |
| GIF | ✅ 支持（仅首帧） |
| WebP | ✅ 完全支持 |
| BMP | ✅ 完全支持 |

#### 15.5 改进的上下文管理

**智能上下文压缩**

Claude Code 引入了更智能的上下文压缩算法：

```
┌─────────────────────────────────────────────────────────────┐
│                   上下文压缩策略                             │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  原始上下文 (100%)                                          │
│     │                                                        │
│     ▼                                                        │
│  ┌─────────────────────────────────────────────────────┐    │
│  │              智能分析阶段                             │    │
│  │  - 识别关键信息                                      │    │
│  │  - 评估信息重要性                                    │    │
│  │  - 检测重复内容                                      │    │
│  └─────────────────────────────────────────────────────┘    │
│     │                                                        │
│     ▼                                                        │
│  ┌─────────────────────────────────────────────────────┐    │
│  │              压缩执行阶段                             │    │
│  │  - 保留关键代码片段                                  │    │
│  │  - 摘要化对话历史                                    │    │
│  │  - 压缩文件内容                                      │    │
│  └─────────────────────────────────────────────────────┘    │
│     │                                                        │
│     ▼                                                        │
│  压缩后上下文 (30-50%)                                       │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

**配置选项**

```json
// ~/.claude/settings.json
{
  "contextManagement": {
    "compressionStrategy": "intelligent",
    "preserveCodeBlocks": true,
    "maxContextRatio": 0.8,
    "summarizeThreshold": 10000,
    "priorityFiles": [
      "CLAUDE.md",
      "package.json",
      "tsconfig.json"
    ]
  }
}
```

#### 15.6 增强的安全特性

**权限精细化控制**

```json
// ~/.claude/settings.json
{
  "permissions": {
    "tools": {
      "Read": {
        "default": "allow",
        "restrictions": {
          "denyPaths": [".env", "*.pem", "*.key"]
        }
      },
      "Write": {
        "default": "ask",
        "restrictions": {
          "allowPaths": ["src/", "tests/"],
          "denyPaths": [".git/", "node_modules/"]
        }
      },
      "Bash": {
        "default": "ask",
        "restrictions": {
          "allowCommands": ["npm", "git", "node"],
          "denyCommands": ["rm -rf /", "sudo"]
        }
      }
    }
  }
}
```

**敏感数据保护**

```markdown
自动检测并保护以下敏感数据：

1. **API 密钥**
   - ANTHROPIC_API_KEY
   - OPENAI_API_KEY
   - AWS_ACCESS_KEY_ID

2. **认证信息**
   - 密码
   - Token
   - Session ID

3. **个人信息**
   - 邮箱地址
   - 手机号码
   - 身份证号

4. **私钥文件**
   - .pem 文件
   - .key 文件
   - SSH 密钥
```

#### 15.7 新的集成能力

**更多 MCP 服务器支持**

```bash
# 列出可用的 MCP 服务器
claude mcp list

# 安装官方 MCP 服务器
claude mcp install @anthropic/mcp-filesystem
claude mcp install @anthropic/mcp-database
claude mcp install @anthropic/mcp-github

# 配置自定义 MCP 服务器
claude mcp add my-server --command "node /path/to/server.js"
```

**第三方集成**

| 集成 | 说明 | 状态 |
|------|------|------|
| GitHub | 仓库操作、PR 管理 | ✅ 可用 |
| GitLab | CI/CD、Issue 管理 | ✅ 可用 |
| Jira | 任务追踪 | ✅ 可用 |
| Slack | 消息通知 | ✅ 可用 |
| Notion | 文档管理 | 🔄 开发中 |
| Figma | 设计协作 | 🔄 开发中 |

#### 15.8 功能路线图

**近期计划**

```markdown
## 2026 Q2 计划

### 已发布
- ✅ Computer Use 基础功能
- ✅ 增强型子代理
- ✅ 多模态支持
- ✅ 智能上下文压缩

### 开发中
- 🔄 Computer Use 高级功能（录音、视频）
- 🔄 更多语言专用子代理
- 🔄 团队协作功能增强

### 计划中
- 📅 本地模型支持
- 📅 离线模式增强
- 📅 自定义模型接入
```

#### 15.9 隐藏功能揭秘（源码泄露发现）

2026 年 3 月源码泄露揭示了多个尚未公开发布的隐藏功能，它们都被锁在 Feature Flag 之后：

**BUDDY——AI 虚拟宠物**

BUDDY 是一个完整的虚拟伴侣系统：

| 特性 | 详情 |
|------|------|
| 物种数量 | 18 种（鸭子、龙、蝾螈、水豚、蘑菇、幽灵等） |
| 稀有度 | 普通到传说（1% 掉落率），以及闪亮变体 |
| 属性 | 调试能力 / 耐心 / 混乱度 / 智慧 / 毒舌指数 |
| 确定性生成 | 基于 userId 哈希确定性生成，同一用户永远同一只 |
| 外观定制 | 装饰性帽子系统 |
| 交互方式 | 以对话气泡出现在输入框旁边 |

BUDDY 不是随机生成的——物种由用户 ID 的哈希值决定，意味着每个用户可以拥有独特的身份认同。

**KAIROS——全天候自治 Agent**

KAIROS 是最具深远影响的隐藏功能：

```
KAIROS 架构：
┌─────────────────────────────────────────┐
│            持续运行的后台层               │
├─────────────────────────────────────────┤
│  观察 → 记录 → 分析 → 行动              │
│                                          │
│  • 维护只可追加的每日日志                │
│  • 基于观察触发主动行动（不等待指令）     │
│  • 夜间运行"梦境"进程整合记忆            │
│  • 可监听 GitHub Webhooks、Slack 消息    │
└─────────────────────────────────────────┘
```

当前 Claude Code 是**被动响应**的——你给任务，它执行。KAIROS 将是**主动的**——一个后台层，随时间积累你工作的上下文，然后无需提示即可采取行动。

**ULTRAPLAN——云端规划**

ULTRAPLAN 将任务的规划阶段交由在云端运行的 Claude Opus 处理，时长可达 30 分钟。你可以在执行开始前通过浏览器界面监控并审批计划。

**协调者模式（Coordinator Mode）**

引入多智能体层：一个 Claude 实例通过邮箱系统管理多个并行工作智能体，每个工作者处理自己的子任务，协调者负责分配工作并整合结果。

**其他未发布功能（Feature Flag 列表）**

| 功能 | 代号 | 说明 |
|------|------|------|
| 语音模式 | `VOICE_MODE` | 与 Claude Code 的语音交互 |
| 浏览器工具 | `WEB_BROWSER_TOOL` | CLI 内部访问浏览器 |
| 守护进程 | `DAEMON` | 后台进程模式 |
| 智能体触发 | `AGENT_TRIGGERS` | 基于事件的自动化激活 |
| 火炬模式 | `TORCH` | 调试增强 |
| 主动模式 | `PROACTIVE` | 主动建议功能 |

这些功能在代码库中都有真实的实现逻辑，而非占位符。总计有 **108 个**未公开的封闭模块。

**卧底模式（Undercover Mode）**

源码中包含对 `USER_TYPE === 'ant'` 的检查（标识 Anthropic 员工）。在公共仓库工作时，系统自动进入"卧底模式"：
- 注入提示词指示"不要暴露身份"、"永远不要提及自己是 AI"
- `Co-Authored-By` 行从 git 提交中剥除
- 内部代号从回复中隐藏

这一设计的初衷是保护 Anthropic 员工的隐私，但剥除 AI 参与元数据的做法引发了社区关于信息披露规范的讨论。

---

## 第三部分：编写指南

### 第16章 如何写好 CLAUDE.md

#### 16.1 CLAUDE.md 概述

CLAUDE.md 是 Claude Code 的核心配置文件，它充当项目的"说明书"，告诉 AI 如何理解和处理您的项目。一个优秀的 CLAUDE.md 可以显著提升 Claude Code 的工作效率和输出质量。

**核心作用**

```
┌─────────────────────────────────────────────────────────────┐
│                   CLAUDE.md 核心作用                         │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐         │
│  │ 项目身份    │  │ 编码规范    │  │ 工作流程    │         │
│  │ 定义        │  │ 约束        │  │ 指导        │         │
│  └─────────────┘  └─────────────┘  └─────────────┘         │
│        │                │               │                    │
│        ▼                ▼               ▼                    │
│  技术栈说明        代码风格要求      开发流程步骤            │
│  项目结构          命名规范          测试要求                │
│  依赖关系          禁止事项          提交规范                │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │              上下文注入 (Context Injection)          │    │
│  │  每次对话自动加载，成为 AI 理解项目的基础            │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

#### 16.2 文件位置与优先级

**位置优先级**

```
优先级 1: ./CLAUDE.md          (当前目录，最高优先级)
优先级 2: ./.claude/CLAUDE.md  (.claude 目录)
优先级 3: ~/.claude/CLAUDE.md  (全局配置，最低优先级)
```

**加载规则**

```markdown
1. 从当前目录向上查找，直到找到 CLAUDE.md 或到达用户主目录
2. 多个 CLAUDE.md 文件会按优先级合并
3. 项目级配置会覆盖全局配置
4. 子目录可以有自己的 CLAUDE.md 覆盖父目录配置
```

#### 16.3 CLAUDE.md 结构模板

**完整模板**

```markdown
# 项目名称

> 一句话描述项目用途

## 项目概述

简要描述项目的目标、背景和核心功能。

## 技术栈

- **前端**: Vue 3 + TypeScript + Naive UI
- **后端**: Node.js + Express
- **数据库**: PostgreSQL
- **构建工具**: Vite
- **测试框架**: Vitest

## 项目结构

\`\`\`
project-root/
├── src/
│   ├── components/     # Vue 组件
│   ├── composables/    # 组合式函数
│   ├── api/           # API 调用
│   ├── stores/        # 状态管理
│   ├── utils/         # 工具函数
│   └── types/         # 类型定义
├── tests/             # 测试文件
├── docs/              # 文档
└── scripts/           # 脚本文件
\`\`\`

## 编码规范

### 命名约定
- 组件: PascalCase (如 `UserProfile.vue`)
- 函数: camelCase (如 `getUserInfo`)
- 常量: UPPER_SNAKE_CASE (如 `API_BASE_URL`)
- 文件: kebab-case (如 `user-service.ts`)

### 代码风格
- 使用 2 空格缩进
- 使用单引号
- 语句末尾不加分号
- 最大行宽 100 字符

### 注释规范
- 所有公共函数必须有 JSDoc 注释
- 复杂逻辑必须有行内注释
- 使用中文注释

## 禁止事项

- ❌ 不要使用 `any` 类型
- ❌ 不要在组件中直接调用 API
- ❌ 不要跳过 TypeScript 类型检查
- ❌ 不要忽略 ESLint 警告
- ❌ 不要提交 console.log 语句

## 工作流程

### 开发流程
1. 从 main 分支创建 feature 分支
2. 编写代码和测试
3. 运行 `npm run lint` 检查代码
4. 运行 `npm test` 确保测试通过
5. 提交 PR 并等待审查

### 提交规范
- feat: 新功能
- fix: 修复 bug
- refactor: 重构
- docs: 文档更新
- test: 测试相关

## 测试要求

- 单元测试覆盖率 >= 80%
- 所有新功能必须有测试
- 修复 bug 必须添加回归测试

## 常用命令

\`\`\`bash
npm run dev      # 启动开发服务器
npm run build    # 构建生产版本
npm test         # 运行测试
npm run lint     # 代码检查
npm run format   # 代码格式化
\`\`\`

## 注意事项

### 性能优化
- 使用虚拟滚动处理大列表
- 图片使用懒加载
- 组件使用动态导入

### 安全要求
- 所有用户输入必须验证
- API 调用必须有错误处理
- 敏感信息不能硬编码

## 参考资源

- [Vue 3 文档](https://vuejs.org/)
- [TypeScript 手册](https://www.typescriptlang.org/docs/)
- [项目 Wiki](./docs/wiki.md)
```

#### 16.4 编写最佳实践

**原则一：明确具体**

```markdown
# ❌ 不好的写法
## 编码规范
代码要写好，要有注释。

# ✅ 好的写法
## 编码规范
### 函数注释
所有公共函数必须有 JSDoc 注释：

\`\`\`typescript
/**
 * 计算两个日期之间的天数差
 * @param startDate - 开始日期
 * @param endDate - 结束日期
 * @returns 天数差（正数表示 endDate 在 startDate 之后）
 */
function getDaysDiff(startDate: Date, endDate: Date): number {
  const diffTime = endDate.getTime() - startDate.getTime();
  return Math.ceil(diffTime / (1000 * 60 * 60 * 24));
}
\`\`\`
```

**原则二：提供示例**

```markdown
# ❌ 不好的写法
## API 调用
使用 composables 封装 API 调用。

# ✅ 好的写法
## API 调用规范

所有 API 调用必须封装在 composables 中：

\`\`\`typescript
// src/composables/useUserApi.ts
import { ref } from 'vue';
import { userApi } from '@/api/user';

export function useUserApi() {
  const loading = ref(false);
  const error = ref<Error | null>(null);

  const getUser = async (id: string) => {
    loading.value = true;
    error.value = null;
    try {
      return await userApi.getUser(id);
    } catch (e) {
      error.value = e as Error;
      throw e;
    } finally {
      loading.value = false;
    }
  };

  return { loading, error, getUser };
}
\`\`\`

使用示例：
\`\`\`typescript
const { loading, error, getUser } = useUserApi();
const user = await getUser('123');
\`\`\`
```

**原则三：说明原因**

```markdown
# ❌ 不好的写法
## 禁止事项
- 不要使用 any 类型

# ✅ 好的写法
## 禁止事项

### 禁止使用 any 类型
**原因**: any 类型会绕过 TypeScript 的类型检查，导致运行时错误难以发现。

**替代方案**:
- 使用 unknown 类型并进行类型守卫
- 定义具体的接口类型
- 使用泛型保持类型灵活性

\`\`\`typescript
// ❌ 错误
function processData( any) {
  return data.value;
}

// ✅ 正确
interface Data {
  value: string;
}

function processData( unknown): string {
  if (typeof data === 'object' && data !== null && 'value' in data) {
    return (data as Data).value;
  }
  throw new Error('Invalid data format');
}
\`\`\`
```

**原则四：保持更新**

```markdown
# ✅ 好的写法 - 标注更新日期
## 编码规范

> 最后更新: 2026-04-07
> 更新人: 开发团队

当项目技术栈或规范发生变化时，及时更新此文件。
```

#### 16.5 不同类型项目的 CLAUDE.md

**前端项目模板**

```markdown
# 前端项目名称

## 技术栈
- 框架: Vue 3 / React 18
- 语言: TypeScript
- 样式: Uno CSS / Tailwind CSS
- 状态管理: Pinia / Zustand
- 构建工具: Vite

## 组件规范
- 组件必须有 PropTypes / TypeScript 接口
- 组件必须有 emits 定义
- 使用组合式 API / Hooks

## 样式规范
- 使用原子化 CSS
- 颜色使用 CSS 变量
- 响应式断点: 640px / 768px / 1024px / 1280px

## 性能要求
- 首屏加载 < 3s
- Lighthouse 性能分数 > 90
- 组件懒加载
```

**后端项目模板**

```markdown
# 后端项目名称

## 技术栈
- 运行时: Node.js 20
- 框架: Express / Fastify
- 语言: TypeScript
- 数据库: PostgreSQL
- ORM: Prisma

## API 规范
- RESTful 风格
- 统一响应格式
- 错误处理中间件

## 安全要求
- 所有端点必须有认证
- 输入验证使用 Zod
- SQL 使用参数化查询

## 数据库规范
- 迁移文件必须可回滚
- 索引命名: idx_表名_字段名
- 外键命名: fk_表名_字段名
```

**全栈项目模板**

```markdown
# 全栈项目名称

## 架构
- 前端: Vue 3 + TypeScript
- 后端: Node.js + Express
- 数据库: PostgreSQL
- 缓存: Redis

## 目录结构
\`\`\`
├── web/          # 前端代码
├── server/       # 后端代码
├── shared/       # 共享类型和工具
└── infra/        # 基础设施配置
\`\`\`

## 开发规范
- 前后端共享类型定义 (shared/types)
- API 变更需要更新 OpenAPI 文档
- 数据库变更需要迁移脚本
```

#### 16.6 高级技巧

**条件性指令**

```markdown
## 环境相关配置

### 开发环境
当 NODE_ENV=development 时：
- 启用详细日志
- 禁用缓存
- 使用 mock 数据

### 生产环境
当 NODE_ENV=production 时：
- 禁用调试日志
- 启用所有缓存
- 使用真实 API
```

**引用外部文件**

```markdown
## 详细文档

更多详细信息请参考：
- [API 文档](./docs/api.md)
- [数据库设计](./docs/database.md)
- [部署指南](./docs/deployment.md)
```

**使用变量**

```markdown
## 项目配置

- 项目名称: {{PROJECT_NAME}}
- API 版本: {{API_VERSION}}
- 默认端口: {{DEFAULT_PORT}}

这些变量在项目初始化时会被替换。
```

#### 16.7 常见错误与修正

| 错误 | 问题 | 修正 |
|------|------|------|
| 过于简短 | 信息不足，AI 无法理解 | 提供详细说明和示例 |
| 过于冗长 | 加载慢，Token 浪费 | 精简内容，分文件引用 |
| 缺少示例 | 抽象描述难以理解 | 添加代码示例 |
| 不更新 | 规范与实际不符 | 定期维护更新 |
| 模糊指令 | "代码要好" | "测试覆盖率 >= 80%" |

#### 16.8 CLAUDE.md 检查清单

```markdown
## CLAUDE.md 完整性检查

### 基础信息
- [ ] 项目名称和描述
- [ ] 技术栈列表
- [ ] 项目结构说明

### 编码规范
- [ ] 命名约定
- [ ] 代码风格
- [ ] 注释规范

### 工作流程
- [ ] 开发流程
- [ ] 提交规范
- [ ] 测试要求

### 约束条件
- [ ] 禁止事项清单
- [ ] 安全要求
- [ ] 性能要求

### 实用信息
- [ ] 常用命令
- [ ] 参考链接
- [ ] 联系方式
```

---

### 第17章 如何写好 Skills

#### 17.1 Skills 概述

Skills（技能）是 Claude Code 的可复用提示词模板，用于定义特定任务的工作流程和输出格式。通过编写高质量的 Skills，可以显著提升 Claude Code 处理特定类型任务的效率和一致性。

**Skills vs CLAUDE.md**

```
┌─────────────────────────────────────────────────────────────┐
│              Skills vs CLAUDE.md 对比                        │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌─────────────────────┐  ┌─────────────────────┐          │
│  │     CLAUDE.md       │  │      Skills         │          │
│  ├─────────────────────┤  ├─────────────────────┤          │
│  │ 项目级配置          │  │ 任务级模板          │          │
│  │ 静态上下文          │  │ 动态工作流          │          │
│  │ 自动加载            │  │ 手动调用            │          │
│  │ 通用规范            │  │ 专用流程            │          │
│  │ 一个项目一个        │  │ 多个技能并存        │          │
│  └─────────────────────┘  └─────────────────────┘          │
│                                                              │
│  使用场景:                   使用场景:                       │
│  - 定义项目规范              - 代码审查流程                  │
│  - 设置编码标准              - 测试生成模板                  │
│  - 提供项目上下文            - 文档生成格式                  │
│  - 约束 AI 行为              - 重构工作流                    │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

#### 17.2 Skills 文件结构

**基本结构**

```markdown
---
name: skill-name
description: 技能的简短描述
trigger: 触发条件（可选）
---

# 技能标题

技能的详细说明和使用方法。

## 参数
- param1: 参数1说明
- param2: 参数2说明

## 执行步骤

1. 第一步
2. 第二步
3. 第三步

## 输出格式

描述期望的输出格式。

## 示例

\`\`\`
/skill-name param1 value1
\`\`\`
```

**完整示例**

```markdown
---
name: api-design
description: 设计 RESTful API 接口
---

# API 设计技能

帮助您设计符合 RESTful 规范的 API 接口。

## 参数
- resource: 资源名称（必填）
- actions: 操作列表（可选，默认为 CRUD）
- version: API 版本（可选，默认为 v1）

## 设计原则

1. **资源命名**
   - 使用名词复数形式
   - 使用小写字母和连字符
   - 避免动词前缀

2. **HTTP 方法**
   - GET: 查询资源
   - POST: 创建资源
   - PUT: 完整更新
   - PATCH: 部分更新
   - DELETE: 删除资源

3. **状态码**
   - 200: 成功
   - 201: 创建成功
   - 400: 请求错误
   - 401: 未授权
   - 404: 资源不存在
   - 500: 服务器错误

## 执行步骤

1. 分析资源属性和关系
2. 设计 URL 路径
3. 定义 HTTP 方法
4. 设计请求/响应格式
5. 添加错误处理

## 输出格式

\`\`\`markdown
## 资源: {resource}

### 端点列表

| 方法 | 路径 | 描述 | 认证 |
|------|------|------|------|
| GET | /api/v1/{resource} | 列表查询 | 可选 |
| GET | /api/v1/{resource}/:id | 详情查询 | 可选 |
| POST | /api/v1/{resource} | 创建资源 | 必需 |
| PUT | /api/v1/{resource}/:id | 更新资源 | 必需 |
| DELETE | /api/v1/{resource}/:id | 删除资源 | 必需 |

### 请求示例

**创建 {resource}**
\`\`\`json
{
  "name": "示例名称",
  "description": "示例描述"
}
\`\`\`

### 响应示例

**成功响应**
\`\`\`json
{
  "code": 200,
  "data": {
    "id": "xxx",
    "name": "示例名称",
    "createdAt": "2026-04-07T00:00:00Z"
  }
}
\`\`\`

### 错误响应
\`\`\`json
{
  "code": 400,
  "error": "参数验证失败",
  "details": ["name 不能为空"]
}
\`\`\`
\`\`\`

## 使用示例

\`\`\`
/api-design user
/api-design product actions=create,read,update
/api-design order version=v2
\`\`\`
```

#### 17.3 Skills 编写最佳实践

**原则一：单一职责**

```markdown
# ❌ 不好的写法 - 一个技能做太多事
---
name: full-stack-development
description: 完成全栈开发所有工作
---

# ✅ 好的写法 - 单一职责
---
name: api-design
description: 设计 RESTful API 接口
---

---
name: database-schema
description: 设计数据库表结构
---

---
name: component-design
description: 设计前端组件
---
```

**原则二：明确输入输出**

```markdown
# ❌ 不好的写法
## 输入
用户提供一些信息。

## 输出
生成一些内容。

# ✅ 好的写法
## 参数
- entityName: 实体名称（必填，字符串）
- fields: 字段列表（必填，数组）
- relations: 关联关系（可选，数组）

## 输出
生成以下内容：
1. TypeScript 接口定义
2. Zod 验证 Schema
3. 示例数据
```

**原则三：提供步骤指导**

```markdown
# ❌ 不好的写法
## 执行
分析代码并给出建议。

# ✅ 好的写法
## 执行步骤

### 步骤 1: 收集信息
- 读取指定文件
- 分析代码结构
- 识别潜在问题

### 步骤 2: 分析评估
- 检查代码规范
- 评估性能影响
- 识别安全风险

### 步骤 3: 生成报告
- 按严重程度排序
- 提供修复建议
- 给出代码示例

### 步骤 4: 确认执行
- 用户确认后执行修改
- 运行测试验证
- 提交变更
```

**原则四：包含示例**

```markdown
## 使用示例

**基本用法**
\`\`\`
/code-review src/utils/
\`\`\`

**指定文件**
\`\`\`
/code-review src/api/user.ts src/services/auth.ts
\`\`\`

**带选项**
\`\`\`
/code-review src/ --focus=security --severity=high
\`\`\`

**预期输出**
\`\`\`
## 代码审查报告

### 文件: src/api/user.ts

#### 高优先级问题
1. **SQL 注入风险** (第 45 行)
   - 问题: 直接拼接 SQL 语句
   - 建议: 使用参数化查询
   - 示例:
     \`\`\`typescript
     // ❌ 错误
     const sql = `SELECT * FROM users WHERE id = ${id}`;

     // ✅ 正确
     const sql = 'SELECT * FROM users WHERE id = ?';
     const result = await db.query(sql, [id]);
     \`\`\`
\`\`\`
```

#### 17.4 常用 Skills 模板

**代码审查 Skill**

```markdown
---
name: code-review
description: 专业代码审查，检查质量、安全和可维护性
---

# 代码审查技能

对代码进行全面审查，发现潜在问题并提供改进建议。

## 参数
- target: 审查目标（文件路径或目录）
- focus: 审查重点（可选：quality/security/performance/all）
- severity: 最低严重级别（可选：critical/high/medium/low）

## 审查维度

### 1. 代码质量
- 可读性和命名规范
- 函数复杂度（圈复杂度 < 10）
- 代码重复（DRY 原则）
- 注释完整性

### 2. 安全性
- 输入验证
- SQL 注入风险
- XSS 漏洞
- 敏感数据处理

### 3. 性能
- 算法复杂度
- 内存使用
- 数据库查询优化
- 缓存策略

### 4. 可维护性
- 模块化程度
- 依赖管理
- 测试覆盖率
- 文档完整性

## 执行步骤

1. **扫描阶段**
   - 读取目标文件
   - 解析代码结构
   - 收集上下文信息

2. **分析阶段**
   - 运行静态分析
   - 检查编码规范
   - 识别潜在问题

3. **评估阶段**
   - 按严重程度分类
   - 计算影响范围
   - 生成修复建议

4. **报告阶段**
   - 生成审查报告
   - 提供代码示例
   - 给出优先级建议

## 输出格式

\`\`\`markdown
# 代码审查报告

## 概述
- 文件数量: X
- 问题总数: Y
- 严重: A | 高: B | 中: C | 低: D

## 详细问题

### 🔴 严重问题

#### 1. [问题标题]
- **文件**: path/to/file.ts
- **位置**: 第 X 行
- **问题**: 问题描述
- **影响**: 影响说明
- **建议**: 修复建议
- **示例**:
  \`\`\`typescript
  // 修复后的代码
  \`\`\`

### 🟠 高优先级问题
...

### 🟡 中优先级问题
...

### 🟢 低优先级问题
...

## 改进建议

1. 立即处理严重问题
2. 计划处理高优先级问题
3. 后续迭代处理中低优先级问题
\`\`\`

## 使用示例

\`\`\`
/code-review src/
/code-review src/auth/ --focus=security
/code-review src/api/ --severity=high
\`\`\`
```

**测试生成 Skill**

```markdown
---
name: generate-tests
description: 为代码自动生成测试用例
---

# 测试生成技能

为指定代码生成全面的测试用例，确保高覆盖率。

## 参数
- target: 目标文件路径
- framework: 测试框架（jest/vitest/mocha，默认 vitest）
- coverage: 目标覆盖率（默认 80）

## 测试类型

### 1. 单元测试
- 正常输入测试
- 边界条件测试
- 异常输入测试
- 返回值验证

### 2. 集成测试
- 模块间交互
- API 调用
- 数据库操作

### 3. 边界测试
- 空值处理
- 极值处理
- 类型边界

## 执行步骤

1. 分析代码结构
2. 识别测试点
3. 生成测试用例
4. 添加 mock 和 fixture
5. 验证覆盖率

## 输出格式

\`\`\`typescript
import { describe, it, expect, beforeEach, vi } from 'vitest';
import { functionToTest } from './module';

describe('functionToTest', () => {
  // 正常情况
  describe('正常输入', () => {
    it('应该返回正确结果', () => {
      expect(functionToTest('input')).toBe('expected');
    });
  });

  // 边界情况
  describe('边界条件', () => {
    it('空字符串应该返回 null', () => {
      expect(functionToTest('')).toBeNull();
    });

    it('null 应该抛出错误', () => {
      expect(() => functionToTest(null)).toThrow();
    });
  });

  // 异常情况
  describe('异常处理', () => {
    it('无效输入应该抛出错误', () => {
      expect(() => functionToTest(123)).toThrow(TypeError);
    });
  });
});
\`\`\`

## 使用示例

\`\`\`
/generate-tests src/utils/helper.ts
/generate-tests src/api/user.ts --framework=jest
/generate-tests src/services/ --coverage=90
\`\`\`
```

**文档生成 Skill**

```markdown
---
name: generate-docs
description: 为代码生成文档
---

# 文档生成技能

为代码生成清晰的文档，包括 API 文档、README 和代码注释。

## 参数
- target: 目标文件或目录
- format: 输出格式（markdown/html/json，默认 markdown）
- style: 文档风格（jsdoc/tsdoc/openapi）

## 文档类型

### 1. API 文档
- 端点描述
- 请求参数
- 响应格式
- 错误代码

### 2. 代码注释
- 函数说明
- 参数描述
- 返回值说明
- 使用示例

### 3. README
- 项目介绍
- 安装指南
- 使用说明
- 配置选项

## 输出格式

\`\`\`markdown
# API 文档

## 模块: User API

### 概述
用户管理相关的 API 端点。

---

## 端点: GET /api/users

### 描述
获取用户列表。

### 请求参数

| 参数 | 类型 | 必填 | 描述 |
|------|------|------|------|
| page | number | 否 | 页码，默认 1 |
| limit | number | 否 | 每页数量，默认 20 |
| search | string | 否 | 搜索关键词 |

### 响应

**成功响应 (200)**
\`\`\`json
{
  "data": [
    {
      "id": "string",
      "name": "string",
      "email": "string"
    }
  ],
  "pagination": {
    "total": 100,
    "page": 1,
    "limit": 20
  }
}
\`\`\`

### 示例

\`\`\`bash
curl -X GET "https://api.example.com/api/users?page=1&limit=10"
\`\`\`
\`\`\`

## 使用示例

\`\`\`
/generate-docs src/api/
/generate-docs src/ --format=html
/generate-docs src/services/auth.ts --style=jsdoc
\`\`\`
```

#### 17.5 Skills 文件管理

**目录结构**

```
.claude/
├── skills/
│   ├── development/
│   │   ├── code-review.md
│   │   ├── generate-tests.md
│   │   └── refactor.md
│   ├── documentation/
│   │   ├── generate-docs.md
│   │   └── update-readme.md
│   └── api/
│       ├── api-design.md
│       └── openapi-gen.md
└── CLAUDE.md
```

**全局 Skills**

```bash
# 全局 Skills 位置
~/.claude/skills/

# 项目级 Skills 位置
./.claude/skills/
```

**安装 Skills**

```bash
# 从本地文件安装
cp my-skill.md ~/.claude/skills/

# 从 Git 仓库安装
git clone https://github.com/user/claude-skills.git ~/.claude/skills/

# 使用符号链接
ln -s /path/to/skills ~/.claude/skills/my-skills
```

#### 17.6 高级 Skills 技巧

**条件执行**

```markdown
## 执行条件

仅在以下条件满足时执行此技能：

1. 目标文件必须存在
2. 项目必须包含 package.json
3. 必须已安装相关依赖

\`\`\`markdown
<!-- 如果条件不满足，输出 -->
⚠️ 无法执行此技能：
- 文件 xxx 不存在
- 缺少必要依赖: xxx
\`\`\`
```

**链式调用**

```markdown
## 后续步骤

执行完此技能后，建议执行：

1. `/generate-tests {target}` - 生成测试用例
2. `/code-review {target}` - 审查生成的代码
3. `/generate-docs {target}` - 更新文档
```

**参数验证**

```markdown
## 参数验证

执行前验证参数：

\`\`\`
如果 entityName 为空:
  输出: ❌ 错误: 必须提供 entityName 参数
  显示帮助信息
  退出

如果 fields 不是数组:
  输出: ❌ 错误: fields 必须是数组格式
  显示示例
  退出
\`\`\`
```

#### 17.7 Skills 检查清单

```markdown
## Skills 完整性检查

### 基础信息
- [ ] 技能名称（简洁、描述性）
- [ ] 技能描述（清晰说明用途）
- [ ] 触发条件（如适用）

### 参数定义
- [ ] 所有参数已列出
- [ ] 必填/可选已标注
- [ ] 默认值已说明

### 执行流程
- [ ] 步骤清晰可执行
- [ ] 每步有明确目标
- [ ] 错误处理已考虑

### 输出规范
- [ ] 输出格式已定义
- [ ] 包含示例输出
- [ ] 格式易于理解

### 使用示例
- [ ] 基本用法示例
- [ ] 高级用法示例
- [ ] 预期输出展示
```

#### 17.8 Skills 与 CLAUDE.md 协同

**协同模式**

```
┌─────────────────────────────────────────────────────────────┐
│              Skills 与 CLAUDE.md 协同工作                    │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │                   CLAUDE.md                          │    │
│  │  定义项目规范、编码标准、禁止事项                     │    │
│  │  作为所有对话的基础上下文                            │    │
│  └─────────────────────────────────────────────────────┘    │
│                          │                                   │
│                          ▼                                   │
│  ┌─────────────────────────────────────────────────────┐    │
│  │                    Skills                            │    │
│  │  在 CLAUDE.md 规范基础上执行特定任务                 │    │
│  │  遵循项目规范，生成符合标准的输出                    │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                              │
│  示例:                                                       │
│  CLAUDE.md 定义: "所有函数必须有 JSDoc 注释"                │
│  Skills 执行: 生成的代码自动包含 JSDoc 注释                 │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

**最佳实践**

```markdown
1. CLAUDE.md 定义通用规则
   - 编码规范
   - 命名约定
   - 禁止事项

2. Skills 定义具体流程
   - 任务步骤
   - 输出格式
   - 验证规则

3. 两者协同工作
   - Skills 遵循 CLAUDE.md 规范
   - CLAUDE.md 为 Skills 提供上下文
   - 共同确保输出质量
```

---

## 第四部分：交互与工作流

### 第18章 交互模式

#### 18.1 交互模式概述

Claude Code 提供了多种交互模式，满足不同场景下的使用需求。理解这些模式可以帮助您更高效地使用 Claude Code。

**交互模式分类**

```
┌─────────────────────────────────────────────────────────────┐
│                   Claude Code 交互模式                        │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐         │
│  │ 交互式模式   │  │ 单次执行模式 │  │ 管道模式    │         │
│  │ Interactive │  │ One-shot    │  │ Pipeline    │         │
│  └─────────────┘  └─────────────┘  └─────────────┘         │
│        │                │               │                    │
│        ▼                ▼               ▼                    │
│  持续对话          单次查询          流式处理                │
│  多轮交互          快速任务          脚本集成                │
│  上下文保持        独立执行          自动化流程              │
│                                                              │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐         │
│  │ Vim 模式    │  │ 恢复模式    │  │ 后台模式    │         │
│  │ Vim Mode    │  │ Resume      │  │ Background  │         │
│  └─────────────┘  └─────────────┘  └─────────────┘         │
│        │                │               │                    │
│        ▼                ▼               ▼                    │
│  Vim 键绑定        继续会话          后台执行                │
│  编辑器风格        恢复历史          异步任务                │
│  高效导航          上下文恢复        长时间任务              │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

#### 18.2 交互式模式（Interactive Mode）

**启动方式**

```bash
# 基本启动
claude

# 指定工作目录
claude --cwd /path/to/project

# 指定模型
claude --model claude-opus-4-6
```

**交互界面**

```
┌─────────────────────────────────────────────────────────────┐
│                    Claude Code 交互界面                      │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ╭───────────────────────────────────────────────────────╮  │
│  │ Claude Code v1.x                                       │  │
���  │ Model: claude-sonnet-4-6                               │  │
│  │ Working Directory: /Users/user/project                 │  │
│  ╰───────────────────────────────────────────────────────╯  │
│                                                              │
│  User: 请帮我分析这个项目的结构                              │
│                                                              │
│  Claude: 我来分析项目结构...                                 │
│                                                              │
│  [使用 Glob 工具扫描目录]                                    │
│                                                              │
│  项目结构分析结果:                                           │
│  - src/                                                     │
│    - components/                                            │
│    - api/                                                   │
│    - utils/                                                 │
│  ...                                                        │
│                                                              │
│  ─────────────────────────────────────────────────────────  │
│  > _                                                        │
│                                                              │
│  快捷键:                                                     │
│  - Ctrl+C: 中断当前操作                                      │
│  - Ctrl+D: 退出会话                                          │
│  - Ctrl+L: 清屏                                              │
│  - ↑/↓: 历史命令                                             │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

**交互式模式特性**

| 特性 | 说明 |
|------|------|
| 多轮对话 | 支持连续对话，上下文自动保持 |
| 工具调用 | AI 自动调用工具完成任务 |
| 权限确认 | 高风险操作需要用户确认 |
| 实时反馈 | 显示工具执行过程和结果 |
| 会话管理 | 支持保存、恢复会话 |

#### 18.3 Vim 模式

Claude Code 支持 Vim 风格的键绑定，为熟悉 Vim 的用户提供高效的编辑体验。

**启用 Vim 模式**

```bash
# 方式一：命令行参数
claude --vim

# 方式二：配置文件
# ~/.claude/settings.json
{
  "vimMode": true
}

# 方式三：环境变量
export CLAUDE_VIM_MODE=true
claude
```

**Vim 模式键绑定**

```
┌─────────────────────────────────────────────────────────────┐
│                    Vim 模式键绑定                            │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  普通模式 (Normal Mode)                                      │
│  ─────────────────────                                       │
│  h, j, k, l     光标移动                                     │
│  w, b, e        单词移动                                     │
│  0, $           行首/行尾                                    │
│  gg, G          文件首/尾                                    │
│  /pattern       向下搜索                                     │
│  ?pattern       向上搜索                                     │
│  n, N           下一个/上一个匹配                            │
│  dd             删除行                                       │
│  yy             复制行                                       │
│  p, P           粘贴                                        │
│  u              撤销                                        │
│  Ctrl+r         重做                                        │
│                                                              │
│  插入模式 (Insert Mode)                                      │
│  ─────────────────────                                       │
│  i              在光标前插入                                 │
│  a              在光标后插入                                 │
│  I              在行首插入                                   │
│  A              在行尾插入                                   │
│  o              在下方新建行插入                             │
│  O              在上方新建行插入                             │
│  Esc            返回普通模式                                 │
│                                                              │
│  命令模式 (Command Mode)                                     │
│  ─────────────────────                                       │
│  :w             保存                                        │
│  :q             退出                                        │
│  :wq            保存并退出                                   │
│  :q!            强制退出                                     │
│  :%s/old/new/g  全局替换                                     │
│                                                              │
│  可视模式 (Visual Mode)                                      │
│  ─────────────────────                                       │
│  v              字符选择                                     │
│  V              行选择                                       │
│  Ctrl+v         块选择                                       │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

**Vim 模式配置**

```json
// ~/.claude/settings.json
{
  "vimMode": true,
  "vimKeybindings": {
    "normal": {
      "jj": "esc",           // jj 退出插入模式
      ";;": "command",       // ;; 进入命令模式
      "space": "leader"      // 空格作为 leader 键
    },
    "insert": {
      "jk": "esc"            // jk 退出插入模式
    }
  },
  "vimSettings": {
    "relativeNumber": true,  // 相对行号
    "cursorLine": true,      // 高亮当前行
    "wrap": false,           // 不自动换行
    "tabstop": 2,            // Tab 宽度
    "expandtab": true        // 用空格代替 Tab
  }
}
```

#### 18.4 单次执行模式

**基本用法**

```bash
# 单次查询
claude -p "解释这个函数的作用"

# 带文件上下文
claude -p "审查这段代码" src/utils.ts

# 从标准输入
cat error.log | claude -p "分析这个错误"
```

**使用场景**

| 场景 | 命令示例 |
|------|----------|
| 快速问答 | `claude -p "如何在 TypeScript 中定义接口？"` |
| 代码审查 | `claude -p "审查代码质量" src/api.ts` |
| 错误分析 | `npm test 2>&1 \| claude -p "修复测试失败"` |
| 文档生成 | `claude -p "生成 API 文档" src/api/` |
| 代码解释 | `claude -p "解释这段代码的逻辑" src/complex.ts` |

**高级选项**

```bash
# 指定模型
claude -p "复杂分析任务" --model claude-opus-4-6

# 限制输出
claude -p "生成摘要" --max-tokens 500

# 调整温度
claude -p "创意写作" --temperature 0.9

# 禁用工具
claude -p "纯文本回答" --no-tools

# 允许特定工具
claude -p "读取并分析文件" --allow-tools Read,Grep
```

#### 18.5 管道模式

**基本管道操作**

```bash
# Git diff 分析
git diff | claude -p "审查这些更改"

# 日志分析
tail -f app.log | claude -p "监控并分析日志"

# 命令输出处理
find . -name "*.ts" | claude -p "分析这些文件"

# 组合管道
git log --oneline -10 | claude -p "总结最近的提交"
```

**管道模式工作流**

```
┌─────────────────────────────────────────────────────────────┐
│                    管道模式工作流                             │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  数据源 → 管道 → Claude Code → 输出                          │
│                                                              │
│  示例 1: Git 工作流                                          │
│  ┌─────────┐    ┌─────────┐    ┌─────────┐                 │
│  │ git     │ → │ claude  │ → │ 审查    │                 │
│  │ diff    │    │ -p      │    │ 报告    │                 │
│  └─────────┘    └─────────┘    └─────────┘                 │
│                                                              │
│  示例 2: 日志分析                                            │
│  ┌─────────┐    ┌─────────┐    ┌─────────┐                 │
│  │ tail    │ → │ claude  │ → │ 错误    │                 │
│  │ -f log  │    │ -p      │    │ 分析    │                 │
│  └─────────┘    └─────────┘    └─────────┘                 │
│                                                              │
│  示例 3: 测试失败处理                                        │
│  ┌─────────┐    ┌─────────┐    ┌─────────┐                 │
│  │ npm     │ → │ claude  │ → │ 修复    │                 │
│  │ test    │    │ -p      │    │ 方案    │                 │
│  └─────────┘    └─────────┘    └─────────┘                 │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

**实用管道脚本**

```bash
#!/bin/bash
# auto-review.sh - 自动代码审查脚本

# 获取修改的文件
FILES=$(git diff --name-only HEAD~1)

# 对每个文件进行审查
for FILE in $FILES; do
  echo "审查: $FILE"
  git diff HEAD~1 -- "$FILE" | claude -p "审查这个文件的更改"
  echo "---"
done
```

```bash
#!/bin/bash
# error-analyzer.sh - 错误日志分析脚本

# 实时监控日志
tail -f /var/log/app.log | while read line; do
  if echo "$line" | grep -q "ERROR"; then
    echo "$line" | claude -p "分析这个错误并提供修复建议"
  fi
done
```

#### 18.6 恢复模式

**会话恢复**

```bash
# 继续上次会话
claude --continue

# 恢复特定会话
claude --resume session-2026-04-07-001

# 列出所有会话
claude --list-sessions
```

**会话管理**

```
┌─────────────────────────────────────────────────────────────┐
│                    会话管理界面                              │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  可用会话:                                                   │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐   │
│  │ ID: session-2026-04-07-001                          │   │
│  │ 时间: 2026-04-07 14:30:00                           │   │
│  │ 目录: /Users/user/project-a                         │   │
│  │ 摘要: 代码重构讨论                                   │   │
│  │ 消息数: 15                                          │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐   │
│  │ ID: session-2026-04-07-002                          │   │
│  │ 时间: 2026-04-07 16:45:00                           │   │
│  │ 目录: /Users/user/project-b                         │   │
│  │ 摘要: Bug 修复                                       │   │
│  │ 消息数: 8                                           │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                              │
│  操作:                                                       │
│  - 输入序号恢复会话                                          │
│  - 输入 'd' 删除会话                                         │
│  - 输入 'q' 退出                                             │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

**会话存储位置**

```
~/.claude/sessions/
├── session-2026-04-07-001.json
├── session-2026-04-07-002.json
└── session-2026-04-07-003.json
```

#### 18.7 后台模式

**启动后台任务**

```bash
# 后台执行长时间任务
claude -p "分析整个项目的代码质量" --background

# 指定输出文件
claude -p "生成项目文档" --background --output docs/report.md

# 查看后台任务状态
claude --jobs

# 查看特定任务输出
claude --job-output job-001
```

**后台任务管理**

```
┌─────────────────────────────────────────────────────────────┐
│                    后台任务管理                              │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  运行中的任务:                                               │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐   │
│  │ Job ID: job-001                                     │   │
│  │ 状态: 运行中                                         │   │
│  │ 启动时间: 14:30:00                                  │   │
│  │ 任务: 分析整个项目的代码质量                          │   │
│  │ 进度: ████████░░░░░░░░ 50%                          │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                              │
│  ┌─────────────────────────────────────────────────────┐   │
│  │ Job ID: job-002                                     │   │
│  │ 状态: 已完成                                         │   │
│  │ 启动时间: 14:00:00                                  │   │
│  │ 完成时间: 14:25:00                                  │   │
│  │ 任务: 生成项目文档                                   │   │
│  │ 输出: docs/report.md                                │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                              │
│  操作:                                                       │
│  - claude --job-stop job-001  停止任务                       │
│  - claude --job-output job-001 查看输出                      │
│  - claude --job-log job-001 查看日志                         │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

#### 18.8 交互模式最佳实践

**选择合适的模式**

| 场景 | 推荐模式 | 说明 |
|------|----------|------|
| 日常开发 | 交互式 | 持续对话，上下文保持 |
| 快速查询 | 单次执行 | 快速获取答案 |
| CI/CD 集成 | 管道模式 | 自动化流程 |
| Vim 用户 | Vim 模式 | 高效编辑 |
| 长时间任务 | 后台模式 | 异步执行 |
| 继续工作 | 恢复模式 | 恢复上下文 |

**效率提升技巧**

```markdown
1. **使用快捷键**
   - 熟练使用 Ctrl+C/D/L 等快捷键
   - Vim 模式下使用 Vim 键绑定
   - 自定义常用命令别名

2. **合理使用管道**
   - 将命令输出直接传递给 Claude
   - 避免手动复制粘贴
   - 构建自动化工作流

3. **会话管理**
   - 为不同项目创建独立会话
   - 定期清理不需要的会话
   - 使用有意义的会话名称

4. **后台任务**
   - 长时间任务使用后台模式
   - 定期检查任务状态
   - 合理设置输出文件
```

---

### 第19章 工作流程

#### 19.1 工作流程概述

Claude Code 的工作流程设计旨在最大化开发效率，同时保持代码质量和项目一致性。

**工作流程层次**

```
┌─────────────────────────────────────────────────────────────┐
│                   Claude Code 工作流程                       │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │              项目级工作流程                           │    │
│  │  - 项目初始化                                        │    │
│  │  - 架构设计                                          │    │
│  │  - 技术选型                                          │    │
│  └─────────────────────────────────────────────────────┘    │
│                          │                                   │
│                          ▼                                   │
│  ┌─────────────────────────────────────────────────────┐    │
│  │              功能级工作流程                           │    │
│  │  - 需求分析                                          │    │
│  │  - 功能实现                                          │    │
│  │  - 代码审查                                          │    │
│  └─────────────────────────────────────────────────────┘    │
│                          │                                   │
│                          ▼                                   │
│  ┌─────────────────────────────────────────────────────┐    │
│  │              任务级工作流程                           │    │
│  │  - 代码编写                                          │    │
│  │  - 测试编写                                          │    │
│  │  - Bug 修复                                          │    │
│  └─────────────────────────────────────────────────────┘    │
│                          │                                   │
│                          ▼                                   │
│  ┌─────────────────────────────────────────────────────┐    │
│  │              操作级工作流程                           │    │
│  │  - 文件操作                                          │    │
│  │  - 命令执行                                          │    │
│  │  - 结果验证                                          │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

#### 19.2 功能开发工作流

**标准开发流程**

```
┌─────────────────────────────────────────────────────────────┐
│                    功能开发工作流                             │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  1. 需求理解                                                 │
│     │                                                        │
│     ▼                                                        │
│  ┌─────────────────────────────────────────────────────┐    │
│  │ 用户: 我需要实现用户登录功能                          │    │
│  │ Claude: 我来理解需求...                              │    │
│  │ - 登录方式: 用户名/密码？OAuth？                      │    │
│  │ - 安全要求: Token？Session？                          │    │
│  │ - 前后端分离？                                       │    │
│  └─────────────────────────────────────────────────────┘    │
│     │                                                        │
│     ▼                                                        │
│  2. 方案设计                                                 │
│     │                                                        │
│     ▼                                                        │
│  ┌─────────────────────────────────────────────────────┐    │
│  │ Claude: 基于需求，我建议以下方案:                      │    │
│  │                                                      │    │
│  │ 技术方案:                                            │    │
│  │ - JWT Token 认证                                     │    │
│  │ - bcrypt 密码加密                                    │    │
│  │ - Redis Session 管理                                 │    │
│  │                                                      │    │
│  │ 文件结构:                                            │    │
│  │ - src/api/auth.ts (API 端点)                         │    │
│  │ - src/middleware/auth.ts (认证中间件)                │    │
│  │ - src/utils/jwt.ts (JWT 工具)                        │    │
│  └─────────────────────────────────────────────────────┘    │
│     │                                                        │
│     ▼                                                        │
│  3. TDD 实现                                                 │
│     │                                                        │
│     ▼                                                        │
│  ┌─────────────────────────────────────────────────────┐    │
│  │ Claude: 按照 TDD 流程实现:                            │    │
│  │                                                      │    │
│  │ 步骤 1: 编写测试                                     │    │
│  │ [创建 tests/auth.test.ts]                           │    │
│  │                                                      │    │
│  │ 步骤 2: 运行测试 (应失败)                             │    │
│  │ [执行 npm test]                                      │    │
│  │                                                      │    │
│  │ 步骤 3: 实现代码                                     │    │
│  │ [创建 src/api/auth.ts]                              │    │
│  │                                                      │    │
│  │ 步骤 4: 运行测试 (应通过)                             │    │
│  │ [执行 npm test]                                      │    │
│  │                                                      │    │
│  │ 步骤 5: 重构优化                                     │    │
│  │ [优化代码结构]                                       │    │
│  └─────────────────────────────────────────────────────┘    │
│     │                                                        │
│     ▼                                                        │
│  4. 代码审查                                                 │
│     │                                                        │
│     ▼                                                        │
│  ┌─────────────────────────────────────────────────────┐    │
│  │ Claude: 执行代码审查...                              │    │
│  │                                                      │    │
│  │ 发现问题:                                            │    │
│  │ 1. [高] 密码未加盐 - src/utils/auth.ts:15            │    │
│  │ 2. [中] 缺少错误处理 - src/api/auth.ts:28            │    │
│  │                                                      │    │
│  │ 修复建议:                                            │    │
│  │ [提供修复代码]                                       │    │
│  └─────────────────────────────────────────────────────┘    │
│     │                                                        │
│     ▼                                                        │
│  5. 提交代码                                                 │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

**交互示例**

```
用户: 实现用户登录功能，使用 JWT 认证

Claude: 我来按照标准工作流实现这个功能。

## 1. 需求确认

基于您的描述，我理解需要实现：
- 用户名/密码登录
- JWT Token 认证
- 登录状态保持

请确认是否正确？

用户: 是的，还需要支持刷新 Token

Claude: 好的，我已更新需求。

## 2. 设计方案

我将创建以下文件：
- src/api/auth.ts - 认证 API
- src/middleware/auth.ts - 认证中间件
- src/utils/jwt.ts - JWT 工具函数
- src/types/auth.ts - 类型定义

## 3. 开始实现

首先编写测试用例...

[创建 tests/auth.test.ts]

```typescript
describe('Auth API', () => {
  it('should login with valid credentials', async () => {
    const response = await request(app)
      .post('/api/auth/login')
      .send({ username: 'test', password: 'password' });

    expect(response.status).toBe(200);
    expect(response.body.token).toBeDefined();
  });
});
```

运行测试... [失败 - 预期行为]

现在实现代码...

[创建 src/api/auth.ts]

...

测试通过！功能已实现。

## 4. 代码审查

我来审查刚才的代码...

发现 2 个问题需要修复：
1. 密码比较应使用 bcrypt.compare
2. 缺少 Token 过期处理

是否需要我修复这些问题？

用户: 请修复

Claude: 已修复所有问题。

## 5. 总结

已完成用户登录功能：
- ✅ 用户名/密码登录
- ✅ JWT Token 生成
- ✅ Token 刷新机制
- ✅ 测试覆盖率 85%

需要提交代码吗？
```

#### 19.3 Bug 修复工作流

**Bug 修复流程**

```
┌─────────────────────────────────────────────────────────────┐
│                    Bug 修复工作流                             │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  1. 问题报告                                                 │
│     │                                                        │
│     ▼                                                        │
│  ┌────────────────────────���────────────────────────────┐    │
│  │ 用户: 用户登录后偶尔会自动登出                        │    │
│  │                                                      │    │
│  │ Claude: 我来分析这个问题。                           │    │
│  │ 请提供以下信息:                                       │    │
│  │ - 错误日志                                           │    │
│  │ - 复现步骤                                           │    │
│  │ - 发生频率                                           │    │
│  └─────────────────────────────────────────────────────┘    │
│     │                                                        │
│     ▼                                                        │
│  2. 问题定位                                                 │
│     │                                                        │
│     ▼                                                        │
│  ┌─────────────────────────────────────────────────────┐    │
│  │ Claude: 分析错误日志...                              │    │
│  │                                                      │    │
│  │ [读取日志文件]                                       │    │
│  │ [搜索相关代码]                                       │    │
│  │ [分析 Token 验证逻辑]                                │    │
│  │                                                      │    │
│  │ 发现问题:                                            │    │
│  │ - Token 验证中间件未处理刷新场景                      │    │
│  │ - Redis 连接断开时未重连                             │    │
│  └─────────────────────────────────────────────────────┘    │
│     │                                                        │
│     ▼                                                        │
│  3. 修复实施                                                 │
│     │                                                        │
│     ▼                                                        │
│  ┌─────────────────────────────────────────────────────┐    │
│  │ Claude: 修复方案:                                    │    │
│  │                                                      │    │
│  │ 修复 1: 添加 Token 刷新逻辑                          │    │
│  │ [编辑 src/middleware/auth.ts]                        │    │
│  │                                                      │    │
│  │ 修复 2: 添加 Redis 重连机制                          │    │
│  │ [编辑 src/utils/redis.ts]                            │    │
│  │                                                      │    │
│  │ 添加回归测试:                                        │    │
│  │ [创建 tests/auth-refresh.test.ts]                   │    │
│  └─────────────────────────────────────────────────────┘    │
│     │                                                        │
│     ▼                                                        │
│  4. 验证测试                                                 │
│     │                                                        │
│     ▼                                                        │
│  ┌─────────────────────────────────────────────────────┐    │
│  │ Claude: 运行测试验证修复...                          │    │
│  │                                                      │    │
│  │ [执行 npm test]                                      │    │
│  │                                                      │    │
│  │ 结果: 所有测试通过 ✅                                │    │
│  │                                                      │    │
│  │ 回归测试:                                            │    │
│  │ - Token 刷新测试 ✅                                  │    │
│  │ - Redis 重连测试 ✅                                  │    │
│  │ - 原有测试 ✅                                        │    │
│  └─────────────────────────────────────────────────────┘    │
│     │                                                        │
│     ▼                                                        │
│  5. 总结归档                                                 │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

#### 19.4 代码审查工作流

**审查流程**

```
┌─────────────────────────────────────────────────────────────┐
│                    代码审查工作流                             │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │ 1. 触发审查                                          │    │
│  │    /code-review src/                                │    │
│  │    git diff | claude -p "审查更改"                   │    │
│  └─────────────────────────────────────────────────────┘    │
│                          │                                   │
│                          ▼                                   │
│  ┌─────────────────────────────────────────────────────┐    │
│  │ 2. 多维度分析                                        │    │
│  │    - 代码质量                                        │    │
│  │    - 安全性                                          │    │
│  │    - 性能                                            │    │
│  │    - 可维护性                                        │    │
│  └─────────────────────────────────────────────────────┘    │
│                          │                                   │
│                          ▼                                   │
│  ┌─────────────────────────────────────────────────────┐    │
│  │ 3. 生成报告                                          │    │
│  │    - 问题列表 (按严重程度排序)                        │    │
│  │    - 修复建议                                        │    │
│  │    - 代码示例                                        │    │
│  └─────────────────────────────────────────────────────┘    │
│                          │                                   │
│                          ▼                                   │
│  ┌─────────────────────────────────────────────────────┐    │
│  │ 4. 确认修复                                          │    │
│  │    - 用户选择要修复的问题                            │    │
│  │    - 自动应用修复                                    │    │
│  │    - 运行测试验证                                    │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

#### 19.5 长时间运行任务

**处理长时间任务**

```markdown
## 长时间任务类型

1. **大规模重构**
   - 跨多个文件的代码重构
   - 架构调整
   - 技术栈迁移

2. **全面分析**
   - 整个项目代码审查
   - 安全漏洞扫描
   - 性能分析

3. **批量操作**
   - 批量文件处理
   - 批量代码生成
   - 批量测试运行

## 处理策略

### 策略一：任务分解

将大任务分解为小任务：

用户: 重构整个项目的状态管理

Claude: 这是一个大型任务，我建议分解为以下步骤：

1. 分析当前状态管理结构
2. 设计新的状态管理方案
3. 逐模块迁移
4. 测试验证

是否按此步骤执行？

### 策略二：后台执行

使用后台模式处理长时间任务：

```bash
# 后台执行
claude -p "分析整个项目代码质量" --background

# 查看进度
claude --jobs

# 获取结果
claude --job-output job-001
```

### 策略三：检查点保存

定期保存进度，支持中断恢复：

```markdown
用户: 继续之前的重构任务

Claude: 检测到未完成的重构任务：
- 已完成: 用户模块 (100%)
- 进行中: 订单模块 (60%)
- 待处理: 商品模块

是否从订单模块继续？
```
```

#### 19.6 团队协作工作流

**团队协作模式**

```
┌─────────────────────────────────────────────────────────────┐
│                    团队协作工作流                             │
├────────────────────────────────────────────────────────────��┤
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │ 共享 CLAUDE.md                                       │    │
│  │                                                      │    │
│  │ 项目根目录的 CLAUDE.md 加入版本控制                   │    │
│  │ - 统一编码规范                                       │    │
│  │ - 共享项目知识                                       │    │
│  │ - 团队工作流程                                       │    │
│  └─────────────────────────────────��───────────────────┘    │
│                          │                                   │
│                          ▼                                   │
│  ┌─────────────────────────────────────────────────────┐    │
│  │ 共享 Skills                                          │    │
│  │                                                      │    │
│  │ .claude/skills/ 目录加入版本控制                     │    │
│  │ - 团队专用技能                                       │    │
│  │ - 代码审查模板                                       │    │
│  │ - 文档生成流程                                       │    │
│  └─────────────────────────────────────────────────────┘    │
│                          │                                   │
│                          ▼                                   │
│  ┌─────────────────────────────────────────────────────┐    │
│  │ PR 集成                                              │    │
│  │                                                      │    │
│  │ CI/CD 流程中集成 Claude Code                         │    │
│  │ - 自动代码审查                                       │    │
│  │ - 文档自动更新                                       │    │
│  │ - 测试覆盖率检查                                     │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

**团队协作配置示例**

```yaml
# .github/workflows/claude-review.yml
name: Claude Code Review

on:
  pull_request:
    types: [opened, synchronize]

jobs:
  review:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Setup Claude Code
        run: npm install -g @anthropic-ai/claude-code

      - name: Run Code Review
        env:
          ANTHROPIC_API_KEY: ${{ secrets.ANTHROPIC_API_KEY }}
        run: |
          git diff origin/main...HEAD | claude -p "
            审查这个 PR 的代码更改，关注：
            1. 代码质量
            2. 安全问题
            3. 性能影响
            4. 测试覆盖

            输出格式为 GitHub PR 评论格式。
          " > review-comment.md

      - name: Post Review Comment
        uses: actions/github-script@v6
        with:
          script: |
            const fs = require('fs');
            const comment = fs.readFileSync('review-comment.md', 'utf8');
            await github.rest.issues.createComment({
              owner: context.repo.owner,
              repo: context.repo.repo,
              issue_number: context.issue.number,
              body: comment
            });
```

#### 19.7 工作流最佳实践

**效率优化**

```markdown
1. **善用快捷键和别名**
   ```bash
   # 添加到 ~/.bashrc 或 ~/.zshrc
   alias cc='claude'
   alias ccr='claude --continue'
   alias ccp='claude -p'
   alias ccd='claude --diagnostics'
   ```

2. **构建自动化脚本**
   - 将常用操作封装为脚本
   - 使用管道模式集成现有工具
   - 配合 CI/CD 实现自动化

3. **合理使用子代理**
   - 大任务并行处理
   - 专业任务使用专业子代理
   - 结果汇总和整合

4. **保持上下文清晰**
   - 及时清理不需要的会话
   - 使用 CLAUDE.md 保持项目知识
   - 利用记忆系统存储重要信息
```

---

### 第20章 CLI 命令大全

#### 20.1 命令行界面概述

Claude Code 提供了丰富的命令行选项，覆盖从基础操作到高级配置的各种需求。

**命令格式**

```bash
claude [全局选项] [命令] [命令选项] [参数]
```

#### 20.2 全局选项

| 选项 | 简写 | 说明 | 示例 |
|------|------|------|------|
| `--help` | `-h` | 显示帮助信息 | `claude --help` |
| `--version` | `-v` | 显示版本信息 | `claude --version` |
| `--model` | `-m` | 指定使用的模型 | `claude --model claude-opus-4-6` |
| `--cwd` | | 指定工作目录 | `claude --cwd /path/to/project` |
| `--config` | `-c` | 指定配置文件 | `claude --config ./claude.json` |
| `--verbose` | `-V` | 详细输出模式 | `claude --verbose` |
| `--quiet` | `-q` | 静默模式 | `claude --quiet` |
| `--json` | | JSON 格式输出 | `claude --json` |
| `--no-color` | | 禁用颜色输出 | `claude --no-color` |

#### 20.3 核心命令

**启动命令**

```bash
# 启动交互式会话
claude

# 启动并指定模型
claude --model claude-sonnet-4-6

# 启动并加载特定配置
claude --config ./project-config.json

# 启动 Vim 模式
claude --vim
```

**单次执行命令**

```bash
# 基本用法
claude -p "提示词"

# 带文件上下文
claude -p "分析代码" src/main.ts

# 多文件
claude -p "比较文件" file1.ts file2.ts

# 从标准输入
echo "代码内容" | claude -p "解释代码"

# 管道模式
git diff | claude -p "审查更改"
```

**会话管理命令**

```bash
# 继续上次会话
claude --continue

# 恢复特定会话
claude --resume <session-id>

# 列出所有会话
claude --list-sessions
claude --sessions

# 删除会话
claude --delete-session <session-id>

# 导出会话
claude --export-session <session-id> > session.json

# 导入会话
claude --import-session < session.json
```

#### 20.4 文件操作命令

```bash
# 指定文件上下文
claude --file src/main.ts
claude -f src/main.ts

# 多文件
claude --file src/a.ts --file src/b.ts

# 使用 glob 模式
claude --file "src/**/*.ts"

# 排除文件
claude --exclude "node_modules/**" --exclude "*.test.ts"
```

#### 20.5 Git 相关命令

```bash
# Git diff 分析
claude --git diff
claude --git diff HEAD~1

# Git log 分析
claude --git log --oneline -10

# Git 状态
claude --git status

# 分支比较
claude --git diff main..feature-branch

# 暂存区分析
claude --git diff --cached
```

#### 20.6 模型配置命令

```bash
# 指定模型
claude --model claude-opus-4-6
claude -m claude-haiku-4-5

# 可用模型
# - claude-opus-4-6    (最强推理)
# - claude-sonnet-4-6  (平衡性能)
# - claude-haiku-4-5   (快速经济)

# 设置最大 Token
claude --max-tokens 8192

# 设置温度
claude --temperature 0.7

# 设置 Top P
claude --top-p 0.9
```

#### 20.7 工具控制命令

```bash
# 禁用所有工具
claude --no-tools

# 允许特定工具
claude --allow-tools Read,Write,Grep

# 禁用特定工具
claude --deny-tools Bash

# 工具权限模式
claude --permission-mode auto    # 自动批准
claude --permission-mode ask     # 总是询问
claude --permission-mode deny    # 默认拒绝
```

#### 20.8 配置管理命令

```bash
# 查看配置
claude config show
claude config list

# 设置配置项
claude config set model claude-sonnet-4-6
claude config set maxTokens 4096
claude config set temperature 0.7

# 获取配置项
claude config get model

# 重置配置
claude config reset

# 编辑配置文件
claude config edit
```

#### 20.9 诊断与调试命令

```bash
# 运行诊断
claude --diagnostics

# 检查更新
claude --check-update

# 查看系统信息
claude --system-info

# 调试模式
claude --debug

# 日志级别
claude --log-level debug

# 输出日志
claude --log-file ./claude.log
```

#### 20.10 后台任务命令

```bash
# 后台执行
claude --background
claude -b

# 指定输出文件
claude --background --output result.md

# 列出后台任务
claude --jobs

# 查看任务状态
claude --job-status <job-id>

# 查看任务输出
claude --job-output <job-id>

# 停止任务
claude --job-stop <job-id>

# 等待任务完成
claude --job-wait <job-id>
```

#### 20.11 MCP 相关命令

```bash
# 列出 MCP 服务器
claude mcp list

# 添加 MCP 服务器
claude mcp add <name> <command>

# 移除 MCP 服务器
claude mcp remove <name>

# 测试 MCP 连接
claude mcp test <name>

# 查看 MCP 工具
claude mcp tools <name>
```

#### 20.12 Skills 相关命令

```bash
# 列出技能
claude skills list
claude --list-skills

# 安装技能
claude skills install <path>

# 创建技能
claude skills create <name>

# 编辑技能
claude skills edit <name>

# 验证技能
claude skills validate <name>
```

#### 20.13 记忆管理命令

```bash
# 查看记忆
claude memory show
claude memory list

# 添加记忆
claude memory add <type> <content>

# 删除记忆
claude memory remove <id>

# 清除记忆
claude memory clear

# 导出记忆
claude memory export > memory.json

# 导入记忆
claude memory import < memory.json
```

#### 20.14 斜杠命令（Slash Commands）

Claude Code 支持在交互模式中使用斜杠命令：

| 命令 | 说明 |
|------|------|
| `/help` | 显示帮助信息 |
| `/exit` | 退出会话 |
| `/clear` | 清除对话历史 |
| `/save` | 保存当前会话 |
| `/load` | 加载会话 |
| `/undo` | 撤销上一步操作 |
| `/redo` | 重做操作 |
| `/model` | 切换模型 |
| `/tools` | 显示可用工具 |
| `/status` | 显示会话状态 |
| `/tokens` | 显示 Token 使用量 |
| `/review` | 启动代码审查 |
| `/test` | 运行测试 |
| `/commit` | 生成提交信息 |
| `/doc` | 生成文档 |
| `/refactor` | 启动重构模式 |
| `/debug` | 启动调试模式 |

#### 20.15 命令组合示例

**日常开发工作流**

```bash
# 启动开发环境
claude --model claude-sonnet-4-6 --cwd ~/project

# 快速代码审查
git diff | claude -p "审查代码更改"

# 运行测试并修复
npm test 2>&1 | claude -p "修复测试失败"

# 生成提交信息
git diff --cached | claude -p "生成 conventional commit 信息"
```

**CI/CD 集成**

```bash
# 自动代码审查
claude -p "审查代码质量" --file "src/**/*.ts" --json > review.json

# 生成变更日志
git log v1.0.0..HEAD | claude -p "生成变更日志" > CHANGELOG.md

# 文档更新
claude -p "更新 API 文档" src/api/ --output docs/api.md
```

**批量操作**

```bash
# 批量代码格式化
find src -name "*.ts" | xargs -I {} claude -p "格式化代码" {} --output {}

# 批量添加注释
claude -p "为所有公共函数添加 JSDoc 注释" src/ --background
```

#### 20.16 命令速查表

**完整命令速查表**

| 类别 | 命令 | 说明 |
|------|------|------|
| **启动** | `claude` | 启动交互会话 |
| | `claude -p` | 单次执行 |
| | `claude --continue` | 继续会话 |
| **模型** | `--model` | 指定模型 |
| | `--max-tokens` | 最大 Token |
| | `--temperature` | 温度参数 |
| **文件** | `--file` | 指定文件 |
| | `--exclude` | 排除文件 |
| **Git** | `--git diff` | Git diff |
| | `--git log` | Git log |
| **工具** | `--no-tools` | 禁用工具 |
| | `--allow-tools` | 允许工具 |
| | `--deny-tools` | 禁用工具 |
| **配置** | `config show` | 显示配置 |
| | `config set` | 设置配置 |
| | `config reset` | 重置配置 |
| **会话** | `--resume` | 恢复会话 |
| | `--list-sessions` | 列出会话 |
| | `--delete-session` | 删除会话 |
| **后台** | `--background` | 后台执行 |
| | `--jobs` | 列出任务 |
| | `--job-output` | 查看输出 |
| **诊断** | `--diagnostics` | 运行诊断 |
| | `--debug` | 调试模式 |
| | `--version` | 版本信息 |

---

## 第五部分：实战场景

### 第21章 代码重构场景

#### 21.1 大规模 JS → TS 迁移

**场景：** 遗留 JavaScript 项目重构为 TypeScript

**Step 1：让 Claude Code 分析现状**

```bash
# 启动 Claude Code，进入项目目录
cd legacy-js-project
claude

# 在 Claude Code 中输入
> 分析这个项目的代码结构，统计文件类型和代码量
```

Claude Code 会使用 Glob 和 Read 工具扫描项目，输出类似：

```
项目分析报告：
- JavaScript 文件: 127 个 (45,231 行)
- 模块间依赖关系: 高度耦合的 23 个模块
- 测试覆盖率: 约 35%
- 建议迁移顺序: utils → models → services → api → components
```

**Step 2：利用 Plan 模式制定迁移计划**

```bash
# 按 Shift+Tab 切换到 Plan 模式
> 请为 JS → TS 迁移创建详细计划，包括每个阶段的具体任务
```

Plan 模式下 Claude Code 只会规划，不执行代码修改，避免贸然行动。

**Step 3：使用子代理并行迁移**

```bash
# 回到 Edit 模式，开始执行
> 按照计划开始迁移 src/utils/ 目录下的所有文件
```

Claude Code 会：
1. 为每个文件添加 TypeScript 类型注解
2. 将 `.js` 重命名为 `.ts`
3. 修复类型错误
4. 保留原有逻辑不变

**实际输出示例：**

```typescript
// 迁移前: src/utils/format.js
function formatPrice(price, currency) {
  return `${currency} ${price.toFixed(2)}`;
}

// 迁移后: src/utils/format.ts
interface FormatPriceOptions {
  locale?: string;
  minimumFractionDigits?: number;
}

function formatPrice(
  price: number,
  currency: string,
  options: FormatPriceOptions = {}
): string {
  const { locale = 'en-US', minimumFractionDigits = 2 } = options;
  return new Intl.NumberFormat(locale, {
    style: 'currency',
    currency,
    minimumFractionDigits,
  }).format(price);
}
```

#### 21.2 组件拆分重构

**场景：** 一个 800 行的超大 Vue 组件需要拆分

```bash
> 这个 UserDashboard.vue 太大了，请帮我拆分为更小的组件

Claude: 我来分析组件结构。

[使用 Read 工具读取 UserDashboard.vue]

分析发现 5 个可独立的功能区域：

1. UserHeader (头部信息)    → src/components/UserHeader.vue
2. UserStats (统计卡片)     → src/components/UserStats.vue
3. ActivityFeed (活动列表)  → src/components/ActivityFeed.vue
4. SettingsPanel (设置面板) → src/components/SettingsPanel.vue
5. NotificationBar (通知栏) → src/components/NotificationBar.vue

开始拆分...

[使用 Write 创建 5 个新组件]
[使用 Edit 修改 UserDashboard.vue，导入新组件]
```

**重构后的 UserDashboard.vue：**

```vue
<template>
  <div class="dashboard">
    <UserHeader :user="user" />
    <UserStats :stats="stats" />
    <div class="dashboard-content">
      <ActivityFeed :activities="activities" />
      <SettingsPanel v-if="showSettings" @close="showSettings = false" />
    </div>
    <NotificationBar :notifications="unreadNotifications" />
  </div>
</template>

<script setup lang="ts">
import UserHeader from './UserHeader.vue'
import UserStats from './UserStats.vue'
import ActivityFeed from './ActivityFeed.vue'
import SettingsPanel from './SettingsPanel.vue'
import NotificationBar from './NotificationBar.vue'

// 原来 800 行的逻辑已分散到各子组件中
// 这里只保留跨组件的协调逻辑
</script>
```

#### 21.3 使用 refactor-cleaner 代理清理死代码

```bash
# 在 Claude Code 中直接调用 refactor-cleaner 代理
> /agent refactor-cleaner "分析项目中的未使用代码并安全删除"
```

refactor-cleaner 会运行 knip、depcheck、ts-prune 等工具，识别并安全删除：
- 未使用的导出函数
- 未引用的组件
- 多余的依赖包
- 重复的工具函数

---

### 第22章 调试与修复场景

#### 22.1 系统化 Bug 定位

**场景：** 生产环境间歇性数据不一致

**Step 1：描述问题，让 Claude Code 制定排查策略**

```bash
> 生产环境偶尔出现用户余额不一致的问题。
> 错误日志显示 "Balance mismatch: expected 100.00, got 99.50"
> 发生频率约 0.1%，集中在高并发时段
```

Claude Code 会自动：
1. 使用 Grep 搜索 "Balance mismatch" 定位错误源
2. 使用 Read 读取相关代码文件
3. 分析调用链路

**Step 2：Claude Code 的排查输出**

```
## Bug 分析报告

### 问题根因
文件: src/services/transaction-service.ts:89

async processTransaction(userId: string, amount: number) {
  // ❌ 问题 1: 先读后写，没有加锁
  const balance = await this.getBalance(userId);
  // 高并发时，另一个请求可能在这里修改了余额
  const newBalance = balance - amount;
  await this.updateBalance(userId, newBalance);
}

### 调用链分析
1. POST /api/transactions → TransactionController.create()
2. → TransactionService.processTransaction()  ← 问题在这里
3. → BalanceRepository.update()

### 修复方案

```typescript
async processTransaction(userId: string, amount: number) {
  // ✅ 方案 1: 使用数据库事务 + 行锁
  return await this.db.$transaction(async (tx) => {
    const user = await tx.user.findUnique({
      where: { id: userId },
      select: { balance: true },
    });
    // FOR UPDATE 行锁确保并发安全
    if (user.balance < amount) {
      throw new InsufficientBalanceError();
    }
    return await tx.user.update({
      where: { id: userId },
      data: { balance: { decrement: amount } },
    });
  });
}
```
```

#### 22.2 使用 Esc+Esc 回退错误修改

当 Claude Code 的修改引入了新问题：

```bash
# 双击 Esc 键回退到修改前的状态
[Esc] [Esc]

# Claude Code 会提示：
# "Rewind to before the last change? (y/n)"
# 选择 y 后，所有文件变更都会被撤销
```

这是 Claude Code 的"后悔药"功能——按 `Esc+Esc` 可以回退代码和对话到之前的状态。

#### 22.3 日志分析调试

```bash
# 将日志文件直接喂给 Claude Code 分析
> 分析这个错误日志，找出根本原因: @error.log

# 或者使用管道
> ! tail -n 100 /var/log/app/error.log | claude -p "分析这些错误"
```

Claude Code 可以理解堆栈跟踪、错误模式，并定位到具体代码行。

---

### 第23章 项目初始化场景

#### 23.1 使用 /init 快速初始化 CLAUDE.md

```bash
# 进入项目目录并启动 Claude Code
cd my-new-project
claude

# 使用 /init 命令自动生成 CLAUDE.md
> /init
```

Claude Code 会自动分析：
- `package.json` 中的依赖和脚本
- 项目目录结构
- ESLint / Prettier 配置
- 测试框架
- 构建工具

然后生成一份详尽的 `CLAUDE.md` 文件。

#### 23.2 完整的 Vue 3 项目初始化

```bash
> 帮我创建一个 Vue 3 + TypeScript + Naive UI + UnoCSS 的项目

Claude: 我来为你创建完整的项目结构。

[执行多个命令]
```

**Claude Code 执行的操作序列：**

```bash
# 1. 创建 Vite 项目
npm create vite@latest my-app -- --template vue-ts
cd my-app

# 2. 安装核心依赖
npm install naive-ui @vueuse/core pinia vue-router

# 3. 安装开发依赖
npm install -D unocss @unocss/preset-uno @unocss/transformer-directives
npm install -D vitest @vue/test-utils happy-dom
npm install -D eslint @typescript-eslint/parser prettier
```

**自动创建的项目结构：**

```
my-app/
├── src/
│   ├── assets/              # 静态资源
│   ├── components/          # 通用组件
│   │   ├── AppHeader.vue
│   │   └── AppFooter.vue
│   ├── composables/         # 组合式函数
│   │   └── useTheme.ts
│   ├── layouts/             # 布局组件
│   │   └── DefaultLayout.vue
│   ├── pages/               # 页面
│   │   ├── index.vue
│   │   └── about.vue
│   ├── router/              # 路由
│   │   └── index.ts
│   ├── stores/              # Pinia 状态管理
│   │   └── user.ts
│   ├── types/               # TypeScript 类型
│   │   └── index.ts
│   ├── utils/               # 工具函数
│   │   └── request.ts
│   ├── App.vue
│   └── main.ts
├── tests/                   # 测试目录
│   └── components/
├── CLAUDE.md               # Claude Code 配置
├── uno.config.ts           # UnoCSS 配置
├── vite.config.ts          # Vite 配置
├── tsconfig.json           # TypeScript 配置
└── package.json
```

**自动生成的 CLAUDE.md：**

```markdown
# My App

## 技术栈
- Vue 3 + TypeScript + Composition API
- Naive UI 组件库
- UnoCSS 原子化 CSS
- Pinia 状态管理
- Vue Router 路由

## 开发命令
- 启动: `npm run dev`
- 构建: `npm run build`
- 测试: `npm run test`
- Lint: `npm run lint`

## 编码规范
- 组件使用 `<script setup lang="ts">`
- 使用 Composition API，禁止 Options API
- 样式使用 UnoCSS 原子类
- 函数命名: camelCase
- 组件命名: PascalCase
- 文件命名: kebab-case
```

#### 23.3 为已有项目添加 Claude Code 支持

```bash
# 进入已有项目
cd existing-project
claude

# 让 Claude Code 了解项目并创建配置
> 分析这个项目，创建 CLAUDE.md 和 .claude/settings.json

# 使用 # 快捷键快速添加规则
> # 所有 API 接口必须添加错误处理
> # 提交前必须运行 npm test
> # 使用 Conventional Commits 格式
```

每次用 `#` 添加的内容都会自动写入 `CLAUDE.md`，实现增量式的项目配置积累。

#### 23.4 使用 Hooks 自动化项目工作流

在 `.claude/settings.json` 中配置自动化钩子：

```json
{
  "hooks": {
    "PostToolUse": [
      {
        "matcher": "Write",
        "command": "npx prettier --write $FILE_PATH"
      }
    ],
    "Stop": [
      {
        "matcher": "*",
        "command": "npm run type-check && npm test"
      }
    ]
  }
}
```

这样 Claude Code 每次写入文件后会自动格式化，会话结束前会自动运行类型检查和测试。

---

## 第六部分：深入原理

> **说明：** 本部分内容基于 2026 年 3 月 31 日 Claude Code v2.1.88 通过 npm 包中遗留的 `cli.js.map` 文件意外泄露的完整 TypeScript 源码（1906 个源文件，512,685 行代码）进行分析。运行时为 Bun，终端 UI 使用 React + Ink，协议层接入 MCP 和 LSP。

### 第24章 架构设计总览

#### 24.1 架构哲学：Harness 而非 Agent

理解 Claude Code 的设计需要从核心主张出发：**系统工程只负责提供环境（Harness），不试图干预模型的推理过程**。它不强加固定工作流，不用复杂决策树干预模型判断，只提供工具、上下文管理和权限边界，然后让模型自由执行。

剥掉所有细节，架构本质是：

```
一个 Agent 循环 + 工具集（bash/read/write/edit/grep…）
    + 按需技能加载 + 上下文压缩
    + 子 Agent 派生 + 任务依赖图
    + 工作树隔离并行 + 权限治理
```

**技术选型一览：**

| 类别 | 选型 | 说明 |
|------|------|------|
| 语言 | TypeScript | 类型安全，51.2 万行代码 |
| 运行时 | Bun | 非 Node.js，启动速度更快 |
| 终端 UI | React + Ink | 组件化终端 UI，81K 行 |
| CLI 框架 | Commander.js | 50+ 斜杠命令 |
| Schema 校验 | Zod v4 | 工具输入/输出校验 |
| 布局引擎 | Yoga (WASM) | Flexbox 终端布局 |

#### 24.2 代码规模全景

源码 `src/` 下有 35 个顶层目录，代码量分布如下：

```
src/
├── utils/          (180K 行)  # 权限系统、Bash 安全检查、模型管理等核心逻辑
├── components/     ( 81K 行)  # 140+ Ink 终端 UI 组件
├── services/       ( 53K 行)  # API、MCP 客户端、OAuth、上下文压缩等
├── tools/          ( 50K 行)  # 40+ Agent 工具实现
├── commands/       ( 26K 行)  # 50+ 斜杠命令
├── hooks/                     # 权限检查钩子
├── bridge/                    # IDE 双向通信
├── cli/                       # 输出格式化
├── skills/                    # 内置 Skill
├── memdir/                    # 记忆目录
├── coordinator/               # 多 Agent 协调器
├── buddy/                     # 虚拟宠物系统
├── main.tsx                   # 入口文件
├── query.ts                   # 主循环（系统心脏）
├── QueryEngine.ts             # 核心引擎（1295 行）
└── Tool.ts                    # 工具基类（792 行）
```

**关键架构观察：**

1. **`query.ts` 是整个系统的心脏**：所有数据流最终汇聚于此——上下文、工具、压缩、API 调用
2. **AgentTool 递归调用 `query.ts`**：子 Agent 是一个新的 query loop 实例，拥有独立上下文，可无限嵌套
3. **`utils/` 是隐藏的核心**：180K 行，超过总量的 1/3，藏着最复杂的业务逻辑
4. **UI 复杂度接近 GUI 应用**：100K+ 行 UI 代码，权限弹窗、多 Agent 面板、diff 预览等

#### 24.3 整体架构分层

```
┌──────────────────────────────────────────────────────────────┐
│                    Presentation Layer                         │
│  CLI Parser (Commander.js) │ TUI/REPL (React+Ink) │ Output   │
├──────────────────────────────────────────────────────────────┤
│                    Agent Layer (query.ts)                     │
│  QueryEngine │ Context Manager │ Agent Loop │ Permission     │
│  Subagent    │ Memory System   │ Coordinator │ Session       │
├──────────────────────────────────────────────────────────────┤
│                    Tool Layer (Tool.ts)                       │
│  40+ Built-in Tools │ MCP Client │ Skill System │ Hooks     │
│  StreamingToolExecutor │ ToolSearch (按需加载)                │
├──────────────────────────────────────────────────────────────┤
│                    Infrastructure Layer                       │
│  Anthropic API │ Config System │ Prompt Cache │ Storage      │
│  Telemetry     │ Feature Flags │ OAuth        │ GrowthBook   │
└──────────────────────────────────────────────────────────────┘
```

#### 24.4 核心组件交互流程

```
用户输入
  │
  ▼
main.tsx ──启动前预取──► MDM 配置 + macOS 钥匙串（利用 ~135ms 模块加载窗口）
  │
  ▼
query.ts ──五步预处理流水线──┐
  │                           │
  ├─ 1. 收集上下文            │  CLAUDE.md + Git 状态 + MCP 指令
  ├─ 2. 构建 System Prompt    │  静态段 + 动态段（分界符分隔）
  ├─ 3. 组装工具描述           │  字母表排序确保缓存命中
  ├─ 4. 调用 Anthropic API    │  AsyncGenerator 流式输出
  └─ 5. 执行工具 / 返回结果    │  StreamingToolExecutor 协调
  │                           │
  ▼                           │
模型响应 ──如有工具调用──► 循环回 query.ts
  │
  ▼
最终回复展示给用户
```

#### 24.5 入口文件工程细节

`main.tsx` 的启动优化值得一提：

```typescript
// 在任何 import 语句执行前，先通过副作用触发预取
// 利用后续模块加载约 135ms 的时间窗口并行完成
prefetchMDMConfig();       // MDM 配置预读
prefetchKeychainAccess();  // macOS 钥匙串预取
// 实测节省约 65ms 启动时间

// Feature Flag 系统，隐藏功能的开关硬编码于此
const FEATURE_FLAGS = {
  KAIROS: false,     // 常驻自治模式
  PROACTIVE: false,  // 主动建议
  TORCH: false,      // 调试增强
  BUDDY: false,      // 虚拟宠物
  ULTRAPLAN: false,  // 云端规划
  VOICE_MODE: false, // 语音交互
  DAEMON: false,     // 后台进程
};
```

#### 24.6 扩展性设计

Claude Code 的每一层都可以独立演进，关注点分离清晰：

| 扩展点 | 实现方式 | 说明 |
|--------|----------|------|
| **Tools** | `Tool.ts` 接口 + Zod Schema | 添加新的操作能力 |
| **Skills** | SKILL.md + 三层渐进加载 | 定义可复用的提示词模板 |
| **Hooks** | PreToolUse / PostToolUse / Stop | 在关键节点注入自定义逻辑 |
| **MCP** | JSON-RPC Protocol (stdio/http) | 集成外部工具和服务 |
| **Subagents** | frontmatter 配置 + Agent 工具 | 创建专用子代理处理特定任务 |
| **Plugins** | 动态加载 | 动态加载扩展模块 |
│                              │                                          │
│                              ▼                                          │
│  ┌─────────────────────────────────────────────────────────────────┐   │
│  │                    Infrastructure Layer                          │   │
│  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐             │   │
│  │  │   Config    │  │    API      │  │   Storage   │             │   │
│  │  │   System    │  │   Client    │  │   System    │             │   │
│  │  │  配置系统    │  │  API客户端  │  │   存储系统   │             │   │
│  │  └─────────────┘  └─────────────┘  └─────────────┘             │   │
│  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐             │   │
│  │  │   Logger    │  │   Cache     │  │   Plugin    │             │   │
│  │  │   System    │  │   System    │  │   Loader    │             │   │
│  │  │  日志系统    │  │   缓存系统   │  │  插件加载器  │             │   │
│  │  └─────────────┘  └─────────────┘  └─────────────┘             │   │
│  └─────────────────────────────────────────────────────────────────┘   │
│                                                                         │
└────────────────────────────────────────────────────────────────────────┘
```
### 第25章 Prompt Cache 与上下文压缩机制

#### 25.1 Prompt Cache：字节级的成本控制

这是 Claude Code 与普通 AI 套壳应用差距最显著的地方。整个系统对缓存的处理达到了精细化程度。

**分段策略**

提示词被切成两段，之间用硬编码标记 `SYSTEM_PROMPT_DYNAMIC_BOUNDARY` 分隔：

```
┌──────────────────────────────────────────────────────┐
│              Static Segment（静态段）                  │
│  - 模型身份定义                                       │
│  - 系统安全规则                                       │
│  - 代码风格约束                                       │
│  - 工具基础指南                                       │
│  缓存命中率极高，整个会话几乎不变                       │
├──── SYSTEM_PROMPT_DYNAMIC_BOUNDARY ─────────────────┤
│              Dynamic Segment（动态段）                 │
│  - 当前工作目录                                       │
│  - Git 状态                                           │
│  - MCP 指令                                           │
│  - 用户配置                                           │
│  每次请求可能变化                                      │
└──────────────────────────────────────────────────────┘
```

System Prompt 优先级链：`Override > Coordinator > Agent > Custom > Default`，不同模式注入不同层级的指令。

**确定性排序**

传给模型的工具描述**严格按字母表排序**（内置工具前缀在前，MCP 工具后缀在后）。这个细节不起眼，但很关键：工具描述是 prompt 的一部分，如果顺序每次不同，prompt 哈希就变了，缓存直接失效。配置文件路径同样使用基于内容的哈希值映射，避免路径字符串变化导致缓存击穿。

**状态外置**

当前可用的 Agent 列表原来写在工具描述里，Agent 列表每次变化就得重建整个 prompt 缓存。源码里把这个列表剥离出来，转移到消息附件（Attachments）中。仅这一项，减少了约 **10.2%** 的 Cache Creation Tokens 消耗。

**cache_edits 墓碑化机制**

这是维持长对话性能不退化的关键：

```
对话历史:
┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────┐
│ 消息 1   │ │ 消息 2   │ │ 消息 3   │ │ 消息 4   │
│ (active) │ │ (skipped)│ │ (skipped)│ │ (active) │
└──────────┘ └──────────┘ └──────────┘ └──────────┘
                ↑             ↑
          墓碑化标记      墓碑化标记
          模型跳过        模型跳过
          但保留位置      缓存前缀不断裂
```

长对话上下文超出阈值时，旧消息不会被物理删除，而是被标记为 `skipped`。模型推理时跳过这些消息，但消息依然占据 prompt 中的位置，因此缓存前缀的哈希连续性没有被打断。这解释了为什么 Claude Code 在数小时的长对话中响应速度不会明显降级。

#### 25.2 上下文管理挑战

Claude Code 在实际使用中面临严峻的上下文管理挑战：

**核心问题**

```
┌─────────────────────────────────────────────────────────────────┐
│                    上下文窗口管理挑战                              │
├─────────────────────────────────────────────────────────────────┤
│                                                                  │
│  1. Token 限制                                                   │
│     ┌─────────────────────────────────────────────────────┐    │
│     │  Claude 4.x 最大上下文: ~200K tokens                 │    │
│     │  实际可用: ~150K tokens (保留输出空间)                │    │
│     │  系统提示词: ~10K tokens                             │    │
│     │  工具定义: ~5K tokens                                │    │
│     │  剩余可用: ~135K tokens                              │    │
│     └─────────────────────────────────────────────────────┘    │
│                                                                  │
│  2. 信息密度问题                                                 │
│     - 长对话历史占用大量空间                                      │
│     - 大文件内容可能超出预算                                      │
│     - 工具调用记录累积                                            │
│                                                                  │
│  3. 相关性判断                                                   │
│     - 哪些历史信息对当前任务最重要？                              │
│     - 如何平衡完整性和效率？                                      │
│     - 如何避免丢失关键上下文？                                    │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

#### 25.3 /compact 命令与自动压缩

**自动压缩触发条件**

当上下文使用超过阈值（默认约 190K tokens 的 80%）时，系统会发出警告提示用户运行 `/compact`。同时，Claude Code 也支持自动压缩——当检测到上下文即将溢出时，会 fork 一个独立 Agent 专门执行压缩任务，避免阻塞主对话流。

**压缩实现机制**

```
/compact 执行流程：

1. 收集当前对话历史 messages[]
2. 构造压缩提示词，要求生成结构化摘要
3. 调用 LLM 生成摘要（使用独立的系统提示词）
4. 清空原对话历史
5. 用摘要作为新对话的初始上下文
6. 清除文件系统和代码风格缓存
```

实际的压缩 prompt 核心内容：

```
"Provide a detailed but concise summary of our conversation above.
Focus on information that would be helpful for continuing the conversation,
including what we did, what we're doing, which files we're working on,
and what we're going to do next."
```

**九段式结构化摘要**

长对话压缩后的摘要遵循严格的 9 段式结构：

| 段号 | 内容 | 说明 |
|------|------|------|
| 1 | 会话目标 | 当前任务的核心目标 |
| 2 | 已完成任务 | 已经做了什么 |
| 3 | 未完成任务 | 还需要做什么 |
| 4 | 关键决策和理由 | 做了哪些技术选择，为什么 |
| 5 | 代码变更摘要 | 修改了哪些文件，做了什么改动 |
| 6 | 发现的问题 | 遇到了什么错误或障碍 |
| 7 | 待验证的假设 | 还需要确认的事项 |
| 8 | 用户偏好 | 用户表达过的编码风格、工作方式偏好 |
| 9 | 上下文关键信息 | 必须保留的其他重要信息 |

每段有明确格式要求，压缩后模型能快速恢复上下文状态。

**熔断机制**

源码中还内置了熔断器（Circuit Breaker）设计：连续 3 次工具调用失败时，系统会自动中断当前执行链并进入异常处理流程，避免无限循环消耗上下文。

#### 25.4 上下文预算分配策略

```
200K tokens 上下文窗口分配：
┌──────────────────────────────────────────┐
│ System Prompt (静态)        ~10K tokens  │
│ 工具定义 (40+ 工具)        ~5K tokens   │
│ CLAUDE.md + 配置           ~3K tokens   │
│ Git 状态 + 环境信息        ~2K tokens   │
├──────────────────────────────────────────┤
│ 对话历史                   ~10K tokens  │  ← 压缩目标区域
│ 文件内容 (动态加载)        ~80K tokens  │
│ 工具调用结果               ~40K tokens  │
├──────────────────────────────────────────┤
│ 输出预留                   ~32K tokens  │
│ 安全缓冲                   ~18K tokens  │  ← 避免使用最后 20%
└──────────────────────────────────────────┘
```
```
---

### 第26章 源码分析：核心引擎

> 本章基于泄露的 TypeScript 源码分析，Claude Code 使用 TypeScript 开发、Bun 构建、React + Ink 渲染终端 UI。

#### 26.1 Agentic Loop：五步流水线与流式执行

官方文档用一张图概括了核心循环：`Gather context → Take action → Verify results`，循环直到任务完成。源码里，这个循环的核心是 `query.ts` 中的 AsyncGenerator 流水线。

**QueryEngine.ts（1295 行）——核心引擎**

`submitMessage()` 是一个 `AsyncGenerator<SDKMessage>`，每次 `yield` 一个消息片段，上层通过 `for await...of` 消费：

```typescript
// QueryEngine.ts 核心结构（简化）
async function* submitMessage(
  messages: Message[],
  systemPrompt: string[],
  tools: Tool[],
  options: QueryOptions
): AsyncGenerator<SDKMessage> {
  // 五步预处理流水线
  // Step 1: 构建系统提示词（静态段 + 动态段）
  const fullSystemPrompt = buildSystemPrompt(systemPrompt, options);

  // Step 2: 组装工具描述（严格字母表排序 → 缓存命中）
  const toolDescriptions = tools
    .sort((a, b) => a.name.localeCompare(b.name))
    .map(t => t.toSchema());

  // Step 3: 转换消息格式 + cache_edits 墓碑化
  const apiMessages = transformMessages(messages);

  // Step 4: 调用 Anthropic API（流式）
  const stream = await anthropicClient.messages.create({
    model: options.model,
    system: fullSystemPrompt,
    messages: apiMessages,
    tools: toolDescriptions,
    stream: true,
    max_tokens: options.maxTokens,
  });

  // Step 5: 流式消费响应，遇到 tool_use 时执行工具
  for await (const event of stream) {
    if (event.type === 'content_block_delta') {
      yield event;  // 流式文本输出
    }
    if (event.type === 'tool_use') {
      const result = await executeToolCall(event);
      yield result;  // 工具结果
      // 递归：将工具结果加入消息，继续下一轮 API 调用
    }
  }
}
```

这个设计让流式响应、工具调用、中断恢复的处理方式统一了——都是生产者-消费者模型。

#### 26.2 工具系统与四层权限管道

**Tool.ts（792 行）——工具抽象**

每个工具包含 20+ 个方法：名称、执行函数、Zod Schema 校验、权限检查、是否只读、是否可并发、结果大小上限等。

```typescript
// Tool.ts 核心接口（简化）
interface ToolDefinition {
  name: string;
  description: string;
  inputSchema: ZodSchema;           // Zod v4 校验
  needsPermissions: boolean;        // 是否需要权限
  isReadOnly: boolean;              // 是否只读
  isConcurrencySafe(): boolean;     // 能否并行执行
  maxResultSize: number;            // 结果大小上限
  defer_loading?: boolean;          // 是否按需加载
  call(input: unknown): AsyncGenerator<ToolMessage>;
}
```

**StreamingToolExecutor** 负责协调，将工具调用分区为并发批次和串行批次。

**按需加载（ToolSearch）**

把 40+ 工具描述全塞进 prompt 会消耗大量 tokens。解法是非核心工具标记 `defer_loading: true`，模型默认看不到具体定义，只知道有 `ToolSearch` 可用，需要时传入关键词动态加载。这和 Web 开发的 Code Splitting 是同一个思路。

**四层权限管道**

工具执行不是简单的允许/拒绝，而是四层递进：

```
工具调用请求
     │
     ▼
┌─ Layer 1: Config Rules ────────────────────────────┐
│  .claude/settings.json 中的用户自定义规则           │
│  静态匹配 → allow / deny / 继续                    │
└────────────────────────────────────────────────────┘
     │ 未匹配
     ▼
┌─ Layer 2: 语义分析 ───────────────────────────────┐
│  tree-sitter 解析命令 AST                          │
│  检查读写路径是否在工作目录范围内                    │
│  原则："宁可误杀不可漏放"                           │
└────────────────────────────────────────────────────┘
     │ 未拦截
     ▼
┌─ Layer 3: AI Classifier ──────────────────────────┐
│  侧查询一个更小的 LLM 异步判断命令安全性            │
│  结果是结构化 JSON + Zod Schema 校验               │
│  超时/失败 → fallback 策略是 ask                   │
│  (Auto Mode 下生效)                                │
└────────────────────────────────────────────────────┘
     │ 分类为安全
     ▼
┌─ Layer 4: 用户手动确认 ───────────────────────────┐
│  前三层都通过后仍不在白名单 → 弹窗询问              │
│  用户选择可记忆为 alwaysAllow / alwaysDeny          │
└────────────────────────────────────────────────────┘
```

**投机性预检（Speculative Classifier）**

值得单独提的优化：当模型还在流式输出 BashTool 参数时，系统已经并行启动分类器了。模型输出完成时，分类结果可能已就绪，消除了串行等待。

```typescript
// 伪代码：投机性预检
function startSpeculativeClassifierCheck(partialToolInput: string) {
  // 模型还在输出参数，提前启动分类
  classifierPromise = classifyCommand(partialToolInput);
}

async function onToolInputComplete(fullToolInput: string) {
  // 如果预检结果已就绪且输入未变，直接使用
  const result = await classifierPromise;
  if (result.inputMatches(fullToolInput)) return result;
  // 否则重新分类
  return classifyCommand(fullToolInput);
}
```

#### 26.3 Coordinator + Subagent：隔离探索

长任务中 Agent 上下文会被探索性操作污染。Claude Code 的解法是 **Coordinator + Fork Subagent** 两层架构：

```
┌─────────────────────────────────────────────────┐
│               Coordinator (协调者)                │
│  只有 3 个工具: Agent / SendMessage / TaskStop   │
│  不能直接操作文件                                 │
│  负责: 规划 → 拆分 → 分发 → 整合                │
├─────────────────────────────────────────────────┤
│                    ↓ 派生                        │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐     │
│  │ Worker 1 │  │ Worker 2 │  │ Worker 3 │     │
│  │ 独立上下文│  │ 独立上下文│  │ 独立上下文│     │
│  │ 携带工具 │  │ 携带工具 │  │ 携带工具 │     │
│  └──────────┘  └──────────┘  └──────────┘     │
│       │              │              │            │
│       └──── <task-notification> ────┘            │
│              提炼后的结论传回                      │
└─────────────────────────────────────────────────┘
```

三个关键机制：
1. **子 Agent 继承父对话的 Prompt Cache**（共享缓存前缀，创建成本低）
2. **探索产生的所有中间输出限制在子 Agent 上下文里**，不污染主对话
3. **子 Agent 完成后通过 XML 格式 `<task-notification>` 传回提炼后的结论**

系统支持 6 种任务类型：

| 类型 | 说明 |
|------|------|
| `local_bash` | 本地 Bash 命令 |
| `local_agent` | 本地子 Agent |
| `remote_agent` | 远程 Agent |
| `in_process_teammate` | 进程内并行 Agent |
| `local_workflow` | 本地工作流 |
| `dream` | 记忆整合（KAIROS 相关） |

其中 `in_process_teammate` 可平行唤醒多个 Agent 同时执行。源码里还集成了 iTerm2 和 Terminal.app 的 AppleScript 控制指令，派生新 Teammate 时自动切分终端窗格。

#### 26.4 网络工具的工程取舍

网络检索工具不是通用搜索引擎，而是对约 **85 个主流技术文档域**（React、Vue 等前端框架及基础设施）设置白名单进行全量抽取；非白名单域只提取短摘要。解析 DOM 时直接丢弃 `<head>` 中的结构化数据，只保留 `<body>` 内容。这是以牺牲通用性换取上下文稳定性的工程取舍。

#### 26.5 逆向分析方法论

如果你想自己分析 Claude Code 源码，以下是社区验证过的方法：

```
步骤 1: 获取 cli.mjs（4.6MB 压缩后代码）
步骤 2: js-beautify 美化 → 19 万行代码
步骤 3: acorn 解析 AST，按 top-level scope 分割为 chunks
步骤 4: LLM 识别每个 chunk 所属的开源项目（React、Ink、aws-sdk 等）
步骤 5: 人为审核识别结果，纠正误判
步骤 6: 按项目重新合并 chunks
步骤 7: 分离出 Claude Code 自身代码（6 个碎片）
步骤 8: 逐碎片向 LLM 提问，累积跨碎片的上下文
```

这种方法在 LLM 帮助下可在 1 小时内完成，结果比传统人工逆向更好。

---

### 第26章附录：社区 Rust 重新实现参考

> 以下为社区 Rust 重新实现版本的参考目录结构（如 claurst、claude-code-rust），非官方 TypeScript 源码。仅供参考。

```
claude-code-rust/ (社区实现)
├── src/
│   ├── main.rs              # 入口点
│   ├── cli/                 # CLI 相关
│   │   ├── mod.rs
│   │   ├── parser.rs        # 命令解析
│   │   └── output.rs        # 输出处理
│   ├── core/                # 核心引擎
│   │   ├── mod.rs
│   │   ├── session.rs       # 会话管理
│   │   ├── context.rs       # 上下文管理
│   │   └── message.rs       # 消息处理
│   ├── tools/               # 工具系统
│   │   ├── mod.rs
│   │   ├── dispatcher.rs    # 工具调度
│   │   ├── read.rs          # Read 工具
│   │   ├── write.rs         # Write 工具
│   │   └── bash.rs          # Bash 工具
│   ├── api/                 # API 交互
│   │   ├── mod.rs
│   │   ├── client.rs        # API 客户端
│   │   └── stream.rs        # 流式响应
│   ├── memory/              # 记忆系统
│   │   ├── mod.rs
│   │   ├── store.rs         # 存储实现
│   │   └── loader.rs        # 加载器
│   └── extension/           # 扩展系统
│       ├── mod.rs
│       ├── plugin.rs        # 插件管理
│       ├── skill.rs         # 技能系统
│       └── hook.rs          # 钩子系统
├── Cargo.toml
└── README.md
```

#### 26.2 核心数据结构

**Session 结构**

```rust
// src/core/session.rs

use chrono::{DateTime, Utc};
use std::collections::VecDeque;

/// 会话结构
pub struct Session {
    /// 会话 ID
    pub id: SessionId,

    /// 工作目录
    pub working_dir: PathBuf,

    /// 对话历史
    pub messages: VecDeque<Message>,

    /// 创建时间
    pub created_at: DateTime<Utc>,

    /// 最后更新时间
    pub updated_at: DateTime<Utc>,

    /// 会话状态
    pub state: SessionState,

    /// 上下文管理器
    context: ContextManager,

    /// 配置
    config: SessionConfig,
}

/// 会话状态
#[derive(Debug, Clone, PartialEq)]
pub enum SessionState {
    /// 活跃状态
    Active,

    /// 暂停状态
    Paused,

    /// 已完成
    Completed,

    /// 错误状态
    Error(String),
}

/// 会话配置
#[derive(Debug, Clone)]
pub struct SessionConfig {
    /// 使用的模型
    pub model: Model,

    /// 最大 Token 数
    pub max_tokens: usize,

    /// 温度参数
    pub temperature: f32,

    /// 启用的工具
    pub enabled_tools: HashSet<String>,

    /// 权限配置
    pub permissions: Permissions,
}

impl Session {
    /// 创建新会话
    pub fn new(working_dir: PathBuf, config: SessionConfig) -> Self {
        Self {
            id: SessionId::new(),
            working_dir,
            messages: VecDeque::new(),
            created_at: Utc::now(),
            updated_at: Utc::now(),
            state: SessionState::Active,
            context: ContextManager::new(config.max_tokens),
            config,
        }
    }

    /// 添加用户消息
    pub fn add_user_message(&mut self, content: String) {
        let message = Message {
            role: Role::User,
            content: MessageContent::Text(content),
            timestamp: Utc::now(),
        };
        self.messages.push_back(message);
        self.updated_at = Utc::now();
    }

    /// 添加助手消息
    pub fn add_assistant_message(&mut self, content: MessageContent) {
        let message = Message {
            role: Role::Assistant,
            content,
            timestamp: Utc::now(),
        };
        self.messages.push_back(message);
        self.updated_at = Utc::now();
    }

    /// 构建请求上下文
    pub fn build_context(&self) -> Result<RequestContext, Error> {
        self.context.build(
            &self.messages,
            &self.working_dir,
            &self.config,
        )
    }
}
```

**Message 结构**

```rust
// src/core/message.rs

use chrono::{DateTime, Utc};
use serde::{Deserialize, Serialize};

/// 消息角色
#[derive(Debug, Clone, Serialize, Deserialize, PartialEq)]
#[serde(rename_all = "lowercase")]
pub enum Role {
    User,
    Assistant,
    System,
}

/// 消息内容
#[derive(Debug, Clone, Serialize, Deserialize)]
#[serde(tag = "type", rename_all = "snake_case")]
pub enum MessageContent {
    /// 纯文本
    Text(String),

    /// 包含工具调用
    ToolUse {
        id: String,
        name: String,
        input: serde_json::Value,
    },

    /// 工具结果
    ToolResult {
        tool_use_id: String,
        content: String,
        is_error: bool,
    },

    /// 组合内容
    Composite(Vec<MessageContent>),
}

/// 消息结构
#[derive(Debug, Clone, Serialize, Deserialize)]
pub struct Message {
    /// 角色
    pub role: Role,

    /// 内容
    pub content: MessageContent,

    /// 时间戳
    #[serde(skip)]
    pub timestamp: DateTime<Utc>,
}

impl Message {
    /// 计算消息的 Token 数量
    pub fn token_count(&self) -> usize {
        match &self.content {
            MessageContent::Text(text) => {
                // 简化的 Token 计数
                text.split_whitespace().count()
            }
            MessageContent::ToolUse { input, .. } => {
                // 估算 JSON 的 Token 数
                serde_json::to_string(input)
                    .map(|s| s.split_whitespace().count())
                    .unwrap_or(0)
            }
            MessageContent::ToolResult { content, .. } => {
                content.split_whitespace().count()
            }
            MessageContent::Composite(parts) => {
                parts.iter().map(|p| {
                    Message {
                        role: self.role.clone(),
                        content: p.clone(),
                        timestamp: self.timestamp,
                    }.token_count()
                }).sum()
            }
        }
    }
}
```

#### 26.3 对话管理实现

```rust
// src/core/session.rs (续)

impl Session {
    /// 发送消息并获取响应
    pub async fn send_message(
        &mut self,
        content: String,
        api_client: &ApiClient,
    ) -> Result<Message, Error> {
        // 1. 添加用户消息
        self.add_user_message(content);

        // 2. 构建请求上下文
        let context = self.build_context()?;

        // 3. 调用 API
        let response = api_client
            .send_message(context)
            .await?;

        // 4. 处理响应
        let assistant_message = self.process_response(response).await?;

        // 5. 添加助手消息
        self.add_assistant_message(assistant_message.content.clone());

        Ok(assistant_message)
    }

    /// 处理 API 响应
    async fn process_response(
        &mut self,
        response: ApiResponse,
    ) -> Result<Message, Error> {
        match response.content {
            MessageContent::Text(text) => {
                Ok(Message {
                    role: Role::Assistant,
                    content: MessageContent::Text(text),
                    timestamp: Utc::now(),
                })
            }
            MessageContent::ToolUse { id, name, input } => {
                // 执行工具
                let result = self.execute_tool(&name, input).await?;

                // 创建工具结果消息
                Ok(Message {
                    role: Role::Assistant,
                    content: MessageContent::ToolResult {
                        tool_use_id: id,
                        content: result,
                        is_error: false,
                    },
                    timestamp: Utc::now(),
                })
            }
            MessageContent::Composite(parts) => {
                // 处理组合内容
                let mut results = Vec::new();

                for part in parts {
                    if let MessageContent::ToolUse { id, name, input } = part {
                        let result = self.execute_tool(&name, input).await?;
                        results.push(MessageContent::ToolResult {
                            tool_use_id: id,
                            content: result,
                            is_error: false,
                        });
                    } else {
                        results.push(part);
                    }
                }

                Ok(Message {
                    role: Role::Assistant,
                    content: MessageContent::Composite(results),
                    timestamp: Utc::now(),
                })
            }
            _ => Err(Error::InvalidResponse("Unexpected content type".into())),
        }
    }
}
```

#### 26.4 工具调度实现

```rust
// src/tools/dispatcher.rs

use std::collections::HashMap;
use async_trait::async_trait;

/// 工具特征
#[async_trait]
pub trait Tool: Send + Sync {
    /// 工具名称
    fn name(&self) -> &str;

    /// 工具描述
    fn description(&self) -> &str;

    /// 参数 Schema
    fn parameters(&self) -> serde_json::Value;

    /// 执行工具
    async fn execute(&self, params: serde_json::Value) -> Result<String, ToolError>;
}

/// 工具调度器
pub struct ToolDispatcher {
    /// 注册的工具
    tools: HashMap<String, Box<dyn Tool>>,

    /// 权限检查器
    permission_checker: PermissionChecker,

    /// 执行历史
    execution_history: Vec<ToolExecution>,
}

/// 工具执行记录
#[derive(Debug, Clone)]
pub struct ToolExecution {
    pub tool_name: String,
    pub params: serde_json::Value,
    pub result: Result<String, ToolError>,
    pub timestamp: DateTime<Utc>,
    pub duration: Duration,
}

impl ToolDispatcher {
    /// 创建新的调度器
    pub fn new() -> Self {
        let mut dispatcher = Self {
            tools: HashMap::new(),
            permission_checker: PermissionChecker::new(),
            execution_history: Vec::new(),
        };

        // 注册内置工具
        dispatcher.register_builtin_tools();

        dispatcher
    }

    /// 注册内置工具
    fn register_builtin_tools(&mut self) {
        self.register(Box::new(ReadTool::new()));
        self.register(Box::new(WriteTool::new()));
        self.register(Box::new(EditTool::new()));
        self.register(Box::new(BashTool::new()));
        self.register(Box::new(GrepTool::new()));
        self.register(Box::new(GlobTool::new()));
    }

    /// 注册工具
    pub fn register(&mut self, tool: Box<dyn Tool>) {
        self.tools.insert(tool.name().to_string(), tool);
    }

    /// 执行工具
    pub async fn execute(
        &mut self,
        tool_name: &str,
        params: serde_json::Value,
        permissions: &Permissions,
    ) -> Result<String, ToolError> {
        let start = std::time::Instant::now();

        // 1. 查找工具
        let tool = self.tools.get(tool_name)
            .ok_or_else(|| ToolError::NotFound(tool_name.to_string()))?;

        // 2. 检查权限
        self.permission_checker.check(tool_name, permissions)?;

        // 3. 验证参数
        self.validate_params(tool, &params)?;

        // 4. 执行工具
        let result = tool.execute(params.clone()).await;

        // 5. 记录执行历史
        self.execution_history.push(ToolExecution {
            tool_name: tool_name.to_string(),
            params,
            result: result.clone(),
            timestamp: Utc::now(),
            duration: start.elapsed(),
        });

        result
    }

    /// 验证参数
    fn validate_params(
        &self,
        tool: &dyn Tool,
        params: &serde_json::Value,
    ) -> Result<(), ToolError> {
        // 使用 JSON Schema 验证
        let schema = tool.parameters();
        let validator = jsonschema::validator_for(&schema)
            .map_err(|e| ToolError::Validation(e.to_string()))?;

        let result = validator.validate(params);
        if let Err(errors) = result {
            let error_messages: Vec<String> = errors
                .map(|e| e.to_string())
                .collect();
            return Err(ToolError::Validation(error_messages.join(", ")));
        }

        Ok(())
    }

    /// 获取所有工具定义
    pub fn get_tool_definitions(&self) -> Vec<ToolDefinition> {
        self.tools.values().map(|tool| {
            ToolDefinition {
                name: tool.name().to_string(),
                description: tool.description().to_string(),
                parameters: tool.parameters(),
            }
        }).collect()
    }
}
```

#### 26.5 API 交互实现

```rust
// src/api/client.rs

use reqwest::Client;
use serde::{Deserialize, Serialize};
use futures::StreamExt;

/// API 客户端
pub struct ApiClient {
    /// HTTP 客户端
    client: Client,

    /// API Key
    api_key: String,

    /// API 基础 URL
    base_url: String,
}

/// API 请求
#[derive(Debug, Serialize)]
pub struct ApiRequest {
    pub model: String,
    pub messages: Vec<ApiMessage>,
    #[serde(skip_serializing_if = "Option::is_none")]
    pub max_tokens: Option<usize>,
    #[serde(skip_serializing_if = "Option::is_none")]
    pub temperature: Option<f32>,
    #[serde(skip_serializing_if = "Option::is_none")]
    pub tools: Option<Vec<ToolDefinition>>,
    pub stream: bool,
}

/// API 响应
#[derive(Debug, Deserialize)]
pub struct ApiResponse {
    pub id: String,
    pub content: MessageContent,
    pub stop_reason: Option<String>,
    pub usage: Usage,
}

/// 使用统计
#[derive(Debug, Deserialize)]
pub struct Usage {
    pub input_tokens: usize,
    pub output_tokens: usize,
}

impl ApiClient {
    /// 创建新的 API 客户端
    pub fn new(api_key: String) -> Self {
        Self {
            client: Client::new(),
            api_key,
            base_url: "https://api.anthropic.com/v1".to_string(),
        }
    }

    /// 发送消息
    pub async fn send_message(
        &self,
        context: RequestContext,
    ) -> Result<ApiResponse, ApiError> {
        let request = ApiRequest {
            model: context.model.to_string(),
            messages: context.messages,
            max_tokens: Some(context.max_tokens),
            temperature: Some(context.temperature),
            tools: Some(context.tools),
            stream: false,
        };

        let response = self.client
            .post(format!("{}/messages", self.base_url))
            .header("x-api-key", &self.api_key)
            .header("anthropic-version", "2023-06-01")
            .json(&request)
            .send()
            .await?;

        if !response.status().is_success() {
            let error = response.text().await?;
            return Err(ApiError::Request(error));
        }

        let api_response = response.json().await?;
        Ok(api_response)
    }

    /// 流式发送消息
    pub async fn send_message_stream(
        &self,
        context: RequestContext,
    ) -> Result<impl futures::Stream<Item = Result<StreamEvent, ApiError>>, ApiError> {
        let request = ApiRequest {
            model: context.model.to_string(),
            messages: context.messages,
            max_tokens: Some(context.max_tokens),
            temperature: Some(context.temperature),
            tools: Some(context.tools),
            stream: true,
        };

        let response = self.client
            .post(format!("{}/messages", self.base_url))
            .header("x-api-key", &self.api_key)
            .header("anthropic-version", "2023-06-01")
            .json(&request)
            .send()
            .await?;

        if !response.status().is_success() {
            let error = response.text().await?;
            return Err(ApiError::Request(error));
        }

        // 转换为事件流
        let stream = response.bytes_stream()
            .map(|result| {
                match result {
                    Ok(bytes) => {
                        // 解析 SSE 事件
                        let text = String::from_utf8_lossy(&bytes);
                        self.parse_stream_event(&text)
                    }
                    Err(e) => Err(ApiError::Stream(e.to_string())),
                }
            });

        Ok(stream)
    }

    /// 解析流事件
    fn parse_stream_event(&self, text: &str) -> Result<StreamEvent, ApiError> {
        // 解析 Server-Sent Events 格式
        for line in text.lines() {
            if line.starts_with("data: ") {
                let json = &line[6..];
                let event: StreamEvent = serde_json::from_str(json)?;
                return Ok(event);
            }
        }

        Err(ApiError::Stream("No valid event found".into()))
    }
}
```

---

### 第27章 源码分析：扩展与记忆系统

#### 27.1 Skills 系统内部机制

Skills 系统采用**三层渐进加载**策略，与 Web 开发的懒加载理念一致：

```
┌─────────────────────────────────────────────────┐
│         Layer 1: 索引层（始终加载）              │
│  find-skills 扫描所有 SKILL.md 的 frontmatter    │
│  只加载 name + description（~50 tokens/skill）   │
├─────────────────────────────────────────────────┤
│         Layer 2: 路由层（LLM 判断）              │
│  当用户请求到达时，LLM 根据索引匹配最相关的 Skill │
│  基于 description 做语义匹配                     │
├─────────────────────────────────────────────────┤
│         Layer 3: 内容层（按需加载）              │
│  只有被选中的 Skill 才会完整加载 SKILL.md 内容   │
│  + scripts/ + references/ + assets/             │
└─────────────────────────────────────────────────┘
```

**SKILL.md 结构规范：**

```markdown
---
name: my-skill
description: 一行描述，用于 LLM 路由匹配
---

## Overview
技能概述，控制在 500 行以内

## Workflow
工作流程定义

## References
指向 references/ 目录中的详细文档（按需读取）
```

**双重上下文注入：** Skill 内容通过两种方式注入到模型上下文中：
1. **System Prompt 注入**：核心指令作为系统提示词的一部分
2. **User Message 附件**：详细参考资料作为用户消息的附件

#### 27.2 记忆系统架构

记忆系统完全基于本地文件系统，没有使用向量数据库。

**短期记忆（Session 级别）**

- 对话历史存储在内存中
- 超出上下文窗口时触发 `/compact` 压缩
- 压缩后的摘要作为新对话的初始上下文

**长期记忆（CLAUDE.md 体系）**

四层层级结构，按优先级从高到低：

```
优先级 1: ./.claude/CLAUDE.md    (项目 .claude 目录内)
优先级 2: ./CLAUDE.md            (项目根目录)
优先级 3: 父目录 CLAUDE.md       (向上查找)
优先级 4: ~/.claude/CLAUDE.md    (全局配置)

合并规则: 所有层级的 CLAUDE.md 内容合并，
         子目录规则覆盖父目录规则
```

**Auto Memory（自动记忆）**

```
src/services/autoDream/   ← 内部代号 KAIROS
├── consolidation prompt  ← 记忆整合提示词
├── consolidation lock    ← 防并发锁
└── 设计:
    - 系统空闲时进行记忆巩固（"梦境"进程）
    - 维护只可追加的每日日志
    - 可监听外部事件流（GitHub Webhooks、Slack 消息）
    - 在用户离开时响应

src/services/teamMemorySync/
├── secretScanner.ts      ← 同步时扫描敏感信息
└── 设计:
    - 多人协作场景的 Agent 记忆同步
    - 团队级别的知识共享
```

**记忆文件结构：**

```markdown
---
name: memory-name
description: 一行描述，用于相关性判断
type: user | feedback | project | reference
---

记忆内容...
```

记忆索引文件 `MEMORY.md` 始终加载到对话上下文中，每条记忆一行指针，总计不超过 200 行。

#### 27.3 Hooks 系统内部实现

Hooks 系统通过事件总线模式实现，支持以下生命周期钩子：

```typescript
// Hooks 生命周期
interface HookDefinition {
  type: 'PreToolUse' | 'PostToolUse' | 'Stop'
       | 'SubagentStart' | 'SubagentStop';
  matcher: string;     // 工具名称匹配模式，如 "Bash" 或 "*"
  command: string;     // 要执行的 shell 命令
  timeout?: number;    // 超时时间（毫秒）
}

// 配置在 .claude/settings.json 中
{
  "hooks": {
    "PreToolUse": [{
      "matcher": "Bash",
      "command": "echo 'About to run bash command: $TOOL_INPUT'"
    }],
    "PostToolUse": [{
      "matcher": "Write",
      "command": "prettier --write $FILE_PATH"  // 自动格式化
    }],
    "Stop": [{
      "matcher": "*",
      "command": "npm test"  // 会话结束前运行测试
    }]
  }
}
```

**Hook 执行流程：**

```
工具调用请求
    │
    ▼
PreToolUse Hooks ──执行匹配的钩子──┐
    │                               │
    │  ← 钩子可修改参数或阻止执行    │
    ▼                               │
工具执行                            │
    │                               │
    ▼                               │
PostToolUse Hooks ──执行匹配的钩子  │
    │                               │
    │  ← 钩子可自动格式化、检查等     │
    ▼                               │
返回结果                            │
```

#### 27.4 MCP 协议集成

MCP（Model Context Protocol）在 Claude Code 中是解耦的重要手段：把高变动频率的工具集剥离为主进程之外的 MCP Server，核心 Harness 代码保持稳定。

```typescript
// MCP 客户端核心结构
class MCPClient {
  // 支持多种传输协议
  transport: 'stdio' | 'http' | 'sse' | 'ws';

  // 三种能力发现
  async discoverTools(): Promise<Tool[]>;
  async discoverResources(): Promise<Resource[]>;
  async discoverPrompts(): Promise<Prompt[]>;

  // JSON-RPC 2.0 通信
  async request(method: string, params: unknown): Promise<unknown>;
  async notification(method: string, params: unknown): void;
}
```

**MCP Server 配置：**

```json
// .claude/mcp.json
{
  "mcpServers": {
    "database": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-postgres"],
      "env": {
        "DATABASE_URL": "${DATABASE_URL}"
      }
    },
    "filesystem": {
      "command": "npx",
      "args": ["@modelcontextprotocol/server-filesystem", "/path/to/data"]
    }
  }
}
```

#### 27.5 遥测与 A/B 测试

源码中的遥测系统追踪的内容包括：
- **挫败感信号**：从用户行为模式推断（如反复取消、频繁重试）
- **"继续"按钮点击频率**：衡量中断体验
- **工具使用分布**：优化工具排序和加载策略
- **GrowthBook A/B 测试**：功能开关与实验分组
- **Feature Flag 系统**：108 个未公开的封闭模块
    skill_registry: SkillRegistry,

    /// 钩子总线
    hook_bus: HookBus,

    /// MCP 客户端
    mcp_clients: Vec<McpClient>,
}

impl ExtensionSystem {
    /// 创建新的扩展系统
    pub fn new(config: &ExtensionConfig) -> Result<Self, Error> {
        Ok(Self {
            plugin_manager: PluginManager::new(&config.plugin_paths)?,
            skill_registry: SkillRegistry::new(&config.skill_paths)?,
            hook_bus: HookBus::new(),
            mcp_clients: Vec::new(),
        })
    }

    /// 初始化扩展系统
    pub async fn initialize(&mut self) -> Result<(), Error> {
        // 加载插件
        self.plugin_manager.load_all().await?;

        // 加载技能
        self.skill_registry.load_all()?;

        // 连接 MCP 服务器
        for server_config in &self.config.mcp_servers {
            let client = McpClient::connect(server_config).await?;
            self.mcp_clients.push(client);
        }

        Ok(())
    }
}
```

#### 27.2 Hook 系统实现

```rust
// src/extension/hook.rs

use std::collections::HashMap;
use async_trait::async_trait;

/// 钩子类型
#[derive(Debug, Clone, Hash, Eq, PartialEq)]
pub enum HookType {
    PreToolUse,
    PostToolUse,
    Stop,
}

/// 钩子特征
#[async_trait]
pub trait Hook: Send + Sync {
    /// 钩子名称
    fn name(&self) -> &str;

    /// 匹配器
    fn matcher(&self) -> &str;

    /// 执行钩子
    async fn execute(&self, context: HookContext) -> Result<HookResult, HookError>;
}

/// 钩子上下文
#[derive(Debug, Clone)]
pub struct HookContext {
    pub tool_name: String,
    pub params: serde_json::Value,
    pub result: Option<String>,
    pub session: SessionId,
}

/// 钩子结果
#[derive(Debug, Clone)]
pub enum HookResult {
    /// 继续
    Continue,

    /// 修改参数
    ModifyParams(serde_json::Value),

    /// 阻止执行
    Block(String),

    /// 修改结果
    ModifyResult(String),
}

/// 钩子总线
pub struct HookBus {
    /// 注册的钩子
    hooks: HashMap<HookType, Vec<Box<dyn Hook>>>,

    /// 执行器
    executor: tokio::runtime::Handle,
}

impl HookBus {
    /// 创建新的钩子总线
    pub fn new() -> Self {
        Self {
            hooks: HashMap::new(),
            executor: tokio::runtime::Handle::current(),
        }
    }

    /// 注册钩子
    pub fn register(&mut self, hook_type: HookType, hook: Box<dyn Hook>) {
        self.hooks
            .entry(hook_type)
            .or_insert_with(Vec::new)
            .push(hook);
    }

    /// 触发钩子
    pub async fn trigger(
        &self,
        hook_type: HookType,
        context: HookContext,
    ) -> Result<HookResult, HookError> {
        let hooks = self.hooks.get(&hook_type);

        if let Some(hooks) = hooks {
            for hook in hooks {
                // 检查匹配器
                if self.matches(&context.tool_name, hook.matcher()) {
                    let result = hook.execute(context.clone()).await?;

                    // 如果返回 Block 或 Modify，立即返回
                    match &result {
                        HookResult::Block(_) |
                        HookResult::ModifyParams(_) |
                        HookResult::ModifyResult(_) => {
                            return Ok(result);
                        }
                        HookResult::Continue => {
                            // 继续执行下一个钩子
                        }
                    }
                }
            }
        }

        Ok(HookResult::Continue)
    }

    /// 匹配工具名称
    fn matches(&self, tool_name: &str, matcher: &str) -> bool {
        if matcher == "*" {
            return true;
        }

        // 支持正则表达式
        if let Ok(re) = regex::Regex::new(matcher) {
            return re.is_match(tool_name);
        }

        // 支持通配符
        if matcher.contains('|') {
            let patterns: Vec<&str> = matcher.split('|').collect();
            return patterns.iter().any(|p| tool_name == *p);
        }

        tool_name == matcher
    }
}

/// 命令钩子实现
pub struct CommandHook {
    name: String,
    matcher: String,
    command: String,
}

impl CommandHook {
    pub fn new(name: String, matcher: String, command: String) -> Self {
        Self { name, matcher, command }
    }
}

#[async_trait]
impl Hook for CommandHook {
    fn name(&self) -> &str {
        &self.name
    }

    fn matcher(&self) -> &str {
        &self.matcher
    }

    async fn execute(&self, context: HookContext) -> Result<HookResult, HookError> {
        // 替换环境变量
        let command = self.replace_variables(&self.command, &context);

        // 执行命令
        let output = tokio::process::Command::new("sh")
            .arg("-c")
            .arg(&command)
            .output()
            .await?;

        if output.status.success() {
            Ok(HookResult::Continue)
        } else {
            let error = String::from_utf8_lossy(&output.stderr);
            Err(HookError::Execution(error.to_string()))
        }
    }
}

impl CommandHook {
    /// 替换变量
    fn replace_variables(&self, template: &str, context: &HookContext) -> String {
        let mut result = template.to_string();

        result = result.replace("$CLAUDE_TOOL_NAME", &context.tool_name);
        result = result.replace(
            "$CLAUDE_PARAMS",
            &serde_json::to_string(&context.params).unwrap_or_default()
        );

        if let Some(ref result_str) = context.result {
            result = result.replace("$CLAUDE_RESULT", result_str);
        }

        result
    }
}
```

#### 27.3 技能系统实现

```rust
// src/extension/skill.rs

use std::collections::HashMap;
use std::path::PathBuf;

/// 技能定义
#[derive(Debug, Clone, Deserialize)]
pub struct Skill {
    /// 技能名称
    pub name: String,

    /// 描述
    pub description: String,

    /// 触发条件
    #[serde(default)]
    pub trigger: Option<String>,

    /// 内容
    pub content: String,

    /// 参数定义
    #[serde(default)]
    pub parameters: HashMap<String, ParameterDef>,
}

/// 参数定义
#[derive(Debug, Clone, Deserialize)]
pub struct ParameterDef {
    pub description: String,
    #[serde(default)]
    pub required: bool,
    #[serde(default)]
    pub default: Option<String>,
}

/// 技能注册表
pub struct SkillRegistry {
    /// 已注册的技能
    skills: HashMap<String, Skill>,

    /// 技能路径
    skill_paths: Vec<PathBuf>,
}

impl SkillRegistry {
    /// 创建新的技能注册表
    pub fn new(skill_paths: &[PathBuf]) -> Self {
        Self {
            skills: HashMap::new(),
            skill_paths: skill_paths.to_vec(),
        }
    }

    /// 加载所有技能
    pub fn load_all(&mut self) -> Result<(), Error> {
        for path in &self.skill_paths {
            self.load_from_path(path)?;
        }
        Ok(())
    }

    /// 从路径加载技能
    fn load_from_path(&mut self, path: &PathBuf) -> Result<(), Error> {
        if path.is_dir() {
            for entry in std::fs::read_dir(path)? {
                let entry = entry?;
                let file_path = entry.path();
                if file_path.extension().map(|e| e == "md").unwrap_or(false) {
                    self.load_skill_file(&file_path)?;
                }
            }
        } else if path.extension().map(|e| e == "md").unwrap_or(false) {
            self.load_skill_file(path)?;
        }
        Ok(())
    }

    /// 加载技能文件
    fn load_skill_file(&mut self, path: &PathBuf) -> Result<(), Error> {
        let content = std::fs::read_to_string(path)?;

        // 解析 frontmatter
        let skill = self.parse_skill(&content)?;

        self.skills.insert(skill.name.clone(), skill);

        Ok(())
    }

    /// 解析技能
    fn parse_skill(&self, content: &str) -> Result<Skill, Error> {
        // 解析 YAML frontmatter
        let parts: Vec<&str> = content.splitn(3, "---").collect();

        if parts.len() < 3 {
            return Err(Error::Parse("Invalid skill format".into()));
        }

        let frontmatter: SkillFrontmatter = serde_yaml::from_str(parts[1])?;
        let body = parts[2].trim();

        Ok(Skill {
            name: frontmatter.name,
            description: frontmatter.description,
            trigger: frontmatter.trigger,
            content: body.to_string(),
            parameters: frontmatter.parameters.unwrap_or_default(),
        })
    }

    /// 获取技能
    pub fn get(&self, name: &str) -> Option<&Skill> {
        self.skills.get(name)
    }

    /// 应用技能
    pub fn apply(&self, name: &str, args: &[String]) -> Result<String, Error> {
        let skill = self.skills.get(name)
            .ok_or_else(|| Error::NotFound(name.to_string()))?;

        // 替换参数
        let mut content = skill.content.clone();

        for (key, value) in self.parse_args(args)? {
            content = content.replace(&format!("{{{}}}", key), &value);
        }

        Ok(content)
    }

    /// 解析参数
    fn parse_args(&self, args: &[String]) -> Result<Vec<(String, String)>, Error> {
        let mut result = Vec::new();

        for arg in args {
            if arg.contains('=') {
                let parts: Vec<&str> = arg.splitn(2, '=').collect();
                if parts.len() == 2 {
                    result.push((parts[0].to_string(), parts[1].to_string()));
                }
            }
        }

        Ok(result)
    }
}
```

#### 27.4 MCP 协议实现

```rust
// src/extension/mcp.rs

use tokio::io::{AsyncRead, AsyncWrite};
use tokio::process::{Child, Command};
use serde::{Deserialize, Serialize};

/// MCP 客户端
pub struct McpClient {
    /// 服务器进程
    process: Option<Child>,

    /// 传输层
    transport: McpTransport,

    /// 服务器信息
    server_info: ServerInfo,
}

/// MCP 传输层
pub struct McpTransport {
    stdin: Box<dyn AsyncWrite + Unpin + Send>,
    stdout: Box<dyn AsyncRead + Unpin + Send>,
}

/// MCP 请求
#[derive(Debug, Serialize)]
pub struct McpRequest {
    pub jsonrpc: String,
    pub id: u64,
    pub method: String,
    #[serde(skip_serializing_if = "Option::is_none")]
    pub params: Option<serde_json::Value>,
}

/// MCP 响应
#[derive(Debug, Deserialize)]
pub struct McpResponse {
    pub jsonrpc: String,
    pub id: u64,
    #[serde(skip_serializing_if = "Option::is_none")]
    pub result: Option<serde_json::Value>,
    #[serde(skip_serializing_if = "Option::is_none")]
    pub error: Option<McpError>,
}

/// MCP 错误
#[derive(Debug, Deserialize)]
pub struct McpError {
    pub code: i32,
    pub message: String,
}

/// 服务器信息
#[derive(Debug, Clone)]
pub struct ServerInfo {
    pub name: String,
    pub version: String,
    pub capabilities: Capabilities,
}

/// 能力
#[derive(Debug, Clone, Deserialize)]
pub struct Capabilities {
    #[serde(default)]
    pub resources: bool,
    #[serde(default)]
    pub tools: bool,
    #[serde(default)]
    pub prompts: bool,
}

impl McpClient {
    /// 连接到 MCP 服务器
    pub async fn connect(config: &McpServerConfig) -> Result<Self, McpError> {
        // 启动服务器进程
        let mut process = Command::new(&config.command)
            .args(&config.args)
            .envs(&config.env)
            .stdin(std::process::Stdio::piped())
            .stdout(std::process::Stdio::piped())
            .spawn()?;

        // 创建传输层
        let stdin = process.stdin.take().unwrap();
        let stdout = process.stdout.take().unwrap();

        let transport = McpTransport {
            stdin: Box::new(stdin),
            stdout: Box::new(stdout),
        };

        let mut client = Self {
            process: Some(process),
            transport,
            server_info: ServerInfo {
                name: String::new(),
                version: String::new(),
                capabilities: Capabilities {
                    resources: false,
                    tools: false,
                    prompts: false,
                },
            },
        };

        // 初始化连接
        client.initialize().await?;

        Ok(client)
    }

    /// 初始化连接
    async fn initialize(&mut self) -> Result<(), McpError> {
        let response = self.send_request("initialize", Some(serde_json::json!({
            "protocolVersion": "2024-11-05",
            "clientInfo": {
                "name": "claude-code",
                "version": "1.0.0"
            }
        }))).await?;

        if let Some(result) = response.result {
            let server_info: ServerInfo = serde_json::from_value(result)?;
            self.server_info = server_info;
        }

        Ok(())
    }

    /// 发送请求
    async fn send_request(
        &mut self,
        method: &str,
        params: Option<serde_json::Value>,
    ) -> Result<McpResponse, McpError> {
        let request = McpRequest {
            jsonrpc: "2.0".to_string(),
            id: rand::random(),
            method: method.to_string(),
            params,
        };

        // 发送请求
        let request_str = serde_json::to_string(&request)?;
        self.transport.stdin.write_all(request_str.as_bytes()).await?;
        self.transport.stdin.write_all(b"\n").await?;
        self.transport.stdin.flush().await?;

        // 读取响应
        let mut response_str = String::new();
        self.transport.stdout.read_line(&mut response_str).await?;

        let response: McpResponse = serde_json::from_str(&response_str)?;

        if let Some(error) = response.error {
            return Err(error);
        }

        Ok(response)
    }

    /// 获取工具列表
    pub async fn list_tools(&mut self) -> Result<Vec<ToolDefinition>, McpError> {
        let response = self.send_request("tools/list", None).await?;

        if let Some(result) = response.result {
            let tools: ToolsList = serde_json::from_value(result)?;
            return Ok(tools.tools);
        }

        Ok(Vec::new())
    }

    /// 调用工具
    pub async fn call_tool(
        &mut self,
        name: &str,
        arguments: serde_json::Value,
    ) -> Result<String, McpError> {
        let response = self.send_request("tools/call", Some(serde_json::json!({
            "name": name,
            "arguments": arguments
        }))).await?;

        if let Some(result) = response.result {
            let tool_result: ToolResult = serde_json::from_value(result)?;
            return Ok(tool_result.content);
        }

        Err(McpError {
            code: -1,
            message: "No result".to_string(),
        })
    }
}

/// 工具列表
#[derive(Debug, Deserialize)]
struct ToolsList {
    tools: Vec<ToolDefinition>,
}

/// 工具结果
#[derive(Debug, Deserialize)]
struct ToolResult {
    content: String,
}
```

---

## 第七部分：生态融合

### 第28章 CLI-Anything 集成

#### 28.1 CLI-Anything 概述

CLI-Anything 是一个通用的 CLI 扩展框架，它提供了一种标准化的方式来构建和扩展命令行工具。

**核心特性**
- 模块化命令系统
- 插件架构
- 配置管理
- 输出格式化

#### 28.2 集成方案设计

```
┌────────────────────────────────────────────────────┐
│              CLI-Anything 集成架构                   │
├────────────────────────────────────────────────────┤
│                                                     │
│  ┌─────────────────────────────────────────────┐   │
│  │              CLI-Anything Core               │   │
│  │  ┌─────────┐  ┌─────────┐  ┌─────────┐     │   │
│  │  │ Command │  │ Plugin  │  │ Output  │     │   │
│  │  │ Router  │  │ Manager │  │ Format  │     │   │
│  │  └─────────┘  └─────────┘  └─────────┘     │   │
│  └─────────────────────────────────────────────┘   │
│                        │                            │
│                        ▼                            │
│  ┌─────────────────────────────────────────────┐   │
│  │            Claude Code Adapter               │   │
│  │  ┌─────────────┐  ┌─────────────┐          │   │
│  │  │ Command     │  │ Output      │          │   │
│  │  │ Mapper      │  │ Transformer │          │   │
│  │  └─────────────┘  └─────────────┘          │   │
│  └─────────────────────────────────────────────┘   │
│                        │                            │
│                        ▼                            │
│  ┌─────────────────────────────────────────────┐   │
│  │            Claude Code Core                  │   │
│  └─────────────────────────────────────────────┘   │
│                                                     │
└────────────────────────────────────────────────────┘
```

#### 28.3 实现步骤

**1. 创建适配器**

```rust
// src/integration/cli_anything/adapter.rs

use cli_anything::{Command, Plugin, OutputFormat};
use crate::core::Session;

/// Claude Code 适配器
pub struct ClaudeCodeAdapter {
    session: Session,
}

impl ClaudeCodeAdapter {
    /// 创建新适配器
    pub fn new(session: Session) -> Self {
        Self { session }
    }

    /// 将 CLI-Anything 命令映射到 Claude Code
    pub fn map_command(&self, cmd: Command) -> ClaudeCommand {
        match cmd.name.as_str() {
            "ask" => ClaudeCommand::Query(cmd.args.join(" ")),
            "edit" => ClaudeCommand::Edit {
                file: cmd.args[0].clone(),
                changes: cmd.args[1..].to_vec(),
            },
            "run" => ClaudeCommand::Execute(cmd.args.join(" ")),
            _ => ClaudeCommand::Unknown(cmd),
        }
    }

    /// 转换输出格式
    pub fn transform_output(
        &self,
        output: ClaudeOutput,
        format: OutputFormat,
    ) -> String {
        match format {
            OutputFormat::Json => {
                serde_json::to_string(&output).unwrap_or_default()
            }
            OutputFormat::Markdown => {
                self.to_markdown(&output)
            }
            OutputFormat::Plain => {
                output.to_string()
            }
        }
    }

    fn to_markdown(&self, output: &ClaudeOutput) -> String {
        let mut md = String::new();

        md.push_str(&format!("## {}\n\n", output.title));
        md.push_str(&output.content);

        if !output.files.is_empty() {
            md.push_str("\n\n### 相关文件\n\n");
            for file in &output.files {
                md.push_str(&format!("- {}\n", file));
            }
        }

        md
    }
}
```

**2. 注册为插件**

```rust
// src/integration/cli_anything/plugin.rs

use cli_anything::{Plugin, PluginMeta, CommandResult};

pub struct ClaudeCodePlugin {
    adapter: ClaudeCodeAdapter,
}

impl Plugin for ClaudeCodePlugin {
    fn meta(&self) -> PluginMeta {
        PluginMeta {
            name: "claude-code".to_string(),
            version: "1.0.0".to_string(),
            description: "Claude Code integration".to_string(),
        }
    }

    fn commands(&self) -> Vec<String> {
        vec![
            "ask".to_string(),
            "edit".to_string(),
            "run".to_string(),
            "explain".to_string(),
        ]
    }

    async fn execute(&self, cmd: Command) -> CommandResult {
        let claude_cmd = self.adapter.map_command(cmd.clone());

        match claude_cmd {
            ClaudeCommand::Query(prompt) => {
                let response = self.adapter.session
                    .send_message(prompt)
                    .await?;

                CommandResult::Success(
                    self.adapter.transform_output(
                        response,
                        cmd.output_format
                    )
                )
            }
            _ => CommandResult::Unsupported,
        }
    }
}
```

#### 28.4 使用场景

**增强型 CLI 工具**

```bash
# 使用 CLI-Anything 调用 Claude Code
cli-anything ask "解释这个函数的作用" --file src/utils.ts

# 多命令组合
cli-anything chain "analyze src/ | suggest improvements | apply"
```

---

### 第29章 OpenCLI 集成

#### 29.1 OpenCLI 概述

OpenCLI 是一个开放的 CLI 架构标准，旨在提供跨平台、可互操作的命令行工具生态系统。

**核心特性**
- 标准化命令定义
- 跨平台兼容
- 插件发现机制
- 统一的配置管理

#### 29.2 集成策略

```
┌────────────────────────────────────────────────────┐
│               OpenCLI 集成架构                       │
├────────────────────────────────────────────────────┤
│                                                     │
│  ┌─────────────────────────────────────────────┐   │
│  │              OpenCLI Registry                │   │
│  │  ┌─────────┐  ┌─────────┐  ┌─────────┐     │   │
│  │  │ Command │  │ Plugin  │  │ Config  │     │   │
│  │  │ Specs   │  │ Index   │  │ Schema  │     │   │
│  │  └─────────┘  └─────────┘  └─────────┘     │   │
│  └─────────────────────────────────────────────┘   │
│                        │                            │
│                        ▼                            │
│  ┌─────────────────────────────────────────────┐   │
│  │            OpenCLI Bridge                    │   │
│  │  ┌─────────────┐  ┌─────────────┐          │   │
│  │  │ Spec        │  │ Protocol    │          │   │
│  │  │ Translator  │  │ Adapter     │          │   │
│  │  └─────────────┘  └─────────────┘          │   │
│  └─────────────────────────────────────────────┘   │
│                        │                            │
│                        ▼                            │
│  ┌─────────────────────────────────────────────┐   │
│  │            Claude Code Core                  │   │
│  └─────────────────────────────────────────────┘   │
│                                                     │
└────────────────────────────────────────────────────┘
```

#### 29.3 实现细节

**命令规范转换**

```rust
// src/integration/opencli/bridge.rs

use opencli::{CommandSpec, ParameterSpec};
use crate::tools::ToolDefinition;

/// OpenCLI 桥接器
pub struct OpenCliBridge;

impl OpenCliBridge {
    /// 将 Claude Code 工具转换为 OpenCLI 命令规范
    pub fn tool_to_spec(tool: &ToolDefinition) -> CommandSpec {
        CommandSpec {
            name: tool.name.clone(),
            description: tool.description.clone(),
            version: "1.0.0".to_string(),
            parameters: Self::convert_parameters(&tool.parameters),
            returns: opencli::ReturnSpec {
                type_: "string".to_string(),
                description: "Tool execution result".to_string(),
            },
        }
    }

    /// 转换参数定义
    fn convert_parameters(schema: &serde_json::Value) -> Vec<ParameterSpec> {
        let mut params = Vec::new();

        if let Some(properties) = schema.get("properties") {
            for (name, def) in properties.as_object().unwrap_or(&serde_json::Map::new()) {
                params.push(ParameterSpec {
                    name: name.clone(),
                    type_: def.get("type")
                        .and_then(|t| t.as_str())
                        .unwrap_or("string")
                        .to_string(),
                    description: def.get("description")
                        .and_then(|d| d.as_str())
                        .unwrap_or("")
                        .to_string(),
                    required: schema.get("required")
                        .and_then(|r| r.as_array())
                        .map(|arr| arr.iter().any(|v| v == name))
                        .unwrap_or(false),
                    default: def.get("default").cloned(),
                });
            }
        }

        params
    }

    /// 将 OpenCLI 命令转换为 Claude Code 调用
    pub fn spec_to_call(spec: &CommandSpec, args: serde_json::Value) -> ToolCall {
        ToolCall {
            name: spec.name.clone(),
            input: args,
        }
    }
}
```

**协议适配**

```rust
// src/integration/opencli/protocol.rs

use opencli::{Request, Response, StatusCode};

/// OpenCLI 协议适配器
pub struct ProtocolAdapter {
    session: Session,
}

impl ProtocolAdapter {
    /// 处理 OpenCLI 请求
    pub async fn handle_request(&mut self, request: Request) -> Response {
        match request.method.as_str() {
            "execute" => {
                self.execute_tool(request).await
            }
            "list" => {
                self.list_tools(request)
            }
            "describe" => {
                self.describe_tool(request)
            }
            _ => {
                Response {
                    status: StatusCode::MethodNotAllowed,
                    body: serde_json::json!({
                        "error": "Unknown method"
                    }),
                }
            }
        }
    }

    /// 执行工具
    async fn execute_tool(&mut self, request: Request) -> Response {
        let tool_name = request.params.get("tool")
            .and_then(|t| t.as_str())
            .unwrap_or("");

        let args = request.params.get("args")
            .cloned()
            .unwrap_or(serde_json::json!({}));

        match self.session.execute_tool(tool_name, args).await {
            Ok(result) => Response {
                status: StatusCode::Ok,
                body: serde_json::json!({
                    "result": result
                }),
            },
            Err(e) => Response {
                status: StatusCode::InternalServerError,
                body: serde_json::json!({
                    "error": e.to_string()
                }),
            },
        }
    }

    /// 列出工具
    fn list_tools(&self, _request: Request) -> Response {
        let tools = self.session.get_tool_definitions();

        Response {
            status: StatusCode::Ok,
            body: serde_json::json!({
                "tools": tools.iter()
                    .map(|t| OpenCliBridge::tool_to_spec(t))
                    .collect::<Vec<_>>()
            }),
        }
    }

    /// 描述工具
    fn describe_tool(&self, request: Request) -> Response {
        let tool_name = request.params.get("name")
            .and_then(|n| n.as_str())
            .unwrap_or("");

        match self.session.get_tool_definition(tool_name) {
            Some(tool) => Response {
                status: StatusCode::Ok,
                body: serde_json::to_value(OpenCliBridge::tool_to_spec(&tool))
                    .unwrap_or_default(),
            },
            None => Response {
                status: StatusCode::NotFound,
                body: serde_json::json!({
                    "error": "Tool not found"
                }),
            },
        }
    }
}
```

#### 29.4 扩展案例

**自定义命令实现**

```rust
// src/integration/opencli/custom_commands.rs

use opencli::{Command, CommandResult};

/// 注册自定义命令
pub fn register_custom_commands(registry: &mut CommandRegistry) {
    registry.register("claude:review", ReviewCommand);
    registry.register("claude:test", TestCommand);
    registry.register("claude:refactor", RefactorCommand);
}

/// 代码审查命令
struct ReviewCommand;

#[async_trait]
impl Command for ReviewCommand {
    async fn execute(&self, args: &[String]) -> CommandResult {
        let files = args.to_vec();

        // 调用 Claude Code 进行代码审查
        let prompt = format!(
            "请审查以下文件的代码质量：\n{}",
            files.join("\n")
        );

        let session = Session::current();
        let result = session.send_message(prompt).await?;

        CommandResult::Success(result)
    }
}

/// 测试生成命令
struct TestCommand;

#[async_trait]
impl Command for TestCommand {
    async fn execute(&self, args: &[String]) -> CommandResult {
        let file = args.get(0).cloned().unwrap_or_default();

        let prompt = format!(
            "为 {} 生成测试用例，确保覆盖率达到 80%% 以上",
            file
        );

        let session = Session::current();
        let result = session.send_message(prompt).await?;

        CommandResult::Success(result)
    }
}
```

---

## 第八部分：对比分析

### 第30章 与 Codex 智能体对比

#### 30.1 概述

Claude Code 和 OpenAI Codex Agent 都是当前领先的 AI 编程助手，但它们在设计理念、架构实现和应用场景上存在显著差异。本章将深入分析两者的异同，帮助开发者选择最适合自己需求的工具。

#### 30.2 核心架构对比

**Claude Code 架构**

```
┌─────────────────────────────────────────────────────────────┐
│                    Claude Code 架构                          │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │                 CLI 原生架构                          │    │
│  │  - 终端直接运行                                      │    │
│  │  - Shell 环境深度集成                                │    │
│  │  - 管道操作支持                                      │    │
│  └─────────────────────────────────────────────────────┘    │
│                          │                                   │
│                          ▼                                   │
│  ┌─────────────────────────────────────────────────────┐    │
│  │              多模型支持                               │    │
│  │  - Haiku 4.5 (快速经济)                              │    │
│  │  - Sonnet 4.6 (平衡性能)                             │    │
│  │  - Opus 4.6 (深度推理)                               │    │
│  └─────────────────────────────────────────────────────┘    │
│                          │                                   │
│                          ▼                                   │
│  ┌─────────────────────────────────────────────────────┐    │
│  │              扩展系统                                 │    │
│  │  - Skills (技能)                                     │    │
│  │  - Plugins (插件)                                    │    │
│  │  - Hooks (钩子)                                      │    │
│  │  - MCP (协议)                                        │    │
│  │  - Subagents (子代理)                                │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

**Codex Agent 架构**

```
┌─────────────────────────────────────────────────────────────┐
│                    Codex Agent 架构                          │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │                 Agent Loop 架构                       │    │
│  │  - 迭代推理循环                                      │    │
│  │  - 自主任务执行                                      │    │
│  │  - 目标驱动设计                                      │    │
│  └─────────────────────────────────────────────────────┘    │
│                          │                                   │
│                          ▼                                   │
│  ┌─────────────────────────────────────────────────────┐    │
│  │              模型驱动                                 │    │
│  │  - GPT-4 系列                                        │    │
│  │  - o1 推理模型                                       │    │
│  │  - o3 深度推理                                       │    │
│  └─────────────────────────────────────────────────────┘    │
│                          │                                   │
│                          ▼                                   │
│  ┌─────────────────────────────────────────────────────┐    │
│  │              工具生态                                 │    │
│  │  - Code Interpreter                                  │    │
│  │  - File Operations                                   │    │
│  │  - Web Browsing                                      │    │
│  │  - Custom Tools                                      │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

#### 30.3 详细对比分析

| 维度 | Claude Code | Codex Agent |
|------|-------------|-------------|
| **运行环境** | 终端 CLI | API / ChatGPT |
| **交互方式** | 命令行对话 | 自然语言对话 |
| **上下文管理** | 项目级持久化 | 会话级临时性 |
| **工具调用** | 内置 + 自定义扩展 | 内置工具集 |
| **记忆系统** | 多层长期记忆 | 无持久记忆 |
| **子代理** | 30+ 专业子代理 | 无子代理概念 |
| **MCP 协议** | 原生支持 | 不支持 |
| **离线能力** | 部分支持 | 完全依赖网络 |

#### 30.4 Agent Loop 机制对比

**Claude Code 的交互模式**

```
用户输入 → 上下文构建 → 模型推理 → 工具调用 → 结果返回
    ↑                                              │
    └──────────────── 对话循环 ←───────────────────┘
```

Claude Code 采用**对话驱动**的交互模式：
- 用户主导对话流程
- 每次交互相对独立
- 通过记忆系统保持上下文连续性

**Codex Agent 的循环机制**

```
┌─────────────────────────────────────────────────────────────┐
│                    Codex Agent Loop                          │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│                    ┌─────────────┐                          │
│                    │   目标定义   │                          │
│                    └─────────────┘                          │
│                          │                                   │
│                          ▼                                   │
│    ┌───────────────────────────────────────────────┐       │
│    │                  Agent Loop                    │       │
│    │  ┌─────────┐  ┌─────────┐  ┌─────────┐       │       │
│    │  │ 思考    │→ │ 行动    │→ │ 观察    │       │       │
│    │  │ (Think) │  │ (Act)   │  │(Observe)│       │       │
│    │  └─────────┘  └─────────┘  └─────────┘       │       │
│    │       ↑                          │            │       │
│    │       └──────────────────────────┘            │       │
│    └───────────────────────────────────────────────┘       │
│                          │                                   │
│                          ▼                                   │
│                    ┌─────────────┐                          │
│                    │   目标达成   │                          │
│                    └─────────────┘                          │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

Codex Agent 采用**自主循环**的执行模式：
- 定义目标后自主执行
- 迭代推理和行动
- 持续观察和调整直到目标达成

#### 30.5 工具系统对比

**Claude Code 工具系统**

```rust
// Claude Code 工具定义示例
pub struct Tool {
    name: String,
    description: String,
    parameters: JsonSchema,
    permissions: PermissionLevel,
}

// 内置工具
- Read: 读取文件
- Write: 写入文件
- Edit: 编辑文件
- Bash: 执行命令
- Grep: 搜索内容
- Glob: 匹配文件
- Agent: 派发子代理

// 扩展机制
- Skills: 提示词模板
- Plugins: 功能扩展包
- MCP: 外部服务集成
```

**Codex Agent 工具系统**

```python
# Codex Agent 工具定义示例
class Tool:
    name: str
    description: str
    parameters: dict

# 内置工具
- code_interpreter: 代码执行
- file_operations: 文件操作
- web_browsing: 网页浏览
- image_generation: 图像生成

# 扩展机制
- Custom Tools: 自定义工具
- Function Calling: 函数调用
```

#### 30.6 记忆系统对比

**Claude Code 多层记忆**

```
┌─────────────────────────────────────────────────────────────┐
│                  Claude Code 记忆系统                         │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐         │
│  │  短期记忆    │  │  工作记忆    │  │  长期记忆    │         │
│  │  (Session)  │  │ (Context)   │  │  (Memory)   │         │
│  └─────────────┘  └─────────────┘  └─────────────┘         │
│        │                │               │                    │
│        ▼                ▼               ▼                    │
│  当前会话内容      CLAUDE.md        磁盘持久化              │
│  对话历史         项目配置         跨会话知识               │
│                                                              │
│  记忆类型:                                                   │
│  - user: 用户偏好                                           │
│  - feedback: 反馈记忆                                       │
│  - project: 项目知识                                        │
│  - reference: 外部引用                                      │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

**Codex Agent 记忆特点**

```
┌─────────────────────────────────────────────────────────────┐
│                  Codex Agent 记忆特点                         │
├─────────────────────────────────────────────────────────────┤
│                                                              │
│  ┌─────────────────────────────────────────────────────┐    │
│  │                 会话级记忆                            │    │
│  │  - 对话历史保留在会话中                              │    │
│  │  - 会话结束后记忆消失                                │    │
│  │  - 无持久化存储                                      │    │
│  └─────────────────────────────────────────────────────┘    │
│                                                              │
│  依赖模型上下文窗口:                                         │
│  - GPT-4: 128K tokens                                       │
│  - GPT-4 Turbo: 128K tokens                                 │
│  - o1: 200K tokens                                          │
│                                                              │
│  无跨会话知识迁移能力                                        │
│                                                              │
└─────────────────────────────────────────────────────────────┘
```

#### 30.7 子代理系统对比

**Claude Code 子代理优势**

| 特性 | Claude Code | Codex Agent |
|------|-------------|-------------|
| 子代理支持 | ✅ 30+ 专业子代理 | ❌ 无 |
| 并行执行 | ✅ 多代理并行 | ❌ 单线程 |
| 上下文隔离 | ✅ 独立上下文 | ❌ 共享上下文 |
| 专业分工 | ✅ 领域专用 | ❌ 通用处理 |
| 结果聚合 | ✅ 智能聚合 | ❌ 无 |

**子代理类型对比**

```
Claude Code 专业子代理:

代码审查类:
├── code-reviewer (通用审查)
├── python-reviewer (Python 专用)
├── rust-reviewer (Rust 专用)
├── go-reviewer (Go 专用)
├── java-reviewer (Java 专用)
└── security-reviewer (安全审查)

构建解决类:
├── build-error-resolver (通用构建)
├── rust-build-resolver (Rust 构建)
├── go-build-resolver (Go 构建)
└── cpp-build-resolver (C++ 构建)

规划类:
├── Plan (架构规划)
├── planner (实现规划)
└── architect (系统设计)

Codex Agent:
└── (无子代理概念)
```

#### 30.8 实际使用场景对比

**场景一：大型项目重构**

```
任务: 将单体应用拆分为微服务

Claude Code 方案:
┌─────────────────────────────────────────────────────────────┐
│ 1. 派发 Plan 子代理分析项目结构                             │
│    ↓                                                        │
│ 2. 并行派发多个审查子代理:                                   │
│    - code-reviewer: 代码质量                                │
│    - security-reviewer: 安全风险                            │
│    - architect: 架构建议                                    │
│    ↓                                                        │
│ 3. 主代理汇总结果，制定重构计划                             │
│    ↓                                                        │
│ 4. 记忆系统保存重构知识，跨会话复用                         │
└─────────────────────────────────────────────────────────────┘

Codex Agent 方案:
┌─────────────────────────────────────────────────────────────┐
│ 1. 用户描述重构目标                                         │
│    ↓                                                        │
│ 2. Agent Loop 自主分析项目                                  │
│    ↓                                                        │
│ 3. 生成重构建议                                             │
│    ↓                                                        │
│ 4. 会话结束，知识丢失                                       │
└─────────────────────────────────────────────────────────────┘
```

**场景二：持续开发工作流**

```
任务: 日常开发中的代码编写和审查

Claude Code 优势:
- ✅ 记忆系统记住项目规范和偏好
- ✅ CLAUDE.md 自动加载项目配置
- ✅ 子代理专业化处理不同任务
- ✅ Hooks 自动化代码格式化和测试

Codex Agent 优势:
- ✅ 自然语言交互更直观
- ✅ 自主执行能力强
- ❌ 每次会话需要重新说明项目背景
- ❌ 无自动化工作流支持
```

#### 30.9 技术实现对比

**Claude Code (Rust 实现)**

```rust
// Claude Code 核心架构 (Rust)
pub struct ClaudeCode {
    session: Session,
    context: ContextManager,
    tools: ToolDispatcher,
    memory: MemorySystem,
    subagents: SubagentDispatcher,
}

impl ClaudeCode {
    pub async fn process(&mut self, input: UserInput) -> Result<Response> {
        // 1. 加载记忆
        let memory = self.memory.load().await?;

        // 2. 构建上下文
        let context = self.context.build(&input, &memory)?;

        // 3. 判断是否需要子代理
        if self.needs_subagent(&input) {
            return self.dispatch_subagent(&input).await;
        }

        // 4. 调用模型
        let response = self.model.generate(context).await?;

        // 5. 处理工具调用
        let result = self.handle_tool_calls(response).await?;

        // 6. 更新记忆
        self.memory.update(&result).await?;

        Ok(result)
    }
}
```

**Codex Agent (Python 实现)**

```python
# Codex Agent 核心架构 (Python)
class CodexAgent:
    def __init__(self, model: str, tools: List[Tool]):
        self.model = model
        self.tools = tools
        self.history = []

    async def run(self, goal: str) -> Result:
        while not self.is_goal_achieved():
            # 1. 思考阶段
            thought = await self.think(goal)

            # 2. 行动阶段
            action = await self.decide_action(thought)

            # 3. 执行阶段
            observation = await self.execute(action)

            # 4. 记录历史
            self.history.append((thought, action, observation))

        return self.compile_result()
```

#### 30.10 优缺点总结

**Claude Code**

| 优点 | 缺点 |
|------|------|
| ✅ 终端原生，深度集成 | ❌ 需要 CLI 使用经验 |
| ✅ 多层记忆系统 | ❌ 配置相对复杂 |
| ✅ 专业子代理支持 | ❌ 学习曲线较陡 |
| ✅ MCP 协议扩展 | ❌ 依赖 Anthropic API |
| ✅ 开源生态丰富 | ❌ 模型选择受限 |
| ✅ 项目级上下文 | |

**Codex Agent**

| 优点 | 缺点 |
|------|------|
| ✅ 自然语言交互友好 | ❌ 无持久记忆 |
| ✅ 自主执行能力强 | ❌ 无子代理支持 |
| ✅ 多模型支持 | ❌ 无 MCP 协议 |
| ✅ 集成在 ChatGPT | ❌ 无项目级上下文 |
| ✅ 工具生态成熟 | ❌ 定制化能力弱 |
| ✅ 图像处理能力 | ❌ 离线能力弱 |

#### 30.11 选择建议

**推荐使用 Claude Code 的场景**

```
1. 专业开发工作流
   - 需要项目级上下文管理
   - 需要跨会话知识积累
   - 需要自动化工作流 (Hooks)

2. 复杂项目维护
   - 大型代码库理解
   - 多模块并行分析
   - 专业领域审查

3. 团队协作开发
   - 共享项目配置
   - 统一编码规范
   - 知识库建设

4. 自定义扩展需求
   - 开发专用工具
   - 集成外部服务 (MCP)
   - 创建专业子代理
```

**推荐使用 Codex Agent 的场景**

```
1. 快速原型开发
   - 探索性编程
   - 概念验证
   - 一次性任务

2. 非技术用户
   - 自然语言交互
   - 无需 CLI 经验
   - 图形界面友好

3. 多模态任务
   - 图像处理
   - 文档分析
   - 内容创作

4. 临时性问题解决
   - 快速问答
   - 代码片段生成
   - 算法解释
```

#### 30.12 未来发展趋势

**Claude Code 发展方向**

```
1. 更强大的子代理系统
   - 更多专业领域子代理
   - 子代理间协作能力
   - 自定义子代理框架

2. 增强的记忆系统
   - 语义化记忆检索
   - 知识图谱构建
   - 团队知识共享

3. 深度 IDE 集成
   - VS Code 扩展
   - JetBrains 插件
   - 实时协作

4. 本地化能力
   - 本地模型支持
   - 离线模式增强
   - 隐私保护
```

**Codex Agent 发展方向**

```
1. 增强的自主能力
   - 更长的自主执行
   - 复杂任务分解
   - 自我纠错机制

2. 多模态融合
   - 图像理解增强
   - 语音交互
   - 视频处理

3. 工具生态扩展
   - 更多内置工具
   - 第三方集成
   - 自定义工具框架

4. 企业级功能
   - 团队协作
   - 权限管理
   - 审计日志
```

#### 30.13 总结

Claude Code 和 Codex Agent 代表了 AI 编程助手的两种不同设计哲学：

| 维度 | Claude Code | Codex Agent |
|------|-------------|-------------|
| **设计理念** | 工具增强型 | 自主执行型 |
| **用户角色** | 引导者 | 监督者 |
| **适用场景** | 专业开发 | 快速原型 |
| **学习曲线** | 较陡 | 平缓 |
| **定制能力** | 强 | 弱 |
| **记忆能力** | 强 | 弱 |

两者并非竞争关系，而是互补关系。在实际开发中，可以根据具体任务选择合适的工具：

- **日常开发工作**：优先使用 Claude Code
- **快速探索任务**：使用 Codex Agent
- **复杂项目维护**：Claude Code + 专业子代理
- **多模态任务**：Codex Agent

---

## 附录

### A. 配置文件参考

#### A.1 全局配置

**位置：** `~/.claude/config.json`

```json
{
  "model": "claude-sonnet-4-6",
  "maxTokens": 4096,
  "temperature": 0.7,
  "autoApprove": false,
  "permissions": {
    "allow": ["Read", "Glob", "Grep"],
    "deny": []
  },
  "hooks": {
    "PostToolUse": []
  },
  "mcpServers": {}
}
```

#### A.2 项目配置

**位置：** `./.claude/config.json`

```json
{
  "projectName": "my-project",
  "model": "claude-sonnet-4-6",
  "permissions": {
    "allow": ["Read", "Write", "Edit", "Bash"],
    "deny": []
  }
}
```

#### A.3 CLAUDE.md 模板

```markdown
# 项目说明

## 技术栈
- 前端：Vue 3 + TypeScript
- 后端：Node.js + Express

## 编码规范
- 使用 2 空格缩进
- 函数必须有注释
- 测试覆盖率 >= 80%

## 禁止事项
- 不要使用 any 类型
- 不要跳过测试
```

---

### B. 命令速查表

| 命令 | 说明 | 示例 |
|------|------|------|
| `claude` | 启动交互会话 | `claude` |
| `claude -p` | 单次查询 | `claude -p "解释这段代码"` |
| `claude --continue` | 继续上次会话 | `claude --continue` |
| `claude --resume` | 恢复特定会话 | `claude --resume session-001` |
| `claude --model` | 指定模型 | `claude --model claude-opus-4-6` |
| `claude --file` | 指定文件上下文 | `claude --file src/main.ts` |
| `claude --git` | Git 操作 | `claude --git diff` |
| `claude --config` | 配置管理 | `claude --config show` |
| `claude --diagnostics` | 诊断工具 | `claude --diagnostics` |

---

### C. 常见问题解答

#### Q1: 如何提高 Claude Code 的响应速度？

**A:**
1. 使用 Haiku 模型处理简单任务
2. 减少上下文中的文件数量
3. 使用更具体的提示词
4. 配置自动批准常用操作

#### Q2: 如何处理敏感信息？

**A:**
1. 不要在 CLAUDE.md 中存储敏感信息
2. 使用环境变量
3. 配置 Hook 检查敏感信息
4. 定期审查记忆内容

#### Q3: 如何在团队中共享配置？

**A:**
1. 将 `.claude/` 目录加入版本控制
2. 使用符号链接共享全局配置
3. 创建团队专用的技能和插件

#### Q4: 上下文窗口不够用怎么办？

**A:**
1. 分解大任务为多个小会话
2. 使用记忆系统存储重要信息
3. 优化 CLAUDE.md 内容
4. 使用 `--file` 精确指定文件

---

### D. 参考资源索引

#### D.1 官方文档

| 资源 | 说明 |
|------|------|
| [Claude Code 官方文档](https://code.claude.com/docs) | Anthropic 官方文档 |
| [Claude Code 中文文档](https://code.claude.com/docs/zh-CN/quickstart) | 官方中文快速入门 |
| [Anthropic API 文档](https://docs.anthropic.com) | API 参考文档 |
| [MCP 协议规范](https://modelcontextprotocol.io) | Model Context Protocol |

#### D.2 中文学习资源

| 资源 | 说明 |
|------|------|
| [Claude Code 中文指南](https://claudecode.blueshirtmap.com/guide.html) | 系统性入门指南 |
| [AI Code Mirror](https://www.aicodemirror.com/docs) | 进阶使用教程 |
| [Claude Code 使用指南](https://github.com/Hoper-J/AI-Guide-and-Demos-zh_CN) | 安装与进阶技巧 |
| [Easy Vibe 教程](https://datawhalechina.github.io/easy-vibe/zh-cn/stage-3/core-skills/basics/) | 教程式讲解 |
| [Claude Coding](https://claudecoding.dev/) | 开发实践分享 |
| [学习 Claude Code](https://www.xuanyuancode.com/learn-claude-code) | 系统学习资源 |

#### D.3 源码解析项目

| 项目 | 说明 |
|------|------|
| [claude-code-book](https://github.com/lintsinghua/claude-code-book) | Claude Code 源码解析书籍 |
| [claude-code-source-code](https://github.com/shipinlei/claude-code-source-code) | 源码分析 |
| [learn-coding-agent](https://github.com/sanbuphy/learn-coding-agent) | 编程代理学习 |
| [claurst](https://github.com/Kuberwastaken/claurst) | Rust 实现版本 |
| [claude-code-rust](https://github.com/lorryjovens-hub/claude-code-rust) | Rust 实现版本 |
| [analysis_claude_code](https://github.com/ThreeFish-AI/analysis_claude_code) | 代码分析 |

#### D.4 记忆系统资源

| 资源 | 说明 |
|------|------|
| [Claude Code 记忆系统](https://www.runoob.com/claude-code/claude-code-memory.html) | 记忆功能详解 |
| [长期记忆系统](https://github.com/lintsinghua/claude-code-book/blob/main/第二部分-核心系统篇/06-记忆系统-Agent的长期记忆.md) | 长期记忆机制 |
| [记忆功能官方文档](https://code.claude.com/docs/zh-CN/memory) | 官方记忆文档 |

#### D.5 子代理资源

| 资源 | 说明 |
|------|------|
| [子代理详解](https://zhuanlan.zhihu.com/p/1961849176238294642) | Subagent 深入分析 |
| [子代理使用指南](https://www.runoob.com/claude-code/claude-code-subagent.html) | 使用教程 |
| [官方子代理文档](https://code.claude.com/docs/zh-CN/sub-agents) | 官方文档 |

#### D.6 CLAUDE.md 资源

| 资源 | 说明 |
|------|------|
| [CLAUDE.md 配置指南](https://www.claude-code-hub.org/docs/config/claude-md) | 配置详解 |
| [如何编写 CLAUDE.md](https://linguista.bearblog.dev/how-to-write-the-claudemd/) | 编写技巧 |
| [高效 CLAUDE.md](https://www.echovic.com/blog/ai/how-to-write-effective-claude-md/) | 最佳实践 |
| [CLAUDE.md 教程](https://www.xuanyuancode.com/learn-claude-code/tutorials/cu3) | 详细教程 |
| [CLAUDE.md 实践](https://zhuanlan.zhihu.com/p/2009744974980331332) | 知乎专栏 |
| [EasyClaude CLAUDE.md](https://easyclaude.com/post/claude-code-claude-md) | 完整指南 |

#### D.7 Skills 资源

| 资源 | 说明 |
|------|------|
| [Skills 官方文档](https://code.claude.com/docs/zh-CN/skills) | 官方技能文档 |
| [EasyClaude Skills](https://easyclaude.com/post/claude-code-skills) | 技能编写指南 |
| [Skills 教程](https://datawhalechina.github.io/easy-vibe/zh-cn/stage-3/core-skills/skills/) | 详细教程 |
| [UI/UX Skills](https://www.runoob.com/claude-code/claude-code-ui-ux-pro-max-skill.html) | UI/UX 技能 |
| [如何编写 Skills](https://www.tophci.com/posts/251206-how-to-write-skills) | 编写技巧 |
| [Skills 实践](https://zhuanlan.zhihu.com/p/1996724780209047225) | 知乎专栏 |

#### D.8 交互模式资源

| 资源 | 说明 |
|------|------|
| [交互模式官方文档](https://code.claude.com/docs/zh-CN/interactive-mode) | 官方交互模式文档 |
| [Vim 模式指南](https://blog.frognew.com/2026/01/claude-code-interactive-mode-vim.html) | Vim 模式详解 |
| [CLI 交互参考](https://www.runoob.com/claude-code/claude-code-cli.html) | CLI 交互参考 |
| [Claude Code 工作原理](https://code.claude.com/docs/zh-CN/how-claude-code-works) | 工作原理说明 |

#### D.9 工作流程资源

| 资源 | 说明 |
|------|------|
| [工作流程官方文档](https://code.claude.com/docs/zh-CN/common-workflows) | 常见工作流程 |
| [Claude Code Hub 工作流](https://www.claude-code-hub.org/docs/workflows) | 工作流程指南 |
| [长时间任务处理](https://datawhalechina.github.io/easy-vibe/zh-cn/stage-3/core-skills/long-running-tasks/) | 长时间任务 |
| [ClaudeCN 工作流](https://claudecn.com/docs/claude-code/workflows/) | 工作流详解 |

#### D.10 命令大全资源

| 资源 | 说明 |
|------|------|
| [命令大全](https://zhuanlan.zhihu.com/p/2021884176811434286) | 知乎命令大全 |
| [斜杠命令参考](https://www.runoob.com/claude-code/claude-code-slash-commands.html) | 斜杠命令 |
| [CLI 命令参考](https://www.runoob.com/claude-code/claude-code-cli-ref.html) | CLI 完整参考 |
| [Claude Code Hub 命令](https://www.claude-code-hub.org/docs/commands) | 命令文档 |

#### D.11 生态项目

| 项目 | 说明 |
|------|------|
| [CLI-Anything](https://github.com/HKUDS/CLI-Anything) | CLI 扩展框架 |
| [OpenCLI](https://github.com/jackwener/opencli) | 开放 CLI 架构 |
| [Kode-Agent](https://github.com/shareAI-lab/Kode-Agent) | 代理框架 |
| [learn-claude-code](https://github.com/shareAI-lab/learn-claude-code) | 学习笔记 |
| [claude-code-sdk-java](https://github.com/CyclingBits/claude-code-sdk-java) | Java SDK |
| [claude-howto](https://github.com/luongnv89/claude-howto) | 使用教程 |

#### D.12 Codex Agent 参考

| 资源 | 说明 |
|------|------|
| [Codex Agent Loop 解析](https://openai.com/zh-Hans-CN/index/unrolling-the-codex-agent-loop/) | OpenAI 官方解析 |

---

**文档版本：** 7.0.0
**最后更新：** 2026-04-29
**贡献者：** Claude Code 学习社区