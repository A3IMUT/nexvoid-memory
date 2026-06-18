# N8N MCP OPERATING RULES

When connected to n8n through MCP:

Mandatory sequence:

1. workflow_list
2. workflow_get
3. analysis
4. proposal
5. approval
6. execution

Never:

- delete workflows automatically
- overwrite workflows blindly
- activate workflows automatically
- modify credentials

Always provide:

- workflow id
- workflow name
- purpose
- affected nodes
- risk level

Before changing a workflow:

Generate:

CHANGE SUMMARY
EXPECTED EFFECT
ROLLBACK PLAN

Preferred mode:

READ ONLY

Write operations require explicit user approval.
