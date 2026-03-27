---
name: claude-agent-bridge
description: Generates a Claude skill file from a .agentic agent's IDENTITY.md. Use when you want to make an agent available in Claude Code or Cowork. Takes an agent name, reads its IDENTITY.md, and produces .claude/skills/{name}/SKILL.md that references the canonical identity rather than duplicating it.
compatibility: Claude Code, Cowork
---

# claude-agent-bridge

Produces a Claude skill file from an agent defined in `.agentic/agents/`. The generated file is a thin Claude-native wrapper — it does not duplicate the agent's identity; it references it. The same output file works across Claude Code and Cowork; no separate file is needed for each surface.

## Inputs

- **Agent name**: the directory name of the agent under `.agentic/agents/` (e.g. `kernel`, `agent-foundry`)

## Process

1. Verify `.agentic/agents/{name}/IDENTITY.md` exists. If not, stop and report.
2. Read `IDENTITY.md` and extract:
   - **Role** field: used as the skill `description` frontmatter value
   - **Activation** field: used to inform the trigger conditions appended to the description
   - **Scope** field: used to determine tool permissions using [assets/tool-selection.md](assets/tool-selection.md)
3. Generate `.claude/skills/{name}/SKILL.md` using [assets/agent.template.md](assets/agent.template.md)
4. Create `.claude/skills/{name}/` directory if it does not exist
5. Confirm the output path to the operator
6. If the agent's `IDENTITY.md` includes a **Model** preference field, note the recommended model in the confirmation message but do not embed it in the generated file

## Rules

- Never copy or paraphrase content from `IDENTITY.md` into the generated file body. The body must reference back to the source file using a relative Markdown link: `[IDENTITY](.agentic/agents/{name}/IDENTITY.md)`
- Do not add tools that are not justified by the agent's Scope — use [assets/tool-selection.md](assets/tool-selection.md) strictly
- The `description` frontmatter value must be a single sentence taken from or closely derived from the Role field — not invented. Append the Activation field as a trigger clause (e.g. "Activate when…")
- If the agent's Scope explicitly states it does not write or execute, omit all write and execute tools
- The `compatibility` frontmatter value is always `Claude Code, Cowork` regardless of the agent's domain

## Claude API / SDK Usage

For use with the Claude API directly, no bridge file is required. The agent's `.agentic/agents/{name}/IDENTITY.md` is already the system prompt — pass its contents as the `system` parameter when calling the API. Mention this to the operator after generating the skill file.
