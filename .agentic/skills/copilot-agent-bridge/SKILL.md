---
name: copilot-agent-bridge
description: Generates a GitHub Copilot .agent.md file from a .agentic agent's IDENTITY.md. Use when you want to make an agent available in GitHub Copilot Chat. Takes an agent name, reads its IDENTITY.md, and produces .github/agents/{name}.agent.md that references the canonical identity rather than duplicating it.
compatibility: GitHub Copilot in VS Code
---

# copilot-agent-bridge

Produces a GitHub Copilot `.agent.md` file from an agent defined in `.agentic/agents/`. The generated file is a thin Copilot-specific wrapper — it does not duplicate the agent's identity; it references it.

## Inputs

- **Agent name**: the directory name of the agent under `.agentic/agents/` (e.g. `kernel`, `agent-foundry`)

## Process

1. Verify `.agentic/agents/{name}/IDENTITY.md` exists. If not, stop and report.
2. Read `IDENTITY.md` and extract:
   - **Role** field: used as the Copilot `description` frontmatter value
   - **Activation** field: used to inform the `argument-hint` frontmatter value
   - **Scope** field: used to determine whether the agent is read-only (no edit tools needed) or requires write access — informs tool selection
3. Determine tools using the tool selection guide in [assets/tool-selection.md](assets/tool-selection.md)
4. Generate `.github/agents/{name}.agent.md` using [assets/agent.template.md](assets/agent.template.md)
5. Create `.github/agents/` directory if it does not exist
6. Confirm the output path to the operator

## Rules

- Never copy or paraphrase content from `IDENTITY.md` into the generated file body. The body must reference back to the source file using the Copilot file reference syntax: `[IDENTITY](.agentic/agents/{name}/IDENTITY.md)`
- Do not add tools that are not justified by the agent's Scope
- The `description` frontmatter value must be a single sentence taken from or closely derived from the Role field — not invented
- If the agent's Scope explicitly states it does not write or execute, omit all edit and terminal tools
