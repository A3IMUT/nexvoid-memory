## Update — 2026-06-23

Classification: CORE

Workflow: Nexvoid_Memory_Service (id BmlFZkYzamGDvgoB)

Changes:

- Added `get_file` action — reads any file in the repo by path via the existing GitHub PAT credential. Returns content (decoded from base64) plus the file's sha.
- Added `update_file` action — writes/updates any file in the repo. Supports two modes: `replace` (overwrites the file) and `prepend` (inserts new content before the existing body, same pattern as `save_idea`). Uses GET-sha then PUT-content, consistent with the existing GitHub commit flow.
- Existing actions (`save_idea`, `search_ideas`, `get_recent_ideas`, `memory_stats`) were not modified — only new branches were added to the Action Router (switch), preserving backward compatibility.
- Credentials for the new HTTP nodes (Fetch File from GitHub, Fetch File for Update, Commit File Update) required manual binding to the existing `GitHub PAT` credential in the n8n UI — the workflow-code update path did not auto-assign them.

Result:

Nexvoid_Memory_Service is now the single entry point for both reading and writing any file in the `A3IMUT/nexvoid-memory` repository, not just `IDEAS.md`. This is the foundation for Claude (and future agents) to update project documentation (DECISIONS.md, CHANGELOG.md, WORKFLOWS.md, skills files) directly, without manual copy-paste between sessions.

Verified via live test calls against the real GitHub repo (not just pinned/synthetic test data).

---

# Nexvoid Workflows

Каталог проверенных workflow.
# WORKFLOWS

Version: 2.0
Status: ACTIVE
Scope: Nexvoid Workflow Registry

---

# PURPOSE

This document serves as the central registry of all major workflows inside the Nexvoid ecosystem.

It describes:

- workflow purpose
- status
- classification
- dependencies
- ownership

---

# WORKFLOW CLASSIFICATION

CORE
Critical infrastructure.

EXPERIMENTAL
Development and research workflows.

TEST
Validation and debugging workflows.

OBSOLETE
Archived workflows scheduled for removal.

---

# CORE WORKFLOWS

## Nexvoid_Commander

Classification: CORE

Status:
PARTIALLY OPERATIONAL

Purpose:

Central coordination layer of Nexvoid.

Responsibilities:

- user interaction
- routing
- memory requests
- specialist coordination
- orchestration

Current Capabilities:

- Memory Curator
- Recent Ideas Retrieval
- Conversational Assistant

Planned Capabilities:

- Search Ideas
- Memory Statistics
- Signal Watcher Routing
- Workflow Hunter Routing
- Content Synthesist Routing

Dependencies:

- Telegram
- Nexvoid_Memory_Service
- GitHub

---

## Nexvoid_Memory_Service

Classification: CORE

Status:
OPERATIONAL

Purpose:

Dedicated memory backend.

Actions:

- get_recent_ideas
- search_ideas
- memory_stats

Planned:

- lessons_search
- decisions_search
- signals_search

Dependencies:

- GitHub Knowledge Base

---

# MEMORY WORKFLOWS

## Memory Curator

Classification: CORE

Status:
OPERATIONAL

Purpose:

Save ideas and knowledge into GitHub memory.

Flow:

Telegram

↓

Commander

↓

Memory Curator

↓

GitHub

↓

Confirmation

---

## Recent Ideas Retrieval

Classification: CORE

Status:
OPERATIONAL

Purpose:

Retrieve ideas from a specified time period.

Examples:

- today
- last day
- last 7 days

Route:

[ROUTE:get_recent_ideas:N]

---

## Search Ideas

Classification: CORE

Status:
IMPLEMENTED
NOT YET EXPOSED

Purpose:

Keyword search across IDEAS.md.

Features:

- title search
- description search
- ranking
- date sorting

Next Step:

Expose through Commander routing.

---

## Memory Statistics

Classification: CORE

Status:
IMPLEMENTED
NOT YET EXPOSED

Purpose:

Project memory analytics.

Metrics:

- total ideas
- ideas today
- ideas last 7 days
- oldest idea
- newest idea

Next Step:

Expose through Commander routing.

---

# AI MEDIA FACTORY

Classification: CORE

Status:
IN DEVELOPMENT

Purpose:

Autonomous content production system.

Pipeline:

Signal

↓

Research

↓

Draft

↓

Review

↓

Publish

↓

Distribution

↓

Archive

Planned Channels:

- Ghost
- Telegram
- RSS
- Dzen

Owner:

Factory Operator

---

# SPECIALIST WORKFLOWS

## Signal Watcher

Classification: EXPERIMENTAL

Status:
PLANNED

Purpose:

Monitor:

- AI news
- trends
- opportunities
- weak signals

Outputs:

SIGNALS.md
Content Queue

---

## Workflow Hunter

Classification: EXPERIMENTAL

Status:
PLANNED

Purpose:

Discover:

- n8n workflows
- MCP servers
- AI agents
- repositories

Outputs:

WORKFLOWS.md
Research Notes

---

## Void Explorer

Classification: EXPERIMENTAL

Status:
PLANNED

Purpose:

Research and validation.

Outputs:

LESSONS.md

---

## Automation Architect

Classification: CORE

Status:
ACTIVE DEVELOPMENT

Purpose:

Design and build:

- workflows
- agents
- infrastructure

Outputs:

GitHub repositories
n8n workflows

---

## Content Synthesist

Classification: EXPERIMENTAL

Status:
PLANNED

Purpose:

Transform research into content.

Outputs:

- articles
- Telegram posts
- Ghost drafts

---

## Digital Guardian

Classification: CORE

Status:
ACTIVE

Purpose:

Maintain:

- documentation
- standards
- changelog
- project memory

Outputs:

- CHANGELOG
- CONTEXT
- CONSTITUTION

---

# MCP LAYER

## MiMo Operator

Classification: EXPERIMENTAL

Status:
VERIFIED

Capabilities:

- read workflows
- inspect architecture
- create workflows
- activate workflows
- execute workflows

Restrictions:

Governed by:

NEXVOID_CONSTITUTION_V2.md

---

# FUTURE WORKFLOWS

Planned:

- Signal Pipeline
- Content Pipeline
- Ghost Publisher
- Telegram Publisher
- RSS Distributor
- Dzen Distributor
- Lead Factory
- Affiliate Engine
- Marketplace Automation

---

# CURRENT PRIORITIES

1. Expose Search Ideas in Commander
2. Expose Memory Statistics in Commander
3. Activate Commander Production Layer
4. Build AI Media Factory MVP
5. Launch Ghost Publishing
6. Launch Telegram Network
7. Generate First Automated Revenue

---

END OF DOCUMENT
