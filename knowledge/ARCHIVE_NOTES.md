# ARCHIVE_NOTES

Status: ARCHIVE — no operational authority
Created: 2026-06-28 (see DECISIONS.md D-017)

---

# PURPOSE

Historical archive of documents that existed in the repository but are now superseded, obsolete, or were never formally approved. Kept for traceability only — do not use as operational guidance.

---

# ARCHIVED: NEXVOID_CONTEXT_3_2.md ("Nexvoid Labs" era)

**Status: SUPERSEDED** by knowledge/CONTEXT.md

This document (formerly root/NEXVOID_CONTEXT_3_2.md) described an earlier iteration of the project under the brand "Nexvoid Labs". Key differences from the current project:

- Brand: "Nexvoid Labs" (current brand: Nexvoid)
- Domains: nexvoid.ru, blog.nexvoid.ru (current: n8n.nexvoid.ru, nexvoid.ru)
- GitHub org: NexvoidLabs (current: A3IMUT)
- Only 3 characters: Void Explorer, Automation Architect, Digital Guardian (current: 6 characters)
- Ghost CMS on MariaDB infrastructure under docker network ai-media-net
- Telegram channel with mandatory manual moderation before publishing

Infrastructure referenced in that document (Ghost CMS, MariaDB ai-media-mariadb):
Status as of 2026-06-28: FROZEN — experiment paused, direction unclear.

**SECURITY NOTE:** The original document contained database passwords and Ghost Admin API key in plaintext. These have been REDACTED here per D-013 (secrets must not live in documentation). If this infrastructure is ever reactivated, generate fresh credentials — do not attempt to recover the originals from git history for reuse, as they were in a public repository.

Redacted values:
- MariaDB root password: [REDACTED — generate new if reactivating]
- MariaDB ghost user password: [REDACTED — generate new if reactivating]
- Ghost Admin API key: [REDACTED — generate new if reactivating]

---

# ARCHIVED: Unapproved "Stage 1-5" monetization draft

**Status: NOT APPROVED** — never passed D-005 procedure

A draft alternative monetization roadmap (Stage 1-5: Cash Flow → Lead Gen Network → Content Factory → Workflow Marketplace → SaaS) was found embedded in NEXVOID_CONTEXT_4.0_ENTERPRISE.md §12, marked by the author as "I would completely replace the old roadmap here."

This draft was not formally approved. The active strategic plan remains NEXVOID_ROADMAP_2026.md v1.0 (Stages 0-7).

If this structure is preferred over the current roadmap — requires:
1. Explicit operator decision
2. New DECISIONS.md entry
3. Update to NEXVOID_ROADMAP_2026.md with version bump

---

END OF DOCUMENT