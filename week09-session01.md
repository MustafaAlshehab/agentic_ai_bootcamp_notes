# Introduction to Agentic AI: Concepts, Components & LangChain Tools

## TL;DR
This session introduced agentic AI as the evolution beyond generative AI — where LLMs gain the ability to **act** (call tools, APIs, functions) in addition to generating text. The instructor covered the six characteristics of AI agents, the five core components (brain, tools, memory, orchestrator, supervisor), ran a hands-on breakout exercise on hiring automation to illustrate autonomy and human-in-the-loop, and then demonstrated how to create custom tools in LangChain using three techniques.

## Topics Covered
- Generative AI vs. AI agents (generate vs. act)
- ReAct model (Reasoning + Acting)
- Single-agent vs. multi-agent systems
- Alexa as an agentic AI analogy
- Six characteristics of AI agents: autonomy, goal-oriented, planning, reasoning, adaptability, context awareness
- Five components of AI agents: brain, tools, memory, orchestrator, supervisor
- Breakout exercise: automating a hiring workflow
- Biases, ethics, and the need for human-in-the-loop
- LangChain built-in tools (DuckDuckGo Search, YouTube Search)
- Three techniques for creating custom tools in LangChain
- Tool binding and toolkits
- Preview: LangChain practical project with ReAct agent (next session)

## Key Concepts Explained

### Generative AI vs. AI Agents
- Generative AI uses an LLM to **generate** answers from its training data or provided context, but it cannot perform actions in the real world.
- An AI agent wraps an LLM with **tools** so it can also **act** — book tickets, call APIs, execute code, etc.
- The combination of reasoning (LLM) and acting (tools) is called the **ReAct** pattern.

### Single-Agent vs. Multi-Agent Systems
- A single agent can call multiple tools on its own.
- In a multi-agent system, different agents handle different domains (e.g., one for YouTube, one for weather). You decide the split based on the use case.
- The overall workflow — whether single or multi-agent — is called an **agentic AI workflow**.

### Autonomy & Human-in-the-Loop
- Full automation (100% AI-driven) is risky: biases in hiring, hallucinated decisions, uncontrolled spending, etc.
- **Human-in-the-loop** adds checkpoints where a human must approve before the workflow proceeds.
- **Guardrails** are hard-coded rules or ethical boundaries the agent must always follow regardless of LLM output.
- The instructor emphasized: "There are many scenarios where you need to make your agents pause or stop so that you can intervene."

### Planning & Reasoning
- **Planning** = breaking a goal into sub-goals, generating candidate plans, evaluating each plan on efficiency/cost/risk, and selecting tools.
- **Reasoning** = interpreting information, drawing conclusions, and making decisions during execution. Includes deciding when to involve humans and how to handle errors (failure & recovery).

### Adaptability
- Agents must be flexible enough to switch strategies when a plan isn't working (e.g., switching from LinkedIn posting to headhunters).
- The **goal never changes** for a given workflow — only the strategy adapts.

### Context Awareness
- The agent must continuously track progress, tool responses, and whether the workflow is still aligned with the goal.

### Components of an AI Agent
- **Brain**: the LLM — handles goal interpretation, planning, reasoning, tool selection, and communication.
- **Tools**: APIs and functions the agent can invoke.
- **Memory**: short-term (active chat session, recent messages) and long-term (goal, past interactions).
- **Orchestrator**: the framework executing the workflow (e.g., LangGraph) — handles task sequencing, retries, and delegation.
- **Supervisor**: the top-level node that receives user input, interprets intent, and delegates to the correct agent/tool. Combines the agent brain + human-in-the-loop.

### Custom Tools in LangChain (Three Techniques)
1. **`@tool` decorator** (simplest) — add `@tool` above a Python function and include a docstring. The function becomes a `StructuredTool` object invoked via `.invoke()`.
2. **`StructuredTool.from_function()`** — pass a regular function plus name, description, and an argument schema (Pydantic `BaseModel`).
3. **`BaseTool` subclass** (most complex) — define a class inheriting from `BaseTool` with `name`, `description`, `args_schema`, and a `_run` method.

### Tool Binding & Toolkits
- **Tool binding**: combining multiple tools into a list and passing them to the agent executor alongside the LLM.
- **Toolkit**: a named collection of related tools (like a plumber's toolbox), useful when a use case involves many tools.

## Tools, Libraries & Commands Mentioned

| Item | Notes |
|------|-------|
| LangChain | Framework for building LLM-powered applications; used for generative AI |
| LangGraph | Evolved from LangChain; designed specifically for agentic AI workflows (orchestrator) |
| CrewAI | Multi-agent framework to be covered in a future session |
| n8n | Workflow automation platform to be covered later |
| DuckDuckGo Search | Built-in LangChain tool (`DuckDuckGoSearchRun`) for web search |
| YouTube Search | Built-in LangChain tool for searching YouTube videos |
| `langchain_core.tools` | Module containing `StructuredTool` and `BaseTool` classes |
| `langchain_community.tools` | Module containing built-in tools and the `@tool` decorator |
| Groq (API) | Cloud LLM inference; used in the Alexa-like mock example |
| Google Colab | Environment used for running the notebook exercises |
| AWS EC2 | Mentioned as deployment target for agentic apps |

## Code & Examples

### Converting a function to a tool with `@tool`

```python
from langchain_core.tools import tool

@tool
def sum(a: int, b: int) -> int:
    """This tool tries to add two numerical inputs."""
    return a + b

# sum(5, 6)  # This will NOT work — sum is no longer a plain function
result = sum.invoke({"a": 5, "b": 6})  # Correct way to call a tool
print(result)       # 11
print(sum.name)     # "sum"
print(sum.description)  # "This tool tries to add two numerical inputs."
print(sum.args)     # {'a': ..., 'b': ...}
```

### Creating a tool with `StructuredTool`

```python
from langchain_core.tools import StructuredTool
from pydantic import BaseModel

class MultiplyInput(BaseModel):
    a: int  # First number
    b: int  # Second number

def multiply_function(a: int, b: int) -> int:
    return a * b

multiply_tool = StructuredTool.from_function(
    func=multiply_function,
    name="multiply_tool",
    description="Multiplies two numbers",
    args_schema=MultiplyInput,
)

print(multiply_tool.invoke({"a": 3, "b": 5}))  # 15
```

### Tool binding with an agent executor (conceptual)

```python
from langchain_xai import ChatXAI

llm = ChatXAI(model="grok-4-latest")
tools = [sum, multiply_tool]

# In the agent executor, pass both:
# agent_executor = create_agent(model=llm, tools=tools)
```

### Using built-in DuckDuckGo Search

```python
from langchain_community.tools import DuckDuckGoSearchRun

search_tool = DuckDuckGoSearchRun()
results = search_tool.invoke("latest AI news")
print(results)
print(search_tool.name)         # "duckduckgo_search"
print(search_tool.description)  # wrapper description
```

## Q&A / Discussion Points
- **"Where does workflow fit in the characteristics?"** — Under planning; planning defines the workflow/execution sequence.
- **"Next to APIs for tools, what is F for?"** — Functions. Tools are APIs and/or Python functions decorated with `@tool`.
- **"Humans to feed in the job specification, AI to post it?"** — The instructor confirmed this is the right idea; the discussion was about fully automated flow first, then identifying where humans are essential.
- **Breakout room discussion highlights**: Bias (gender, age, nationality proxy discrimination via resumes), applicants gaming AI with AI-generated resumes, overly rigorous screening, lack of soft-skill/culture-fit assessment, uncontrolled ad spend, and the dehumanizing nature of a fully automated hiring pipeline.

## Action Items & Assignments
- [ ] Download the Week 9 notebook from the shared drive and run it in Google Colab
- [ ] Explore additional built-in LangChain tools (Wikipedia, Google Search, Brave, file system, etc.)
- [ ] Prepare for tomorrow's session: hands-on LangChain project building an Alexa-like multi-tool agent (weather, YouTube, currency exchange)

## Open Questions / Unclear Spots
- The instructor mentioned "LangChain community tools import Tool" initially but corrected to `langchain_core.tools` — the exact import path for `@tool` may vary between LangChain versions.
- The distinction between orchestrator and supervisor was introduced but not deeply elaborated; more detail expected in the LangGraph sessions.

## Flashcards

**Q: What is the key difference between generative AI and an AI agent?**
A: Generative AI can only generate/answer; an AI agent can also **act** by calling tools, APIs, and functions to perform real-world tasks.

**Q: What does the ReAct pattern stand for, and what does it combine?**
A: **Re**asoning + **Act**ing. The LLM handles reasoning (thought process) and tools handle acting (executing functions/APIs).

**Q: Name the five components of an AI agent.**
A: Brain (LLM), Tools (APIs/functions), Memory (short-term & long-term), Orchestrator (e.g., LangGraph), and Supervisor (intent routing + human-in-the-loop).

**Q: What is the difference between short-term and long-term memory in an AI agent?**
A: Short-term memory holds the active session context and recent messages (lost between sessions). Long-term memory stores the goal and past interactions (persists across sessions).

**Q: What are the three techniques for creating custom tools in LangChain?**
A: (1) `@tool` decorator on a function, (2) `StructuredTool.from_function()` with a schema, (3) subclassing `BaseTool` with a `_run` method.

**Q: Why does calling `sum(5, 6)` fail after adding the `@tool` decorator?**
A: Because `@tool` converts the function into a `StructuredTool` object. You must use `sum.invoke({"a": 5, "b": 6})` instead.

**Q: What three attributes does every LangChain tool expose?**
A: `.name`, `.description`, and `.args` (argument schema).

**Q: Why is human-in-the-loop critical in agentic AI workflows?**
A: Fully automated systems risk bias, hallucinated decisions, uncontrolled costs, and ethical violations. Human checkpoints allow intervention and course correction.

**Q: What is tool binding in LangChain?**
A: Combining multiple tools into a list and passing them to the agent executor alongside the LLM, so the agent knows which tools it can call.

**Q: What is the role of the supervisor node in a multi-agent system?**
A: It receives user input, interprets intent, and delegates the task to the correct agent, which then calls the appropriate tool.
