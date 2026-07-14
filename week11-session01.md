# n8n Workflow Automation: Lead Capturing, Social Media Automation & AI News Agent

## TL;DR
This session was a hands-on deep dive into n8n, covering three complete workflows: a lead-capturing form pipeline, a social media post automation using AI, and an AI-powered news summarizer agent with tools and memory. The instructor demonstrated how n8n nodes connect to build end-to-end automations and highlighted the difference between AI models and AI agents within n8n.

## Topics Covered
- Schedule adjustment: swapping week 11 (n8n) and week 12 (LangGraph project) content
- n8n overview: low-code automation platform for AI and non-AI workflows
- n8n alternatives: LangFlow, Make, Zapier, OpenAI Agent Builder, corporate tools
- n8n account creation and platform walkthrough (workflows, credentials, executions)
- Workflow 1: Lead-capturing form → Google Sheets → Gmail notification
- Publishing workflows and using production URLs
- Workflow 2: Social media automation (Google Docs research → OpenAI → Google Docs/LinkedIn)
- Automating topic input via Google Sheets triggers
- Workflow 3: AI-powered news summarizer agent with tools (News API, Gmail) and memory
- AI model vs AI agent distinction in n8n
- Demonstrating the role of memory in conversational AI agents

## Key Concepts Explained

### n8n Platform Architecture
- A low-code workflow automation tool that works by connecting nodes in a visual interface
- Each node has an input (left) and output (right); the output of one node feeds into the next
- Three key sections: Workflows (your projects), Credentials (stored API/auth connections), Executions (run history)
- 14-day free trial; free OpenAI credits included for AI workflows

### n8n Node Types
- **Triggers**: start the workflow — manual trigger (click to start), form submission, Google Sheets row added, chat message received, scheduled, webhook
- **Action nodes**: perform operations — Google Sheets (append/update), Gmail (send), Google Docs (get/update), LinkedIn (create post)
- **AI nodes**: AI Model (single LLM call) vs AI Agent (LLM + tools + memory — a full conversational agent)
- Nodes turn green on successful execution and red on errors

### AI Model vs AI Agent in n8n
- An **AI Model** node makes a single LLM call with a prompt and returns a response — no memory, no tool access
- An **AI Agent** node has five components: brain (LLM), tools (callable actions), memory (conversation history), orchestrator (n8n itself), and supervisor (human in the loop)
- Use AI Model for one-shot tasks (e.g., generate a LinkedIn post); use AI Agent for conversational or multi-tool workflows

### Memory in AI Agents
- Without memory, the agent cannot recall information from earlier in the conversation (e.g., forgetting the user's name after a few exchanges)
- With memory attached, the agent retains conversation context across messages
- Demonstrated by removing the memory node, asking the agent to recall a name, and showing it fails — then re-attaching memory and showing it succeeds

### Test URL vs Production URL
- Test URL: for development and manual testing; requires clicking "Execute" to trigger
- Production URL: for deployment; auto-triggers on events (e.g., form submission) without manual intervention
- Must publish the workflow before the production URL becomes active

## Tools, Libraries & Commands Mentioned

| Item | Notes |
|------|-------|
| n8n | Low-code workflow automation platform (n8n.io); central tool for the session |
| LangFlow | Low-code AI builder from the LangChain family; alternative to n8n |
| Make | Workflow automation platform (make.com); similar to n8n |
| Zapier | Automation platform; mentioned as another n8n alternative |
| OpenAI Agent Builder | OpenAI's no-code agent builder; not available in all regions |
| Infosys Topaz | Corporate B2B no-code AI platform |
| Oracle Agent Builder | Oracle's B2B no-code agent platform |
| Google Sheets | Used as data store for leads and as topic trigger for social media automation |
| Google Docs | Used to store research content and generated LinkedIn posts |
| Gmail (n8n node) | Used to send automated lead notification emails and news summaries |
| LinkedIn (n8n node) | Used to auto-post content; person mode had issues, organization mode worked |
| OpenAI (n8n node) | GPT-4o mini used for content generation; n8n provides free API credits |
| xAI (Grok) | Used as LLM for the AI agent in the news summarizer workflow |
| newsapi.org | Free news API used by the AI agent's "Get News" tool |
| Groq | Used for quick research summarization in the social media workflow |

## Code & Examples

### Workflow 1: Lead-Capturing Pipeline (3 Nodes)

**Node 1 — On Form Submission:**
- Form title: "Data Analytics Masters"
- Description: "If you're interested in learning data analytics in 2026, share your details and we'll get back to you with the next steps"
- Fields: Name (text), WhatsApp (text), Country (text), Education (dropdown: Below Bachelors / Bachelors / Masters or above), Working Professional (text)
- Custom button label: "Get Started" (instead of default "Submit")
- Option to turn off n8n attribution footer

**Node 2 — Google Sheets (Append or Update Row):**
- Sheet name: "DA Masters Leads June 2026"
- Column to match on: Name (for deduplication)
- Map fields by dragging from Node 1 output

**Node 3 — Gmail (Send Message):**
- Subject: "Lead Inquiry for Data Analytics Master 2026"
- Body: template with dragged field references for name, WhatsApp, country, education

### Workflow 2: Social Media Automation (4-5 Nodes)

**Approach A — Manual research input:**
1. Manual Trigger → Google Docs (Get Document — read research content) → OpenAI (GPT-4o mini — generate LinkedIn post) → Google Docs (Update Document — write post) → LinkedIn (Create Post as organization)

**Approach B — Automated topic input:**
1. Google Sheets Trigger (On Row Added) → OpenAI (generate post based on topic from sheet) → Google Docs (update with generated post) → LinkedIn (post)

**Prompt used for LinkedIn post generation (approximate):**

```text
Based on the provided topic, can you please do some research and prepare
a beautiful LinkedIn post that should start with a proper hook, as in a
question to the audience to grab attention, and end the post with
necessary hashtags that are SEO-friendly. Restrict the post to have
formatting options like //, **, etc., because I will directly be using
the post to post it on LinkedIn. Add icons, emojis to the post as and
when needed. Output only.
```

### Workflow 3: AI-Powered News Summarizer Agent

**Architecture:**
- Chat node (when chat message received) → AI Agent node
- AI Agent sub-nodes: Brain (xAI or OpenAI GPT-4o), Memory (window buffer), Tool 1 (Get News via newsapi.org), Tool 2 (Send Mail via Gmail)

**Agent components mapped to theory:**

```text
Brain       = xAI / OpenAI (the LLM)
Tools       = Get News (newsapi.org API), Send Mail (Gmail)
Memory      = Window buffer memory node
Orchestrator = n8n (the platform itself)
Supervisor   = Human in the loop (the user)
```

**Memory demonstration:**
1. Remove memory node → reset chat → tell bot your name → ask a few questions → ask "What's my name?" → bot says "I don't know"
2. Re-attach memory node → reset chat → tell bot your name → ask "What's my name?" → bot correctly responds

### Importing Shared Workflows

```text
1. Download the JSON file from Google Drive
2. In n8n: Create new workflow → Click three-dot menu → Import from File
3. Upload the JSON
4. Configure credentials (Google account, Gmail, API keys) where you see errors
5. Execute and test
```

## Q&A / Discussion Points
- **Q: Is there an open-source alternative to n8n?**
  A: No, these tools are not open source. n8n provides a 14-day free trial and free OpenAI credits, but the platform itself is not free/open-source. No known open-source alternatives exist for this category.

- **Q: What about Zapier?**
  A: Yes, Zapier is also used for automations and is another alternative in the same space.

- **Q: LinkedIn person mode not working for posting?**
  A: The instructor could not get the "post as person" option to work despite being authenticated. The "post as organization" mode worked correctly using a company page where the user is admin.

- **Q: Why wasn't the HTML email rendering properly in Gmail?**
  A: The instructor noted that other students received properly formatted HTML emails, but his Gmail was rendering them as broken HTML. The root cause was not identified; saving the HTML to a file and opening directly showed it rendered correctly.

## Action Items & Assignments
- [ ] Create an n8n account at n8n.io (14-day free trial)
- [ ] Download all three workflow JSON files from Google Drive and import them into n8n
- [ ] Create a newsapi.org account and generate an API key for the news summarizer
- [ ] Practice all three workflows: lead capturing, social media automation, news agent
- [ ] Try removing the memory node from the AI agent to observe the difference
- [ ] Try removing tools from the AI agent to see how it affects behavior
- [ ] Investigate how to fetch only the latest row from Google Sheets for the social media workflow (open challenge for next class)

## Open Questions / Unclear Spots
- How to make the Google Sheets trigger pick only the latest/newest topic row instead of all rows — left as a challenge for students to solve before the next class
- LinkedIn "post as person" mode failed to load the profile despite valid authentication — root cause unknown
- HTML email formatting was broken in the instructor's Gmail but worked for other students — cause not investigated
- [unclear: Zep Analytics] — the instructor's company page used for LinkedIn posting; name approximate from transcript

## Flashcards

**Q: What are the three key sections in the n8n sidebar?**
A: Workflows (your projects), Credentials (stored authentication/API connections), and Executions (history of workflow runs).

**Q: What is the difference between an AI Model node and an AI Agent node in n8n?**
A: An AI Model node makes a single LLM call with no memory or tools. An AI Agent node is a full conversational agent with five components: brain (LLM), tools, memory, orchestrator (n8n), and supervisor (human in the loop).

**Q: What are the five components of an AI agent as demonstrated in n8n?**
A: Brain (the LLM), Tools (callable actions like API calls), Memory (conversation history), Orchestrator (n8n platform), and Supervisor (human in the loop).

**Q: How do you share an n8n workflow with someone else?**
A: Export the workflow as a JSON file. The recipient creates a new workflow, clicks the three-dot menu → Import from File, uploads the JSON, and configures their own credentials.

**Q: What happens when you remove the memory node from an n8n AI agent?**
A: The agent loses the ability to recall information from earlier in the conversation. For example, it will forget the user's name even if it was mentioned just a few messages ago.

**Q: What is the difference between n8n's test URL and production URL?**
A: Test URL is for development (requires manual execution). Production URL is for deployment and auto-triggers on events. The workflow must be published before the production URL becomes active.

**Q: Name four low-code/no-code automation platforms that compete with n8n.**
A: LangFlow, Make (make.com), Zapier, and OpenAI Agent Builder.

**Q: In the lead-capturing workflow, what does the "column to match on" setting do in the Google Sheets node?**
A: It acts as a primary key for deduplication. If a new form submission has the same value in the matched column (e.g., same name), it updates the existing row instead of creating a duplicate.

**Q: Why does the instructor recommend against directly automating LinkedIn posts via n8n?**
A: LinkedIn can detect AI automation and may ban your profile. It's safer to generate posts into Google Docs and manually copy-paste them to LinkedIn.

**Q: Does n8n provide any free AI credits?**
A: Yes, n8n provides free OpenAI API credits that can be selected when configuring an OpenAI node (choose "n8n free OpenAI API credits" from the credentials dropdown).
