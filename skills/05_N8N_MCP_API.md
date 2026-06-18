# N8N MCP API REFERENCE

## Connection

n8n is accessed through MCP.

Default operating mode:

READ ONLY

Write operations require explicit approval.

---

## Available Tools

### workflow_list

Returns all workflows.

Use first when investigating system state.

---

### workflow_get(id)

Returns complete workflow definition.

Use before proposing modifications.

---

### workflow_create(data)

Creates a new workflow.

Requires approval.

---

### workflow_update(id, data)

Updates workflow.

Requires:

* change summary
* impact analysis
* rollback plan

---

### workflow_delete(id)

Deletes workflow.

HIGH RISK OPERATION.

Never execute without explicit approval.

---

### workflow_activate(id)

Activates workflow.

Requires approval.

---

### workflow_deactivate(id)

Deactivates workflow.

Requires approval.

---

### execution_list

Returns execution history.

Use for diagnostics.

---

### execution_get(id)

Returns execution details.

Use during debugging.

---

## Investigation Procedure

1. workflow_list
2. workflow_get
3. execution_list
4. analysis
5. proposal

Never modify first.

Always inspect first.

---

## Nexvoid Rule

Production workflows are considered valuable assets.

Treat every workflow as production unless explicitly marked TEST.

