# Practical Handbook Redesign

## Goal

Upgrade the repository from a seed summary collection into a bilingual, practical handbook for building agent harnesses.

把仓库从“首批摘要集合”升级成一个双语、可操作的 agent harness 搭建手册。

## Why The Redesign Was Needed

The original version was useful for orientation, but too shallow for builders. It summarized important articles, yet did not sufficiently answer the questions a developer actually has:

- what should I build first
- which platform or framework should I choose
- how do I structure tools, state, artifacts, and eval
- how do I copy the best patterns from OpenAI, Anthropic, Google, and LangGraph

旧版本适合入门定位，但对真正要开工的人来说不够深。它能告诉读者“有哪些重要文章”，却不能充分回答这些真正会遇到的问题：

- 第一版到底先搭什么
- 平台和框架应该怎么选
- tools、state、artifacts 和 eval 应该怎么组织
- OpenAI、Anthropic、Google、LangGraph 的好方法到底该怎么抄

## New Information Architecture

The redesigned repository has four layers:

1. `foundations/`: mental models and step-by-step build guides
2. `companies/`: company playbooks focused on platform and runtime patterns
3. `frameworks/`: implementation-layer guidance for LangChain, LangGraph, and Deep Agents
4. `experts/`: supporting notes on evals, mental models, and failure analysis

重构后的仓库分成四层：

1. `foundations/`：底层心智模型与分步搭建指南
2. `companies/`：面向平台与 runtime 设计的公司打法
3. `frameworks/`：LangChain、LangGraph、Deep Agents 的实现层选择指南
4. `experts/`：评测、心智模型和失败分析的补充材料

## Content Principles

- prioritize primary sources
- mix dated engineering posts with current official docs
- prefer explanation and build guidance over article summarization
- keep all core material bilingual
- optimize for operators and builders, not casual readers

- 优先使用一手资料
- 混合使用有日期的工程文章与当前官方文档
- 以讲解和搭建指导为主，而不是做文章摘要
- 核心内容保持中英双语
- 优先服务实际搭建者和操作者，而不是泛阅读用户

## Included In This Redesign

- rewritten `README.md`
- rewritten `knowledge-base/index.md`
- rewritten `knowledge-base/sources.md`
- added `foundations/agent-harness-blueprint.md`
- added `foundations/how-to-build-an-agent-harness.md`
- expanded `companies/openai.md`
- expanded `companies/anthropic.md`
- added `companies/google.md`
- added `frameworks/langchain-langgraph.md`

## Source Strategy

Use original publication dates for blog and research posts when available. Use `Accessed 2026-04-22` for official docs without visible publication dates.

对博客和研究文章保留原始发布日期；对没有明确日期的官方文档，统一使用 `Accessed 2026-04-22`。

## Intended Outcome

After the redesign, a reader should be able to:

- understand what an agent harness is
- choose a suitable starting architecture
- build a first working implementation
- decide when to move from simple loops to longer-running or multi-role systems
- evaluate and harden the harness before production

重构完成后，读者应该能做到：

- 理解 agent harness 到底是什么
- 选择合适的起步架构
- 搭出第一版可运行实现
- 判断何时该从简单 loop 升级到长任务或多角色系统
- 在上线前完成评测和加固
