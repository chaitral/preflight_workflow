# preflight_workflow
Built as a take-home assignment for the Technical Program Manager (Engineering Operations) role.

# Workflow Architecture
Webhook (POST /preflight-check)
    │
    ▼
Code Node — Data Lookup
(filter dataset by team + stage, compute severity)
    │
    ▼
IF Node — Status Check
(overallStatus !== "Good"?)
    │
    ├── TRUE (Warning / Poor)
    │       │
    │       ▼
    │   Code Node — Build Alert Message
    │   (⚠️ / 🚨 checklist + remediation steps)
    │
    └── FALSE (Good / No Records)
            │
            ▼
        Code Node — Build OK Message
        (✅ all-clear or ℹ️ no history)
    │
    ▼
Merge Node
    │
    ▼
Respond to Webhook
(returns JSON with Markdown notification field)

## How to run

1. Install n8n: `npm install -g n8n && n8n start`
2. Import `preflight_workflow.json` via the n8n UI (Import Workflow)
3. Activate the workflow using the toggle
4. POST to `http://localhost:5678/webhook/preflight-check` with:
   { "team_name": "Alpha Squad", "current_sdlc_stage": "Coding" }

## Architecture & Scaling Notes

