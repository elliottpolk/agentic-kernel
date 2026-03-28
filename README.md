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

Use the built-in `copilot-bridge` skill. It reads the source file for any agent, skill, or workflow and generates a thin wrapper under `.github/`.

**To bridge a component to Copilot**, ask GitHub Copilot:

> "Use the copilot-bridge skill to bridge the `{name}` agent/skill/workflow to Copilot."

| Artifact | Output |
|---|---|
| Agent | `.github/agents/{name}.agent.md` |
| Skill | `.github/skills/{name}/SKILL.md` |
| Workflow | `.github/prompts/{name}.prompt.md` |

No content is copied into the generated files — they reference their canonical source. Changes to the source automatically apply the next time Copilot reads the wrapper.

### Claude

Use the built-in `claude-bridge` skill. It reads the source file for any agent, skill, or workflow and generates a thin wrapper under `.claude/`.

**To bridge a component to Claude**, ask Claude:

> "Use the claude-bridge skill to bridge the `{name}` agent/skill/workflow to Claude."

| Artifact | Output |
|---|---|
| Agent | `.claude/skills/{name}/SKILL.md` |
| Skill | `.claude/skills/{name}/SKILL.md` |
| Workflow | `.claude/commands/{name}.md` |

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
