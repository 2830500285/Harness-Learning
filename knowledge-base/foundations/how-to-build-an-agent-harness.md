# How To Build An Agent Harness

This guide is intentionally practical. The goal is not to explain every theory. The goal is to help you build the first version that is actually usable, then improve it in the right order.

这份指南故意偏实操。目标不是讲完所有理论，而是帮你先搭出第一版真正能用的 harness，再按正确顺序把它做强。

## Step 0: Pick A Narrow Job

Do not start with “build a general agent.”

Start with one narrow job that has:

- a clear input
- a clear output
- a small tool set
- an obvious definition of done

不要从“做一个通用 agent”开始。

先选一个狭窄任务，而且它必须具备：

- 明确输入
- 明确输出
- 小而清晰的工具集合
- 明确的完成定义

Good first jobs:

- answer support questions from docs plus account lookup
- edit files in a repo and run tests
- research a topic and return cited notes

## Step 1: Write The Task Contract First

Before coding the loop, write the contract.

在写 runtime loop 之前，先写任务契约。

Minimum contract fields:

```yaml
task_id: string
goal: string
constraints:
  - no destructive writes without approval
  - cite sources when claiming facts
done_when:
  - final answer delivered
  - required checks passed
artifacts:
  - plan.md
  - notes.md
  - final.md
```

Why this matters:

- it gives the model a bounded mission
- it tells the harness what to store
- it defines what verification must happen

## Step 2: Keep State Outside The Model

Do not rely on the conversation alone as your state store.

不要把“聊天上下文”当成唯一状态存储。

Create an explicit state object:

```python
from dataclasses import dataclass, field
from typing import Any

@dataclass
class HarnessState:
    task_id: str
    goal: str
    status: str = "running"
    plan: list[str] = field(default_factory=list)
    tool_history: list[dict[str, Any]] = field(default_factory=list)
    artifacts: dict[str, str] = field(default_factory=dict)
    retries: int = 0
```

This object can live in memory for simple cases, but serious systems should persist it in a database, filesystem, or session service.

这个对象在最简单的系统里可以先放内存；但只要任务稍微长一点，就应该落到数据库、文件系统或 session service 里。

## Step 3: Start With A Small Tool Surface

Weak harnesses often fail because they expose too many tools too early.

很多失败的 harness，不是模型太弱，而是一开始暴露的工具太多。

Start with 2 to 5 tools. For each tool, define:

- what it is allowed to do
- exact input schema
- exact output shape
- retry policy
- whether approval is needed

推荐的第一批工具：

- `search_docs`
- `read_file`
- `run_test`
- `fetch_record`
- `write_draft`

## Step 4: Implement The Loop

Your first harness loop should be boring and explicit.

第一版 harness loop 应该“无聊但清楚”，而不是炫技。

```python
def run_harness(task, state, model, tools):
    while state.status == "running":
        response = model.next_action(task=task, state=state)

        if response.type == "final":
            if verify_final(response, state):
                state.status = "completed"
                return response.output
            state.retries += 1
            continue

        if response.type == "tool_call":
            tool_output = tools.execute(response.name, response.arguments)
            state.tool_history.append(
                {
                    "tool": response.name,
                    "arguments": response.arguments,
                    "output": tool_output,
                }
            )
            continue

        if response.type == "artifact":
            save_artifact(response.path, response.content)
            state.artifacts[response.path] = response.path
            continue

        raise ValueError(f"Unexpected response type: {response.type}")
```

This loop is deliberately simple. Add complexity only when a real failure mode forces it.

这个循环故意保持简单。只有在真实失败模式出现时，才继续加复杂度。

## Step 5: Persist Artifacts Early

Persist these from day one:

- the current plan
- working notes
- retrieved evidence
- generated output drafts
- evaluator report

从第一天开始，就把下面这些东西落盘：

- 当前计划
- 工作笔记
- 检索证据
- 生成中的草稿
- evaluator 报告

Why:

- you can resume tasks
- you can inspect failure modes
- you can hand work to another agent or human

## Step 6: Add Verification Before Fancy Autonomy

Add at least one hard check before expanding autonomy.

在扩大自治范围之前，先加至少一个硬验证点。

Examples:

- run tests after code edits
- require citations for research outputs
- validate JSON against schema
- compare extracted values against source text
- run a separate evaluator model

常见做法：

- 改代码后必须跑测试
- 研究结果必须附引用
- JSON 必须过 schema 校验
- 抽取值要和原始文本对照
- 增加独立 evaluator 模型

## Step 7: Add Human Approval Gates

Good approval gates are narrow and concrete.

好的审批门槛应该是窄而具体的。

Examples:

- before deleting files
- before sending external email or message
- before executing payments or purchases
- before merging to a protected branch

If you ask for approval on every tiny action, you destroy flow.
If you ask for approval on nothing, you create risk.

如果每个小动作都要审批，系统就没法流畅工作。
如果什么都不审批，风险就会失控。

## Step 8: Add Observability

At minimum, capture:

- start time / end time
- model calls
- tool calls
- artifacts produced
- verification outcomes
- failure reasons

最少要采集：

- 开始时间 / 结束时间
- model 调用
- tool 调用
- 产出的 artifacts
- verification 结果
- 失败原因

For small systems, a structured log file is enough.
For larger systems, use traces and a dedicated observability stack.

小系统用结构化日志就够。
大系统要上 trace 和专门的可观测平台。

## Step 9: Build An Eval Harness

Do not wait for production incidents to discover your harness is drifting.

不要等到生产事故才发现 harness 在漂移。

Build a small eval set:

- 20 to 50 representative tasks
- expected outputs or grading rules
- failure tags such as hallucination, tool misuse, timeout, bad formatting

做一套小型 eval 集：

- 20 到 50 个代表性任务
- 期望输出或打分规则
- 失败标签，例如 hallucination、tool misuse、timeout、格式错误

Run evals:

- before major prompt changes
- before tool schema changes
- before model upgrades
- before permission policy changes

## Step 10: Only Then Add Multi-Agent Structure

Move to planner / worker / evaluator or other multi-agent structures only after the single-agent loop has clear failure signatures.

只有当单 agent loop 的失败模式已经很清楚时，才升级到 planner / worker / evaluator 之类的多 agent 结构。

Good upgrade triggers:

- one agent keeps losing the global plan
- evaluation quality is biased because the same agent wrote and graded
- different tools require separate permission or context domains

## A Good First-Week Implementation Plan

### Day 1

- define the task contract
- implement 2 tools
- build the first loop

### Day 2

- persist state and artifacts
- add one verification check

### Day 3

- add logs and traces
- create 10 to 20 eval tasks

### Day 4

- tighten prompts, schemas, and tool docs
- fix top failure modes

### Day 5

- decide whether you need long-running sessions, human approval, or multiple roles

## What To Avoid

- building for every use case before one use case works
- exposing a huge tool catalog at the start
- keeping all context inside one massive chat thread
- adding multi-agent complexity before evals exist
- measuring success only by demo quality

- 还没跑通一个用例就试图覆盖所有用例
- 一开始就给模型暴露超大工具集
- 把所有上下文都塞进一条超长对话
- 还没有 eval 就先上多 agent 架构
- 只看 demo 漂不漂亮，不看稳定性

## Final Rule

The right build order is usually:

task contract -> small tool loop -> explicit state -> artifacts -> verification -> observability -> evals -> more autonomy

正确的搭建顺序通常是：

任务契约 -> 小工具循环 -> 显式状态 -> artifacts -> verification -> observability -> evals -> 更强自治
