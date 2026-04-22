# Hamel Husain

## Source

- Title: Evals Skills for Coding Agents
- Date: 2026-03-02
- Link: <https://hamel.dev/blog/posts/evals-skills/>

## English Summary

Hamel's argument is that modern coding agents need more than tools and traces: they need evaluation skills. The article connects harness engineering with product evals and shows that an agent should know how to audit an eval pipeline, analyze failures, write judge prompts, validate evaluators, and support human review. The important shift is from "the agent can do things" to "the agent can tell whether its work is good."

## 中文摘要

Hamel 的核心观点是：现代 coding agent 不只是要会调用工具、读取 traces，还必须具备评测能力。文章把 harness engineering 与 product evals 连接起来，说明一个 agent 应该知道如何审计 eval pipeline、分析失败案例、编写 judge prompt、校准 evaluator，以及支持人工复核。这里最重要的转变是：不只是让 agent "会做事"，而是让它 "知道自己做得好不好"。

## Why It Matters

Without evals, harness improvements are hard to trust.

如果没有 evals，harness 的“优化”很难真正可信。

## Actionable Takeaways

- Add failure analysis to your harness, not only success demos. / harness 要有失败分析，不要只展示成功案例。
- Teach agents to use eval platforms meaningfully, not just connect to them. / 不要只让 agent 连上 eval 平台，还要让它会正确使用。
- Observability without judgment is incomplete. / 只有观测没有判断，体系是不完整的。
