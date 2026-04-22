# Harness Learning

An opinionated bilingual knowledge base about **AI agent harness design**.

一个聚焦 **AI Agent Harness 设计** 的中英双语知识库。

## What "Harness" Means Here

In this repository, **harness** means the engineering layer around an LLM or agent:

- prompts and system instructions
- tools and execution environment
- context, memory, and artifacts
- telemetry, traces, and verification
- evals, feedback loops, and iteration workflows

在这个仓库里，**harness** 指的是围绕 LLM 或 Agent 的工程层：

- 提示词与系统指令
- 工具与执行环境
- 上下文、记忆与工件
- 遥测、追踪与验证
- 评测、反馈闭环与迭代流程

## Scope

This first version focuses on material from:

- Anthropic / Claude
- OpenAI
- well-known practitioners and researchers

首个版本重点整理以下来源：

- Anthropic / Claude
- OpenAI
- 知名实战专家与研究者

The notes are **curated summaries**, not mirrors of the original articles.

这些内容是**整理后的摘要与解读**，不是原文搬运。

## Repository Map

- `knowledge-base/index.md`: topic map and reading paths
- `knowledge-base/sources.md`: curated source catalog
- `knowledge-base/companies/`: company notes
- `knowledge-base/experts/`: expert notes
- `docs/plans/`: repo design notes

## Reading Order

If you are new to this topic:

1. `knowledge-base/index.md`
2. `knowledge-base/companies/anthropic.md`
3. `knowledge-base/companies/openai.md`
4. `knowledge-base/experts/simon-willison.md`
5. `knowledge-base/experts/eugene-yan.md`

如果你刚接触这个主题，建议按这个顺序阅读：

1. `knowledge-base/index.md`
2. `knowledge-base/companies/anthropic.md`
3. `knowledge-base/companies/openai.md`
4. `knowledge-base/experts/simon-willison.md`
5. `knowledge-base/experts/eugene-yan.md`

## Curation Rules

- Prefer official company sources for platform and product claims.
- Use expert posts for mental models, evaluation methods, and engineering patterns.
- Keep notes bilingual and concise.
- Favor actionable takeaways over vague commentary.

- 平台和产品能力优先采用官方来源。
- 心智模型、评测方法和工程模式使用专家文章补充。
- 内容保持中英双语且尽量简洁。
- 优先输出可执行启发，而不是空泛评论。

## Status

Seed version: `v0.1`

Current focus:

- agent runtime design
- long-running autonomous workflows
- tool use and app integration
- eval harnesses and feedback loops

当前首版重点：

- agent 运行时设计
- 长时 autonomous workflow
- 工具调用与应用集成
- eval harness 与反馈闭环
