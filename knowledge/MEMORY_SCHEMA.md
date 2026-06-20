# MEMORY_SCHEMA

Version: 1.0
Status: ACTIVE
Classification: CORE
Scope: Nexvoid Memory Service — IDEAS.md record format

---

# PURPOSE

This document defines the data contract for records written to `knowledge/IDEAS.md` by `Nexvoid_Memory_Service`.

It exists so that multiple writers (Commander, future agents: Signal Watcher, Market Oracle, Content Synthesist, etc.) produce records in one consistent format that `search_ideas`, `get_recent_ideas`, and `memory_stats` can reliably parse.

This schema is classified CORE per NEXVOID_CONSTITUTION_V2 §6: never modify without approval, never delete.

---

# RECORD FORMAT

Each record is a plain-text block, fields as `Key: value` lines, terminated by a `---` separator line.

```
Date: 2026-06-20
ID: idea-20260620-143501
Source: Commander
Type: idea
Status: new
Title: Краткий заголовок
Description: Текст идеи, может быть многострочным.
---
```

---

# FIELDS

## Required

| Field | Format | Description |
|---|---|---|
| `Date` | `YYYY-MM-DD` | Creation date. Used for sorting and filtering by `get_recent_ideas` / `memory_stats`. |
| `Title` | string | Short title. Required — records without a title are rejected by `save_idea`. |
| `Description` | string | Free text body. May be empty string but the key must be present. |

## Extended (added in v1.0, optional but recommended)

| Field | Format | Description |
|---|---|---|
| `ID` | `idea-YYYYMMDD-HHMMSS` | Unique identifier, generated at write time. Makes records addressable for future update/delete operations. |
| `Source` | string | Which agent or actor wrote the record (e.g. `Commander`, `Signal Watcher`, `Market Oracle`). Defaults to `Commander` if omitted by caller. |
| `Type` | string | Record category. Defaults to `idea`. Reserved future values: `signal`, `lesson`, `decision-input`. |
| `Status` | string | Lifecycle marker. Defaults to `new` at creation. Reserved future values: `reviewed`, `archived`. |

## Not yet implemented (reserved names, do not reuse for other purposes)

- `Tags` — comma-separated keywords
- `SourceUrl` — originating link, for signals pulled from RSS/Reddit/GitHub
- `Priority` — for Market Oracle / triage use

---

# COMPATIBILITY RULES

1. **Additive only.** New fields may be added in minor versions (1.1, 1.2, ...) without breaking existing readers. Readers must treat unknown fields as ignorable.
2. **No renames, no removals without a major version bump.** Renaming or deleting a field is a breaking change — requires a new DECISIONS.md entry and a version bump to 2.0.
3. **Parsing stays regex-based for v1.** `search_ideas`, `get_recent_ideas`, and `memory_stats` extract fields via line-anchored regex (`Date:`, `Title:`, `Description:`), not a structured parser (no YAML/JSON). Any new field must follow the same `Key: value` line convention to remain parseable without changing existing extraction logic.
4. **Old records remain valid.** Records written before v1.0 (Date/Title/Description only, no ID/Source/Type/Status) are not retroactively rewritten. Readers must tolerate their absence.

---

# OWNERSHIP

Any agent or workflow writing to `knowledge/IDEAS.md` must conform to this schema. Direct writes that bypass `Nexvoid_Memory_Service.save_idea` are discouraged — they risk producing malformed records that break parsing for all other consumers.

---

# CHANGE LOG

## v1.0 — 2026-06-20

- Initial schema definition.
- Added `ID`, `Source`, `Type`, `Status` fields on top of the original `Date`/`Title`/`Description` format.
- `save_idea` action in `Nexvoid_Memory_Service` updated to write this format.

---

END OF DOCUMENT
