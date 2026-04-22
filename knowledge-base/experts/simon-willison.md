# Simon Willison

## Source

- Title: How coding agents work
- Date: 2026-03-16
- Link: <https://simonwillison.net/guides/agentic-engineering-patterns/how-coding-agents-work/>

## Why This Note Matters

Simon gives one of the cleanest mental models in the field:

> a coding agent is a harness for an LLM.

Simon 给出了这个领域里最清晰的心智模型之一：

> coding agent 本质上就是 LLM 的一个 harness。

## English Summary

The article breaks a coding agent into understandable parts: chat-templated prompts, system prompts, tool definitions, tool execution loops, token caching, and reasoning. The value of the piece is not novelty but clarity. It explains that the "agent" is rarely magic: it is usually an LLM plus prompts plus tools plus a loop that keeps feeding observations back into the model.

## 中文摘要

这篇文章把 coding agent 拆成了一套非常易懂的部件：聊天模板化提示、system prompt、工具定义、工具执行循环、token caching 和 reasoning。它的价值不在于提出全新理论，而在于把本来容易被神秘化的 agent 解释清楚。所谓 "agent"，很多时候并不神秘，本质上就是 LLM + prompt + tools + 把观察结果不断喂回模型的循环。

## Key Ideas

- The harness exposes callable tools to the model.
- The harness executes those tools and feeds results back.
- The system prompt quietly shapes almost all behavior.
- Token economics matter because long agent sessions get expensive.

- harness 把可调用工具暴露给模型。
- harness 执行工具，再把结果回灌给模型。
- system prompt 对行为的塑造作用非常大。
- token 经济性不能忽视，因为长会话 agent 成本会快速上升。

## Actionable Takeaways

- When debugging an agent, inspect the loop before blaming the model. / 调试 agent 时，先检查循环和工具链，不要先怪模型。
- Make tool behavior explicit and machine-readable. / 工具行为要明确，且最好让机器可读。
- Preserve cache-friendly history structure when possible. / 尽量保持利于缓存的历史结构。
