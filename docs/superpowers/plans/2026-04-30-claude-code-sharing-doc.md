# Claude Code 对外分享文档 实施计划

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** 创建一套 30 分钟对外分享文档，主题为"让 AI 从写代码到做项目——Claude Code 实战指南"，内容覆盖 Claude Code 核心能力、最佳实践、多模型协作策略。

**Architecture:** 以 Markdown 演示文稿形式呈现，结构为：开场引入(3min) → 核心概念(7min) → 实战技巧(10min) → 多模型协作(5min) → 总结与展望(5min)。每部分包含演讲稿要点和配图说明。

**Tech Stack:** Markdown 文档，可配合 reveal.js 或直接作为演讲大纲使用

---

### Task 1: 创建分享文档主文件结构

**Files:**
- Create: `docs/Claude-Code-分享文稿.md`

- [ ] **Step 1: 创建文档大纲和开场部分**

编写文档的前言和第一部分（开场引入，约3分钟演讲内容）：
- 标题页设计
- AI 编程工具发展现状（2026年格局）
- 为什么选择 Claude Code
- 与其他工具的定位差异

内容应包含：
- Claude Code 的核心定位：终端原生、上下文感知、工具链集成
- 与 Cursor、Copilot、Claude.ai 的对比表
- 引用 Anthropic 官方描述："低层且不强制特定工作流"的设计哲学

- [ ] **Step 2: 编写第二部分——核心概念（7分钟）**

涵盖内容：
1. CLAUDE.md 文件系统
   - 什么是 CLAUDE.md（AI 的"宪法"）
   - 多层级配置继承（项目级 > 根目录 > 全局）
   - 最佳实践示例
2. 记忆系统
   - 三层记忆架构：短期(Session) / 工作(Context) / 长期(Memory)
   - 记忆类型：user/feedback/project/reference
3. 工具系统概览
   - 六大核心工具：Read/Write/Edit/Bash/Grep/Glob
   - 权限管理机制
4. 会话与上下文管理
   - "AI 上下文像牛奶，越新鲜越好"
   - 主动压缩上下文策略

- [ ] **Step 3: 编写第三部分——实战技巧（10分钟）**

引用 Claude Code 之父 Boris Cherny 的 13 条使用心得，并结合项目文档扩展：

1. 并行运行多个 Claude 实例（终端5个 + 网页5-10个）
2. Plan 模式先行（Shift+Tab×2）
3. 团队协作共享 CLAUDE.md
4. 自定义 Slash Commands（/commit-push-pr 等）
5. 子代理系统（Subagent）
   - code-simplifier、verify-app 等
6. Hook 系统自动化
   - PostToolUse 自动格式化
7. MCP 协议集成
   - Slack、BigQuery、Sentry 等
8. 验证循环（最重要！质量提升2-3倍）

每条技巧配演讲要点和代码/配置示例。

- [ ] **Step 4: 编写第四部分——多模型协作策略（5分钟）**

基于"组合拳"系列文章内容：

1. 多模型分工模式
   - Claude：核心逻辑、大型重构、复杂算法
   - Gemini：设计稿转代码、多模态理解
   - Codex/GPT：端到端自动化、Bug 修复
2. 实战案例：1小时完成一个完整功能模块
   - Gemini 生成前端（10min）
   - Claude 生成后端（15min）
   - Codex 整合测试（20min）
3. 国产模型接入（GLM 4.7、Kimi K2 等）
4. 选型参考表

- [ ] **Step 5: 编写第五部分——总结与展望（5分钟）**

1. 核心要点回顾
   - 快捷键速查表
   - 最佳实践清单
2. 从"写代码"到"做项目"的思维转变
   - 开发者角色转变：从执行者到编排者
   - "AI 不只是辅助工具，而是执行者"
3. 学习路径建议
   - 入门：安装配置 + CLAUDE.md
   - 进阶：Skill 系统 + Hook + SubAgent
   - 精通：多模型协作 + 自动化工作流
4. 推荐资源
   - 官方文档：code.claude.com/docs
   - 项目完全指南（本项目）
   - claude-code-tips 开源项目

- [ ] **Step 6: 添加附录和演讲者备注**

1. 快捷键速查表（一页）
2. 配置文件模板
3. 常用 Slash Commands 列表
4. 演讲时间分配建议
5. 互动环节问题建议

- [ ] **Step 7: 最终审校和格式调整**

检查清单：
- 上下文关联性（各部分之间的逻辑衔接）
- 时间控制（总量适合30分钟演讲）
- 代码示例准确性
- 关键数据引用准确性
- 中文表达流畅度
