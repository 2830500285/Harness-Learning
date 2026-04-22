# Knowledge Base Index

## One-Line Definition

An agent harness is the layer that turns a raw model into a usable system.

Agent harness 是把“裸模型”变成“可用系统”的那一层工程能力。

## Topic Map

### 1. Runtime and Tool Loop

Key question:
How does the model call tools, observe results, and continue work safely?

核心问题：
模型如何安全地调用工具、读取结果并持续推进任务？

Recommended notes:

- `companies/openai.md`
- `experts/simon-willison.md`

### 2. Long-Running Agent Work

Key question:
How do we keep context coherent across long sessions and multiple handoffs?

核心问题：
如何在长时间运行、多轮交接的任务中保持上下文连贯？

Recommended notes:

- `companies/anthropic.md`
- `companies/openai.md`

### 3. App and Product Integration

Key question:
How do we connect agents to real products, files, MCP servers, and external systems?

核心问题：
如何把 agent 连接到真实产品、文件、MCP 服务和外部系统？

Recommended notes:

- `companies/openai.md`
- `companies/anthropic.md`

### 4. Evals and Feedback Loops

Key question:
How do we know the harness is improving instead of just getting more complex?

核心问题：
如何确认 harness 真在变好，而不是只是变复杂？

Recommended notes:

- `experts/eugene-yan.md`
- `experts/hamel-husain.md`

### 5. Agent Mental Models

Key question:
What are the conceptual building blocks behind agent systems?

核心问题：
agent 系统背后的基础心智模型是什么？

Recommended notes:

- `experts/lilian-weng.md`
- `experts/simon-willison.md`
- `companies/anthropic.md`

## Reading Paths

### Path A: Beginner

1. `companies/anthropic.md`
2. `experts/simon-willison.md`
3. `experts/lilian-weng.md`

### Path B: Builder

1. `companies/openai.md`
2. `companies/anthropic.md`
3. `experts/eugene-yan.md`
4. `experts/hamel-husain.md`

### Path C: Long-Running Autonomous Coding

1. `companies/anthropic.md`
2. `companies/openai.md`
3. `experts/simon-willison.md`
4. `experts/eugene-yan.md`

## Core Thesis

The strongest pattern across these sources is consistent:

- model quality matters
- but harness quality often matters more in production

这些资料共同指向一个核心判断：

- 模型能力当然重要
- 但在生产环境里，harness 的设计往往更决定上限

## Source Catalog

See `sources.md` for the full first-batch source list.

完整首批来源清单见 `sources.md`。
