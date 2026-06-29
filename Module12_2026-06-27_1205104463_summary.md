# Multi-Agent Systems & Guardrails Implementation (LangGraph + n8n)

## TL;DR
This session covered the differences between single-agent and multi-agent architectures, when to choose each, and how to implement guardrails to protect AI systems from jailbreaks, harmful content, PII leakage, and illegal requests. Practical implementations were demonstrated in both LangGraph (Python) and n8n (low-code), including keyword-based and LLM-assisted guardrails.

## Topics Covered
- Single-agent vs. multi-agent architecture comparison
- Pros and cons of multi-agent systems (scalability, debugging, context bloat)
- Guardrails: definition, types, and implementation strategies
- Input guardrails (system-level vs. per-agent vs. layered)
- Output guardrails
- LangGraph guardrail implementation (keyword-based and LLM-assisted)
- n8n single-agent workflow: "Alexa-like" assistant project
- n8n guardrail node: Check Text for Violations and Sanitize
- Connecting a real-time n8n workflow to a Streamlit front end (briefly discussed)

## Key Concepts Explained

### Guardrails
- Filters placed before (input) or after (output) an AI agent to block harmful, illegal, or policy-violating content from being processed or returned.
- Essential for any production agentic AI system to prevent jailbreaks, PII leakage, toxic outputs, and access to illegal resources.
- The instructor emphasized that input-level guardrails are the **minimum mandatory** requirement for any GenAI/agentic AI solution.

### Types of Guardrails
- **Jailbreak / Prompt Injection** — user attempts to override system instructions (e.g., "Ignore all previous instructions…").
- **Topical / Content Moderation** — blocks harmful or illegal content keywords (bombs, suicide, etc.).
- **URL / Domain filtering** — prevents access to malicious, pornographic, or illegal websites.
- **PII / Sensitive Information** — masks or refuses to store credit card numbers, PINs, personal data.
- **Toxicity / Hate Speech** — filters harmful or dangerous language.
- **Hallucination / Factual Accuracy** — validates output correctness.

### Guardrail Implementation Strategies
- **System-level input guardrail** — single guardrail before all agents; returns generic rejection message on failure.
- **Per-agent input guardrail** — dedicated guardrail per agent with specific rejection messages (recommended for specificity).
- **Layered guardrail** (recommended for code-based systems) — combines: (1) global input guardrail, (2) per-agent guardrails, (3) output guardrail, (4) tool-specific guardrail.

### Single-Agent vs. Multi-Agent Architecture
- **Single-agent**: one agent with all tools attached. Simpler for small use cases (≤4 tools) but suffers from tool overload, context bloat, difficult debugging, and slower performance at scale.
- **Multi-agent**: supervisor agent delegates to specialized sub-agents, each with their own LLM, memory, and tools. Better for scalability, specialized prompts, and easier guardrail implementation.
- The instructor emphasized: for proof-of-concept with few tools, either works; for scalability (20-30+ tools), always choose multi-agent.

### LLM-Assisted Guardrails
- Instead of hard-coding forbidden keywords, use a secondary LLM call with a "guard prompt" to evaluate whether input violates policies.
- More flexible and can catch nuanced violations (e.g., detecting intent to access illegal content without exact keyword matches).

## Tools, Libraries & Commands Mentioned

| Item | Notes |
|------|-------|
| LangGraph | Python framework for building agentic AI state graphs with guardrails |
| n8n | Low-code automation platform for building agentic workflows |
| OpenMeteo API | Free weather API (no auth required); uses latitude/longitude params |
| Frankfurter API | Free forex/currency exchange API (no auth required) |
| NewsAPI.org | News API (free tier, API key required) |
| YouTube Data API v3 | YouTube search via Google Developer Console (API key required) |
| ChatXAI (xAI/Grok) | LLM used as the brain for both LangGraph and n8n agents |
| Groq | Alternative LLM provider used in n8n workflows |
| Google Colab | Used to run LangGraph guardrails notebook |
| Streamlit | Suggested for building front-end UI that invokes n8n workflows via webhook |

## Code & Examples

### LangGraph Guardrail Architecture (Pseudocode)

```python
# State Graph structure
START -> input_guardrail -> [conditional]
    if unsafe: -> system_message -> END
    if safe: -> forex_agent -> news_agent -> output_guardrail -> [conditional]
        if safe: -> final_output -> END
        if unsafe: -> system_message -> END
```

### LangGraph Keyword-Based Input Guardrail (Approximate)

```python
FORBIDDEN_WORDS = ["bomb", "explosive", "hack", "suicide", ...]

def input_guardrail(state):
    user_message = state["messages"][-1].content.lower()
    if any(word in user_message for word in FORBIDDEN_WORDS):
        return {"status": "unsafe", "reason": "Input guardrail: unsafe query detected"}
    return {"status": "passed"}
```

### LangGraph LLM-Assisted Guardrail (Approximate)

```python
guard_prompt = """You are a strict content safety guardrail.
Requests or links to pornography, adult content, explicit material,
or illegal activities are NOT allowed.
Respond with BLOCK or PASS only."""

def input_guardrail(state):
    user_message = state["messages"][-1].content
    if "https" in user_message or "http" in user_message:
        response = guard_llm.invoke([
            SystemMessage(content=guard_prompt),
            HumanMessage(content=user_message)
        ])
        decision = response.content.strip().upper()
        if decision == "BLOCK":
            return {"status": "blocked", "reason": "Policy violation"}
    return {"status": "passed"}
```

### n8n Single-Agent Workflow Setup
1. **Chat Trigger** → sends user message
2. **AI Agent** node with:
   - Chat model (xAI/Grok or OpenAI)
   - Simple memory
   - 4 HTTP tools (Weather, Forex, News, YouTube)
3. System prompt defines agent as "Jarvis" with instructions to route to appropriate tools

### n8n Guardrails Implementation
1. Insert **Guardrail** node between Chat Trigger and AI Agent
2. Configure "Check text for violations" with desired checks (jailbreak, PII, URL, etc.)
3. Connect **Pass** output → Supervisor Agent
4. Connect **Fail** output → Rejection AI Agent (returns "Sorry, I can't answer this")
5. Update Supervisor Agent to receive input from Guardrail node (not directly from Chat)

## Q&A / Discussion Points
- **Q: Why are multi-agent systems better?** A: For this small use case (4 tools), either works. But multi-agent is better for scalability because single-agent suffers from tool overload, context bloat, hallucinations during routing, difficult debugging, and higher token usage.
- **Q: How to avoid hard-coding guardrail keywords?** A: Use an LLM-assisted guardrail with a dedicated guard prompt that evaluates inputs dynamically.
- **Q: How does n8n work in production/real-time?** A: Replace the Chat Trigger with an HTTP Trigger, publish the workflow, and pass the HTTP endpoint URL to the front-end application (e.g., Streamlit chat widget).

## Action Items & Assignments
- [ ] Create a NewsAPI.org account and get an API key
- [ ] Set up Google Developer Console credentials for YouTube Data API v3
- [ ] Execute the LangGraph guardrails notebook on Google Colab
- [ ] Upload and test the n8n single-agent workflow file
- [ ] Add additional guardrail types in n8n (URL, PII, topical) and test them
- [ ] Attend July 2nd class — resume preparation and interview tips (not just doubt-clearing)
- [ ] Tomorrow (June 28): end-to-end project on LangGraph, CrewAI, and n8n

## Open Questions / Unclear Spots
- The multi-agent n8n workflow had issues with sub-agent tools not being triggered (error: "connected tool returned an engine request to its parent agent, which is only supported for top-level node execution"). The instructor acknowledged the bug and said he'd fix it later.
- [unclear: "Charvess"] — likely a project name or variant spelling used for the n8n workflow file name.
- The Streamlit + n8n webhook integration was mentioned but not fully demonstrated; instructor said he'd try to show it the next day.

## Flashcards

**Q: What are the four main types of guardrails for AI systems?**
A: (1) Jailbreak/Prompt Injection, (2) Topical/Content Moderation, (3) URL/Domain Filtering, (4) PII/Sensitive Information protection. Additional types include toxicity filtering and hallucination detection.

**Q: What is the main disadvantage of single-agent architecture at scale?**
A: Tool overload — the system prompt becomes massive, the context window bloats, routing hallucinations increase, debugging becomes a nightmare, and token usage/latency increases.

**Q: What is a "layered guardrail" approach?**
A: A multi-layer strategy combining: Layer 1 (global input guardrail), Layer 2 (per-agent guardrails), Layer 3 (output guardrail), and Layer 4 (tool-specific guardrails).

**Q: How does an LLM-assisted guardrail differ from keyword-based?**
A: Instead of matching hard-coded forbidden words, a secondary LLM evaluates the input against a safety prompt and returns BLOCK/PASS, enabling detection of nuanced or novel policy violations.

**Q: In n8n, what are the two guardrail operations available?**
A: "Check text for violations" (evaluates input against rules like jailbreak, PII, URL) and "Sanitize" (masks sensitive data like credit cards without fully rejecting the input).

**Q: What is the minimum mandatory guardrail for any production AI system?**
A: An input-level guardrail — filtering user messages before they reach the agent/LLM.

**Q: In a multi-agent architecture, what role does the supervisor agent play?**
A: It receives user input and routes/delegates tasks to specialized sub-agents (each with their own LLM, memory, and tools).

**Q: How do you connect an n8n workflow to a front-end application in production?**
A: Replace the Chat Trigger with an HTTP Trigger, publish the workflow, and use the HTTP endpoint URL in the front-end (e.g., Streamlit) to invoke the chat.
