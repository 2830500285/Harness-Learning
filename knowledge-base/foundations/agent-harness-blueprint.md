# Agent Harness Blueprint

## What A Harness Actually Is

An agent harness is the part of the system that decides:

- what the model can see
- what the model can do
- what the model can remember
- how the model proves it did the work correctly
- when the model must stop, ask, retry, or hand off

Agent harness 的本质，是一套决定下面这些事情的工程系统：

- 模型能看到什么
- 模型能做什么
- 模型能记住什么
- 模型如何证明自己做对了
- 模型什么时候必须停止、请求确认、重试或交接

If you only have a prompt and a model, you do not yet have a serious harness.

如果你只有 prompt 和 model，那还不算真正意义上的 harness。

## The Eight Building Blocks

| Block | What it does | If missing, you get | Minimum viable version |
| --- | --- | --- | --- |
| Task contract | Defines the job, success criteria, and stop conditions | wandering, over-execution, unclear outputs | one typed task object plus explicit done criteria |
| Runtime loop | Controls think -> act -> observe -> continue | brittle one-shot behavior | a loop that can process tool calls and resume |
| Tools and environment | Gives the agent safe access to the outside world | hallucinated actions or fake certainty | small tool set with stable schemas |
| State and memory | Preserves short-term and cross-session continuity | repeated work, dropped context, confusion | explicit session state plus artifact storage |
| Artifacts | Stores plans, notes, diffs, reports, and handoff material | all context trapped in chat history | markdown / json artifacts written between steps |
| Verification | Checks whether the work is correct | plausible but wrong outputs | tests, evaluators, or consistency checks |
| Observability | Lets operators inspect what happened | impossible debugging | traces, tool logs, timings, and final status |
| Safety and approvals | Limits risky actions and escalates when needed | unsafe automation | permission model plus approval gates |

## The Smallest Useful Harness

The smallest useful harness is usually not multi-agent.

It is usually:

1. one model
2. one runtime loop
3. two to five tools
4. one state object
5. one verification step
6. one trace/log stream

最小可用 harness 通常并不是多 agent。

它通常是：

1. 一个模型
2. 一个 runtime loop
3. 两到五个工具
4. 一个明确的 state 对象
5. 一个验证步骤
6. 一条 trace 或 log 流

This is one of the most repeated patterns across Anthropic, OpenAI, Google ADK, and LangGraph: do not jump to complex coordination before you have a clean single-agent baseline.

Anthropic、OpenAI、Google ADK 和 LangGraph 的材料里都在反复强调一件事：在拿到干净的单 agent 基线之前，不要过早跳进复杂协作。

## Workflow vs Agent vs Multi-Agent

### Workflow

Use a workflow when the execution path is mostly known in advance.

当执行路径大部分已知时，用 workflow。

Typical examples:

- classify -> route -> summarize
- extract -> transform -> validate
- fetch data -> fill template -> send output

### Single Agent

Use a single agent when tool choice or order must be decided dynamically at runtime, but the task is still contained enough for one worker to own.

当工具选择和执行顺序需要运行时动态决定，但任务范围还足够集中时，用单 agent。

Typical examples:

- a research assistant with search + browse + note tools
- a coding agent with file + shell + test tools
- a customer support agent that can inspect account state and draft a response

### Multi-Agent

Use multi-agent coordination only when you can clearly name the different responsibilities.

只有当你能明确说出不同角色的责任边界时，才该使用多 agent。

Good reasons:

- planner and executor need different context windows or prompts
- evaluator must be isolated from generator bias
- specialized tools belong to separate security or permission domains
- work has natural handoff artifacts between phases

Bad reasons:

- “multi-agent sounds more advanced”
- one messy prompt stopped working
- you want to hide weak task design behind more architecture

## The Core Control Loop

Every serious harness eventually needs a version of this loop:

```text
receive task
load state and artifacts
ask model for next action
execute tool or produce artifact
record observation
verify intermediate result when needed
continue, pause, or finish
```

任何可持续运行的 harness，最终都绕不开下面这个控制循环：

```text
接收任务
加载 state 和 artifacts
询问模型下一步动作
执行工具或生成产物
记录 observation
在关键节点验证中间结果
继续、暂停或结束
```

The point is not “let the model think longer.” The point is to make the agent operate inside a controlled system where actions, observations, and state changes are explicit.

重点不是“让模型多想一会儿”，而是把 agent 放进一个受控系统里，让动作、观察和状态变化全部显式化。

## Artifacts Matter More Than People Expect

For long-running tasks, artifacts are often more important than raw chat history.

Common artifact types:

- task brief
- current plan
- working notes
- retrieved evidence
- generated files or patches
- evaluator report
- final delivery summary

在长时间任务里，artifacts 往往比聊天记录本身更重要。

常见 artifacts 包括：

- 任务说明
- 当前计划
- 工作笔记
- 检索到的证据
- 生成的文件或补丁
- evaluator 报告
- 最终交付总结

Why this matters:

- artifacts survive model swaps
- artifacts survive handoffs
- artifacts are inspectable by humans
- artifacts create resumability

它们重要的原因是：

- artifacts 能跨模型保留
- artifacts 能跨角色交接
- artifacts 能被人检查
- artifacts 让任务可恢复

## Verification Is Part Of The Harness, Not A Separate Phase

Many weak agent systems treat verification as an afterthought.

Strong harnesses bake verification into the loop itself.

弱 agent 系统常把 verification 当作最后补的一层。

强 harness 会把 verification 直接写进主循环。

Examples:

- run tests before returning code
- compare extracted values against schema constraints
- ask a separate evaluator to score plan quality
- require a citation check before delivering research
- force a dry run before making an external write

## What To Log

At minimum, log:

- task id
- model and major parameters
- tool calls and outputs
- artifact paths or ids
- retries and errors
- verification results
- final status

至少记录：

- task id
- 模型与关键参数
- tool 调用及返回值
- artifact 路径或 id
- 重试和错误
- verification 结果
- 最终状态

If you cannot explain why the agent made a decision, your harness is not production-ready.

如果你解释不清 agent 为什么做出某个决策，那这个 harness 就还没准备好进生产。

## A Good Starter Repo Shape

```text
agent-harness/
  app/
  prompts/
  tools/
  sessions/
  artifacts/
  evals/
  traces/
  tests/
```

Recommended meaning:

- `app/`: orchestration code, runtime loop, APIs
- `prompts/`: reusable system prompts and role instructions
- `tools/`: tool adapters and schemas
- `sessions/`: persisted task/session state
- `artifacts/`: plans, notes, outputs, reports
- `evals/`: regression tasks and grading logic
- `traces/`: execution traces or exported logs
- `tests/`: unit and system tests

## Blueprint Checklist

Use this before you call something a real harness.

在你把某个系统称为“真正的 harness”之前，至少过一遍这个清单。

- The task contract is explicit.
- Tool inputs and outputs have schemas.
- State lives outside the model.
- Important artifacts are persisted.
- There is at least one verification gate.
- Operators can inspect traces and failures.
- Risky actions require approvals or scoped permissions.
- The system can resume or retry without losing everything.

- 任务契约是显式的。
- 工具输入输出有 schema。
- 状态不只存在于模型脑内。
- 关键 artifacts 被持久化。
- 至少有一个 verification gate。
- 操作员能查看 traces 和失败原因。
- 高风险动作有审批或权限边界。
- 失败后系统能恢复或重试，而不是全部重来。
