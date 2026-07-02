# TASK_REGISTRY

Version: 1.0
Status: ACTIVE
Last Updated: 2026-06-28

---

# PURPOSE

Single source of open tasks for Nexvoid. Not a status document (for system status see CONTEXT.md) and not a strategy document (see NEXVOID_ROADMAP_2026.md). This is a working list.

---

# IMMEDIATE

| Task | Context |
|---|---|
| Implement notify_user in Commander | Part of Commander refactor (Step 2), not yet implemented |
| Close unprotected TEMP Webhook Test Trigger in Memory Service | Known security risk — unauthenticated |
| Activate new Memory Service version in n8n UI | Updated to 32 nodes (with create_file/delete_file) — activeVersionId still points to old version |
| Delete temporary test workflows from n8n | TEST_Memory_Writer (7gj1xMdqJcmr1U7Y) — delete after repo cleanup done |

---

# STRUCTURAL ROADMAP

Agreed locked sequence (do not reorder without new DECISIONS.md entry):

1. Lock Memory Service API as versioned data contract before new agents write to it. — DONE (D-013, MEMORY_SCHEMA.md v1.0)
2. Refactor Commander into pure router (intent → service call → format), move business logic to separate service workflows. — IN PROGRESS
3. Add approval-layer gate for MCP actions touching CORE/PRODUCTION workflows. — PROCEDURAL DONE (D-014); technical layer (hard blocking) — NOT STARTED
4. Expand new agents (Signal Watcher, Workflow Hunter, etc.) on stable base. — NOT STARTED

---

# REPO CLEANUP (current sprint)

| Action | File | Status |
|---|---|---|
| CREATE | knowledge/CONSTITUTION.md | Done 2026-06-28 |
| CREATE | knowledge/CONTEXT.md | Done 2026-06-28 |
| CREATE | knowledge/TASK_REGISTRY.md | This file |
| CREATE | knowledge/ARCHIVE_NOTES.md | Pending |
| DELETE | root: NEXVOID CONSTITUTIONV2.md | Pending |
| DELETE | root: NEXVOID_CONTEXT_3_2.md | Pending (archive first) |
| DELETE | root: NEXVOID_CONTEXT_4.0_ENTERPRISE.md | Pending |
| UPDATE | knowledge/DECISIONS.md | Add D-017, D-018 |
| UPDATE | skills/01_N8N_MCP_RULES.md | Add create_file credential lesson |

---

# KNOWN ISSUES

| Issue | Details |
|---|---|
| Credentials not auto-bound on update_workflow | Every time Memory Service is updated via MCP SDK, HTTP Request nodes lose their credential bindings. Manual re-binding in n8n UI required after every update_workflow call. See D-017. |
| get_file on directory path returns wrong result | Memory Service get_file with a directory path (e.g. "knowledge") returns stale/wrong data instead of 404. Bug in Format get_file Result node — not critical, just avoid using directory paths. |
| Memory Service activeVersionId lag | After update_workflow, the workflow shows active:true but activeVersionId still points to old version. New version is a draft until manually published in n8n UI. |

---

# EXTERNAL PROJECTS

Городские мастера (City Masters) — Flutter marketplace on same VPS, isolated Docker network gormaster-net.
Status: FROZEN — Flutter source lost, web build and DB dump backed up at ~/backups/
Next step when resumed: restore/rewrite Flutter frontend against stable backend
Infrastructure: gormaster-db (Postgres), gormaster-n8n (port 5680), gormaster-dl (nginx), domains via reg.ru NS

---

END OF DOCUMENT