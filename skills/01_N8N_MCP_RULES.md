# N8N MCP OPERATING RULES

When connected to n8n through MCP:

Mandatory sequence:

1. workflow_list
2. workflow_get
3. analysis
4. proposal
5. approval
6. execution

Never:

- delete workflows automatically
- overwrite workflows blindly
- activate workflows automatically
- modify credentials

Always provide:

- workflow id
- workflow name
- purpose
- affected nodes
- risk level

Before changing a workflow:

Generate:

CHANGE SUMMARY
EXPECTED EFFECT
ROLLBACK PLAN

Preferred mode:

READ ONLY

Write operations require explicit user approval.

---

# WORKFLOW SDK — PRACTICAL LESSONS (2026-06-23)

Learned while extending Nexvoid_Memory_Service with get_file/update_file actions.

## Switch node branching

In generated SDK code, `.onCase()` for a switch node takes a **numeric output index**, not the string `outputKey` used inside the rule definition. The `outputKey` strings are display/UI metadata only.

```
// WRONG — fails with "Method 'onCase' is not allowed" or silent parse failure
.onCase('get_file', someBranch)

// RIGHT
.onCase(4, someBranch)  // index matches position in rules.values[]
```

There is no `.onDefault()` for switch — the fallback branch is just another numeric `.onCase(N, ...)`, where N is the index after all defined rules (matches `renameFallbackOutput` conceptually, but is wired by position).

## Every node needs an `output` sample

`validate_workflow` / `update_workflow` will fail with the opaque error `Cannot convert undefined or null to object` if **any** node in the graph is missing an `output: [...]` sample array — including Set, Code, Switch, and trigger nodes, not just HTTP Request nodes. Add a realistic one-item sample to every node definition before validating. This error gives no indication of *which* node is missing it — if it appears, audit the whole file for missing `output:` fields first, before suspecting routing/syntax issues.

## Credentials are not auto-bound to new HTTP nodes

Adding `credentials: { httpHeaderAuth: newCredential('GitHub PAT') }` to a new HTTP Request node in SDK code, then calling `update_workflow`, does **not** reliably bind the credential — the tool reports the node as "skipped during credential auto-assignment" and the node's live JSON has no `credentials` key. Real executions then fail with `Credentials not found`.

Workaround that is confirmed to work: bind the credential manually in the n8n UI (open node → Credential to connect with → select existing credential → save). After manual binding, the node's JSON shows `credentials: { httpHeaderAuth: { id, name } }` and live calls succeed. Do not assume `newCredential()` in code is sufficient for nodes added via `update_workflow` — verify with a real (non-pinned) execution before relying on a new HTTP node.

## Testing reality check

`test_workflow` with pin data **always** pins HTTP Request nodes to synthetic data — it cannot validate real external calls or real credential bindings, no matter how realistic the pin data looks. A workflow that passes `test_workflow` can still fail in production with `Credentials not found` or similar. To confirm a new HTTP-calling branch actually works, run a real `execute_workflow` (e.g. via a temporary webhook trigger if the workflow has no other directly-executable trigger) and inspect the actual execution result — don't stop at a clean `test_workflow` pass.

## Incremental extension pattern that worked

For a workflow whose action set grows over time (more actions added later), keep each action as a self-contained branch off one switch node: one validation/code node + the actual HTTP call(s) + one formatting node, wired only into its own `.onCase(N, ...)`. This made it possible to add `get_file` and then `update_file` without touching the four pre-existing branches (`save_idea`, `search_ideas`, `get_recent_ideas`, `memory_stats`) at all — each addition was reviewed and tested in isolation.
