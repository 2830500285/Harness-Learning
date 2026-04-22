# LangChain / LangGraph / Deep Agents

## Why This Layer Matters

LangChain and LangGraph are useful not because they are “the standard.” They are useful because they expose a fairly clear layering:

- **LangChain Agents** for fast, high-level agent construction
- **LangGraph** for low-level orchestration and durable execution
- **Deep Agents** for a more opinionated harness around long-running agent work

LangChain / LangGraph 有价值，不是因为它们“名气大”，而是因为它们把层次分得相对清楚：

- **LangChain Agents** 负责高层快速搭建
- **LangGraph** 负责底层 orchestration 与 durable execution
- **Deep Agents** 更像面向长任务的高层 harness

## Primary Sources

- `Agents` (Accessed 2026-04-22)
- `LangGraph overview` (Accessed 2026-04-22)
- `Deep Agents overview` (Accessed 2026-04-22)

Links:

- <https://docs.langchain.com/oss/python/langchain/agents>
- <https://docs.langchain.com/oss/python/langgraph/overview>
- <https://docs.langchain.com/oss/python/deepagents/overview>

## The Three Layers

| Layer | Best for | Main value |
| --- | --- | --- |
| LangChain Agents | fastest path to a working tool-using agent | `create_agent`, middleware, state, streaming, structured output |
| LangGraph | custom orchestration and long-running stateful workflows | durable execution, interrupts, memory, deployment control |
| Deep Agents | higher-level coding / research style harnesses | planning, subagents, richer agent operating environment |

## When To Use Which

### Use LangChain Agents when you want speed

The LangChain docs are explicit that `create_agent` is the production-ready high-level entry point, and that it builds on top of LangGraph internally.

LangChain 文档写得很明确：`create_agent` 是高层、可生产使用的入口，而且它底下本来就是 LangGraph runtime。

Choose it when:

- you want a working agent fast
- the standard tool loop is enough
- you want middleware, state, and streaming without building the graph by hand

### Use LangGraph when control matters more than speed

LangGraph positions itself as a low-level orchestration framework and runtime for long-running, stateful agents.

LangGraph 明确把自己定位成“长时间、带状态 agent”的低层 orchestration framework 和 runtime。

Choose it when:

- the default agent loop is not enough
- you need durable execution
- you need human interrupts or resumability
- you need custom nodes and graph edges

### Use Deep Agents when your problem already looks like a harness

Deep Agents is useful when you are not building a chatbot anymore. You are building something closer to a worker that plans, uses tools repeatedly, may delegate, and needs a richer operating environment.

当你的问题已经不像聊天机器人，而更像“一个会规划、反复调工具、可能分派任务的工作体”时，Deep Agents 这层就有意义了。

## How To Build With LangChain Agents

The LangChain agent docs are unusually practical. They show that a modern agent API needs more than model plus tool binding:

- multiple tool calls
- dynamic tool selection
- middleware hooks
- custom state
- structured output
- streaming

LangChain agent 文档最有价值的地方，在于它不是只教你“model 绑定 tools”，而是直接把现代 agent 真正常用的东西都摆出来了：

- 多次 tool 调用
- 动态 tool 选择
- middleware hooks
- 自定义 state
- structured output
- streaming

Minimal pattern:

```python
from langchain.agents import create_agent
from langchain.tools import tool

@tool
def get_weather(location: str) -> str:
    return f"Weather in {location}: Sunny, 72F"

agent = create_agent(
    "openai:gpt-5",
    tools=[get_weather],
    system_prompt="Be concise and accurate."
)

result = agent.invoke(
    {"messages": [{"role": "user", "content": "What's the weather in Shanghai?"}]}
)
```

Why this is useful:

- very small surface area
- gets you onto a graph-based runtime immediately
- easy to add middleware later

## Middleware Is The Real Power Lever

The LangChain docs emphasize middleware for a reason. Middleware is where many harness decisions actually belong.

LangChain 文档会重点讲 middleware，是有原因的。很多真正属于 harness 的决策，本来就应该放在 middleware 里。

Examples:

- dynamic model selection
- filtering tools by permissions or context
- handling tool errors
- injecting dynamic system prompts
- adding logging or policy checks

典型用途：

- 动态切换模型
- 按权限或上下文过滤工具
- 处理 tool error
- 注入动态 system prompt
- 增加日志与策略检查

## How To Build With LangGraph

LangGraph is where you go when the stock loop becomes too small.

当默认 agent loop 已经装不下你的需求时，就该进入 LangGraph。

The overview highlights the four reasons people move down the stack:

- durable execution
- human-in-the-loop
- memory
- production-ready deployment

概括起来就是四个字：更可控、更可恢复。

Minimal graph example from the docs:

```python
from langgraph.graph import StateGraph, MessagesState, START, END

def mock_llm(state: MessagesState):
    return {"messages": [{"role": "ai", "content": "hello world"}]}

graph = StateGraph(MessagesState)
graph.add_node(mock_llm)
graph.add_edge(START, "mock_llm")
graph.add_edge("mock_llm", END)
app = graph.compile()
```

In real harnesses you replace `mock_llm` with:

- model node
- tool node
- evaluator node
- approval node
- persistence node

真实 harness 里，你会把 `mock_llm` 换成：

- model node
- tool node
- evaluator node
- approval node
- persistence node

## How Deep Agents Fits

Deep Agents sits higher than raw LangGraph. Conceptually, it gives you a more opinionated harness for agents that need to plan, operate over files or environments, and potentially delegate.

Deep Agents 站得比纯 LangGraph 更高。它更像一个“已经带了一部分操作系统味道”的 agent harness，适合会规划、会操作文件/环境、甚至会分派子任务的 agent。

This is especially relevant for:

- coding agents
- long-running research agents
- agents that need structured sub-task decomposition

## Recommended Decision Rule

Use this rule of thumb:

1. start with LangChain `create_agent`
2. move to LangGraph when orchestration becomes the problem
3. move to Deep Agents when the whole problem is now “agent operating environment design”

建议用这条升级路线：

1. 先从 LangChain `create_agent` 起步
2. 当 orchestration 成为主要问题时，切到 LangGraph
3. 当问题本质已经变成“agent operating environment design”时，再考虑 Deep Agents

## LangChain / LangGraph Build Checklist

- Start with the highest-level abstraction that still stays legible.
- Keep tools small and schemas explicit.
- Use middleware before rewriting the whole stack.
- Move down to LangGraph only when you need control, resumability, or custom graph structure.
- Add LangSmith or equivalent tracing early.

- 从“最高但仍可读”的抽象层开始。
- 保持工具小而清晰，schema 显式。
- 先用 middleware 解决问题，不要动不动重写整套架构。
- 只有在需要控制力、可恢复性或自定义图结构时，才下沉到 LangGraph。
- 尽早接入 LangSmith 或同类 tracing 能力。

## Common Mistakes

- picking LangGraph first for a problem that a stock agent loop can already solve
- staying in high-level abstractions after orchestration has clearly become the bottleneck
- adding many tools without dynamic filtering or policy checks
- not tracing runs while trying to debug graph behavior

- 明明普通 agent loop 足够，却一上来就选 LangGraph
- orchestration 已经成瓶颈，却还死守高层抽象
- 工具越来越多，却没有动态过滤和权限策略
- 调 graph 行为时不做 tracing
