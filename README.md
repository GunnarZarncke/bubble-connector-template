# Bubble Connector — template

Private knowledge base with published feeds. A fork is one person's project or a shared project. Your notes stay in this repository. A feed under
`knowledge-export/<feed>/` can be published with `bin/kx` as its own Git history. Other people's
feeds are cloned into `knowledge-import/` as submodules. **Do not put private knowledge in this
template repo** — fork first.

## Get started

1. **Fork** this repository into your account or org.
2. **Clone your fork** (not the template upstream):

```bash
git clone git@github.com:<you>/bubble_connector.git
cd bubble_connector
```

3. **Open it in an agent-enabled IDE** and tell the agent:

> Read `AGENT_INSTRUCTIONS.md` and follow it.

Setup, the fork questions, and the privacy rules are in that file. The `kx` commands, what they
do and do not hide, and how to recover a failed publish are in `KNOWLEDGE_EXCHANGE.md`.

```bash
bin/kx publish general --message "What changed for readers"
bash tests/kx_test.sh
```

**Important:** never open pull requests from your fork **back to this template repo**.
