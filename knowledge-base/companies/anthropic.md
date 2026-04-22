# Anthropic / Claude

## Overview

Anthropic's strongest contribution in this set is clarity:
start simple, distinguish workflows from agents, and treat harness design as a first-class performance lever.

Anthropic 在这批资料里的最大价值是“清晰”：
先从简单模式开始，区分 workflow 与 agent，并把 harness 设计视为决定性能上限的一等公民。

## 1. Building effective agents

- Date: 2024-12-19
- Link: <https://www.anthropic.com/research/building-effective-agents>

### English Summary

Anthropic argues that the best production systems usually rely on simple, composable patterns instead of heavy abstractions. The article draws a useful distinction between **workflows** and **agents**: workflows follow predefined code paths, while agents dynamically decide how to use tools and sequence work. A recurring recommendation is to start with the simplest possible LLM system and only add agentic complexity when the task truly requires it.

### 中文摘要

Anthropic 强调，真正效果好的生产系统往往不是最“框架化”的，而是由简单、可组合的模式构成。文章把 **workflow** 和 **agent** 做了清晰区分：workflow 走预定义流程，agent 则会动态决定如何用工具、如何安排步骤。最重要的建议是：先做最简单可行方案，只有在任务确实需要时，再增加 agent 化复杂度。

### Takeaways / 可执行启发

- Prefer direct primitives over magical abstractions. / 优先用清晰原语，不要一开始就堆抽象层。
- Decide whether you need a workflow or a true agent. / 先判断你要的是 workflow 还是真正的 agent。
- Complexity is a cost center unless it unlocks measurable capability. / 复杂度本身是成本，除非它带来可衡量收益。

## 2. Harness design for long-running application development

- Date: 2026-03-24
- Link: <https://www.anthropic.com/engineering/harness-design-long-running-apps>

### English Summary

This article pushes the conversation beyond basic tool loops. Anthropic describes harness design as central to frontier agentic coding, especially for long-running work like frontend generation and autonomous application building. The post highlights multi-agent structures such as **planner -> generator -> evaluator**, and emphasizes structured artifacts to preserve context between sessions instead of relying on one giant conversational thread.

### 中文摘要

这篇文章把讨论从“基础工具循环”推进到了“前沿 harness 设计”。Anthropic 认为，在前端生成和长时间 autonomous 应用开发这种任务中，真正决定上限的不是单纯模型能力，而是 harness 的结构。文中重点介绍了 **planner -> generator -> evaluator** 这样的多 agent 架构，并强调用结构化工件在会话之间传递上下文，而不是把所有内容都塞进一条超长对话里。

### Takeaways / 可执行启发

- For long tasks, use handoff artifacts instead of one infinite chat. / 长任务要靠交接工件，不要指望一条无限长对话。
- Separate planning, generation, and evaluation roles. / 把规划、生成、评估角色拆开。
- Build for multi-hour continuity, not only single-run demos. / harness 要服务多小时连续工作，而不是一次性演示。

## 3. New capabilities for building agents on the Anthropic API

- Date: 2025-05-22
- Link: <https://claude.com/blog/agent-capabilities-api>

### English Summary

Claude's platform update is notable because it moves agent plumbing into the platform layer. The post introduces a code execution tool, MCP connector, Files API, and extended prompt caching. The practical point is not just new features, but reduced integration burden: the platform can manage MCP connections, tool discovery, and error handling automatically, allowing developers to focus more on product logic than harness boilerplate.

### 中文摘要

这次平台更新的重要意义，不只是“多了几个功能”，而是把很多 agent 基础设施下沉到了平台层。文章介绍了代码执行工具、MCP connector、Files API 和更长时间的 prompt caching。它的真正价值在于降低集成成本：平台可以替你处理 MCP 连接、工具发现和错误处理，开发者就能把更多精力放在产品逻辑，而不是重复造 harness 基础设施。

### Takeaways / 可执行启发

- Push generic harness work into the platform whenever possible. / 通用 harness 能交给平台就交给平台。
- Treat files, code execution, and MCP as native agent building blocks. / 把文件、代码执行、MCP 当作 agent 原生能力。
- Long-running context is partly a product problem and partly a caching problem. / 长时上下文既是产品问题，也是缓存问题。

## Anthropic Pattern Summary

Anthropic's pattern can be summarized in one sentence:

> simple primitives first, structured coordination second, long-running continuity third.

Anthropic 的方法可以浓缩成一句话：

> 先用简单原语，再做结构化协作，最后解决长时连续性。
