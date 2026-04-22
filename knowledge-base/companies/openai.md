# OpenAI

## Overview

OpenAI's strongest contribution in this set is runtime design:
how to build an agent-friendly execution layer instead of a thin prompt wrapper.

OpenAI 在这批资料里的突出价值，是运行时设计：
不是只做一层 prompt 包装，而是构建真正适合 agent 的执行层。

## 1. Harness engineering: leveraging Codex in an agent-first world

- Date: 2026-02-11
- Link: <https://openai.com/index/harness-engineering/>

### English Summary

The core idea of this piece is that, in agentic coding, performance depends heavily on the system around the model. "Harness engineering" is framed as the work of improving documentation, execution boundaries, feedback signals, verification, and workflow support so the model can act reliably. The emphasis is pragmatic: better surrounding infrastructure can unlock more value than prompt tweaks alone.

### 中文摘要

这篇文章的核心观点是：在 agentic coding 场景里，真正决定结果的往往不是模型本身，而是模型周围那一整套系统。所谓 "harness engineering"，本质上是在优化文档、执行边界、反馈信号、验证机制和工作流支持，让模型能更稳定地行动。它强调的是非常务实的工程判断：单靠 prompt 微调很难持续拉开差距，真正的提升来自更强的周边基础设施。

### Takeaways / 可执行启发

- Treat docs, verification, and observability as model multipliers. / 文档、验证和可观测性都是模型放大器。
- Build systems the agent can inspect, not just systems the human understands. / 系统既要让人懂，也要让 agent 容易检查。
- Harness quality is part of product quality. / harness 质量本身就是产品质量的一部分。

## 2. Unlocking the Codex harness: how we built the app server

- Date: 2026-02-04
- Link: <https://openai.com/index/unlocking-the-codex-harness/>

### English Summary

This article focuses on the bridge between the agent and a live application environment. The app server idea is important because agents fail when state is ambiguous or interfaces are too brittle. A strong harness provides clean environment access, explicit schemas, and stable tool contracts so the model can reason over real application state instead of guessing.

### 中文摘要

这篇文章重点讲的是 agent 与真实应用环境之间的那座桥。很多 agent 失败，不是因为不会推理，而是因为环境状态不清晰、接口太脆弱。一个好的 harness 应该给模型提供干净的环境访问方式、明确的 schema 和稳定的工具契约，让模型可以基于真实应用状态做判断，而不是靠猜。

### Takeaways / 可执行启发

- Reduce hidden state between your app and the agent. / 减少应用和 agent 之间的隐式状态。
- Stable tool contracts beat clever prompt workarounds. / 稳定的工具契约比花哨的 prompt 补丁更重要。
- The agent should observe the product through a reliable server-side interface. / agent 应通过可靠的服务端接口观察产品状态。

## 3. Why we built the Responses API

- Date: 2025-09-22
- Link: <https://developers.openai.com/blog/responses-api>

### English Summary

The Responses API article explains a runtime choice: move from a message-centric interface to a more stateful, multimodal, tool-native interface. OpenAI highlights polymorphic output items, server-side hosted tools, preserved internal reasoning, and better cache efficiency. The broader lesson is that agent harnesses improve when the runtime itself understands tools, state, and multi-step execution as first-class concepts.

### 中文摘要

Responses API 这篇文章解释的是一种运行时取舍：从“消息中心”的接口，转向更有状态、更原生支持工具、更适合多模态和多步骤执行的接口。OpenAI 强调了多态输出项、服务端托管工具、内部保留推理过程以及更好的缓存效率。更大的启发在于：如果运行时本身就把工具、状态和多步执行当作一等概念，agent harness 的质量会明显提升。

### Takeaways / 可执行启发

- A good agent API should model actions, not just chat turns. / 好的 agent API 应该表达动作，而不只是聊天轮次。
- Server-side hosted tools can reduce latency and integration glue. / 服务端托管工具能减少延迟和胶水代码。
- Stateful runtimes outperform stateless wrappers on long tasks. / 在长任务里，有状态运行时通常优于无状态封装。

## OpenAI Pattern Summary

OpenAI's pattern can be summarized as:

> move agent logic down into the runtime, then make the runtime tool-native and state-aware.

OpenAI 的路线可以概括为：

> 把 agent 关键能力下沉到运行时，再让运行时天然理解工具、状态和多步执行。
