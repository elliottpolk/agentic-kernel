# Agentic Kernel

A platform-agnostic foundation for building stateful, multi-agent systems.

## Purpose

AI assistants have no built-in memory between sessions. Without a persistent structure, every conversation starts from scratch — no context, no history, no continuity.

The agentic kernel solves this by giving AI a consistent home in your repo: a defined initialization protocol, a shared memory system, and a registry of agents and capabilities. Any AI assistant that reads `AGENTS.md` at session start knows where it is, what it can do, and what has happened before.

The kernel itself is platform-agnostic. Platform-specific bridges (e.g. GitHub Copilot `.agent.md` files) are generated from the canonical agent definitions, not the other way around.

---

## Setup

### 1. Copy the kernel into your repo

```sh
cp -r .agentic/ /path/to/your-repo/.agentic/
cp AGENTS.md /path/to/your-repo/AGENTS.md
```

### 2. Declare your project

Open `.agentic/manifest.yml` and fill in the project block:

```yaml
project:
  name: "your-project-name"
  description: "What this repo is and what it does."
  context: .agentic/memories/state/project.md
```

Then create `.agentic/memories/state/project.md` and seed it with the facts an AI should know at the start of every session: what the system does, its tech stack, key constraints, team conventions, and any other standing context.

### 3. Add project-specific agents, workflows, and skills

| What | Where | Notes |
|---|---|---|
| Agents | `.agentic/agents/{name}/` | Each needs at minimum `IDENTITY.md`, `notes/`, `memories/` |
| Workflows | `.agentic/workflows/` | Register each in `manifest.yml` |
| Skills | `.agentic/skills/` | Register each in `manifest.yml` |

Register every addition in `.agentic/manifest.yml`.

> Do not modify `.agentic/core/` without propagating changes back upstream to this kernel.

---

## Making Agents Available on Each Platform

Agents are defined canonically in `.agentic/agents/`. To use them on a specific AI platform, generate a thin platform-specific wrapper that references the canonical identity rather than duplicating it.

### GitHub Copilot

Use the built-in `copilot-agent-bridge` skill. It reads an agent's `IDENTITY.md` and generates a `.github/agents/{name}.agent.md` file.

**To generate a Copilot agent wrapper**, ask GitHub Copilot:

> "Use the copilot-agent-bridge skill to generate a Copilot agent for `{agent-name}`."

The generated file will look like this:

```markdown
---
description: "{Role field from IDENTITY.md}"
name: "{agent-name}"
argument-hint: "{Activation field from IDENTITY.md}"
tools: [...]
---

[IDENTITY](.agentic/agents/{agent-name}/IDENTITY.md)
```

The file body contains only a reference back to `IDENTITY.md` — no content is copied. Changes to the agent's identity automatically apply the next time Copilot reads the file.

### Claude

> **Not yet implemented.** A Claude bridge skill is planned. When available, it will follow the same pattern: read `IDENTITY.md`, produce a Claude-native configuration file that references rather than duplicates the canonical identity.

### Gemini

> **Not yet implemented.** A Gemini bridge skill is planned. When available, it will follow the same pattern: read `IDENTITY.md`, produce a Gemini-native configuration file that references rather than duplicates the canonical identity.

---

## Structure Reference

```
.agentic/
├── manifest.yml          # Registry: active agents, workflows, skills, memories
├── core/                 # Universal behavioral rules (treat as immutable)
│   ├── BEHAVIOR.md       # Tone, response style, ADHD-friendly rules, memory protocol
│   └── DECISIONS.md      # NEED/WANT/MAY decision framework
├── agents/               # Domain-specific agent identities
│   └── {agent-name}/
│       ├── IDENTITY.md   # Required: role, domain, scope, operating principles
│       ├── notes/        # Required: ongoing observations and adjustments
│       ├── memories/     # Required: agent-specific interaction history
│       ├── assets/       # Optional: templates and resources
│       └── references/   # Optional: documentation
├── workflows/            # Sequences of actions for specific tasks
├── skills/               # Capabilities and functionalities
└── memories/
    ├── state/            # Mutable reference facts (project, people, systems)
    └── history/          # Append-only narrative (decisions, reasoning, context)
```
