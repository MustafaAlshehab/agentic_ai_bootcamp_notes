# LangChain React Agents, LangGraph Introduction & State Graphs

## TL;DR
This session recapped tool creation and binding in LangChain, then built a complete React agent with weather and currency tools—including a Streamlit front end. The second half introduced LangGraph as the framework for nonlinear agentic AI workflows, covering its core primitives (nodes, edges, conditional edges) and the state graph pattern.

## Topics Covered
- Recap: tools (built-in, custom, toolkit) and tool binding
- Agent executor + React agent architecture in LangChain
- LangChain vs LangGraph: linear vs nonlinear workflows
- Live demo: LangChain React agent with weather & currency tools
- Converting a Colab notebook agent into a Streamlit app
- LangGraph fundamentals: nodes, edges, conditional edges, state graph
- Live demo: LangGraph React agent with state graph
- Preview of upcoming topics (supervisor agents, CrewAI, MCP)

## Key Concepts Explained

### Agent Executor (React Agent)
- The orchestrator that wraps an LLM + bound tools + prompt template. It receives user queries, lets the LLM decide which tool (if any) to call, passes tool output back to the LLM, and returns a natural-language answer.
- Central to both LangChain and LangGraph agent patterns.
- The instructor emphasized the standard three-step flow: **define LLM → bind tools → create React agent**.

### Tool Binding
- Attaching tool definitions to an LLM so it knows which tools are available and their expected parameters (`llm.bind_tools(tools)`).
- Often confused with simply grouping tools into a list. Binding is the explicit step that makes the LLM aware of the tools; grouping is just putting them in a Python list.
- The instructor stressed that `tools = [tool1, tool2]` is **not** binding—`llm.bind_tools(tools)` is.

### LangChain vs LangGraph
- **LangChain** is designed for linear (serial) GenAI pipelines: documents → chunking → vectorization → storage.
- **LangGraph** supports nonlinear, branching workflows needed by agentic AI (conditionals, loops, human-in-the-loop).
- LangChain drawbacks for agents: no native support for human-in-the-loop, difficult to merge broken/branching chains, far more manual code for nonlinear flows.
- Both come from the same creators; they are complementary, not competing.

### State Graph (LangGraph)
- A directed graph where each node is a processing step and the **state** (a dict/object of key-value pairs) is passed and mutated as execution moves from node to node.
- Built with three primitives: `add_node`, `add_edge`, `add_conditional_edge`.
- Every state graph begins at a `START` node and terminates at an `END` node.
- The instructor used a hiring-process analogy: the state tracks fields like `goal`, `jd`, `jd_approved`, `candidates`, etc., and each node updates relevant fields before passing state to the next node.

### Nodes, Edges & Conditional Edges
- **Node**: a discrete processing step (e.g., "agent", "tools").
- **Edge**: a fixed connection between two nodes (execution always flows from A → B).
- **Conditional Edge**: a branching connection where a function decides the next node at runtime (e.g., "should I continue to tools or end?").
- These three primitives are sufficient to express complex agentic workflows in just a few lines of code.

### React Agent Pattern (ReAct)
- Stands for **Reasoning + Acting**. The LLM reasons about which tool to call, acts by calling it, observes the result, and repeats until the query is answered.
- Available in both LangChain (`create_react_agent` from `langchain`) and LangGraph (`create_react_agent` from `langgraph`).

## Tools, Libraries & Commands Mentioned

| Item | Notes |
|------|-------|
| LangChain | Framework for linear GenAI pipelines and basic agent creation |
| LangGraph | Framework for nonlinear agentic AI workflows (state graphs) |
| `ChatXAI` (LangChain) | LLM wrapper used to call xAI's Grok models |
| Grok 3 / Grok 4 Latest | xAI LLM models used in the demos |
| `@tool` decorator | One of three ways to create custom LangChain tools |
| `StructuredTool` | Second way to create custom tools |
| `BaseTool` | Third way (class-based) to create custom tools |
| `create_react_agent` | Creates a ReAct agent in both LangChain and LangGraph |
| `StateGraph` | LangGraph class for defining node/edge workflows |
| `ToolNode` / `tools_condition` | LangGraph helpers for tool execution and conditional routing |
| `MemorySaver` | LangGraph checkpointer for conversation memory |
| OpenMeteo API | Free weather API used by the `get_weather` tool |
| Exchange-rate API | Currency conversion API used by the `get_currency` tool |
| Streamlit | Python framework used to build the chat front-end |
| `streamlit run <file>.py` | Command to launch the Streamlit app |
| Skyscanner / Booking.com / Klook APIs | Hypothetical examples for a travel-planner agent |
| MiniMax Agent | AI tool used to generate architecture diagrams from code |

## Code & Examples

### LangChain React Agent — High-Level Structure (approximate)

```python
from langchain.tools import tool
from langchain_xai import ChatXAI
from langchain.agents import create_react_agent

# 1. Define tools
@tool
def get_weather(location: str) -> str:
    """Fetch weather for a location via OpenMeteo API."""
    # ... call OpenMeteo, return formatted string ...

@tool
def get_currency(from_currency: str, to_currency: str, amount: float) -> str:
    """Convert currency using exchange-rate API."""
    # ... call API, return conversion result ...

# 2. Define LLM
llm = ChatXAI(model="grok-3", temperature=0.3, api_key="...")

# 3. Bind tools
tools = [get_weather, get_currency]
llm_with_tools = llm.bind_tools(tools)

# 4. Create React agent
agent = create_react_agent(
    model=llm_with_tools,
    tools=tools,
    system_prompt="You are a helpful assistant. Use the provided tools when the user asks about weather or currency. For any other question, answer directly using your knowledge."
)

# 5. Run
while True:
    user_input = input("You: ")
    if user_input.lower() in ["exit", "quit", "bye"]:
        break
    response = agent.invoke({"messages": [HumanMessage(content=user_input)]})
    print(response)
```

### LangGraph React Agent with State Graph — Key Section (approximate)

```python
from langgraph.graph import StateGraph, START, END
from langgraph.prebuilt import ToolNode, tools_condition
from langgraph.checkpoint.memory import MemorySaver

# (tools and LLM definition same as above)

# Define agent node function
def agent_node(state):
    response = llm_with_tools.invoke(state["messages"])
    return {"messages": [response]}

# Build the state graph
workflow = StateGraph(AgentState)
workflow.add_node("agent", agent_node)
workflow.add_node("tools", ToolNode(tools))
workflow.add_edge(START, "agent")
workflow.add_conditional_edges("agent", tools_condition)
workflow.add_edge("tools", "agent")

memory = MemorySaver()
app = workflow.compile(checkpointer=memory)
```

### Streamlit Deployment Steps
1. `pip install streamlit langchain langchain-xai langgraph` (and other dependencies)
2. Place the Streamlit app file in a folder
3. `cd` into that folder
4. Run `streamlit run agent_app.py`

## Q&A / Discussion Points
- **Can the agent call a tool multiple times in one turn?** Yes — demonstrated by asking "How much is 500 USD in IDR and INR?" The agent made two separate `get_currency` calls with distinct session IDs.
- **Does the agent use memory for repeated info?** When asked about HKD conversion after already receiving that result, the agent pulled the HKD value from conversation memory rather than calling the tool again.
- **LangChain library versioning issues:** The instructor noted that LangChain/LangGraph APIs have changed significantly over the past 1–2 years. Code from even one year ago may not work. Tip: explicitly tell LLMs to "code as per 2026 and the latest versions" in prompts.
- **MCP Servers:** A student asked about MCP. The instructor said it is not officially part of the current curriculum but will be covered around week 10 or 11, after LangGraph and CrewAI.

## Action Items & Assignments
- [ ] Download and run the LangChain React agent notebook from the Week 9 Google Drive folder
- [ ] Download and run the LangGraph React + State Graph notebook from the Week 9 Google Drive folder
- [ ] Experiment with the Streamlit agent app; customize CSS styling (colors, background, text) as practice
- [ ] (Optional) Convert the LangGraph React agent into a Streamlit application as a self-exercise
- [ ] (Optional) Watch the instructor's 5hr 49min Microsoft Fabric YouTube course if interested in data engineering

## Open Questions / Unclear Spots
- The exact exchange-rate API used in `get_currency` was not named explicitly in the transcript.
- [unclear: "AgentState"] — The agent state class definition was referenced but its full structure was not read out in detail.
- The supervisor agent pattern was mentioned but deferred to the next class.

## Flashcards

**Q1: What does ReAct stand for in the context of AI agents?**
A1: **Reasoning + Acting.** The LLM reasons about which tool to call, acts by invoking it, observes the output, and repeats until it can answer the user.

**Q2: What are the three ways to create custom tools in LangChain?**
A2: (1) `@tool` decorator, (2) `StructuredTool`, (3) `BaseTool` (class-based).

**Q3: What is the key difference between LangChain and LangGraph?**
A3: LangChain handles **linear** GenAI pipelines; LangGraph handles **nonlinear**, branching agentic AI workflows with support for conditionals, loops, and human-in-the-loop.

**Q4: What are the three core primitives in a LangGraph state graph?**
A4: **Nodes** (processing steps), **edges** (fixed connections), and **conditional edges** (branching connections decided at runtime).

**Q5: What is tool binding and how does it differ from grouping tools in a list?**
A5: Tool binding (`llm.bind_tools(tools)`) explicitly attaches tool schemas to the LLM so it knows what tools are available. Grouping (`tools = [t1, t2]`) just puts them in a Python list without informing the LLM.

**Q6: What is a state graph's "state"?**
A6: A dictionary/object that is passed through nodes and mutated at each step. It tracks all relevant data for the workflow (e.g., messages, intermediate results).

**Q7: Every LangGraph state graph must begin and end with which special nodes?**
A7: `START` node and `END` node.

**Q8: Why can't LangChain handle human-in-the-loop workflows well?**
A8: LangChain is designed for linear chains. Pausing execution for human input and resuming at arbitrary points requires nonlinear flow control, which LangGraph provides natively.

**Q9: What does `tools_condition` do in a LangGraph state graph?**
A9: It is a built-in function used with `add_conditional_edges` that checks whether the agent's last response requires a tool call. If yes, execution routes to the tools node; if no, it ends.

**Q10: What command runs a Streamlit application from the terminal?**
A10: `streamlit run <filename>.py`
