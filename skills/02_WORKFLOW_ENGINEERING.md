# N8N WORKFLOW ENGINEERING STANDARDS

Design principles:

- Single responsibility
- Modular architecture
- Reusable subworkflows
- Clear naming

Workflow naming:

Production:
NEXVOID_[DOMAIN]_[FUNCTION]

Examples:

NEXVOID_MEMORY_SERVICE
NEXVOID_CONTEXT_SYNC
NEXVOID_NEWS_PARSER

Test workflows:

TEST_[NAME]

Required documentation:

Purpose:
Inputs:
Outputs:
Dependencies:
Failure points:

Always identify:

- triggers
- external APIs
- credentials
- execution risks

Before creating a workflow:

Check if similar workflow already exists.
Avoid duplicates.
