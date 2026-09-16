---
title: Kernel Composition
authors:
  - name: Elliott Polk
    email: elliott@tkwcafe.com
description: Proposed architecture for persistent, platform agnostic agent context.
created: 2026-09-16
updated: 2026-09-16
tags:
  - architecture
  - kernel-composition
change_log:
  - version: 0.1.0
    date: 2026-09-16
    description: Initial proposal.
---

# Kernel Composition

AI assistants need persistent, platform agnostic project context between sessions. Platform specific configuration models make that context difficult to maintain consistently, because agents, skills, workflows, and instructions are otherwise defined and updated separately for each platform.

## Observations

- Coding agent platforms are converging on common extensibility concepts: repository instructions, specialized agents, reusable skills, installable packages, and external tool protocols.
- `SKILL.md` has the broadest cross platform adoption. Multiple providers explicitly implement the Agent Skills specification or support compatible skill directories.
- `AGENTS.md` adoption is meaningful but incomplete. Some platforms load it directly, while others prioritize provider specific instruction files. Discovery, scope, precedence, and supported content vary.
- MCP has broad support for connecting agents to external tools and context, but it does not standardize agent personas, project instructions, skills, or persistent memory.
- No shared standard defines agent personas. Providers use incompatible metadata, discovery paths, tool controls, delegation models, and invocation behavior.
- Agent Plugins defines a narrow portability floor around `plugin.json`, Agent Skills, and MCP servers. Provider plugin systems commonly add incompatible agents, hooks, commands, rules, workflows, and system instructions.
- Standards support varies across a provider's CLI, editor, cloud agent, code review, and application surfaces. Support in one surface does not imply provider wide compatibility.
- Convergent filenames and directory structures do not guarantee behavioral portability. Activation, loading order, precedence, permissions, and version support remain platform specific.
- No current standard defines a complete portable model for preserving and loading a project's long lived agent context across sessions and platforms.

### Representative Provider Evidence

| Provider | Personas | Skills | Plugins | Instructions |
| --- | --- | --- | --- | --- |
| OpenAI Codex | Specialized subagents and parallel delegation | Native `SKILL.md` | Agent Plugins portability floor for skills and MCP | Layered `AGENTS.md` |
| Anthropic Claude | Markdown subagents with configurable tools, model, memory, and skills | Native `SKILL.md` | `.claude-plugin/plugin.json` bundles skills, agents, hooks, MCP, LSP, and monitors | `CLAUDE.md` and `.claude/rules/`; `AGENTS.md` requires import |
| Google Gemini | Built-in and custom subagents with independent prompts, tools, and context | Agent Skills standard; `.agents/skills/` takes precedence over `.gemini/skills/` | `gemini-extension.json` bundles context, skills, subagents, commands, MCP, hooks, and themes | `GEMINI.md` |
| Alibaba Qoder | Markdown agents with independent context, system prompt, tools, skills, and MCP | `SKILL.md` in Qoder specific directories | `.qoder-plugin/plugin.json` bundles agents, skills, rules, workflows, hooks, output styles, and MCP | CLI loads `AGENTS.md`; IDE also uses `.qoder/rules/` |
| Moonshot AI Kimi Code | Markdown main agents and subagents with interoperable `.agents/agents/` discovery | `SKILL.md` with interoperable `.agents/skills/` discovery | `kimi.plugin.json` or `.kimi-plugin/plugin.json` bundles agents, skills, commands, hooks, system instructions, and MCP | Native `AGENTS.md`; `SYSTEM.md` can replace the main system prompt |
| GitHub Copilot | Custom agent profiles with prompts, tools, and MCP | Agent Skills standard across multiple Copilot surfaces | Copilot CLI implements Agent Plugins 1.0 alongside a legacy format | CLI recognizes `AGENTS.md`; other surfaces also use GitHub specific instruction files |

## Proposed Architectural Solution

The kernel will provide a versioned, manifest driven system for defining and preserving portable agent capabilities and context within a repository. It separates kernel owned contracts from composable capabilities, records explicit composition, preserves context across sessions, and realizes that context on supported platforms.

`core/` is kernel space. It contains the kernel's behavior, decision, and memory contracts, plus provider adapters. `components/` contains a platform agnostic representation of composable capabilities. It can express established cross platform patterns, such as skills and plugins, alongside capabilities without a shared standard, including agent personas, workflows, and custom instructions. Components can reference one another to describe composition and relationships: a plugin can reference several skills, and a persona can declare a handoff to another persona.

`manifest.yml` is the authoritative composition record. It declares active components and their dependencies. Provider bindings are optional and used only when a platform cannot directly load the canonical structure or benefits from a native entry point. A component's presence on disk does not activate it. This decouples kernel composition from provider specific discovery conventions.

An embedded memory graph preserves concise, source controlled context in two logical domains: project context for the repository where the kernel is installed, and agent or work memory created within that repository. It records claims, relationships, provenance, status, and references to source material. The graph stays small and contains only information appropriate for the repository's access boundary. Secrets and information restricted beyond that boundary are excluded.

Adapters transform active components and relevant graph context into the native files, instructions, agents, skills, plugins, commands, and tool configuration supported by each provider. The canonical model remains provider independent. An adapter can support additional provider capabilities without making them requirements of the kernel model.

## Desired Outcome

A repository can define, adopt, activate, and evolve its agent capabilities and long lived context once in a portable, inspectable form. A canonical bootloader and manifest identify the active composition, its relationships, and the repository bounded memory relevant to a session.

Platforms that support common conventions consume the canonical structure directly. Where native realization adds value or direct loading is unavailable, optional bindings and adapters express the same composition in platform specific forms. The canonical model remains the source of truth, so teams can use provider capabilities without duplicating definitions or allowing their persistent context to drift.

## Boundaries

### In Scope

 - A canonical bootloader, kernel contracts, component representation, and manifest that define an active agent composition and its dependencies.
 - Portable representation of agent personas, skills, plugins, workflows, instructions, and references among them.
 - Repository bounded, source controlled graph memory for project context and agent or work memory, including provenance and references to source material.
 - Direct use of the canonical structure by platforms that support its conventions.
 - Optional provider bindings and adapters that realize active components and relevant context through a platform's native capabilities, including additive provider features.

### Out of Scope

 - A universal agent runtime or a standard that requires providers to share invocation, delegation, tool permission, loading, or precedence semantics.
 - Complete feature parity across providers or preservation of every provider specific capability in the canonical model.
 - Hosted memory services, cross repository memory, or storage of secrets and information beyond the repository's access boundary.
 - Provider account management, authentication, billing, model selection, service deployment, or external tool hosting.
 - Replacing existing standards for Agent Skills, Agent Plugins, MCP, or provider owned configuration formats.

## Architectural Constraints

 - The canonical bootloader, `core/`, `components/`, manifest, and embedded memory graph MUST remain provider independent and repository local.
- The manifest MUST be the authoritative record of the kernel composition it manages, including component relationships and dependencies. Providers MAY discover canonical artifacts according to their native conventions.
 - Components MUST have stable identities and explicit references. A component reference MUST remain meaningful independently of any provider's directory layout or invocation model.
 - Canonical component content MUST remain portable. Provider specific syntax, metadata, and behavior belong in optional bindings or adapters.
 - An adapter MUST derive provider artifacts from the active canonical composition. It MUST NOT become an independent source of truth or require unsupported providers to emulate another provider's behavior.
 - Memory MUST remain concise, source controlled, attributable, and bounded by the repository's access boundary. It MUST exclude secrets and information with more restrictive access requirements.
 - The kernel MUST preserve existing external standards where they apply and MAY extend the canonical model only for capabilities that those standards do not define.

## References

- [AGENTS.md](https://agents.md/)
- [Agent Skills](https://agentskills.io/)
- [Agent Plugins](https://agent-plugins.org/)
- [Model Context Protocol](https://modelcontextprotocol.io/)
- [OpenAI Codex customization](https://developers.openai.com/codex/)
- [Claude Code extension features](https://code.claude.com/docs/en/features-overview)
- [Claude Code plugins reference](https://code.claude.com/docs/en/plugins-reference)
- [Gemini CLI Agent Skills](https://geminicli.com/docs/cli/skills/)
- [Gemini CLI extensions](https://geminicli.com/docs/extensions/)
- [Qoder knowledge base](https://docs.qoder.com/cli/knowledge-base)
- [Qoder plugin reference](https://docs.qoder.com/cli/plugins-reference)
- [Kimi Code customization](https://www.kimi.com/code/docs/en/kimi-code-cli/customization/skills.html)
- [Kimi Code plugins](https://www.kimi.com/code/docs/en/kimi-code-cli/customization/plugins.html)
- [GitHub Copilot Agent Skills](https://docs.github.com/en/copilot/concepts/agents/about-agent-skills)
- [GitHub Copilot CLI plugin reference](https://docs.github.com/en/copilot/reference/copilot-cli-reference/cli-plugin-reference)
