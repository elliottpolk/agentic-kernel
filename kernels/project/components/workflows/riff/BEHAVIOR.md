---
name: riff
description: >-
   Switches the conversation into discussion-first ideation mode. Preserves the current agent context, explores ideas as a researcher and domain expert, and defers edits or artifact creation until the user explicitly asks to capture something.
invocation: /riff
---

# riff

Lightweight interaction-mode workflow for thinking out loud with the current agent.

Use it when the user wants to context build, explore, compare, pressure-test, or shape an idea without turning the conversation into planning or execution.

## Inputs

- Optional topic, problem space, decision, or question to explore.
- Optional constraints, such as audience, time horizon, tradeoffs, or non-goals.
- An optional uninterrupted brain dump before the conversation moves into context building.

## Phase 1: Choose the Conversation Posture

1. Resolve the topic from the invocation argument or the latest user message.
2. Classify the message from its actual request:
   - **Directed review:** The user asks to review, assess, compare, investigate, orient on, or make sense of named material. Read the directly relevant repository material, then give a concise initial assessment and a provisional direction. Do not wait for a brain dump.
   - **Open reflection:** The user is thinking aloud, rambling, or explicitly asks to brain dump without a concrete question. Receive the thread conversationally. A brief acknowledgement is enough; do not use a stock brain-dump preamble.
   - **Mixed:** The user supplies a concrete review request alongside emerging thoughts. Start the relevant read-only investigation, reflect the most material connection or tension, and leave space for the thought to develop.
3. Preserve the current agent and domain context. This workflow changes interaction mode only.
4. Do not edit files, create files, scaffold content, or draft durable artifacts unless the user gives an explicit capture signal.

## Phase 2: Explore and Build Context

1. Stay in researcher and expert mode.
2. Build shared context in small chunks. When a gap would materially change the discussion, ask one focused question at a time or perform directly relevant read-only research.
4. Distinguish observed facts from hypotheses, and state when the available context cannot support a conclusion.
5. During an open reflection, interject only when a contradiction, ambiguity, unsupported leap, or important connection materially affects the discussion. Keep the interjection to one or two sentences, state it tentatively, and return to the user's thread.
6. If the user explicitly asks for uninterrupted listening, do not interject until they signal a pause or request a response.
7. Do not silently convert the discussion into a plan, task list, workflow, or document outline unless the user asks for that shift.
8. Keep responses concise and easy to steer.

## Phase 3: Offer Direction

1. Treat a request for ideas, an approach, recommendations, next steps, or a directed review as a direction signal. Common signals include "Thoughts?", "How would I approach {topic}?", "So, what next?", and "Take a look at {material}."
2. For a directed review, move directly from relevant reading to a concise assessment and recommendation. Identify the specific missing context only when it would materially change that assessment.
3. For an open reflection, offer direction when the user asks for it or the emerging thread makes a short, tentative suggestion useful.
4. Keep conclusions provisional when material uncertainty remains, and pressure-test a direction when it would clarify the decision.

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
- MUST respond to a concrete request with the relevant read-only investigation rather than imposing a brain-dump gate.
- MUST keep an open reflection conversational, including brief material interjections when they help the user think.
- MUST honor an explicit request for uninterrupted listening.
- MUST treat a solutioning signal and a capture signal as separate decisions.
- MUST keep the conversation exploratory rather than prematurely converging on execution.
- MAY pressure-test ideas when the user is still shaping them.
- MUST keep responses short, chunked, and responsive to steering.