# MCP (Model Context Protocol) & End-to-End Projects (LangGraph + CrewAI)

## TL;DR
This final technical session introduced MCP (Model Context Protocol) — a standardized way for AI agents to connect to external tools via a client-server architecture — and demonstrated its implementation in n8n. The second half walked through two complete end-to-end "Alexa-like" assistant projects built with LangGraph and CrewAI, including Streamlit front-ends, multi-agent routing, and tool integration.

## Topics Covered
- MCP (Model Context Protocol) — concept, architecture, and step-by-step flow
- MCP vs. traditional tool attachment in agentic AI
- MCP implementation in n8n (server trigger, client node, production URL)
- End-to-end project demo: LangGraph-based assistant with Streamlit UI
- End-to-end project demo: CrewAI-based assistant with Streamlit UI
- Project code walkthrough (state graph, agents, tools, YAML configs)
- Course recap and next steps for portfolio building

## Key Concepts Explained

### MCP (Model Context Protocol)
- A standardized protocol (created by Anthropic) that allows AI agents to connect to external tools, data sources, and APIs through a client-server architecture, rather than attaching tools directly to the agent.
- Solves scalability problems: instead of managing 30+ tools directly on an agent, tools live on an MCP server that exposes them via a manifest file. The agent queries the server for available tools dynamically.
- The instructor emphasized the analogy: like using one USB hub/adapter instead of plugging multiple cables directly into your laptop.

### MCP Architecture Components
- **MCP Host** — the platform running your AI application (e.g., n8n cloud).
- **MCP Client** — attached to the AI agent as a "tool"; maintains a one-to-one connection with the server and handles tool discovery/invocation.
- **MCP Server** — a lightweight program that exposes specific tools through the standardized protocol. Can have more tools internally than it exposes.
- **Manifest File** — the list of available tools sent from server to client during initialization.

### MCP Step-by-Step Flow
1. MCP Client requests the list of tools from MCP Server
2. Server responds with a manifest file (e.g., 50 out of 100 tools exposed)
3. Client passes manifest to AI Agent
4. AI Agent sends manifest + user query to LLM
5. LLM identifies the relevant tool (e.g., "tool #37 — weather API")
6. AI Agent instructs Client to execute that tool
7. Client asks Server to execute the tool
8. Server returns the result to Client
9. Client passes result back to AI Agent
10. Agent sends result through LLM for chat completion, then returns final answer to user

### Google A2A (Agent-to-Agent)
- Google's similar concept to MCP, written as "A2A" (Agent-to-Agent).
- Mentioned briefly as an alternative standard; not demonstrated.

### CrewAI Project Architecture
- Follows the official CrewAI documentation structure: agents.yaml (role, goal, backstory), tasks.yaml (description, expected output), crew.py (agent definitions, task definitions, crew orchestration).
- Uses a **supervisor/routing agent** that determines which specialized agent to call based on the user's input domain.

## Tools, Libraries & Commands Mentioned

| Item | Notes |
|------|-------|
| MCP (Model Context Protocol) | Standardized protocol by Anthropic for tool-server architecture |
| A2A (Agent-to-Agent) | Google's alternative to MCP |
| n8n MCP Server Trigger | Node that hosts tools and exposes them via production URL |
| n8n MCP Client | Node attached as a tool to AI Agent; connects to MCP server endpoint |
| LangGraph | Python framework used for the state-graph-based project |
| CrewAI | Python framework used for the multi-agent crew-based project |
| Streamlit | Front-end framework for both project demos |
| OpenMeteo API | Weather data (free, no auth) |
| Frankfurter API | Forex/currency exchange (free, no auth) |
| ExchangeRate API | Alternative forex API used in the LangGraph project |
| NewsAPI.org | News headlines and search (free tier, API key required) |
| YouTube Data API v3 | YouTube search via Google Developer Console |
| ChatXAI (Grok) | LLM used as the agent brain (xAI model "3 Mini") |
| Google Developer Console | For creating YouTube API credentials |
| GitHub | For pushing portfolio projects |

## Code & Examples

### n8n MCP Architecture Setup

**Steps to convert single-agent to MCP architecture:**
1. Disconnect all tools from the AI Agent
2. Add an **MCP Client** node as the agent's only tool
3. Create a separate **MCP Server Trigger** node
4. Attach all tools (Weather, Forex, News, YouTube) to the MCP Server Trigger
5. Publish the workflow and enable MCP access
6. Copy the **production URL** from the MCP Server Trigger
7. Paste it into the MCP Client's server URL configuration
8. Set authentication to "none" (or configure as needed)
9. In MCP Client, set "tools to include" to "all" or select specific tools

### LangGraph Project Structure

```
project/
  .env                  # API keys (XAI, Forex, News, YouTube)
  app.py                # Streamlit entry point
  graph/
    workflow.py         # State graph definition (nodes, edges, conditional edges)
  tools/
    forex_tool.py       # Forex API integration
    music_tool.py       # YouTube search
    news_tool.py        # News API (headlines + search)
    weather_tool.py     # OpenMeteo (coordinates + weather + forecast)
  agents/
    agents.py           # Agent definitions with prompt templates
  config/
    settings.py         # Loads env vars
  components/
    ...                 # Streamlit UI components
```

### LangGraph State Graph (Approximate)

```python
graph = StateGraph(AgentState)

# Add nodes
graph.add_node("supervisor", supervisor_node)
graph.add_node("weather", weather_node)
graph.add_node("forex", forex_node)
graph.add_node("music", music_node)
graph.add_node("news", news_node)
graph.add_node("general", general_node)

# Add edges
graph.add_edge(START, "supervisor")
graph.add_conditional_edges("supervisor", route_to_agent,
    {"weather": "weather", "forex": "forex", "music": "music",
     "news": "news", "general": "general"})
graph.add_edge("weather", END)
graph.add_edge("forex", END)
graph.add_edge("music", END)
graph.add_edge("news", END)
graph.add_edge("general", END)
```

### CrewAI Project Structure

```
project/
  .env
  app.py                # Streamlit entry point
  src/jarvis/
    crew.py             # Crew definition: agents, tasks, crew, run()
    config/
      agents.yaml       # role, goal, backstory for each agent
      tasks.yaml        # description, expected_output for each task
    tools/
      forex_tool.py
      music_tool.py
      news_tool.py
      weather_tool.py
    agents/
      ...               # Empty files (agents defined in crew.py)
```

### CrewAI agents.yaml (Approximate Structure)

```yaml
weather_agent:
  role: "Weather Information Specialist"
  goal: "Provide accurate real-time weather data for any city"
  backstory: "You are an expert at interpreting weather data..."

forex_agent:
  role: "Currency Exchange Specialist"
  goal: "Provide accurate forex conversion rates"
  backstory: "You are a financial data expert..."

supervisor_agent:
  role: "Request Router"
  goal: "Determine which specialized agent should handle the user request"
  backstory: "You analyze user queries and route them to the correct domain agent..."
```

### CrewAI tasks.yaml Routing Task (Approximate)

```yaml
routing_task:
  description: >
    If it is a general conversation like "Hello, how are you?" reply directly.
    Otherwise, identify which domain the request belongs to and delegate to
    the respective agent (weather, forex, music, news).
  expected_output: "Appropriate response from the correct specialized agent"
```

## Q&A / Discussion Points
- **Q: How long will we have access to the recordings?** A: Likely 1-2 years; students were told to check with the program coordinator (Nazia).
- **Q: Is MCP just like connecting tools?** A (student's analogy): "It's like a mouse connected to a PC via USB — the AI is connected to files and tools." Instructor confirmed this understanding is correct.
- **Q: Why did some MCP tools fail (weather, news, YouTube) while Forex worked?** A: The issue was with HTTP tool query parameters not being properly passed through the MCP architecture, not with MCP itself. Individual tool debugging is needed.

## Action Items & Assignments
- [ ] Set up both projects locally (LangGraph and CrewAI) — change .env API keys only
- [ ] Build at least one n8n agentic AI project (as complex as possible) — search "agentic AI n8n GitHub" for ideas
- [ ] Build at least one Python-based project (LangGraph or CrewAI) relevant to your domain
- [ ] Push projects to GitHub with a PDF report, code, JSON files, and data
- [ ] Send completed projects to the instructor on LinkedIn for quick reviews
- [ ] Attend July 2nd (Thursday) class — resume preparation, LinkedIn optimization, and interview scenario-based questions
- [ ] Consider deploying projects on AWS or Streamlit Cloud (push to GitHub, deploy on Streamlit)
- [ ] Portfolio goal: aim for 5-6 projects total (RAG, fine-tuning, evaluation, n8n, LangGraph/CrewAI)

## Open Questions / Unclear Spots
- Weather, News, and YouTube tools failed when accessed through MCP Client in n8n. The instructor attributed this to query parameter issues in HTTP tools rather than MCP itself, but didn't fully resolve it in class.
- MCP implementation in Python (without n8n) was acknowledged as "a little bit difficult" and left for students to explore independently.
- [unclear: "Frankfooter"] — likely refers to the Frankfurter API (api.frankfurter.app) for currency exchange.
- The CrewAI Streamlit app showed raw "thought" output to the user in one test; the instructor noted it as a bug to fix but didn't resolve it live.

## Flashcards

**Q: What does MCP stand for, and who created it?**
A: Model Context Protocol, created by Anthropic. It provides a standardized way for AI agents to connect to external tools via a client-server architecture.

**Q: What is Google's alternative to MCP called?**
A: A2A (Agent-to-Agent).

**Q: What are the three main components of an MCP architecture?**
A: MCP Host (the platform, e.g., n8n cloud), MCP Client (connects to server, attached to agent as a tool), and MCP Server (lightweight program exposing tools via standardized protocol).

**Q: What is a manifest file in MCP?**
A: A file sent from the MCP Server to the Client listing all available/exposed tools. The server may have more tools internally but only exposes a subset.

**Q: What problem does MCP solve compared to attaching tools directly to an agent?**
A: Scalability — instead of managing 30+ tools directly on an agent (causing tool overload and maintenance issues), tools live on a separate server. If APIs change, you update the server without touching the agent. Tools are discovered dynamically.

**Q: In n8n, how do you connect an MCP Client to an MCP Server?**
A: Copy the production URL from the MCP Server Trigger node, paste it into the MCP Client node's server URL field, publish the workflow, and enable MCP access.

**Q: In CrewAI, what three attributes define an agent in agents.yaml?**
A: Role, Goal, and Backstory.

**Q: In CrewAI, what two attributes define a task in tasks.yaml?**
A: Description and Expected Output.

**Q: What is the role of a "supervisor agent" in both LangGraph and CrewAI multi-agent architectures?**
A: It receives user input, determines which domain/specialized agent should handle the request, and routes the task accordingly.

**Q: What is the recommended minimum portfolio for an AI engineer coming out of this bootcamp?**
A: 5-6 projects: at least one RAG project, one fine-tuning project, one evaluation project, one n8n agentic project, and one Python-based (LangGraph/CrewAI) agentic project.
