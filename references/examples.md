# Examples Reference

## Example 1: Web SaaS dashboard
User asks for a CRM dashboard.

Recommended topology:
- A main controller
- frontend worker
- backend worker
- test review worker
- progress auditor

Required docs:
- `总控开发入口.md`
- requirements
- system design
- API contract
- frontend design
- backend design
- data schema
- multi-agent collaboration spec
- task progress
- manual test checklist

## Example 2: CLI developer tool
User asks for a CLI that batch-renames files.

Recommended topology:
- A main controller
- implementation worker
- test review worker
- progress auditor only at final handoff

Required docs:
- `总控开发入口.md`
- requirements
- CLI command contract
- system design
- task progress
- manual test checklist

## Example 3: AI assistant app
User asks for an LLM assistant with memory.

Recommended topology:
- A main controller
- backend/LLM orchestration worker
- frontend worker if UI exists
- test review worker
- security/privacy reviewer if secrets or user data are involved
- progress auditor

Required docs:
- `总控开发入口.md`
- requirements
- provider abstraction design
- prompt/config strategy
- memory/data design
- privacy/logging boundaries
- task progress
- manual test checklist

## Example 4: Small script
User asks for a small one-file utility.

Recommended topology:
- A main controller only, or A + one implementation worker + final review, depending on risk

Keep docs minimal:
- `总控开发入口.md`
- requirements
- task progress
- manual test checklist
