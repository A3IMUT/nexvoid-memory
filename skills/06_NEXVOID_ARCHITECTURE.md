# NEXVOID ARCHITECTURE

Version: 0.3

This document describes the current operational architecture of the Nexvoid ecosystem.

---

# SYSTEM PURPOSE

Nexvoid is an AI-powered automation ecosystem built around n8n.

Core objectives:

* Collect information
* Process information
* Generate content
* Store knowledge
* Publish content
* Assist operators through AI agents

The system is designed to evolve into a self-documenting AI Operations Platform.

---

# PRIMARY PLATFORM

Runtime:

* n8n
* Docker
* Ubuntu VPS

Main orchestration layer:

* n8n workflows

Knowledge layer:

* GitHub Repository
* Nexvoid Memory Repository

Agent layer:

* AI Agents
* MCP-connected tools

---

# CURRENT N8N STATE

Total workflows: 36

Active workflows: 1

Inactive workflows: 35

Current status:

The platform is operational but not fully consolidated.

Several legacy workflows, test workflows, and duplicate workflows remain in the environment.

---

# CORE PRODUCTION COMPONENTS

## Nexvoid_Commander

Role:

Primary Telegram-based AI assistant.

Responsibilities:

* User interaction
* Memory commands
* Idea management
* Workflow routing

Dependencies:

* Telegram
* Mistral AI
* GitHub

Status:

Operational.

---

## Nexvoid_Memory_Service

Role:

Central memory retrieval service.

Responsibilities:

* Search ideas
* Retrieve recent ideas
* Memory statistics
* Knowledge access

Storage:

IDEAS.md

Dependencies:

* GitHub API

Status:

Operational.

---

## News Parser

Role:

News ingestion pipeline.

Responsibilities:

* RSS collection
* Content scraping
* Deduplication
* Data preparation

Dependencies:

* RSS feeds
* Airtable
* ScrapingBee

Status:

Partially operational.

---

## News Longread Writer

Role:

Long-form content generation.

Responsibilities:

* Article analysis
* Longread generation
* Summary creation
* Distribution

Dependencies:

* OpenRouter
* Gemini Models
* Telegram
* GitHub
* Airtable

Status:

Operational when triggered.

---

## Image Generation Workflows

Role:

Image generation and editing.

Responsibilities:

* Telegram image requests
* AI image generation
* AI image editing

Status:

Active development.

---

# EXTERNAL SERVICES

## Operational

* Telegram Bot API
* GitHub API
* OpenRouter
* Mistral AI
* Airtable
* ScrapingBee

## Not Fully Verified

* Supabase
* Cohere
* Perplexity
* Google Calendar
* Gmail
* Google Sheets
* WhatsApp

## Not Configured

* Ghost CMS publishing

---

# KNOWN ARCHITECTURAL ISSUES

## Duplicate Workflows

Multiple Commander implementations exist.

Examples:

* Commander v1
* Commander HTML
* Nexvoid_Commander

Only one should eventually become canonical.

---

## Legacy Test Workflows

Many TEST workflows remain.

These should be reviewed before deletion.

Never remove workflows without approval.

---

## Documentation Drift

Documentation may not reflect actual workflow state.

Always verify:

Documented State

vs

Live n8n State

before making decisions.

---

# OPERATIONAL PRINCIPLES

When investigating the system:

1. Read workflows first.
2. Inspect executions.
3. Analyze dependencies.
4. Produce findings.
5. Wait for approval.

Never assume documentation is current.

Never modify production workflows without approval.

---

# FUTURE TARGET ARCHITECTURE

NEXVOID

├── Commander
├── Memory Layer
├── Context Sync
├── Media Factory
├── Publishing Layer
├── Monitoring Layer
└── Documentation Layer

Target state:

A self-documenting AI operations platform where:

* workflows generate documentation
* documentation updates memory
* memory informs agents
* agents improve workflows

---

# AGENT INSTRUCTION

If you are an AI agent reading this file:

Treat this document as an architectural overview only.

Before taking action:

* Read live workflow state
* Verify assumptions
* Compare against documentation
* Report discrepancies

Architecture documents are guidance.

Live system state is the source of truth.
