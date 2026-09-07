---
name: riff
description: >-
   Switches the conversation into discussion-first ideation mode. Preserves the current agent context, explores ideas as a researcher and domain expert, and defers edits or artifact creation until the user explicitly asks to capture something.
invocation: /riff
---

# riff

Lightweight interaction-mode workflow for thinking out loud with the current agent.

Use it when the user wants to explore, compare, pressure-test, or shape an idea without turning the conversation into planning or execution.

## Inputs

- Optional topic, problem space, decision, or question to explore.
- Optional constraints, such as audience, time horizon, tradeoffs, or non-goals.
- An optional uninterrupted brain dump before the conversation moves into context building.

## Phase 1: Enter Riff Mode

1. Resolve the topic from the invocation argument or the latest user message.
2. Open with a brief declarative invitation to brain dump, not a question. For example: "Give me the full brain dump. I will hold questions until you are ready to organize it."
3. Do not ask clarifying questions, offer interpretations, summarize, or introduce suggestions while the user is brain dumping. A message turn alone does not mark the dump as complete.
4. Treat a clear transition such as "that is the context", a request for a recap, or a solutioning signal as permission to move into context building.
5. Start in information gathering/context building mode. Treat the interaction as discussion-first ideation, not planning, solutioning, or execution.
6. Preserve the current agent and domain context. This workflow changes interaction mode only.
7. Do not edit files, create files, scaffold content, or draft durable artifacts unless the user gives an explicit capture signal.

## Phase 2: Build Context

1. Stay in researcher and expert mode.
2. Begin with a concise recap of the established facts, assumptions, constraints, and unknowns from the brain dump.
3. Build shared context in small chunks. When a gap would materially change the discussion, ask one focused question at a time or perform directly relevant read-only research.
4. Distinguish observed facts from hypotheses, and state when the available context cannot support a conclusion.
5. Do not propose approaches, compare solutions, make recommendations, or create a plan during this phase.
6. Do not silently convert the discussion into a plan, task list, workflow, or document outline unless the user asks for that shift.
7. Keep responses concise and easy to steer.

## Phase 3: Shift to Solutioning

1. Treat a clear request for ideas, an approach, recommendations, or next steps as a solutioning signal. Common signals include "Thoughts?", "How would I approach {topic}?", and "So, what next?"
2. Do not move to solutioning until the user provides a solutioning signal. If a signal arrives before sufficient context exists, identify the specific context still needed and continue gathering it.
3. Once context is sufficient and the user has signaled solutioning, recap the most relevant facts and assumptions, then offer ideas, approaches, tradeoffs, or recommendations in small chunks.
4. Keep conclusions provisional when material uncertainty remains, and pressure-test a solution when it would clarify the decision.

## Phase 4: Capture Only On Explicit Signal

1. Treat phrases such as "let's capture that", "capture that", "write that up", "turn that into a note", or another clear equivalent as a capture signal.
2. Until a capture signal appears, remain in riff mode and do not materialize artifacts.
3. When a capture signal appears:
   - if the intended artifact and destination are clear, create it
   - if the destination is ambiguous, ask the smallest clarifying question needed before writing anything
4. After the capture step is complete, ask whether to return to riff mode or continue in normal execution mode.

## Rules

- MUST preserve the active agent context unless the user explicitly asks to switch agents.
- MUST NOT make edits or create artifacts before an explicit capture signal.
- MUST invite an uninterrupted brain dump before asking clarifying questions or offering interpretations.
- MUST wait for an explicit transition before it summarizes or probes the brain dump.
- MUST start in information gathering mode before offering suggestions or solutions.
- MUST treat a solutioning signal and a capture signal as separate decisions.
- MUST keep the conversation exploratory rather than prematurely converging on execution.
- MAY pressure-test ideas when the user is still shaping them.
- MUST keep responses short, chunked, and responsive to steering.