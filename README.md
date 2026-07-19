# Bubble Connector — template

File-based shared knowledge base for a research group. Each person gets a folder; `SYNTHESIS.md`
connects the dots. **Do not put private participant data in this template repo** — fork first.

## Get started

1. **Fork** this repository on GitHub (or GitLab, etc.) into your account or org.
2. **Clone your fork** (not the template upstream):

```bash
git clone git@github.com:<you>/bubble_connector.git
cd bubble_connector
```

3. **Open in an agent-enabled IDE** (e.g. [VS Code](https://code.visualstudio.com/) with GitHub
   Copilot, [Cursor](https://cursor.com/), or similar). Open the cloned folder as the workspace.
4. Tell the agent:

> Read `AGENT_INSTRUCTIONS.md` and follow it.

All setup steps — placeholders, participants, git remotes, privacy — are in that file.

**Important:** never open pull requests from your fork **back to this template repo**. Your
instance will contain private group data; PRs upstream could expose it accidentally.
