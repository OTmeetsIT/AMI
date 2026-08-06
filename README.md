# AMI
**Agentic Machine Interface**

AMI is a repository used as the knowledge base for the YouTube walkthrough:  
https://youtu.be/qrltwobnQ70

Its intent is to show an **Agentic Machine Interface** workflow for the **SEL-787**:
- A human gives a task prompt to an AI agent
- The agent reads local knowledge context in this repo (manual notes, references, logic guidance)
- The agent connects to the SEL-787 through a **Serial MCP**
- The agent determines and sends the appropriate device command(s) to achieve the user’s requested task

In short: **human intent → agent reasoning with repo context → direct machine communication**.

## Quick Start

1. Clone:
   ```bash
   git clone https://github.com/OTmeetsIT/AMI.git
   cd AMI
   ```

2. Open this folder in your preferred AI coding/chat agent tool.

3. First prompt to the agent:
   > Read `AGENT.md` first and follow it for this session.
