# CrewAI Content Marketing Pipeline & AWS EC2 Deployment

## TL;DR
This session covered a CrewAI multi-agent content marketing pipeline that generates blog posts and social media content from a single topic, followed by a hands-on deployment of the Streamlit application to an AWS EC2 instance. The instructor demonstrated the full workflow from local development to a publicly accessible deployed app.

## Topics Covered
- CrewAI content marketing pipeline project walkthrough (4 agents, 4 tasks)
- Demo of the deployed Streamlit application
- Code structure and execution flow of CrewAI projects
- LangGraph vs CrewAI comparison (brief)
- Deployment concepts: what deployment means and industry approaches
- AWS EC2 instance creation and configuration
- Deploying a Streamlit + CrewAI app on EC2
- Running the application in the background with `nohup`
- Brief attempt at Railway deployment

## Key Concepts Explained

### CrewAI Multi-Agent Pipeline Architecture
- A pipeline with 4 agents (Trend Researcher, SEO Analyst, Blog Writer, Social Media Repurposer) and 4 corresponding tasks running sequentially
- Why it matters: demonstrates how multiple specialized agents can collaborate to produce varied outputs (blog + LinkedIn + Twitter + Instagram) from a single topic input
- The instructor emphasized the standard CrewAI flow: define LLM → define agents (role, goal, backstory) → define tasks → define crew → kickoff

### CrewAI Project Structure
- Standard structure: `main.py` (entry point), `crew.py` (pipeline definition), `src/` folder with YAML config files for agents and tasks
- Config uses two YAML files: `agents.yaml` (role, goal, backstory per agent) and `tasks.yaml` (description, expected output per task)
- The execution flow is: `main.py` calls `crew.kickoff()` → crew delegates to tasks → tasks use agents → agents use the LLM

### Deployment Fundamentals
- Deployment = making an application publicly accessible via a URL instead of just localhost
- Two common industry patterns: (1) Convert backend to Lambda functions + S3 storage, expose API endpoint to frontend dev; (2) Deploy full-stack Streamlit/Flask app directly to EC2/Azure
- The instructor emphasized always stopping instances after use to avoid unexpected bills

### Running Apps in Background with nohup
- Using `nohup` allows the Streamlit app to keep running even after closing the SSH session
- Without `nohup`, closing PuTTY kills the application
- The working command used: `nohup streamlit run main.py > streamlit.log 2>&1 &`

## Tools, Libraries & Commands Mentioned

| Item | Notes |
|------|-------|
| CrewAI | Multi-agent framework used to build the content pipeline |
| Streamlit | Frontend framework for the content marketing app (runs on port 8501) |
| Groq API / XAI API | LLM provider used for the agents |
| AWS EC2 | Cloud compute instance used for deployment (T2 micro, Ubuntu) |
| PuTTY | SSH client to connect to the EC2 instance on Windows |
| PuTTYgen | Tool to convert .pem key to .ppk format for PuTTY |
| WinSCP | File transfer tool to push project files to EC2 |
| `nohup` | Unix command to run processes in background (persists after logout) |
| Railway | Alternative deployment platform briefly attempted |
| Streamlit Cloud | Free deployment option (connect GitHub repo to streamlit.io) |
| GitHub Secrets | Used to store API keys securely when deploying via GitHub |
| Docker | Mentioned as an alternative deployment method (not demonstrated) |
| n8n | Low-code automation tool previewed for a future session |

## Code & Examples

### CrewAI Project Flow (Conceptual)

```python
# 1. Define LLM
llm = LLM(model="model_name", api_key="...")

# 2. Define Agents (each has role, goal, backstory)
researcher = Agent(role="Content Trend Researcher", goal="...", backstory="...", llm=llm)
seo_analyst = Agent(role="SEO Analyst", goal="...", backstory="...", llm=llm)
blog_writer = Agent(role="Blog Writer", goal="...", backstory="...", llm=llm)
social_media = Agent(role="Social Media Repurposer", goal="...", backstory="...", llm=llm)

# 3. Define Tasks (each has description, expected_output, agent)
research_task = Task(description="...", expected_output="...", agent=researcher)
seo_task = Task(description="...", expected_output="...", agent=seo_analyst)
writing_task = Task(description="...", expected_output="...", agent=blog_writer)
social_task = Task(description="...", expected_output="...", agent=social_media)

# 4. Define Crew
crew = Crew(
    agents=[researcher, seo_analyst, blog_writer, social_media],
    tasks=[research_task, seo_task, writing_task, social_task],
    process=Process.sequential
)

# 5. Kickoff
result = crew.kickoff(inputs={"topic": "RAGs vs fine-tuning"})
```

### EC2 Deployment Steps

```bash
# After SSH into EC2 Ubuntu instance:

# Install Python 3.13 (since Ubuntu had 3.14 which CrewAI doesn't support)
sudo apt update
sudo apt install python3.13 python3.13-venv

# Create and activate virtual environment with Python 3.13
python3.13 -m venv venv
source venv/bin/activate

# Upgrade pip and install dependencies
pip install --upgrade pip
pip install uv
pip install crewai
pip install -r requirements.txt

# Run Streamlit in background
nohup streamlit run main.py > streamlit.log 2>&1 &

# Check logs
less streamlit.log
```

### EC2 Security Group Configuration
- Add inbound rule: Custom TCP, Port 8501, Source 0.0.0.0/0
- This allows public access to the Streamlit app

### Accessing the Deployed App
- URL format: `http://<EC2-Public-DNS>:8501`

## Q&A / Discussion Points
- **Q: Does this app actually post to LinkedIn? What makes it different from generative AI?**
  A: It does not post directly (that would require LinkedIn APIs). It's still agentic AI because multiple agents collaborate on subtasks. The same output could theoretically be achieved with a single generative AI call, but the agentic approach uses specialized agents with distinct roles.

- **Q: How is LangGraph different from CrewAI? Pros and cons?**
  A: No major concrete differences — both are multi-agent frameworks with different templates/styles. Choice is developer preference, similar to TensorFlow vs PyTorch. The instructor plans a future session building the same project in both for direct comparison.

- **Q: Can you deploy using Railway?**
  A: Briefly attempted in class; deployment showed "successful" but the public URL didn't respond correctly. The instructor noted he hadn't used Railway before and moved on to AWS.

## Action Items & Assignments
- [ ] Download the content marketing pipeline zip shared on Google Drive (Week 10 folder)
- [ ] Run the CrewAI project locally with the provided API keys
- [ ] Try deploying the application on your own AWS EC2 instance following the shared deployment steps
- [ ] Give feedback for today's class via the shared poll
- [ ] Prepare questions for the doubt-clearing session (or ask on Discord)

## Open Questions / Unclear Spots
- The `nohup` command initially failed during the demo; the instructor eventually got it working with output redirection but the exact issue was unclear
- Railway deployment showed "successful" but the app was inaccessible — root cause not investigated
- Whether LinkedIn automation is feasible via APIs was left as an open investigation for the instructor

## Flashcards

**Q: What are the four mandatory components when defining a CrewAI agent?**
A: Role, goal, backstory, and LLM assignment.

**Q: What is the standard execution flow in a CrewAI project?**
A: Define LLM → Define Agents → Define Tasks → Define Crew → Call `crew.kickoff()`

**Q: What port does Streamlit use by default, and why does it matter for EC2 deployment?**
A: Port 8501. You must add an inbound security group rule to allow TCP traffic on this port for the app to be publicly accessible.

**Q: What is the purpose of `nohup` when deploying a Streamlit app on EC2?**
A: It keeps the application running in the background even after you close your SSH/PuTTY session. Without it, closing the terminal kills the app.

**Q: What two YAML config files does a CrewAI project typically use?**
A: `agents.yaml` (defines role, goal, backstory for each agent) and `tasks.yaml` (defines description and expected output for each task).

**Q: Why did the instructor need to install Python 3.13 on the EC2 instance?**
A: The Ubuntu instance came with Python 3.14 pre-installed, which CrewAI does not support. A downgrade to 3.13 was required.

**Q: What is the difference between deploying as a Lambda function vs deploying a Streamlit app on EC2?**
A: Lambda functions expose an API endpoint for a frontend developer to integrate; EC2 deployment hosts the full application (backend + Streamlit frontend) as a standalone accessible app.

**Q: What tool converts a .pem file to .ppk format for use with PuTTY?**
A: PuTTYgen.

**Q: What is the instructor's key warning about AWS EC2 instances?**
A: Always stop/terminate instances after use to avoid unexpected bills. Leaving instances running can result in significant charges.

**Q: In the content marketing pipeline, what are the four outputs generated from a single topic?**
A: A blog post, a LinkedIn post, a Twitter thread, and an Instagram post.
