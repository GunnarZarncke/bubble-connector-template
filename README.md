# Bubble Connector — template

A **file-based knowledge base** for a research group, workshop cohort, or collaboration
cluster. Each person gets a folder of LOG/SUMMARY files; a shared `SYNTHESIS.md` lets any
AI assistant answer *"How does my work relate to everyone else's?"*

This repo is the **blank template**. Clone it, customize the placeholders, add participants,
and share the git remote with your group.

## Quick start

### 1. Clone and rename

```bash
git clone <this-template-url> my-bubble-connector
cd my-bubble-connector
```

Remove the example participant when you are ready (or rename it to your first real member):

```bash
rm -rf users/_example
```

### 2. Customize the group config

Search the repo for `TODO:` and replace every placeholder:

| Placeholder | Where | Example |
|---|---|---|
| `<GROUP_NAME>` | `AGENT_INSTRUCTIONS.md`, `SYNTHESIS.md`, project SUMMARY | "Quantum ML Reading Group" |
| `<MAINTAINER>` | `AGENT_INSTRUCTIONS.md` | "Alex Chen" |
| `<CREATED_DATE>` | filenames, headers | `26-07-19` (YY-MM-DD) |
| `<REMOTE_URL>` | your git hosting | `git@github.com:you/bubble-connector.git` |

Rename the project-level files to match your creation date:

```bash
mv YYYY-MM-DD_bubble_connector_LOG.md 26-07-19_bubble_connector_LOG.md
mv YYYY-MM-DD_bubble_connector_SUMMARY.md 26-07-19_bubble_connector_SUMMARY.md
```

Update the paths listed in `AGENT_INSTRUCTIONS.md` to match.

### 3. Add a git remote

```bash
git remote add origin <REMOTE_URL>
git push -u origin main
```

### 4. Add participants

For each person, create `users/<slug>/` (lowercase, underscores — e.g. `alex_chen`):

```
users/alex_chen/
  OVERVIEW.md                         ← person-level index (bio, links, connection hooks)
  <date>_<topic>_LOG.md               ← append-only research journal
  <date>_<topic>_SUMMARY.md           ← condensed current state
```

Copy `users/_example/` as a starting point, or ask an AI agent to bootstrap folders using
`AGENT_BRIEF.md`.

Then update `SYNTHESIS.md`: add a digest per person (≤4,000 chars each) and a **Connections**
section at the bottom.

### 5. Use it with an AI assistant

Point Cursor, Claude, ChatGPT, or similar at this folder and say:

> Read `AGENT_INSTRUCTIONS.md` and follow it.

Typical questions:

- *"How does my work relate to the others'?"*
- *"Who should I talk to about X?"*
- *"Save that we decided Y — share it with the group."*

If your assistant cannot read local files, paste `SYNTHESIS.md` into the chat.

## Folder layout

```
README.md                            ← you are here
AGENT_INSTRUCTIONS.md                ← agent entry point (START HERE for AI)
SYNTHESIS.md                         ← shared digests + cross-person connections
YYYY-MM-DD_bubble_connector_LOG.md   ← group-level chronological journal
YYYY-MM-DD_bubble_connector_SUMMARY.md
AGENT_BRIEF.md                       ← optional: brief for bootstrapping participant folders
users/<slug>/
  OVERVIEW.md
  <date>_<topic>_LOG.md
  <date>_<topic>_SUMMARY.md
```

## The LOG/SUMMARY convention (short version)

- **LOG** — append-only journal. Dated entries separated by `---`, each starting
  `<date> — *distinctive title*`. Never retro-edit old entries; append corrections instead.
- **SUMMARY** — current condensed state. Read this before the LOG.
- **OVERVIEW.md** — person-level README with connection hooks.
- **Epistemic markers** — `[decided]`, `[concluded]`, `[assumed]`, `[superseded]`, `OUTDATED: …`

See `AGENT_INSTRUCTIONS.md` for the full spec agents follow.

## Bootstrapping participant folders with AI

`AGENT_BRIEF.md` is an optional methodology doc. Give it (plus a person's public links or a
self-written intro) to an agent and ask it to produce `OVERVIEW.md` and project LOG/SUMMARY
pairs. Review everything before committing — bootstrapped files should carry a disclaimer
header until the person has verified them.

## Privacy checklist

Before pushing:

- [ ] Every participant **opted in** to being included.
- [ ] No phone numbers in any file.
- [ ] Contact details appear only when the person shared them for this purpose.
- [ ] No secrets (`.env`, credentials, private API keys) — see `.gitignore`.
- [ ] Bootstrapped/simulated content is clearly marked; corrections are welcome.

## Reference implementation

The Foresight 2026 Secure & Sovereign AI Workshop working group built the first real instance
(the "Bubble Memory" group). Ask `<MAINTAINER>` for a link to that snapshot if you want a
filled-in example.

## License

TODO: Add a license (e.g. MIT for the template structure; participant content remains theirs).
