# Claude Code 完全指南

> **版本：** 1.0.0
> **更新日期：** 2026-04-07
> **适用版本：** Claude Code CLI v1.x

---

## 目录

- [序言](#序言)
  - [关于本书](#关于本书)
  - [阅读建议](#阅读建议)
- [第一部分：快速上手](#第一部分快速上手)
  - [第1章 认识 Claude Code](#第1章-认识-claude-code)
  - [第2章 安装与配置](#第2章-安装与配置)
  - [第3章 核心概念](#第3章-核心概念)
  - [第4章 第一个项目](#第4章-第一个项目)
- [第二部分：功能详解](#第二部分功能详解)
  - [第5章 记忆系统](#第5章-记忆系统)
  - [第6章 钩子系统](#第6章-钩子系统)
  - [第7章 命令行工具](#第7章-命令行工具)
  - [第8章 技能系统](#第8章-技能系统)
  - [第9章 插件系统](#第9章-插件系统)
  - [第10章 提示词系统](#第10章-提示词系统)
  - [第11章 工具集](#第11章-工具集)
  - [第12章 MCP 协议](#第12章-mcp-协议)
  - [第13章 子代理系统](#第13章-子代理系统)
- [第三部分：实战场景](#第三部分实战场景)
  - [第14章 代码重构场景](#第14章-代码重构场景)
  - [第15章 调试与修复场景](#第15章-调试与修复场景)
  - [第16章 项目初始化场景](#第16章-项目初始化场景)
- [第四部分：深入原理](#第四部分深入原理)
  - [第17章 架构设计总览](#第17章-架构设计总览)
  - [第18章 上下文压缩机制](#第18章-上下文压缩机制)
  - [第19章 源码分析：核心引擎](#第19章-源码分析核心引擎)
  - [第20章 源码分析：扩展系统](#第20章-源码分析扩展系统)
- [第五部分：生态融合](#第五部分生态融合)
  - [第21章 CLI-Anything 集成](#第21章-cli-anything-集成)
  - [第22章 OpenCLI 集成](#第22章-opencli-集成)
- [附录](#附录)
  - [A. 配置文件参考](#a-配置文件参考)
  - [B. 命令速查表](#b-命令速查表)
  - [C. 常见问题解答](#c-常见问题解答)

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

| 特性 | Claude Code | Cursor | GitHub Copilot | Claude.ai |
|------|-------------|--------|----------------|-----------|
| 运行环境 | 终端 | IDE | IDE | Web |
| 项目级理解 | ✅ | ✅ | ❌ | ❌ |
| 命令执行 | ✅ | ❌ | ❌ | ❌ |
| 自定义工具 | ✅ | ❌ | ❌ | ❌ |
| 离线使用 | ❌ | ❌ | ❌ | ❌ |

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

| 工具 | 功能 | 权限级别 |
|------|------|----------|
| Read | 读取文件内容 | 低风险 |
| Write | 写入文件内容 | 高风险 |
| Edit | 编辑文件 | 高风险 |
| Bash | 执行 Shell 命令 | 高风险 |
| Grep | 搜索文件内容 | 低风险 |
| Glob | 匹配文件路径 | 低风险 |

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

### 第4章 第一个项目

#### 4.1 创建示例项目

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

#### 4.2 基本对话操作

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

#### 4.3 完成一个小功能

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

### 第5章 记忆系统

#### 5.1 记忆系统概述

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

#### 5.2 CLAUDE.md 配置文件

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

#### 5.3 长期记忆机制

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

#### 5.4 记忆管理实践

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

#### 5.5 高级记忆技巧

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

### 第6章 钩子系统

#### 6.1 Hook 机制概述

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

#### 6.2 Hook 类型详解

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

#### 6.3 Hook 配置

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

#### 6.4 Hook 实践案例

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

### 第7章 命令行工具

#### 7.1 命令行概览

Claude Code 提供了丰富的命令行选项：

```bash
claude [选项] [文件...]
```

#### 7.2 核心命令详解

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

#### 7.3 文件操作命令

```bash
# 指定文件上下文
claude --file src/main.ts --file src/utils.ts

# Git 相关操作
claude --git diff HEAD~1
claude --git log --oneline -10

# 差异对比
claude --diff main..feature-branch
```

#### 7.4 配置与诊断命令

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

#### 7.5 高级命令选项

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

#### 7.6 命令组合技巧

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

### 第8章 技能系统

#### 8.1 技能系统概述

技能（Skills）是 Claude Code 的扩展机制，允许您定义可复用的提示词模板和工作流程。

**技能 vs 插件**

| 特性 | 技能 | 插件 |
|------|------|------|
| 定义方式 | Markdown 文件 | 代码包 |
| 复杂度 | 简单 | 复杂 |
| 用途 | 提示词模板 | 功能扩展 |
| 分发 | 文件复制 | npm 包 |

#### 8.2 内置技能

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

#### 8.3 自定义技能开发

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

#### 8.4 技能管理

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

### 第9章 插件系统

#### 9.1 插件系统概述

插件系统提供了比技能更强大的扩展能力，允许您：
- 添加自定义工具
- 扩展 CLI 命令
- 集成外部服务

#### 9.2 安装与管理插件

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

#### 9.3 插件开发指南

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

### 第10章 提示词系统

#### 10.1 提示词基础

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

#### 10.2 CLAUDE.md 提示词

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

#### 10.3 高级提示词技术

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

### 第11章 工具集

#### 11.1 工具系统架构

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

#### 11.2 内置工具详解

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

#### 11.3 工具使用实践

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

### 第12章 MCP 协议

#### 12.1 MCP 协议概述

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

#### 12.2 MCP Server 开发

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

#### 12.3 MCP Client 集成

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

#### 12.4 实战案例

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

### 第13章 子代理系统（Subagent）

#### 13.1 子代理概述

子代理（Subagent）是 Claude Code 的核心特性之一，它允许主代理派发独立的子代理来并行处理复杂任务。这种设计模式极大地提升了 Claude Code 处理大型、复杂项目的能力。

**核心优势**

| 优势 | 说明 |
|------|------|
| **并行处理** | 多个子代理同时执行独立任务，大幅提升效率 |
| **上下文隔离** | 每个子代理拥有独立的上下文窗口，避免信息干扰 |
| **专业分工** | 不同类型的子代理专注于特定领域，提高处理质量 |
| **结果聚合** | 主代理汇总各子代理结果，形成完整解决方案 |

#### 13.2 子代理架构

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

#### 13.3 内置子代理类型

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

#### 13.4 子代理调用方式

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

#### 13.5 子代理工作流程

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

#### 13.6 子代理配置与定制

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

#### 13.7 子代理最佳实践

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

#### 13.8 子代理源码实现（Rust）

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

#### 13.9 子代理使用场景实例

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

## 第三部分：实战场景

### 第14章 代码重构场景

#### 13.1 大规模重构流程

**场景描述**

需要将一个遗留的 JavaScript 项目重构为 TypeScript，同时优化代码结构。

**重构步骤**

```
用户: 我需要将这个 JavaScript 项目重构为 TypeScript，请帮我规划重构步骤

Claude: 我来分析项目结构并制定重构计划。

[使用 Glob 和 Read 工具分析项目]

## 重构计划

### 阶段 1：准备工作（1-2 天）
1. 添加 TypeScript 配置
2. 添加类型声明文件
3. 配置构建流程

### 阶段 2：渐进迁移（按模块）
1. 工具函数（src/utils/）
2. 数据模型（src/models/）
3. 业务逻辑（src/services/）
4. API 层（src/api/）
5. 组件层（src/components/）

### 阶段 3：优化完善
1. 启用严格模式
2. 添加缺失的类型
3. 优化类型定义
4. 更新测试

是否开始执行？
```

#### 13.2 常见重构模式

**提取函数**

```
用户: 这段代码重复了很多次，请帮我提取为独立函数

Claude: 我来分析重复代码并提取为可复用函数。

[使用 Grep 搜索相似代码]

发现 5 处相似的代码片段，核心逻辑是：
- 格式化日期
- 添加时区信息
- 转换为目标格式

我将创建一个 formatDate 函数：

[使用 Write 工具]

```typescript
// src/utils/date.ts
export function formatDate(
  date: Date | string,
  timezone: string = 'UTC',
  format: string = 'YYYY-MM-DD'
): string {
  // 实现...
}
```

现在替换所有重复代码：

[使用 Edit 工具多次]
```

---

### 第15章 调试与修复场景

#### 14.1 Bug 定位技巧

**场景描述**

生产环境出现间歇性的数据不一致问题。

**调试过程**

```
用户: 生产环境偶尔出现用户数据不一致，请帮我分析可能的原因

Claude: 我来分析可能导致数据不一致的代码。

[使用 Grep 搜索相关代码]

## 可能的问题点

### 1. 并发更新问题
文件: src/services/user-service.ts:45
```typescript
// 问题：没有使用事务
async updateUser(userId: string,  UserData) {
  await this.db.users.update(userId, data);
  await this.db.profiles.update(userId, data.profile);
}
```

### 2. 缓存一致性问题
文件: src/cache/user-cache.ts:28
```typescript
// 问题：缓存更新顺序错误
async invalidate(userId: string) {
  await this.cache.delete(`user:${userId}`);
  // 数据库更新可能在缓存删除之后
}
```

### 3. 分布式锁缺失
文件: src/handlers/user-handler.ts:67
```typescript
// 问题：多个实例可能同时处理同一用户的请求
async handleUserUpdate(userId: string) {
  const user = await this.service.getUser(userId);
  // 没有加锁
  await this.service.updateUser(userId, modifiedData);
}
```

## 建议修复方案

1. 使用数据库事务
2. 实现分布式锁
3. 优化缓存更新策略

是否需要我实现这些修复？
```

---

### 第16章 项目初始化场景

#### 15.1 项目脚手架

**场景描述**

创建一个新的 Vue 3 + TypeScript 项目。

**初始化过程**

```
用户: 创建一个新的 Vue 3 项目，使用 TypeScript 和 Vite

Claude: 我来创建一个完整的 Vue 3 + TypeScript 项目结构。

[使用 Bash 工具]
npm create vite@latest my-project -- --template vue-ts

[使用 Write 工具创建配置文件]

## 创建的文件结构

```
my-project/
├── src/
│   ├── components/
│   │   └── HelloWorld.vue
│   ├── composables/
│   │   └── useCounter.ts
│   ├── types/
│   │   └── index.ts
│   ├── utils/
│   │   └── helpers.ts
│   ├── App.vue
│   └── main.ts
├── tests/
│   └── unit/
├── .eslintrc.js
├── .prettierrc
├── tsconfig.json
├── vite.config.ts
└── package.json
```

## 已配置的功能

1. ✅ TypeScript 严格模式
2. ✅ ESLint + Prettier
3. ✅ Vitest 测试框架
4. ✅ 路径别名 (@/)
5. ✅ 自动导入

需要我继续添加其他配置吗？
```

---

## 第四部分：深入原理

### 第17章 架构设计总览

#### 16.1 整体架构

Claude Code 的架构设计遵循模块化和可扩展原则：

```
┌────────────────────────────────────────────────────────────┐
│                    Claude Code 架构                         │
├────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌─────────────────────────────────────────────────────┐   │
│  │                    CLI Layer                         │   │
│  │  ┌─────────┐  ┌─────────┐  ┌─────────┐             │   │
│  │  │ Parser  │  │ Router  │  │ Output  │             │   │
│  │  └─────────┘  └─────────┘  └─────────┘             │   │
│  └─────────────────────────────────────────────────────┘   │
│                          │                                  │
│                          ▼                                  │
│  ┌─────────────────────────────────────────────────────┐   │
│  │                 Core Engine                          │   │
│  │  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐ │   │
│  │  │ Session     │  │ Context     │  │ Tool        │ │   │
│  │  │ Manager     │  │ Manager     │  │ Dispatcher  │ │   │
│  │  └─────────────┘  └─────────────┘  └─────────────┘ │   │
│  └─────────────────────────────────────────────────────┘   │
│                          │                                  │
│                          ▼                                  │
│  ┌─────────────────────────────────────────────────────┐   │
│  │                 Extension Layer                      │   │
│  │  ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌────────┐ │   │
│  │  │ Skills  │  │ Plugins │  │ Hooks   │  │ MCP    │ │   │
│  │  └─────────┘  └─────────┘  └─────────┘  └────────┘ │   │
│  └─────────────────────────────────────────────────────┘   │
│                          │                                  │
│                          ▼                                  │
│  ┌─────────────────────────────────────────────────────┐   │
│  │                 Infrastructure                       │   │
│  │  ┌─────────┐  ┌─────────┐  ┌─────────┐             │   │
│  │  │ Config  │  │ Memory  │  │ API     │             │   │
│  │  │ System  │  │ System  │  │ Client  │             │   │
│  │  └─────────┘  └─────────┘  └─────────┘             │   │
│  └─────────────────────────────────────────────────────┘   │
│                                                             │
└────────────────────────────────────────────────────────────┘
```

#### 16.2 核心组件

**Session Manager（会话管理器）**

负责管理用户会话的生命周期：
- 创建和销毁会话
- 维护对话历史
- 处理会话恢复

**Context Manager（上下文管理器）**

负责管理上下文窗口：
- Token 计数和限制
- 上下文压缩
- 优先级排序

**Tool Dispatcher（工具调度器）**

负责工具的调用和执行：
- 工具注册和发现
- 权限检查
- 执行和结果处理

#### 16.3 交互流程

```
用户输入
   │
   ▼
┌─────────────┐
│ CLI Parser  │ 解析命令和参数
└─────────────┘
   │
   ▼
┌─────────────┐
│ Session     │ 获取或创建会话
│ Manager     │
└─────────────┘
   │
   ▼
┌─────────────┐
│ Context     │ 构建上下文
│ Manager     │ - 加载 CLAUDE.md
│             │ - 加载记忆
│             │ - 加载文件内容
└─────────────┘
   │
   ▼
┌─────────────┐
│ API Client  │ 调用 Claude API
└─────────────┘
   │
   ▼
┌─────────────┐
│ Response    │ 处理响应
│ Handler     │ - 解析工具调用
│             │ - 执行工具
│             │ - 生成输出
└─────────────┘
   │
   ▼
用户输出
```

---

### 第18章 上下文压缩机制

#### 17.1 上下文管理挑战

Claude Code 面临的上下文管理挑战：

1. **Token 限制**：模型有最大上下文窗口限制
2. **信息密度**：如何在有限空间内保留最多有用信息
3. **相关性判断**：判断哪些信息对当前任务最重要

#### 17.2 压缩策略

**摘要压缩**

将长对话历史压缩为简短摘要：

```
原始对话（1000 tokens）:
用户: 请帮我创建一个用户管理模块
Claude: 好的，我来创建...
用户: 添加一个搜索功能
Claude: 我来添加搜索...
用户: 优化搜索性能
Claude: 我来优化...

压缩后（100 tokens）:
[摘要] 创建了用户管理模块，包含 CRUD 功能和搜索功能，已优化搜索性能。
```

**层级压缩**

按重要性分层处理：

```
优先级 1 (必须保留):
- 系统提示词
- CLAUDE.md 内容
- 当前任务描述

优先级 2 (重要):
- 最近的对话
- 关键文件内容
- 错误信息

优先级 3 (可选):
- 历史对话摘要
- 相关文件引用
- 工具调用记录
```

**选择性保留**

基于相关性评分决定保留内容：

```rust
// 伪代码
fn calculate_relevance(content: &str, query: &str) -> f64 {
    let keyword_score = keyword_match(content, query);
    let semantic_score = semantic_similarity(content, query);
    let recency_score = recency_weight(content);

    keyword_score * 0.4 +
    semantic_score * 0.4 +
    recency_score * 0.2
}
```

#### 17.3 实现机制

**重要性评分算法**

```rust
struct ContentItem {
    content: String,
    timestamp: DateTime,
    source: ContentSource,
    relevance_score: f64,
}

impl ContextManager {
    fn prioritize_content(&self, items: &[ContentItem], budget: usize) -> Vec<&ContentItem> {
        let mut scored_items: Vec<_> = items.iter().map(|item| {
            let score = self.calculate_importance(item);
            (item, score)
        }).collect();

        // 按分数排序
        scored_items.sort_by(|a, b| b.1.partial_cmp(&a.1).unwrap());

        // 选择直到达到预算
        let mut selected = Vec::new();
        let mut total_tokens = 0;

        for (item, _) in scored_items {
            let tokens = self.count_tokens(&item.content);
            if total_tokens + tokens <= budget {
                selected.push(item);
                total_tokens += tokens;
            }
        }

        selected
    }

    fn calculate_importance(&self, item: &ContentItem) -> f64 {
        let recency = self.recency_factor(item.timestamp);
        let source_weight = self.source_weight(item.source);
        let relevance = item.relevance_score;

        recency * 0.3 + source_weight * 0.3 + relevance * 0.4
    }
}
```

#### 17.4 优化建议

**提示词优化**

```markdown
# 优化前
用户: 请帮我看看这个文件有什么问题，然后修复它，如果需要的话可以参考其他文件

# 优化后
用户: 修复 src/auth/login.ts 中的类型错误
```

**记忆利用**

```markdown
# CLAUDE.md 中记录常用信息
## 项目结构
- src/api/ - API 调用
- src/components/ - Vue 组件
- src/utils/ - 工具函数

## 常用命令
- npm run dev - 开发服务器
- npm test - 运行测试
```

**分会话策略**

```bash
# 将大任务拆分为多个会话
claude -p "分析项目结构" > analysis.md
claude -p "根据分析结果实现功能 X" analysis.md
```

---

### 第19章 源码分析：核心引擎

#### 18.1 源码结构概览

Claude Code 的 Rust 实现源码结构：

```
claude-code-rust/
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

#### 18.2 核心数据结构

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

#### 18.3 对话管理实现

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

#### 18.4 工具调度实现

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

#### 18.5 API 交互实现

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

### 第20章 源码分析：扩展系统

#### 19.1 扩展系统架构

```rust
// src/extension/mod.rs

/// 扩展系统
pub struct ExtensionSystem {
    /// 插件管理器
    plugin_manager: PluginManager,

    /// 技能注册表
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

#### 19.2 Hook 系统实现

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

#### 19.3 技能系统实现

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

#### 19.4 MCP 协议实现

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

## 第五部分：生态融合

### 第21章 CLI-Anything 集成

#### 20.1 CLI-Anything 概述

CLI-Anything 是一个通用的 CLI 扩展框架，它提供了一种标准化的方式来构建和扩展命令行工具。

**核心特性**
- 模块化命令系统
- 插件架构
- 配置管理
- 输出格式化

#### 20.2 集成方案设计

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

#### 20.3 实现步骤

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

#### 20.4 使用场景

**增强型 CLI 工具**

```bash
# 使用 CLI-Anything 调用 Claude Code
cli-anything ask "解释这个函数的作用" --file src/utils.ts

# 多命令组合
cli-anything chain "analyze src/ | suggest improvements | apply"
```

---

### 第22章 OpenCLI 集成

#### 21.1 OpenCLI 概述

OpenCLI 是一个开放的 CLI 架构标准，旨在提供跨平台、可互操作的命令行工具生态系统。

**核心特性**
- 标准化命令定义
- 跨平台兼容
- 插件发现机制
- 统一的配置管理

#### 21.2 集成策略

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

#### 21.3 实现细节

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

#### 21.4 扩展案例

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

## 参考资源

- [Claude Code 官方文档](https://code.claude.com/docs)
- [Claude Code 中文指南](https://claudecode.blueshirtmap.com/guide.html)
- [Anthropic API 文档](https://docs.anthropic.com)
- [MCP 协议规范](https://modelcontextprotocol.io)
- [CLI-Anything 项目](https://github.com/HKUDS/CLI-Anything)
- [OpenCLI 项目](https://github.com/jackwener/opencli)

---

**文档版本：** 1.0.0
**最后更新：** 2026-04-07
**贡献者：** Claude Code 学习社区