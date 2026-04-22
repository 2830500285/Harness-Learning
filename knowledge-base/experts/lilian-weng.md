# Lilian Weng

## Source

- Title: LLM Powered Autonomous Agents
- Date: 2023-06-23
- Link: <https://lilianweng.github.io/posts/2023-06-23-agent/>

## English Summary

Lilian Weng's article remains one of the best foundational maps of agent systems. It frames an LLM-powered agent around three major components:

- planning
- memory
- tool use

It also discusses self-reflection patterns and the core limitations of current systems, especially context length, long-horizon planning, and the brittleness of natural-language interfaces.

## 中文摘要

Lilian Weng 这篇文章依然是理解 agent 系统最经典的基础地图之一。她把 LLM agent 的核心组成概括为三部分：

- planning
- memory
- tool use

同时，文章也讨论了自反思模式，以及当前系统的几个关键限制，尤其是上下文长度、长程规划能力，以及自然语言接口本身的不稳定性。

## Why It Still Matters

Many newer harness discussions are easier to understand after this article, because it provides the conceptual skeleton behind later engineering work.

很多更新的 harness 讨论，读完这篇之后会更容易理解，因为它提供了后续工程实践背后的概念骨架。

## Actionable Takeaways

- Use this note as a conceptual baseline, not a production recipe. / 把这篇当作概念底图，而不是生产配方。
- Separate planning, memory, and tools when designing your system. / 设计系统时，把规划、记忆和工具分开思考。
- Always ask which bottleneck is fundamental versus harness-induced. / 要分清问题是模型根本限制，还是 harness 设计导致的。
