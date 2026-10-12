---
name: codex-bridge
description: >-
  Bridges .agentic agents, skills, and workflows into OpenAI Codex. Produces thin
  Codex-specific wrapper files that reference canonical sources rather than duplicating
  them. Use when making any .agentic component available in Codex.
compatibility: OpenAI Codex
---

# codex-bridge

Produces Codex wrapper files from components defined in `.agentic/`. No content is copied
from source files — all generated files reference back to their canonical source.

## Artifact Types and Output Paths

| Input | Source | Output |
|---|---|---|
| Agent | `.agentic/agents/{name}/IDENTITY.md` | `.codex/skills/{name}-agent/SKILL.md` |
| Skill | `.agentic/skills/{name}/SKILL.md` | `.codex/skills/{name}/SKILL.md` |
| Workflow | `.agentic/workflows/{name}/WORKFLOW.md` | `.codex/skills/{workflow-skill-name}/SKILL.md` |

Codex exposes reusable repository behavior through Agent Skills. Agents and workflows are
therefore represented as Codex skill wrappers that activate the canonical identity or execute
the canonical workflow.

## Inputs

- **Type**: one of `agent`, `skill`, or `workflow`
- **Name**: the directory name under the relevant `.agentic/` subdirectory

## Process: Agent

1. Verify `.agentic/agents/{name}/IDENTITY.md` exists. If not, stop and report.
2. Read `IDENTITY.md` and extract:
   - **Role** field: used in the wrapper description
   - **Activation** field: appended as a trigger clause to the wrapper description
3. Generate `.codex/skills/{name}-agent/SKILL.md` using [assets/agent.template.md](assets/agent.template.md).
4. Create `.codex/skills/{name}-agent/` if it does not exist.
5. Confirm the output path.
6. If `IDENTITY.md` includes a **Model** preference field, note it in the confirmation but do not embed it in the generated file.

## Process: Skill

1. Verify `.agentic/skills/{name}/SKILL.md` exists. If not, stop and report.
2. Read `SKILL.md` and extract the `name` and `description` frontmatter fields.
3. Generate `.codex/skills/{name}/SKILL.md` using [assets/skill.template.md](assets/skill.template.md).
4. Create `.codex/skills/{name}/` if it does not exist.
5. Confirm the output path.

## Process: Workflow

1. Verify `.agentic/workflows/{name}/WORKFLOW.md` exists. If not, stop and report.
2. Read `WORKFLOW.md` and extract the `name`, `description`, and `invocation` frontmatter fields.
3. Derive `{workflow-skill-name}`:
   - If `invocation` exists and starts with `/`, strip the leading `/` and append `-workflow`.
   - If `invocation` is empty or missing, use `{name}-workflow`.
4. Generate `.codex/skills/{workflow-skill-name}/SKILL.md` using [assets/workflow.template.md](assets/workflow.template.md).
5. Create `.codex/skills/{workflow-skill-name}/` if it does not exist.
6. Confirm the output path.

## Rules

- Never copy or paraphrase content from source files into generated wrapper bodies. Always reference back.
- The `name` and `description` frontmatter values must be derived from the canonical source, not invented.
- Agent wrappers must direct Codex to read the full canonical `IDENTITY.md` before acting in that role.
- Workflow wrappers must direct Codex to read and follow every phase and rule in the canonical `WORKFLOW.md`.
- Keep generated wrappers thin; platform adaptation belongs in the wrapper, while behavioral truth remains canonical under `.agentic/`.
- Do not add unsupported tool declarations or speculative Codex-specific fields.
