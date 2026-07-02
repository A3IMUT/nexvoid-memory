# 01_N8N_MCP_RULES

Classification: CORE
Last Updated: 2026-06-28

Rules and lessons for working with n8n via MCP SDK. Read this BEFORE starting any workflow engineering session.

---

## RULE 1: Read Before Write

Before modifying any workflow: get_workflow_details first. Never reconstruct from memory — the live n8n state is ground truth.

Before touching any knowledge file: get_file first to check current content and sha.

---

## RULE 2: Workflow Classification

Always identify classification before acting:
- CORE: Nexvoid_Commander, Nexvoid_Memory_Service — require D-014 checklist before any change
- TEST_: temporary workflows — prefix TEST_, delete after use
- EXPERIMENTAL: prototypes — no checklist needed but document what changed

---

## RULE 3: D-014 Checklist for CORE/PRODUCTION

Before any update_workflow or create_workflow_from_code touching CORE/PRODUCTION:
1. Name the workflow and its classification
2. Describe what changes and why
3. Name the risk (what breaks if it goes wrong)
4. Get explicit operator confirmation
5. Only then execute

Read-only actions (get_workflow_details, search_workflows, get_execution) — no checklist needed.

---

## LESSON: switchCase requires numeric onCase indexes (2026-06-23)

Named outputKey strings in switchCase rules combined with .onCase('string', ...) crash the validator with "Cannot convert undefined or null to object".

WORKING PATTERN:
- Omit outputKey from rules.values entries
- Use numeric .onCase(0, ...), .onCase(1, ...) etc
- Use .add(router.output(N).to(fallbackNode)) for fallback where N = number of cases
- Use switchCase version 3.2 (not 3.4)

BROKEN (do not use):
- { outputKey: 'save_idea', conditions: {...} } with .onCase('save_idea', ...)
- .onDefault(node) — security violation, not an allowed SDK method

---

## LESSON: Every node requires output: [...] sample data (2026-06-23)

Omitting output from any node() or trigger() call causes: "Cannot convert undefined or null to object"

REQUIRED pattern:
const myNode = node({ type: ..., config: {...}, output: [{ field: 'sample_value' }] });

Output sample does not need to be exhaustive — one representative object is enough.

---

## LESSON: HTTP Request credentials never auto-bind (2026-06-23, confirmed 2026-06-28)

update_workflow and create_workflow_from_code do NOT preserve credential bindings on HTTP Request nodes. The SDK returns "HTTP Request nodes were skipped during credential auto-assignment" every time.

This is a permanent architectural constraint (see D-017), not a bug to be fixed.

REQUIRED PROCESS after any update that adds/changes HTTP Request nodes:
1. Open n8n UI: n8n.nexvoid.ru
2. Navigate to the updated workflow
3. Click each HTTP Request node manually
4. Assign the correct credential (GitHub PAT → httpHeaderAuth)
5. Save

Memory Service (BmlFZkYzamGDvgoB) HTTP nodes requiring GitHub PAT after each update:
Get IDEAS.md (for save), Commit Idea to GitHub, Get IDEAS.md (search), Get IDEAS.md (recent), Get IDEAS.md (stats), Fetch File from GitHub, Fetch File for Update, Commit File Update, Fetch File for Delete, Commit File Delete, Commit File Create

DO NOT publish/activate the new workflow version before re-binding credentials — GitHub API calls will fail with 401.

---

## LESSON: update_file cannot create new files (2026-06-28)

update_file action in Memory Service does a GET first (to obtain sha) then PUT with sha. If the file does not exist, GET returns 404 and the action fails.

For new files: use create_file action (GitHub PUT without sha field).
For existing files: use update_file action (mode: replace or prepend).

Memory Service actions summary:
- create_file: creates a new file (fails with 422 if file already exists — safe, not destructive)
- update_file: updates existing file (fails with 404 if file does not exist)
- delete_file: deletes existing file (fails with 404 if file does not exist)
- get_file: reads existing file (fails with 404 if file does not exist — triggers error workflow!)

---

## LESSON: get_file 404 triggers error workflow and sends Telegram notification (2026-06-28)

Memory Service has an errorWorkflow configured (Lg0k2vNyErqKSWTl). Any failed execution — including benign 404s from probing non-existent paths — triggers this workflow and sends a Telegram error notification to the operator.

CONSEQUENCE: never use get_file to check if a file exists by trying the path and expecting 404. Use the GitHub Git Trees API (GET /repos/{owner}/{repo}/git/trees/main?recursive=1) via a separate TEST_ workflow with manually-bound credentials to get the full file tree first.

---

## LESSON: get_file on directory path returns wrong data (2026-06-28)

Passing a directory path (e.g. "knowledge" without a filename) to get_file does not return a directory listing. The GitHub Contents API returns an array for directories, and the Format get_file Result node cannot handle arrays — it may return stale data or silently fail. Always use exact file paths with extensions.

---

## LESSON: Memory Service activeVersionId vs versionId lag (2026-06-28)

After update_workflow, n8n shows active: true on the workflow but activeVersionId still points to the old version. The new version is a draft — it will not execute in production until manually published in the n8n UI (open workflow → click Publish/Save as active version).

Always verify after credential re-binding: open the workflow in UI and confirm the version is published, not just saved as draft.

---

END OF DOCUMENT