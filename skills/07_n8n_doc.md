# N8N DOCUMENTATION REFERENCE

Version: 1.0
Status: ACTIVE
Classification: CORE
Source: https://docs.n8n.io/
Last Updated: June 2026

---

# 1. WHAT IS N8N

n8n is a workflow automation platform. Visual node-based editor, 400+ integrations, self-hosted or cloud. Core principle: connect any service, automate any process, extend with code.

---

# 2. WORKFLOW STRUCTURE

## Components

- **Nodes** — processing units (triggers, actions, transforms)
- **Connections** — links between nodes, carrying data
- **Sticky Notes** — documentation annotations on canvas
- **Credentials** — stored secrets for authenticating external services

## Data Flow

Every node receives items (array of JSON objects), processes them, outputs items.

`
Trigger → Node A → Node B → Node C
`

Each item = { json: {...}, binary: {...} }

## Execution Order

v1 mode (default for new workflows): runs all nodes in sequence.
Multi-branch: each branch runs independently after split point.

---

# 3. TRIGGER NODES

| Trigger | Use Case |
|---------|----------|
| Manual Trigger | Test workflow by clicking |
| Webhook | Receive HTTP requests (POST/GET) |
| Schedule Trigger | Cron-based (every N minutes/hours/days) |
| Telegram Trigger | Listen for Telegram messages/callbacks |
| RSS Feed Trigger | Watch RSS for new entries |
| n8n Trigger | Trigger from another workflow |
| Error Trigger | Catch errors from other workflows |
| Chat Trigger | AI chat interface |

### Webhook Node

- Generates a URL: https://your-n8n/webhook/<path>
- HTTP methods: GET, POST, PUT, DELETE, etc.
- Returns response via **Respond to Webhook** node
- Can respond immediately or after workflow completes

### Schedule Trigger

- Interval: seconds, minutes, hours, days, weeks
- Cron expressions supported
- Timezone-aware

---

# 4. CORE NODES (ESSENTIAL)

## Data Transformation

| Node | Purpose |
|------|---------|
| **Code** | Run JavaScript/Python. Access $input, $prevNode, $getWorkflowStaticData() |
| **Edit Fields (Set)** | Add/remove/rename fields |
| **Filter** | Keep/discard items by condition |
| **If** | Branch by condition (true/false outputs) |
| **Switch** | Multi-way routing by value |
| **Merge** | Combine data from multiple branches |
| **Sort** | Order items |
| **Limit** | Cap number of items |
| **Split Out** | Unwrap arrays into individual items |
| **Aggregate** | Combine multiple items into one |
| **Summarize** | Group + aggregate (like SQL GROUP BY) |
| **Remove Duplicates** | Deduplicate by field |
| **Rename Keys** | Rename object keys |

## HTTP & External

| Node | Purpose |
|------|---------|
| **HTTP Request** | Call any REST API (GET/POST/PUT/DELETE) |
| **GraphQL** | GraphQL queries |
| **RSS Read** | Parse RSS feeds |
| **Execute Command** | Run shell commands |
| **SSH** | Remote command execution |
| **Read/Write Files from Disk** | Local file I/O |

## Flow Control

| Node | Purpose |
|------|---------|
| **Wait** | Pause execution (time or webhook callback) |
| **Loop Over Items** | Process items in batches |
| **Execute Sub-workflow** | Call another workflow |
| **Stop And Error** | Force failure + trigger error workflow |

---

# 5. EXPRESSIONS

n8n uses JavaScript expressions (double curly braces {{ }}).

## Common Patterns

`javascript
// Current item data
{{ .fieldName }}

// Reference specific node output
{{ Node Name.item.json.field }}

// All items from a node
{{ Node Name.all() }}

// First item from a node
{{ Node Name.first().json }}

// Workflow static data (persists between executions)
{{ ('global') }}

// Current workflow info
{{ .name }}
{{ .id }}

// Execution info
{{ .id }}
{{ .mode }}

// Node parameter access
{{ .method }}
`

## Static Data (Persistent Storage)

`javascript
// In Code node:
const staticData = ('global');
staticData.myKey = 'value';  // persists across executions

// Read:
const value = staticData.myKey;
`

**IMPORTANT**: Static data is per-workflow, per-namespace (global vs node). It persists between executions but is lost if workflow is deleted.

---

# 6. CODE NODE

JavaScript (default) or Python. Runs in a sandboxed environment.

## Accessible Variables

- $input — input items
- $prevNode — previous node name
- $getWorkflowStaticData(scope) — persistent storage
- $execution — execution metadata
- $workflow — workflow metadata
- $now — current DateTime (Luxon)
- $env — environment variables

## Return Format

`javascript
// Single item
return [{ json: { key: 'value' } }];

// Multiple items
return [
  { json: { key: 'value1' } },
  { json: { key: 'value2' } }
];

// With binary data
return [{
  json: { key: 'value' },
  binary: {
    data: await this.helpers.prepareBinaryData(buffer, 'filename.txt')
  }
}];
`

## Available Built-in Modules

Node.js built-ins: crypto, uffer, url, querystring, etc.
External (if enabled): xios, cheerio, moment, etc.

Enable modules in: Settings → Environment Variables → NODE_FUNCTION_ALLOW_BUILTIN and NODE_FUNCTION_ALLOW_EXTERNAL.

---

# 7. HTTP REQUEST NODE

The most-used integration node. Calls any HTTP API.

## Methods

GET, POST, PUT, DELETE, PATCH, HEAD, OPTIONS

## Authentication Options

- **Predefined Credential Type** — built-in integrations (OAuth2, API Key, etc.)
- **Generic Credential Type** — HTTP Header Auth, HTTP Basic Auth, HTTP Digest Auth, HTTP Custom Auth
- **None** — no auth

## Body Options

- **JSON** — raw JSON body
- **Form URL Encoded** — form data
- **Multipart Form Data** — file uploads

## Expression in Body

`javascript
// Reference previous node data in JSON body
{
  "title": "{{ .title }}",
  "content": "{{ .content }}"
}
`

## Response Handling

- Default: response body parsed as JSON
- Options: full response (headers + status), binary response (for files)
- Error handling: Continue On Fail option

---

# 8. CREDENTIALS

Encrypted storage for API keys, tokens, passwords.

## Types Available

- HTTP Header Auth (custom header with token)
- HTTP Basic Auth (username + password)
- OAuth2 (Google, GitHub, etc.)
- API Key (service-specific)
- Bearer Token

## Best Practices

- Never hardcode tokens in workflow JSON
- Always use Credentials for sensitive data
- Share credentials between workflows (Credential Sharing)
- After creating new Credentials, restart n8n to see them in workflow nodes

## Nexvoid Credentials

| Credential | Type | Used For |
|------------|------|----------|
| GitHub PAT | HTTP Header Auth | GitHub API, OpenRouter |
| Nexvoid_assistant | Telegram API | Telegram bot |
| Ghost API | HTTP Header Auth | Ghost CMS publishing |

---

# 9. n8n REST API

Base URL: https://your-n8n/api/v1

## Authentication

Header: X-N8N-API-KEY: <your-api-key>

Generate API key: Settings → API → Create API Key

## Key Endpoints

### Workflows

`
GET    /workflows              — list all workflows
GET    /workflows/{id}         — get workflow by ID
POST   /workflows              — create workflow
PUT    /workflows/{id}         — update workflow
DELETE /workflows/{id}         — delete workflow
POST   /workflows/{id}/activate   — activate
POST   /workflows/{id}/deactivate — deactivate
`

### Executions

`
GET    /executions              — list executions
GET    /executions/{id}         — get execution details
DELETE /executions/{id}         — delete execution
POST   /executions/{id}/retry   — retry failed execution
`

### Credentials

`
GET    /credentials             — list all credentials
POST   /credentials             — create credential
GET    /credentials/{id}        — get credential
DELETE /credentials/{id}        — delete credential
POST   /credentials/{id}/test   — test credential
`

## Pagination

`
GET /workflows?limit=20&cursor=<cursor>
`

## Important Notes

- Workflow JSON in export/import uses 
odes and connections keys
- 	ypeVersion field is required for each node
- webhookId links trigger nodes to webhook URLs
- credentials object references credential IDs by name

---

# 10. ERROR HANDLING

## Error Workflow

1. Create workflow with **Error Trigger** as first node
2. Set it as error workflow in target workflow's Settings
3. On failure, Error Trigger receives:

`json
{
  "execution": {
    "id": "231",
    "url": "https://n8n/execution/231",
    "error": { "message": "...", "stack": "..." },
    "lastNodeExecuted": "Node With Error",
    "mode": "manual"
  },
  "workflow": { "id": "1", "name": "..." }
}
`

## Continue On Fail

Every node has this option. When enabled, errors become output items with error field instead of stopping execution.

## Stop And Error

Forces workflow to fail with custom message. Triggers error workflow.

---

# 11. SUB-WORKFLOWS

- **Execute Sub-workflow** node — calls another workflow
- **Execute Sub-workflow Trigger** — entry point for sub-workflows
- Can pass input data between parent and child
- Useful for modular, reusable workflow components

---

# 12. HOSTING (SELF-HOSTED)

## Docker Setup

`ash
docker run -it --rm \
  --name n8n \
  -p 5678:5678 \
  -v ~/.n8n:/home/node/.n8n \
  n8nio/n8n
`

## Docker Compose (with PostgreSQL)

`yaml
services:
  n8n:
    image: n8nio/n8n
    ports:
      - "5678:5678"
    environment:
      - DB_TYPE=postgresdb
      - DB_POSTGRESDB_HOST=postgres
      - DB_POSTGRESDB_DATABASE=n8n
      - DB_POSTGRESDB_USER=n8n
      - DB_POSTGRESDB_PASSWORD=n8n
    volumes:
      - ./n8n-data:/home/node/.n8n
    depends_on:
      - postgres

  postgres:
    image: postgres:15
    environment:
      - POSTGRES_DB=n8n
      - POSTGRES_USER=n8n
      - POSTGRES_PASSWORD=n8n
    volumes:
      - postgres-data:/var/lib/postgresql/data

volumes:
  postgres-data:
`

## Supported Databases

- SQLite (default, not for production)
- PostgreSQL (recommended)
- MySQL / MariaDB

## Key Environment Variables

| Variable | Purpose |
|----------|---------|
| N8N_BASIC_AUTH_ACTIVE | Enable basic auth |
| N8N_BASIC_AUTH_USER | Auth username |
| N8N_BASIC_AUTH_PASSWORD | Auth password |
| N8N_HOST | Hostname |
| N8N_PORT | Port (default 5678) |
| N8N_PROTOCOL | http or https |
| WEBHOOK_URL | Public URL for webhooks |
| N8N_ENCRYPTION_KEY | Custom encryption key |
| GENERIC_TIMEZONE | Default timezone |
| DB_TYPE | Database type (sqlite/postgresdb/mysql) |
| NODE_FUNCTION_ALLOW_BUILTIN | Allow Node.js modules in Code node |
| NODE_FUNCTION_ALLOW_EXTERNAL | Allow npm packages in Code node |

## Reverse Proxy (Nginx)

`
ginx
server {
    listen 443 ssl;
    server_name n8n.example.com;

    location / {
        proxy_pass http://localhost:5678;
        proxy_set_header Host System.Management.Automation.Internal.Host.InternalHost;
        proxy_set_header X-Real-IP ;
        proxy_set_header X-Forwarded-For ;
        proxy_set_header X-Forwarded-Proto ;

        # WebSocket support
        proxy_http_version 1.1;
        proxy_set_header Upgrade ;
        proxy_set_header Connection "upgrade";
    }
}
`

---

# 13. COMMON PATTERNS FOR NEXVOID

## Telegram Bot Pattern

`
Telegram Trigger → Switch (message/callback) → Code (parse) → Switch (intent) → [branches]
`

## Content Pipeline Pattern

`
RSS Trigger → HTTP Request (fetch) → Code (extract) → AI (generate) → HTTP Request (publish)
`

## GitHub Knowledge Sync Pattern

`
Schedule Trigger → HTTP Request (GitHub API) → Code (diff) → Telegram (notify)
`

## AI Agent Pattern

`
Webhook/Telegram Trigger → Code (parse input) → HTTP Request (OpenRouter) → Code (parse response) → Telegram (reply)
`

---

# 14. TIPS AND GOTCHAS

1. **Credentials restart** — After creating new credentials, restart n8n before using them in workflow nodes
2. **Static data** — $getWorkflowStaticData('global') persists between executions but NOT between workflow redeployments
3. **Webhook URLs** — Production webhooks require activation; test URLs work without activation
4. **Expression errors** — Use {{ }} only in expression-enabled fields; plain text doesn't parse expressions
5. **Binary data** — Large files can cause memory issues; use streaming when possible
6. **Rate limits** — External APIs may rate-limit; use Loop Over Items with Wait node for batching
7. **Timezone** — Set GENERIC_TIMEZONE environment variable; Schedule Trigger uses it
8. **Version control** — Export workflows as JSON for backup; import to restore
9. **Debugging** — Use Executions tab to inspect past runs; pin data to test with fixed inputs
10. **Sub-workflows** — Use for reusable logic; pass data via input parameters

---

# 15. NEXVOID-SPECIFIC NOTES

- n8n instance: https://n8n.nexvoid.ru
- API URL: https://n8n.nexvoid.ru/api/v1
- API Key header: X-N8N-API-KEY
- Ghost CMS requires JWT (HS256) for Admin API, not simple API key
- MariaDB is used for Ghost (PostgreSQL incompatible with Ghost)
- key is reserved in MariaDB — wrap in backticks
- After creating Credentials in n8n, restart the n8n container

---

# REFERENCES

- Official docs: https://docs.n8n.io/
- API reference: https://docs.n8n.io/api/
- Code node: https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.code/
- HTTP Request: https://docs.n8n.io/integrations/builtin/core-nodes/n8n-nodes-base.httprequest/
- Expressions: https://docs.n8n.io/data/expression-reference/
- Error handling: https://docs.n8n.io/flow-logic/error-handling/
- Hosting: https://docs.n8n.io/hosting/

---

END OF DOCUMENT