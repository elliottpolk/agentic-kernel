# Agentic Kernel

A platform-agnostic foundation for building stateful, multi-agent systems.

## Adoption

To use this kernel in a project repo:

1. Copy the `.agentic/` directory into the target repo
2. Copy `AGENTS.md` to the root of the target repo
3. Add project-specific agents to `.agentic/agents/`
4. Add project-specific workflows to `.agentic/workflows/`
5. Add project-specific skills to `.agentic/skills/`
6. Register all additions in `.agentic/manifest.yml`
7. Seed `.agentic/memories/state/` with relevant project facts

Do not modify `.agentic/core/` without propagating changes back upstream to the kernel.
