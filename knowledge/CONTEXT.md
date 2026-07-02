# CONTEXT

Version: 1.0
Status: ACTIVE
Last Updated: 2026-06-28
Source: Replaces root NEXVOID_CONTEXT_4.0_ENTERPRISE.md (see D-017)

---

# 1. PROJECT OVERVIEW

Nexvoid is an AI Orchestration Operating System built around autonomous workflows, AI agents, knowledge management, content production, and automation infrastructure.

Long-term goal: a self-sustaining ecosystem capable of discovering information, generating knowledge, creating content, managing memory, building automations, and generating revenue.

Nexvoid is not a chatbot. Nexvoid is an evolving AI-driven operating system.

Rules and governance: see knowledge/CONSTITUTION.md
Character lore and agent hierarchy: see knowledge/NEXVOID_CHARACTERS_V3.md

---

# 2. CORE MISSION

1. Build self-improving automation infrastructure.
2. Create autonomous content production pipelines.
3. Develop reusable AI systems.
4. Generate recurring revenue streams.
5. Reduce dependence on manual labor.
6. Minimize direct sales activity.
7. Scale through content, automation and partnerships.

---

# 3. CURRENT STRATEGIC PRIORITY

REVENUE GENERATION (not experimentation).

Every project evaluated through:
1. Can it generate revenue?
2. Can it be automated?
3. Can it scale?

If "no" to all three — postpone.

Active strategic plan: see knowledge/NEXVOID_ROADMAP_2026.md

---

# 4. FOUNDER PROFILE

Technical, systems-oriented, automation-oriented. NOT a salesperson or cold caller.

Preferred channels: websites, SEO, Telegram channels, Dzen, affiliate programs, automated lead gen, content marketing, marketplaces, passive funnels.

Avoid: cold calls, manual outreach, active sales meetings, daily client hunting.

The system must attract opportunities rather than chase them. (See D-009)

---

# 5. TECH STACK

Infrastructure: VPS 132.243.115.222, Docker, Portainer, Cloudflare, GitHub (A3IMUT/nexvoid-memory), Nginx Proxy Manager

Automation: n8n (self-hosted, n8n.nexvoid.ru)

Content: Ghost CMS, Telegram, RSS

AI: OpenRouter, Gemini, Claude, OpenAI, MCP tools

Database: PostgreSQL (nexvoid-postgres container, nexvoid_db)

Knowledge: GitHub knowledge/ directory (this repo)

---

# 6. CURRENT SYSTEM STATUS

## Nexvoid Commander
Status: PARTIALLY OPERATIONAL
Working: memory storage, recent idea retrieval, routing, Telegram interaction
Pending: notify_user action (see TASK_REGISTRY.md)

## Nexvoid Memory Service
Status: OPERATIONAL
Workflow ID: BmlFZkYzamGDvgoB
Actions: save_idea, search_ideas, get_recent_ideas, memory_stats, get_file, update_file, create_file, delete_file, create_task
Data contract: see knowledge/MEMORY_SCHEMA.md

## MCP Integration
Status: VERIFIED
Platform: n8n MCP (n8n.nexvoid.ru/mcp-server/http)
Capabilities: read/create/update/execute workflows
Mode: Observer/Architect — see D-011, D-014

## PostgreSQL
Status: CONNECTED
Container: nexvoid-postgres, database: nexvoid_db, table: tasks

## AI Media Factory
Status: IN DEVELOPMENT
See NEXVOID_ROADMAP_2026.md Stage 1

---

# 7. KNOWLEDGE BASE STRUCTURE

knowledge/ (flat, all files at same level):
- CONSTITUTION.md — governance rules
- CONTEXT.md — this file
- DECISIONS.md — decisions log (D-001...D-017+)
- MEMORY_SCHEMA.md — IDEAS.md data contract (CORE)
- NEXVOID_ROADMAP_2026.md — strategic plan
- TASK_REGISTRY.md — open tasks
- NEXVOID_CHARACTERS_V3.md — agent lore
- NEXVOID_CHANGELOG.md — system history
- ARCHIVE_NOTES.md — archived/obsolete material
- IDEAS.md — project ideas
- DECISIONS.md — decisions log
- LESSONS.md — lessons learned
- WORKFLOWS.md — workflow catalog
- SIGNALS.md — market signals
- PROMPTS.md — prompt library

skills/ (compact AI-instruction files, read-only reference):
- 00_NEXVOID_CONSTITUTION.md through 07_n8n_doc.md

---

# 8. OPERATING PRINCIPLE

When a decision is unclear, ask:

"Does this move Nexvoid closer to autonomous revenue generation?"

If not — deprioritize.

---

END OF DOCUMENT