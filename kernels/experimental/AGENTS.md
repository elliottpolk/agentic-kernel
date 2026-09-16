---
version: 1.0.0
scope: universal
role: bootloader
kernel: experimental
status: canary
description: Experimental agents kernel for designing, testing, and validating new features and capabilities.
authors:
  - name: Elliott Polk
    email: elliott@tkwcafe.com
copyright: "Copyright (c) 2026 The Karoshi Workshop"
license: MIT
---

# Agentic Kernel

This file is the bootloader for the experimental kernel: a platform-agnostic foundation for building stateful, multi-agent systems. It begins the initialization protocol and directs you to the kernel's contracts, components, and memory.

The `.agentic/` directory adjacent to this file is the canonical kernel root. All paths referenced in this file are relative to `.agentic/` unless stated otherwise. It holds the authoritative, provider-independent definition of the composition: its contracts in `core/`, its capabilities in `components/`, and its memory in `memories/`. Provider-specific files and adapters may realize those definitions on a given platform, but they are derived artifacts and never an independent source of truth.

## Initialization Protocol

On every session start, complete these steps in order before taking any action:

1. Read `manifest.yaml` to establish the active composition: which components are installed, how they relate, and what memory is available
2. Read the kernel contracts in `core/`:
   - `core/BEHAVIOR.md` for universal behavioral rules
   - `core/DECISIONS.md` for the decision-making framework
   - `core/MEMORY.md` for memory conventions and update rules
3. Load global instructions from `components/instructions/`: files whose frontmatter declares `applyTo: "**"`. Instructions with narrower `applyTo` globs apply when working on files matching that pattern. A file without `applyTo` is never auto-loaded.
4. Read `PROJECT.md` adjacent to this file, if present, to learn what the repository actually is: its fragile zones, accepted trade-offs, and the reasoning behind its current state
5. Orient to current state and recent history as directed by `core/MEMORY.md`

This ordering establishes composition and operating rules before project context is applied.

If a file required by this protocol is missing or unreadable, that is a kernel fault: halt, report what is missing, and do not improvise or continue from assumptions. `PROJECT.md` is the sole exception; it is optional.

## Session Protocol

### Start
- Complete the initialization protocol before taking any action
- Do not assume state. Verify from memory files.
- A persona's `Activation` field describes when it is relevant. How a persona is invoked is determined by the platform; the kernel does not control invocation.

### During
- Follow the behavioral rules in `core/BEHAVIOR.md` at all times
- Apply the decision framework from `core/DECISIONS.md` when prioritizing or resolving conflicts
- Record what was done, decisions made, and open items as directed by `core/MEMORY.md`
- Update `manifest.yaml` immediately when any component or memory file is added or removed

### End
- Ensure the session's changes to reality are recorded per `core/MEMORY.md` before the session closes

## Structure Contract

```
.agentic/
├── manifest.yaml       # Authoritative composition record: active components, relationships, dependencies
├── core/               # Kernel space: contracts every composition runs against
│   ├── BEHAVIOR.md     # Tone, response style, memory protocol
│   ├── DECISIONS.md    # Decision-making framework
│   ├── MEMORY.md       # Memory conventions: naming, update rules, boundaries
│   └── adapters/       # Drivers: per-provider realization of the canonical composition
├── components/         # User space: composable capabilities
│   ├── personas/       # Agent identities (IDENTITY.md + assets/)
│   ├── skills/         # Capabilities following the Agent Skills standard
│   ├── workflows/      # Sequences of actions for specific tasks (BEHAVIOR.md + assets/)
│   ├── instructions/   # Behavioral extensions, optionally scoped by applyTo glob
│   └── plugins/        # Bundles of related components
└── memories/
    ├── state/          # Mutable reference facts about current reality
    └── history/        # Append-only narrative: decisions, reasoning, how we got here
```

`core/` is kernel-owned. It is treated as immutable within an installation and changes only through a kernel upgrade. `components/` and `memories/` are composition-owned: the repository that adopts the kernel extends, replaces, and curates them.

A component's presence on disk does not activate it. `manifest.yaml` is the authoritative record of what is active: it declares which components the composition uses and their dependencies, relationships, and load order. Provider discovery conventions may surface canonical artifacts, but they do not redefine the composition.

`PROJECT.md` at the repository root is not part of the kernel. It is a project artifact: curated, team-authored context describing what the repository actually is, as distinct from what it was designed to be. The kernel consumes it during initialization; the team owns it.

This contract does not prescribe provider invocation, delegation, permission, or precedence behavior. Those remain platform concerns.

