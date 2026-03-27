# Tool Selection Guide

Use the agent's **Scope** field to determine the appropriate tool set. Select the minimal set that satisfies the agent's responsibilities — do not add tools speculatively.

## Read-only agents
For agents that only read, analyse, navigate, or answer questions (no file writes, no code execution):
```
["search", "fetch", "githubRepo", "usages", "semantic_search", "read_file", "grep_search"]
```

## Read + write agents
For agents that create or modify files as part of their core responsibilities:
```
["search", "fetch", "githubRepo", "usages", "semantic_search", "read_file", "grep_search", "replace_string_in_file", "multi_replace_string_in_file", "create_file"]
```

## Read + write + execute agents
For agents that also run commands, tasks, or tests:
```
["search", "fetch", "githubRepo", "usages", "semantic_search", "read_file", "grep_search", "replace_string_in_file", "multi_replace_string_in_file", "create_file", "run_in_terminal", "get_errors"]
```

## Decision rule
If the Scope field contains phrases like "does not write", "read-only", "navigational", or "does not execute" — use the read-only set.
If it creates or modifies files — use read + write.
If it runs commands, builds, tests, or deploys — use read + write + execute.
When in doubt, start with a smaller set. Tools can always be added; unnecessary tools expand attack surface.
