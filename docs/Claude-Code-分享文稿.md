# 让 AI 从「写代码」到「做项目」—— OpenSpec + Superpowers + gstack 实战指南

> **分享时长：** 约 30 分钟
> **适用对象：** 开发团队、技术负责人、对 AI 编程感兴趣的工程师
> **前置准备：** 已安装 Claude Code、了解基本编程概念

---

## 目录

1. [开场引入（3 分钟）](#1-开场引入3-分钟)
2. [三件套概述（3 分钟）](#2-三件套概述3-分钟)
3. [OpenSpec：需求先行（5 分钟）](#3-openspec需求先行5-分钟)
4. [Superpowers：执行方法论（7 分钟）](#4-superpowers执行方法论7-分钟)
5. [gstack：虚拟工程团队（5 分钟）](#5-gstack虚拟工程团队5-分钟)
6. [实战整合：完整工作流（5 分钟）](#6-实战整合完整工作流5-分钟)
7. [总结与资源（2 分钟）](#7-总结与资源2-分钟)

---

## 1. 开场引入（3 分钟）

### 1.1 三个老问题

**各位好，今天我想先问大家一个问题：**

你们用 AI 写代码多久了？效果怎么样？

我见过很多团队用 AI 辅助开发，但真把它放进生产级项目，通常卡在三个地方：

| 问题 | 表现 | 后果 |
|------|------|------|
| **需求理解偏差** | 让 AI 加功能，写出来发现跟想的不是一回事 | 返工，浪费 |
| **执行过程黑盒** | AI 写了什么、怎么写的、测了没有，很难把控 | 质量没底 |
| **没有真实验证** | 代码看起来对了，但页面渲染、接口连通、部署后表现，AI 自己验证不了 | 上线翻车 |

这不只是"AI 不够聪明"的问题，而是**工作流缺失**的问题。

### 1.2 从「写代码」到「做项目」的转变

**写代码 vs 做项目，区别是什么？**

```
写代码：
我想好了怎么做 → 告诉 AI → AI 生成代码 → 我审查修改

做项目：
告诉 AI 要什么 → AI 自己想办法 → 写代码 → 测试 → 验证 → 部署
```

关键差异在于：**做项目需要一个完整的工作流，而不是一个聪明的助手。**

今天要分享的，就是一套经过验证的 AI 开发工作流：**OpenSpec + Superpowers + gstack**。

> 这三个开源项目分别解决需求对齐、执行标准化、验证交付三个问题，组合起来就是一套完整的 AI 增强开发工作流。
>
> — 来自 Garry Tan（Y Combinator CEO）

---

## 2. 三件套概述（3 分钟）

### 2.1 三者各司其职

```
┌─────────────────────────────────────────────────────────────────────┐
│                    OpenSpec + Superpowers + gstack                    │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   OpenSpec        Superpowers          gstack                        │
│   (需求层)          (执行层)            (验证层)                       │
│                                                                      │
│   管「想清楚」       管「做出来」          管「验证好」                   │
│                                                                      │
│   • 需求规范        • 头脑风暴            • 浏览器测试                  │
│   • 技术方案        • 任务拆解            • QA 测试                   │
│   • 实施清单        • TDD 开发            • 发布部署                   │
│                    • 子代理执行          • 监控告警                    │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

**一句话总结：**

- **OpenSpec** 管需求 — 先把「做什么」想清楚
- **Superpowers** 管执行 — 按标准流程把代码写出来
- **gstack** 管验证 — 确保代码真的能跑、真的可用

### 2.2 协作数据流

```
需求输入
    │
    ▼
┌──────────────┐     proposal.md
│   OpenSpec   │ ──→ design.md
│  (需求对齐)   │     tasks.md
└──────────────┘
    │
    ▼ tasks.md
┌──────────────┐     brainstorming → worktree → 小任务
│  Superpowers │ ──→ subagent 执行 → TDD → 代码审查
│  (规范执行)   │     分支收尾
└──────────────┘
    │
    ▼ 代码产出
┌──────────────┐     /browse 截图验证
│    gstack    │ ──→ /qa 端到端测试
│  (验证交付)   │     /ship + /land-and-deploy + /canary
└──────────────┘
    │
    ▼
生产上线
```

**分工边界（非常重要）：**

- OpenSpec **只产出规范文档，不写代码**
- Superpowers **只按规范执行编码流程，不直接操作浏览器或部署**
- gstack **只做验证和交付动作，不参与需求分析或架构决策**

三者之间通过**文件和命令**传递信息，不是通过共享内存或隐式状态。

---

## 3. OpenSpec：需求先行（5 分钟）

### 3.1 核心概念：双文件夹模型

OpenSpec 的核心是双文件夹模型：

```
openspec/
├── specs/     # 当前系统的事实来源（规范文件）
└── changes/   # 每次变更的完整提案
```

每份变更包含三个文件：

| 文件 | 作用 | 回答的问题 |
|------|------|-----------|
| `proposal.md` | 为什么要做 | 背景、目标、成功标准、不做会怎样 |
| `design.md` | 技术方案 | 架构决策、接口设计、数据流、依赖关系 |
| `tasks.md` | 实施清单 | 可执行的具体任务，作为 Superpowers 的输入 |

### 3.2 为什么规格先行能降低返工？

> 实测对比：使用 OpenSpec 后，相同需求下的 Token 消耗降低 30%-50%，返工率下降 60% 以上。

**传统方式的沟通漏斗：**

```
我想的需求 ──→ 我说的需求 ──→ AI 理解的需求 ──→ AI 写的代码
    ↓              ↓              ↓              ↓
 100%           80%            60%            40%
```

**OpenSpec 的沟通漏斗：**

```
需求文档 ←── AI + 人共同确认
    ↓
技术方案 ←── AI + 人共同确认
    ↓
实施清单 ←── AI + 人共同确认
    ↓
写代码   ←── 已经有共识，减少返工
```

**核心价值：这三个文件本身就是 AI 和人之间的「契约」。AI 在动手写代码之前，先在这三份文档里和你对齐。**

### 3.3 快速上手

**安装：**

```bash
npm install -g @fission-ai/openspec@latest
```

**初始化项目：**

```bash
cd your-project
openspec init
```

这会自动创建 `openspec/specs/` 和 `openspec/changes/` 目录结构。

**核心命令：**

```bash
# 提出变更提案
/opsx:propose add-dark-mode

# 按 tasks.md 逐项执行
/opsx:apply

# 归档到历史，更新规格库
/opsx:archive
```

### 3.4 输出示例：proposal.md

```markdown
# Proposal: add-user-notifications

## Why
用户在消息中心找不到新消息提醒，导致错过重要更新。

## What changes
- 新增邮件通知
- 新增应用内通知
- 新增推送通知（可选）

## Success Criteria
- 用户能在设置中关闭/开启各类通知
- 通知送达率 > 99%
- 邮件发送延迟 < 5 秒
```

---

## 4. Superpowers：执行方法论（7 分钟）

### 4.1 什么是 Superpowers？

Superpowers 是一套**不可跳过的 7 步工作流**，把 AI 写代码的过程标准化、可观测、可审查。

GitHub：obra/superpowers（16.6 万 Star，MIT 协议）

### 4.2 七步工作流详解

```
┌─────────────────────────────────────────────────────────────────────┐
│                      Superpowers 7 步工作流                            │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   1. brainstorming ──→ 苏格拉底式提问，澄清任务细节                    │
│          │               在写代码前暴露隐藏假设                          │
│          ▼                                                             │
│   2. using-git-worktrees ──→ 创建隔离分支，保护主分支                  │
│          │               避免污染主干                                  │
│          ▼                                                             │
│   3. writing-plans ──→ 拆解为 2-5 分钟可执行小任务                   │
│          │               每个任务都有明确验收标准                       │
│          ▼                                                             │
│   4. subagent-driven-development ──→ 每个任务派独立子代理              │
│          │               隔离上下文，减少干扰                          │
│          ▼                                                             │
│   5. test-driven-development ──→ RED → GREEN → REFACTOR             │
│          │               每段代码都有测试覆盖                           │
│          ▼                                                             │
│   6. requesting-code-review ──→ 两阶段：规范合规 + 代码质量           │
│          │               人在关键环节把关                              │
│          ▼                                                             │
│   7. finishing-a-development-branch ──→ 验证测试、合并决策             │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

**每一步都不可跳过。这不是官僚主义，而是生产环境的底线。**

### 4.3 关键步骤详解

**1. Brainstorming：暴露隐藏假设**

这不是普通的讨论，而是苏格拉底式提问：

```
用户: 帮我加一个用户通知功能

AI [Brainstorming]:
在开始之前，我有几个问题需要确认：

1. 通知的触发条件是什么？（用户行为？系统事件？定时任务？）
2. 通知的送达方式有哪几种？（邮件？推送？站内消息？）
3. 如果用户没有及时查看通知，应该怎么处理？（过期？重新发送？）
4. 通知的历史记录需要保留多久？（7天？30天？永久？）
5. 是否有通知频率限制？（同一类型通知多久发一次？）

请回答以上问题，我们再继续。
```

**为什么要先 brainstorming？**

> 在写代码前暴露隐藏假设 —— 这是避免返工最关键的一步。

**3. Writing Plans：任务拆解**

把大需求拆成 2-5 分钟可执行的小任务：

```markdown
# 任务拆解：用户通知功能

## Task 1: 创建数据库表（2 分钟）
- 文件: db/migrations/001_create_notifications.sql
- 验收: 表创建成功，字段完整

## Task 2: 实现邮件发送服务（5 分钟）
- 文件: src/services/email.ts
- 验收: 能发送测试邮件

## Task 3: 实现通知记录服务（5 分钟）
- 文件: src/services/notification.ts
- 验收: 能创建、查询、标记已读

... 以此类推
```

**4. Subagent 驱动开发**

每个任务启动独立子代理，隔离上下文：

```
Task 1 ──→ 子代理 A ──→ 独立审查
Task 2 ──→ 子代理 B ──→ 独立审查
Task 3 ──→ 子代理 C ──→ 独立审查
    │
    └── 主代理协调进度、解决跨任务依赖
```

**5. TDD 循环**

```
RED:    先写测试（预期失败）
   │
   ▼
GREEN:  写最小代码（让测试通过）
   │
   ▼
REFACTOR: 重构优化（保持功能不变）
```

### 4.4 安装与使用

**在 Claude Code 中：**

```
/plugin install superpowers@claude-plugins-official
```

或在 Codex CLI 中：

```
/plugins
# 搜索 superpowers，安装
```

---

## 5. gstack：虚拟工程团队（5 分钟）

### 5.1 Garry Tan 的虚拟工程团队

gstack 是 Y Combinator CEO Garry Tan 的 Claude Code 完整配置，GitHub 8.2 万 Star。

它的核心理念是：**把 Claude Code 变成一个 23 人的虚拟工程团队。**

Garry Tan 自己的数据：

> 2026 年代码产出是 2013 年的 **240 倍**（按逻辑变更计算，不是原始行数）。

### 5.2 gstack 命令一览

| 命令 | 角色 | 做什么 |
|------|------|--------|
| `/office-hours` | YC 导师 | 6 个灵魂拷问，重新定义产品方向 |
| `/plan-ceo-review` | CEO | 重新思考问题，找到 10 分产品 |
| `/plan-eng-review` | 工程经理 | 锁死架构、数据流、边界情况、测试 |
| `/plan-design-review` | 高级设计师 | 设计维度打分 0-10，AI Slop 检测 |
| `/review` | 代码审查员 | 自动修复问题，阻塞严重 bug |
| `/qa` | QA 工程师 | 真实浏览器测试，点击流程 |
| `/ship` | 发布工程师 | 跑测试、开 PR、自动部署 |
| `/retro` | 复盘教练 | 每周工程复盘 |
| `/careful` | 安全护栏 | 危险命令拦截（rm -rf、DROP TABLE、force-push） |

### 5.3 核心命令详解

**/browse — 浏览器截图验证**

```
/browse https://staging.myapp.com
```

自动打开浏览器，截图验证渲染效果。这是解决"AI 觉得对了但页面是乱的"的神器。

**/qa — 端到端测试**

```
/qa https://staging.myapp.com
```

用真实浏览器点击完整流程，发现 bug 自动修复。

**/ship — 自动发布**

```
/ship
```

自动执行：检测 base → 跑测试 → review diff → 写 CHANGELOG → 开 PR。

### 5.4 安全护栏

gstack 的 `/careful` 命令会拦截危险操作：

```bash
# 触发 /careful 后才会执行
rm -rf /
DROP TABLE users
git push --force
git reset --hard
kubectl delete pod --all
```

### 5.5 安装

```bash
git clone --single-branch --depth 1 \
  https://github.com/garrytan/gstack.git \
  ~/.claude/skills/gstack

cd ~/.claude/skills/gstack && ./setup
```

setup 会把 28 个 skill 符号链接到 `~/.claude/skills/` 顶层。

> 注意：setup 依赖 bun，没装的话先装：
> `curl -fsSL https://bun.sh/install | bash`

---

## 6. 实战整合：完整工作流（5 分钟）

### 6.1 完整流水线：从想法到上线

```
想法
  │
  ▼
┌─────────────────────────────────────────────────────────────────────┐
│  阶段 1: 需求对齐（OpenSpec）                                          │
├─────────────────────────────────────────────────────────────────────┤
│  /opsx:propose add-user-notifications                                │
│  ├──→ proposal.md（为什么要做）                                        │
│  ├──→ design.md（技术方案）                                           │
│  └──→ tasks.md（实施清单）                                            │
└─────────────────────────────────────────────────────────────────────┘
  │
  ▼
┌─────────────────────────────────────────────────────────────────────┐
│  阶段 2: 编码执行（Superpowers）                                       │
├─────────────────────────────────────────────────────────────────────┤
│  /brainstorming ──→ 澄清模糊点                                        │
│  /writing-plans ──→ 拆成 2-5 分钟小任务                               │
│  /subagent-driven-development ──→ 子代理执行                          │
│  /test-driven-development ──→ RED → GREEN → REFACTOR                 │
│  /requesting-code-review ──→ 两阶段审查                               │
└─────────────────────────────────────────────────────────────────────┘
  │
  ▼
┌─────────────────────────────────────────────────────────────────────┐
│  阶段 3: 验证交付（gstack）                                           │
├─────────────────────────────────────────────────────────────────────┤
│  /browse ──→ 截图验证页面渲染                                          │
│  /qa ──→ 端到端测试                                                   │
│  /ship ──→ 发布流程                                                   │
│  /land-and-deploy ──→ 合并 PR，等 CI，验证生产                         │
│  /canary ──→ 监控上线后错误率和性能                                    │
└─────────────────────────────────────────────────────────────────────┘
```

**关键原则：没有测试/截图/QA 报告，不算完成。**

### 6.2 任务分流策略

不是所有任务都需要走完整流程：

| 任务类型 | 流程 |
|----------|------|
| **只读任务** | 分析、解释、代码阅读 — 直接处理 |
| **轻量任务** | 单文件修改、明确 bug — 跳过 brainstorming，直接实现 + 定向验证 |
| **中任务** | 多文件但边界清晰 — OpenSpec + 简短 brainstorming + 实现 + /browse |
| **大任务** | 跨模块、新架构 — 完整闭环：OpenSpec → brainstorming → TDD → /qa → /ship |

### 6.3 实战案例：5 天完成 AI 视频生成系统

**来自社区的真实案例：**

- **后端**（Spring Boot）：2k 行代码
- **前端**（Vue 3 + Element Plus）：4k 行代码

**经验总结：**

1. **每个任务完成后立即 commit** — 小步迭代比大爆炸更可控
2. **第一个任务完成后写 CLAUDE.md** — 后续任务可以直接续上，省去重新熟悉项目的时间
3. **前端效果不好时用 /frontend-design** — 告诉 AI "CSS 参考苹果官网，红白配色"
4. **安装 context7 skill** — 自动查找最新版本代码，避免版本不匹配

### 6.4 CLAUDE.md 核心规则配置

要让三件套跑起来，需要在 `~/.claude/CLAUDE.md` 中配置裁决规则：

```markdown
# Claude Code 配置：OpenSpec + superpowers + gstack

主干由三个插件组成：
- OpenSpec —— 规范与需求层（proposal / design / tasks）
- superpowers —— 思考与流程层（plan / brainstorm / debug / TDD / review / verify）
- gstack —— 执行与外部世界层（browser / QA / ship / deploy / canary / 护栏）

## 核心原则

1. **规范先行**：任何需求变更必须先过 OpenSpec，调用 /opsx:propose
2. **流程归 superpowers**：brainstorm、plan、debug、TDD、verify、code review
3. **执行归 gstack**：浏览器、QA、ship、deploy、canary、retro
4. **证据优先**：没有测试/截图/QA 报告不算完成
5. **歧义先 brainstorm**：任何创造性工作前先调用 brainstorming

## 任务分流

- 轻量任务：跳过 brainstorming，直接实现 + 定向验证
- 中任务：OpenSpec → 简短 brainstorming → 实现 + /browse
- 大任务：完整闭环（OpenSpec → TDD → /qa → /ship）

## 安全护栏

- rm -rf / DROP TABLE / force-push 必须先过 /careful
- /ship 和 /land-and-deploy 必须用户明确确认
```

---

## 7. 总结与资源（2 分钟）

### 7.1 核心要点回顾

```
┌─────────────────────────────────────────────────────────────────────┐
│                         今日要点速记                                  │
├─────────────────────────────────────────────────────────────────────┤
│                                                                      │
│   1. OpenSpec = 规格先行                                              │
│      → proposal / design / tasks 三件套                              │
│      → 让 AI 和人在写代码前先达成共识                                  │
│                                                                      │
│   2. Superpowers = 执行标准化                                        │
│      → 7 步不可跳过的工作流                                          │
│      → brainstorming → TDD → 子代理 → 代码审查                        │
│                                                                      │
│   3. gstack = 验证交付                                               │
│      → 23 个虚拟角色覆盖全流程                                       │
│      → /browse / /qa / /ship / /canary                              │
│                                                                      │
│   组合效果 > 单个工具：三件套各司其职，流水线作业                       │
│                                                                      │
└─────────────────────────────────────────────────────────────────────┘
```

### 7.2 效果对比

| 维度 | 纯 AI 写代码 | OpenSpec | OpenSpec + Superpowers | 三件套组合 |
|------|-------------|----------|------------------------|------------|
| 需求对齐 | ❌ 容易跑偏 | ✅ 规格先行 | ✅ 规格+头脑风暴 | ✅ 规格+头脑风暴+CEO 审查 |
| 代码质量 | ⚠️ 取决于 prompt | ⚠️ 取决于 prompt | ✅ TDD 保证质量 | ✅ TDD + 代码审查 + QA |
| 测试覆盖 | ❌ 通常没有 | ❌ 通常没有 | ✅ 红绿重构循环 | ✅ 红绿重构 + 真实浏览器 QA |
| 审查机制 | ❌ 没有 | ❌ 没有 | ✅ 子代理互审 | ✅ 多级审查（CEO/工程/QA/安全） |
| 适合场景 | 小改小修 | 明确需求的项目 | 中大型功能开发 | 企业级项目全流程 |

### 7.3 学习路径建议

| 阶段 | 时间 | 目标 | 关键动作 |
|------|------|------|----------|
| **入门** | 第 1 周 | 跑通规格驱动开发 | 安装 OpenSpec、完成第一个 proposal |
| **进阶** | 第 2-4 周 | 掌握 Superpowers 工作流 | 用 TDD 完成第一个功能模块 |
| **精通** | 第 2-3 月 | 构建自动化工作流 | 三件套串联，实现需求→上线全流程 |

### 7.4 立即行动

**安装三件套：**

```bash
# 1. 安装 OpenSpec
npm install -g @fission-ai/openspec@latest

# 2. 安装 Superpowers
# 在 Claude Code 中: /plugin install superpowers@claude-plugins-official

# 3. 安装 gstack
git clone --single-branch --depth 1 \
  https://github.com/garrytan/gstack.git \
  ~/.claude/skills/gstack && cd ~/.claude/skills/gstack && ./setup
```

**从一个功能开始：**

```
/opsx:propose add-user-notifications
→ 查看生成的 proposal.md / design.md / tasks.md
→ /brainstorming 澄清细节
→ 开始开发
```

### 7.5 推荐资源

**开源仓库：**

- [Fission-AI/OpenSpec](https://github.com/Fission-AI/OpenSpec) — 4.2 万 Star
- [obra/superpowers](https://github.com/obra/superpowers) — 16.6 万 Star
- [garrytan/gstack](https://github.com/garrytan/gstack) — 8.2 万 Star

**本项目文档：**

- `docs/Claude-Code-完全指南.md` — 约 3000 行的 Claude Code 全面参考

---

> **感谢聆听！**
> 如有问题，欢迎会后交流。
> 本文档完整版收录于项目 `docs/` 目录。
