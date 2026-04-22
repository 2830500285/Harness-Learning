# Google / Gemini / ADK

## Why Google's Stack Matters

Google's stack is valuable because it separates two concerns clearly:

1. **Gemini API** as the model capability layer
2. **ADK** as the orchestration layer for building stateful agents

Google 这套东西最值得学的地方，在于它把两层能力分得比较清楚：

1. **Gemini API** 负责模型和工具能力
2. **ADK** 负责更完整的 agent orchestration

This is a useful mental model for builders:
model capability and agent runtime are related, but they are not the same thing.

这对开发者来说是个很好的心智模型：模型能力层和 agent runtime 层互相关联，但并不是同一回事。

## Primary Sources

- `Function calling with the Gemini API` (Accessed 2026-04-22)
- `Python quickstart` for ADK (Accessed 2026-04-22)
- `Intro to conversational context: Session, State, and Memory` (Accessed 2026-04-22)
- `Why evaluate agents` (Accessed 2026-04-22)

Links:

- <https://ai.google.dev/gemini-api/docs/function-calling>
- <https://adk.dev/get-started/python/>
- <https://adk.dev/sessions/>
- <https://adk.dev/evaluate/>

## How Google Frames The Problem

### 1. Gemini handles tool reasoning

Google's Gemini documentation treats function calling as a native part of the model interaction loop. The docs also describe automatic function calling and compositional calling, which is important for multi-step work.

Gemini 的官方文档把 function calling 当成模型交互里的原生能力，并且明确谈到了 automatic function calling 和 compositional function calling，这对多步任务很关键。

The important lesson is:

- tool calls are part of the reasoning loop
- the model may need several tool steps
- the tool results need stable identities and structured return values

### 2. ADK handles sessions, runtime, and local development flow

ADK is not just a helper library for one function call.

ADK 不是“帮你包一层 function call”的小工具。

It is closer to an agent application framework:

- create agent projects
- run them locally
- inspect them in a web UI
- manage session state
- evaluate agent behavior

## How To Build With Google ADK

### Step 1: Start from the Python quickstart

The ADK quickstart is opinionated in a helpful way. For a first project, the shortest path is:

```bash
pip install google-adk
adk create my_agent
cd my_agent
```

Then configure your API key, typically via `.env`:

```bash
GOOGLE_API_KEY=your_key_here
```

Then use the local commands that ADK documents:

```bash
adk run .
adk web
```

中文理解：

- `adk create` 用来快速生成工程骨架
- `adk run .` 用来本地运行 agent
- `adk web` 提供本地交互和调试界面

### Step 2: Define a small root agent first

A minimal ADK-style agent usually has:

- a name
- a model
- an instruction
- a small tool list

一个最小 ADK agent，通常只需要：

- `name`
- `model`
- `instruction`
- 一小组 tools

Representative pattern:

```python
from google.adk.agents import Agent

def get_weather(city: str) -> dict:
    return {"city": city, "forecast": "sunny", "temp_c": 27}

root_agent = Agent(
    name="weather_agent",
    model="gemini-2.0-flash",
    description="Answers weather questions.",
    instruction="Use the provided tools to answer weather questions accurately.",
    tools=[get_weather],
)
```

The key point is not the weather example. The key point is that ADK expects you to think in terms of an agent runtime with tools, not just a one-off completion call.

### Step 3: Use Gemini function calling when the agent must act

Gemini's function calling guide is the lower-level capability layer under a lot of agent behavior.

Gemini 的 function calling 指南，其实就是很多 agent 行为的底层能力层。

Use it when:

- the model must call internal business functions
- you need multi-step tool composition
- you need the model to inspect results and continue

Important design rule:

- tool outputs must be structured enough for the model to continue correctly

关键设计原则：

- tool 的返回值必须足够结构化，模型才能继续做对

### Step 4: Treat Session, State, and Memory separately

The ADK sessions material is especially useful because it teaches a cleaner vocabulary.

ADK 的 sessions 文档很值得看，因为它把几个常被混用的概念拆开了。

Use this mental split:

- **Session**: the container for one ongoing conversation or task thread
- **State**: mutable structured data associated with that session
- **Memory**: longer-lived context or recall beyond a single turn

建议这样理解：

- **Session**：一次持续任务或对话线程的容器
- **State**：附着在当前 session 上、会变化的结构化数据
- **Memory**：超出单轮范围的更长期上下文

This is important because many weak agent systems throw all three concepts into a single message history and then wonder why resuming, debugging, and evaluation are painful.

### Step 5: Use ADK eval as part of the build loop

Google's ADK eval guidance is stronger than many teams realize. It explicitly treats both the **trajectory** and the **final response** as things worth checking.

Google 的 ADK eval 指南比很多团队平时做的更完整，因为它不仅看最终答案，也看执行 **trajectory**。

That is exactly the right mindset for harness work.

Why this matters:

- an answer can look correct even if the tool path was terrible
- poor trajectories cause latency, cost, and brittleness
- trajectory eval catches misuse before users do

Use eval when:

- changing prompts
- changing tool schemas
- changing models
- changing session persistence logic

ADK documents several ways to run evals, including local workflows, web interfaces, and `pytest`-style execution. That makes it easier to keep eval close to development instead of turning it into a separate research project.

## When Google's Stack Is A Good Fit

Choose Gemini + ADK when:

- you want a documented path from tool calling to full agent runtime
- you want sessions and state to be first-class
- you want a local development loop with agent-specific tooling
- you want eval to sit close to runtime behavior

以下场景尤其适合选 Google 这条线：

- 你希望从 function calling 平滑升级到完整 agent runtime
- 你希望 sessions 和 state 成为一等公民
- 你希望本地开发有明确的 agent 工具链
- 你希望 eval 和实际 runtime 行为连得很近

## Google Build Checklist

- Start from the ADK project skeleton.
- Keep the root agent small before adding more roles.
- Use Gemini function calling for real action loops.
- Persist session and state explicitly.
- Evaluate both trajectory and final response.
- Use the local web and CLI tooling to shorten the debug loop.

- 从 ADK 工程骨架开始。
- 在加更多角色之前，先把 root agent 保持小而清晰。
- 真正需要执行动作时，使用 Gemini function calling。
- 显式持久化 session 和 state。
- 同时评估 trajectory 和最终答案。
- 充分利用本地 web/CLI 工具缩短调试回路。

## Common Mistakes

- treating Gemini function calling as the entire harness
- confusing session state with long-term memory
- evaluating only the final answer
- adding more agents before one root agent is stable

- 把 Gemini function calling 当成完整 harness
- 把 session state 和长期 memory 混为一谈
- 只评最终答案，不看轨迹
- 一个 root agent 还没稳定，就先加更多 agent
