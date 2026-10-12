---
name: memory-compact
description: Compacts aged memory files to control context growth. Summarizes history older than the configured hot window into compact digests, drops the aged detail, and sweeps stale state entries. Git is the cold archive; nothing is cold-stored in the repo.
---

# memory-compact

Compacts aged memory files to prevent unbounded context growth. Summarizes history older than the hot window into compact digests, drops the aged detail, and sweeps stale state entries.

**Trigger:** Invoke when asked to compact memory, when history files exceed the configured size threshold, or as part of a periodic maintenance routine.

## Settings

Read compaction settings from `manifest.yml` under `memories.compaction`:

```yaml
memories:
  state: .agentic/memories/state
  history: .agentic/memories/history
  compaction:
    hot_window_days: 30   # history older than this is eligible for compaction
    trigger_kb: 50        # flag when total history size exceeds this threshold
```

If `manifest.yml` has no `compaction:` block, use the defaults: `hot_window_days: 30`, `trigger_kb: 50`.

## Protocol

### Step 1: Check Size

1. Compute the total size of all files in the history directory (`memories.history`).
2. If total size is below `trigger_kb` and invocation was not explicit (i.e., auto-triggered), report the current size and exit without compacting.
3. If total size is at or above `trigger_kb`, or if invocation was explicit, proceed.

### Step 2: Compact History

For each file in the history directory:

1. Identify all entries older than `hot_window_days` from today's date.
2. If no aged entries exist in this file, skip it.
3. Collect the aged entries and produce a compact summary:
   - Key decisions made and the reasoning behind each
   - Key facts established
   - Notable patterns or open items carried forward
   - Date range covered
4. Replace the aged entries in the file with the following digest block:

```markdown
<!-- compacted: {today's date} | covers: {earliest-date} to {latest-date} -->
## Compacted Summary ({earliest-date} to {latest-date})

**Key decisions:** {brief list of decisions and reasoning}
**Key facts established:** {brief list}
**Notable patterns or open items carried forward:** {brief list}
```

5. MUST NOT modify entries within the hot window.

### Step 3: Sweep State

For each file in the state directory (`memories.state`):

1. Identify entries or files that indicate any of the following:
   - A person who has left the org or team
   - A system or project that is decommissioned or archived
   - A decision superseded by a newer decision
   - Content that clearly no longer reflects current reality
2. Remove stale entries. If a file is entirely stale, delete it.
3. Update any files that are partially stale: remove the outdated content and update the `Last updated:` field.

### Step 4: Register the Compaction Run

Append a brief entry to the current history file noting:

- Date of compaction run
- Files compacted and date ranges covered
- State files or entries removed

### Step 5: Report

Briefly report what was done:

- Number of history files compacted and date ranges covered
- Number of state files or entries removed
- Total history size after compaction

Do not produce verbose output. One summary block is sufficient.

### Step 6: Commit

Commit all changes produced by this compaction run with a message in this form:

```
chore: compact memory ({date-range})

Compacted history older than {hot_window_days}d. Swept stale state entries.
```

## Notes

- The original history detail is dropped after compaction. Git is the cold archive.
- MUST NOT alter entries within the hot window.
- State files are not summarized; they are updated or deleted.
- If compaction is invoked on a repo where history files do not yet exist or are empty, report that and exit cleanly.
