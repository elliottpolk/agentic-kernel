# Agentic Kernel

A platform-agnostic foundation for building stateful, multi-agent systems.

## Purpose

Two problems make AI-assisted development harder than it needs to be.

**Memory.** AI assistants have no built-in memory between sessions nor between platforms. Without a persistent structure, every conversation starts from scratch: no context, no history, no continuity.

**Platform fragmentation.** Every AI coding platform defines its own format for enabling agent personas, skills, memory management, and workflows. A Copilot agent looks nothing like a Claude command or a Gemini skill. Teams working across tools end up maintaining separate, duplicated definitions for each platform, and switching between coding agents or providers means abandoning whatever context and history the previous one accumulated.

The agentic kernel is an attempt at solving both. It gives AI a consistent home in your repo: a defined initialization protocol, a shared memory system, and a registry of agent personas and capabilities. Any AI assistant that reads `AGENTS.md` at session start knows where it is, what it can do, and what has happened before. Bridge skills generate thin, platform-specific wrappers from the canonical definitions in `.agentic/`, so teams write once and deploy to any platform.

---

> **Status: work in progress.** This repository is mid-refactor toward the structure described below. Some pieces are stubs or placeholders, and details may change as the work lands.

## Repository Structure

```
├── kernels/      # Kernel specs: complete, adoptable kernel definitions
├── components/   # Component catalog: reusable, provider-independent capabilities
├── adapters/     # Per-provider adapter specs
├── akctl/        # Management CLI (predates the refactor; rebuild planned)
└── docs/         # Architecture documentation
```

### Kernels

`kernels/` holds the kernel specifications. A kernel is a complete definition that any repository can adopt: an `AGENTS.md` bootloader that starts the initialization protocol, a manifest recording the active composition, `core/` contracts for behavior, decisions, and memory, slots for components, and a memory store.

| Kernel | Purpose | Status |
|---|---|---|
| [project](kernels/project/) | Software development (applications, services, libraries, systems) in a source-controlled repository, operated through a coding agent | canary |
| [journal](kernels/journal/) | Personal journal and knowledge management | legacy |
| [legacy](kernels/legacy/) | The original `.agentic/` layout, kept for reference while the refactor completes | legacy |

### Components

`components/` is the catalog of portable, provider-independent building blocks. A kernel composition activates components by declaring them in its manifest; presence on disk alone does not activate anything.

| Kind | Description |
|---|---|
| Personas | Agent identities (architect, cartographer, engineer, historian, project-manager, tester), each defined by an `IDENTITY.md` plus optional assets |
| Skills | Capabilities following the Agent Skills standard |
| Workflows | Sequences of actions for specific tasks, such as `seed` (stub a new artifact) and `riff` (explore an idea before capturing anything) |
| Instructions | Behavioral extensions, optionally scoped by an `applyTo` glob |
| Plugins | Bundles of related components |

### Adapters

`adapters/` holds the per-provider adapter specs. Adapters realize the canonical composition through each provider's native capabilities (agents, skills, commands, instructions) when a platform cannot load the canonical structure directly. Adapter output is always derived; the canonical definitions in the kernel remain the single source of truth.

Specs are planned for Claude, Codex, GitHub Copilot, Gemini, Kimi, and Qoder. All are currently placeholders.

### Tooling

`akctl/` is the management CLI for the kernel. The current version predates the refactor: it scaffolds and maintains the legacy kernel layout (`init` and `sync`). It will be rebuilt as a combined marketplace view and management tool, used to browse the catalog of kernels and components, install them into a project, and keep installations in sync with upstream.

### Architecture

`docs/architecture/kernel-composition/` specifies the target model:

| Document | Covers |
|---|---|
| [intent](docs/architecture/kernel-composition/intent.md) | Motivation and cross-platform observations |
| [bootloader](docs/architecture/kernel-composition/bootloader.md) | The `AGENTS.md` entry point and initialization protocol |
| [manifest](docs/architecture/kernel-composition/manifest.md) | The authoritative composition record |
| [core](docs/architecture/kernel-composition/core.md) | Behavior, decision, and memory contracts |
| [components](docs/architecture/kernel-composition/components.md) | Portable representation of capabilities and relationships |
| [memory](docs/architecture/kernel-composition/memory.md) | Repository-bounded context as an attributable graph |
| [adapters](docs/architecture/kernel-composition/adapters.md) | Provider realization rules |
| [tooling](docs/architecture/kernel-composition/tooling.md) | Install, inspect, validate, and sync lifecycle |

