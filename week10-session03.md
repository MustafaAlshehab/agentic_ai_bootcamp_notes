# Interview Prep, CV Review & Introduction to n8n Workflow Automation

## TL;DR
This doubt-clearing session covered practical career advice including a live CV review and interview preparation for a RAG-focused AI Solutions Manager role, followed by an introduction to n8n as a workflow automation platform. The instructor demonstrated a simple lead-capturing workflow (form → Google Sheets → Gmail) to give students their first hands-on taste of n8n.

## Topics Covered
- CV/resume review for a student preparing for an AI Solutions Manager interview
- Interview preparation tips for RAG-related roles
- Strategies for answering "how to improve RAG accuracy" questions
- Deployment knowledge expectations in AI engineering roles
- Project feedback discussion
- Introduction to n8n and the low-code/no-code automation landscape
- n8n alternatives: LangFlow, Make, OpenAI Agent Builder, corporate tools
- Hands-on n8n demo: lead-capturing workflow (form → Google Sheets → Gmail)

## Key Concepts Explained

### Google's XYZ Resume Format
- A resume bullet format emphasizing what you accomplished (X), measured by (Y), by doing (Z)
- Why it matters: makes your experience concrete and measurable for recruiters
- The instructor emphasized including ROI or accuracy improvements, even if small — "If you enhanced the system's accuracy by a certain percentage, mention it"

### Improving RAG Chatbot Accuracy (the "Last 4%" Question)
- When a company claims 96% accuracy, first clarify what metric they mean — document relevance score, correct answer rate, etc.
- Potential improvement strategies: tweak LLM parameters (temperature), revisit chunking strategy, experiment with different embeddings, review document parsing quality, check retrieval mechanisms
- The instructor called this a "100% guaranteed interview question" for RAG-focused roles

### AI Engineer vs. Full-Stack Deployment Responsibilities
- AI engineers are primarily responsible for backend code and API endpoints, not frontend integration
- Deployment means making code callable via endpoints (e.g., AWS Lambda → S3 → endpoint to frontend dev)
- Every company uses different tech stacks (AWS, Azure, GCP, on-premises, etc.), so focus on understanding fundamentals rather than memorizing one stack
- In startups, you may need to wear the frontend hat too

### n8n Workflow Automation Platform
- A low-code/no-code workflow automation platform for both AI and non-AI flows
- Works by connecting nodes (each node has input on the left and output on the right)
- Offers 14-day free trial; free credits can be extended with multiple accounts
- Workflows can be exported as JSON files and imported by others

## Tools, Libraries & Commands Mentioned

| Item | Notes |
|------|-------|
| n8n | Low-code workflow automation platform (n8n.io) |
| LangFlow | Low-code AI builder from the LangChain family; instructor did not prefer it |
| Make | Automation platform similar to n8n (make.com) |
| OpenAI Agent Builder | OpenAI's no-code agent platform (not available in all regions) |
| Infosys Topaz | Corporate no-code AI platform (B2B, not publicly available) |
| Oracle Agent Builder | Oracle's no-code agent platform (B2B) |
| LangGraph | Agentic AI framework; key interview topic for RAG roles |
| LangChain | Linear AI workflow framework |
| Google Sheets | Used as the data store in the n8n lead-capturing demo |
| Gmail (n8n node) | Used to send automated email notifications on new leads |
| AWS EC2 | Referenced as a deployment method from a previous session |

## Code & Examples

### n8n Lead-Capturing Workflow (3 Nodes)

**Node 1 — On Form Submission:**
- Form title: "Data Science AI" / "Data Analytics Master's Program"
- Form fields: Name (text), WhatsApp (text), Country Code (text), Education Level (dropdown: Bachelor's, Master's or above, Below Bachelor's)
- Has both a test URL (for development) and production URL (for embedding in ads)

**Node 2 — Google Sheets (Append or Update Row):**
- Authenticate Google account → select spreadsheet → map form fields to columns
- Column to match on = Name (acts as primary key for deduplication)
- Drag-and-drop mapping from Node 1 output to sheet columns

**Node 3 — Gmail (Send Message):**
- Authenticate Gmail → set recipient, subject, email type (text)
- Email body constructed by dragging field references from previous node output
- Example subject: "Data Analytics Boot Camp — New Lead"

**Deploying the Workflow:**
- Click "Publish" to activate the production URL
- Embed the production URL in ad campaigns
- Upon form submission, the entire workflow auto-triggers (no manual execution needed)

**Sharing Workflows:**
- Download workflow as JSON → share file
- Recipient: Create workflow → three dots → Import from File → configure authentication

## Q&A / Discussion Points
- **Q: How should I pitch my projects in a recruiter call?**
  A: For the initial recruiter call, explain your experience and relevant areas (RAG, LLM integrations). Save deep technical details for the technical round.

- **Q: What technical questions should I expect for a RAG-focused role?**
  A: Expect questions on RAG workflow (PDF → chunking → embedding → retrieval), LangGraph flow and state graphs, how to create and bind tools, LangChain vs LangGraph differences, and how to draw flowcharts for given scenarios.

- **Q: Are there extra resources for learning deployment beyond the course?**
  A: The instructor recommended focusing on the course material and understanding fundamentals rather than trying to learn every cloud platform. Use AI chatbots to research specific deployment questions.

## Action Items & Assignments
- [ ] Study model deployments, especially GenAI models (review the EC2 deployment video from a previous session)
- [ ] Prepare for the interview question: "How would you improve a RAG chatbot from 96% to 100% accuracy?"
- [ ] Revisit the LangGraph class — study state graphs, tool creation, tool binding
- [ ] Use ChatGPT or similar to practice RAG accuracy improvement scenarios
- [ ] Study REST APIs and Python fundamentals for technical interviews
- [ ] Create an n8n account at n8n.io (14-day free trial)
- [ ] Download the lead-capturing workflow JSON from Google Drive (AGI3 → N8N folder)

## Open Questions / Unclear Spots
- The exact meaning of "96% accuracy" in the job listing was unclear — could mean document relevance, correct answer rate, or ticket automation success rate
- [unclear: Autonomylize] — the company name mentioned in the recruiter's message; spelling uncertain from transcript
- [unclear: Solodome] — referenced in relation to a student project; exact name uncertain

## Flashcards

**Q: What is Google's XYZ resume format?**
A: A bullet format that states what you accomplished (X), how it was measured (Y), and what you did to achieve it (Z). It emphasizes concrete results and ROI.

**Q: Name four strategies to improve RAG chatbot accuracy.**
A: (1) Tweak LLM parameters like temperature, (2) revisit the chunking strategy, (3) experiment with different embedding models, (4) review how documents are parsed and whether retrieval is working correctly.

**Q: In n8n, what is the relationship between a node's input and output?**
A: Each node has input (left) and output (right). The output of one node becomes the input of the next connected node. The first node has no input.

**Q: What is the difference between n8n's test URL and production URL?**
A: The test URL is for development and manual testing. The production URL is for deployment — it auto-triggers the workflow on form submission and can be embedded in ads or shared publicly.

**Q: What is the instructor's advice on learning deployment for AI engineering roles?**
A: Focus on understanding the fundamentals (endpoints, APIs, code structure) rather than learning every cloud platform, since every company uses a different tech stack.

**Q: How do you share an n8n workflow with someone else?**
A: Export the workflow as a JSON file. The recipient creates a new workflow, clicks the three-dot menu → Import from File, uploads the JSON, and then configures their own authentication credentials.

**Q: Why does the instructor recommend against directly automating LinkedIn posts?**
A: LinkedIn can detect AI automation and may ban your profile. It's safer to generate posts into Google Docs and manually copy-paste them to LinkedIn.
