---
title: AGENTS.md
authors:
  - name: Elliott Polk
    email: elliott@tkwcafe.com
description:
created: 2026-09-16
updated: 2026-09-16
tags:
  - architecture
  - kernel-composition
change_log:
  - version: 0.1.0
    date: 2026-09-16
    description: Initial stub.
---

# AGENTS.md

Provide the portable entry point that begins the kernel initialization protocol and directs an agent to the active composition, contracts, applicable instructions, and relevant memory.

## Role

The repository root `AGENTS.md` MUST identify its target kernel and canonical `.agentic/` root. It MUST define the provider-independent session protocol for an agent operating within that repository.

## Required Frontmatter

The bootloader MUST begin with structured frontmatter that declares its identity, target kernel, ownership, and licensing:

```yaml
---
version: "{semver}"
scope: {universal|local}
role: bootloader
kernel: "{target-kernel-name}"
status: {stable|canary}
description: "{description}"
authors:
  - name: "{name}"
    email: "{email}"
copyright: "{copyright}"
license: {license}
---
```

`version` identifies the bootloader contract. `kernel` identifies the target kernel by its stable canonical name and MUST NOT be inferred from the bootloader title, body, or directory layout. `manifest.yml` MUST identify the same target kernel and a compatible kernel version; the exact compatibility rules belong in the implementation specification.

## Required Content

Every bootloader MUST contain four content elements: a root instruction, an initialization protocol, a session protocol, and a structure contract. The root instruction MUST immediately follow the document's sole H1. The remaining elements follow as named H2 sections.

## Root Instruction

The root instruction MUST explain the target kernel's purpose and establish its canonical `.agentic/` root. It MUST identify the canonical definitions as authoritative and explain that provider-specific files or adapters can realize those definitions without becoming an independent source of truth.

## Initialization Protocol

The `## Initialization Protocol` section MUST define the ordered work an agent completes before taking action:

1. Read `manifest.yml` to establish the repository's installed composition and relationships.
2. Read the kernel behavior, decision, and memory contracts in `core/`.
3. Load applicable repository instructions.
4. Read relevant state and recent history from `memories/`.

This ordering establishes composition and operating rules before project context is applied.

## Session Protocol

The `## Session Protocol` section MUST define the responsibilities that follow initialization:

- At session start, an agent MUST complete initialization before acting and verify mutable state from memory.
- During a session, an agent MUST follow core contracts, update state and history when reality changes, and maintain the manifest when the installed composition changes.
- At session end, an agent MUST ensure changed state is represented in `memories/` and append a session summary to history.

## Structure Contract

The `## Structure Contract` section MUST identify `.agentic/` as the canonical repository-local kernel root and describe the responsibilities of `manifest.yml`, `core/`, `components/`, `instructions/`, and `memories/`. It MUST distinguish kernel-owned contracts from repository-specific composition and context. It MUST NOT prescribe provider invocation, delegation, permission, or precedence behavior. The exact directory tree and file schemas belong in the implementation specification.