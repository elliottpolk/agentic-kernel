# Memory

How agents read, write, and curate the composition's memory: named files in the `memories/` directory.

Memory is the composition's long-term context. It exists so a future session, on any platform, can orient without reconstructing reality from scratch. It is not a scratchpad, a task tracker, or a transcript.

## What Belongs in Memory

Each memory file answers one question: "What is true right now that any future agent session would need to know?"

Write a memory when you learn a durable fact:

- People: who someone is, their role, and how to engage them effectively (teammates, maintainers, key contributors, stakeholders)
- Structure: how the work is organized and who owns what (teams, governance bodies, maintainer areas, module ownership)
- Environment: the systems, tools, and constraints the project operates under (platforms, services, pipelines, upstream dependencies)
- The project itself: its intent, canonical role, and standing conventions
- Decisions that are settled (not pending)

Do NOT write day-to-day state:

- Task progress, open items, and session scratch belong to the platform's task mechanism (see BEHAVIOR.md), not memory
- Anything ephemeral or meaningful only within a single session
- Pending or future events written as fact: write "as of {date}: pending" or leave them out until they settle
- Content addressed to a human ("you should know..."): that is a briefing, not memory
- Behavioral rules: those belong in contracts (BEHAVIOR.md, IDENTITY.md), not memory

Memory files reflect current reality only:

- One file per entity (person, team, system, project), not per interaction
- Replace outdated content in place and update the `Last updated:` field
- No changelogs of old values; outdated facts are replaced, not preserved
- When an entity no longer exists (person leaves, system decommissioned, project ends), remove the file or mark it inactive

## Naming

| Subject | Convention | Example |
|---|---|---|
| Person | `{first-last}.md` | `jordan-okafor.md` |
| Team, system, or domain | Short descriptive slug | `release-process.md` |
| Governance body | Short name slug | `tech-council.md` |
| Project | `project.md` | |

These are conventions, not requirements. The composition decides which memory files exist; a small project may only ever need `project.md`.

## Reading Memory

- At session start, read the memory files relevant to the work at hand.
- Do not trust memory blindly. Verify against reality before acting on a remembered fact (see BEHAVIOR.md).

## Boundaries

- Platform-native memory mechanisms (vendor stores, session recall) are supplementary and platform-specific; the kernel does not define them. `memories/` is the canonical, portable store.
- Day-to-day operational state (which tasks are open, which step is next) is not memory.
- Narrative history (how things came to be, past decision reasoning) is not governed here. If the composition wants it, capture it in dated project artifacts, not agent memory.
- The test: if a fact would matter to a session months from now, it belongs in memory. If it matters only until the current work is done, it does not.