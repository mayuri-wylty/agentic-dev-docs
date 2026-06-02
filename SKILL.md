---
name: agentic-dev-docs
description: Strict manual-trigger skill for docs-first agentic software development planning. Use only when the user explicitly names `agentic-dev-docs`, says `use agentic-dev-docs skill`, invokes `$agentic-dev-docs`, or directly asks to run this exact skill.
---

# Agentic Dev Docs

Version: 0.6.2

## Manual Trigger Only

Use this skill only when the user explicitly names `agentic-dev-docs`, `use agentic-dev-docs skill`, `$agentic-dev-docs`, or asks to run this exact skill. Do not auto-trigger for ordinary planning, coding, docs, or multi-agent discussion.

## Purpose

Turn a project idea or task spec into a docs-first development control pack. The pack lets a new main-control development session read startup docs, confirm it is the single A main controller, create or reuse workstreams from the project shape, coordinate implementation, testing, Review, progress audit, and handoff.

## Phase Boundary

Planning and documentation generation are not product implementation.

During requirements, collaboration design, plan, and documentation generation:
- Do not create product code, scaffolds, dependencies, codegen outputs, migrations, or dev servers.
- Do not modify product code directories.
- Generate or edit development docs only after the user confirms the plan or explicitly asks to implement the documentation plan.

Main-control development execution is a separate phase. It starts only when the user opens a new session with `00_新窗口启动提示词.md` or explicitly asks to start from `总控开发入口.md`. In that phase, implementation workstreams may write only within their authorized scopes.

## Hard Rules

1. Exactly one `A-main-control`.
2. A main control coordinates, decides conflicts, maintains progress, schedules Review, audits, and reports. It does not implement product-code work in multi-agent execution.
3. Product code, schema, OpenAPI, typed client, tests, and deployment-script changes must be assigned to an authorized workstream or logical workstream.
4. Human labels such as Developer A, Developer B, QA, and Tech Lead are responsibility or Review labels, not agent identities. Convert them to project-shaped workstreams or Review owners.
5. `A-main-control` must not appear as implementer, writable owner, or primary assignee for product implementation tasks. If it does, the generated docs are wrong and must be fixed.
6. Workstreams come from the project shape, not fixed frontend/backend/SDK templates.
7. Every writable role must be in the permission matrix before assignment.
8. Contract, Review, audit, and explorer roles are read-only unless the docs explicitly authorize narrow writes.
9. Real subagents are created only when the user asks for multi-agent, subagent, delegation, or parallel development. Otherwise use logical workstream records.
10. Final closeout is serial: environment check -> implementation evidence -> Review pass -> audit pass -> A reports to user.

## A Main Control Must Not "Just Do It"

If a task has been assigned to a workstream, A main control must not do it directly because it is small, convenient, or already open.

To move a workstream task to A direct execution, all three must happen first:
- User explicitly authorizes the change.
- Permission matrix is updated.
- Progress docs record the responsibility change.

If no real subagent exists, A still must create a logical workstream record and execute under that workstream's scope, log, evidence, and Review rules. Do not mix A-control identity with worker identity.

## Project Shape First

Before creating workstreams, document the project shape:
- Project type: CLI, Web, desktop, mobile, AI app, data app, multi-service system, docs/paper project, or other.
- Deliverables: code, config, scripts, docs, tests, deployment, data, model.
- Tech stack and runtime: language, framework, package manager, database, browser, container, OS.
- Parallel boundaries: independent directories, modules, services, pages, commands, data domains, doc chapters.
- Shared risks: API, DTO, schema, migration, typed client, shared components, config, deployment scripts, permissions, security.

## Workstream Rules

Each writable workstream must have:
- instance name;
- type: implementation-workstream, docs-workstream, contract-review, test-review, security-review, progress-audit, one-shot-investigation;
- lifecycle: persistent, workstream, or one-shot;
- whether multiple instances are allowed;
- reuse scope;
- writable scope;
- forbidden scope;
- required evidence;
- recommended reasoning effort: low, medium, high, or xhigh;
- Review owner;
- creation condition;
- close condition.

Typical generated names should match the project, such as `sdk-cli-workstream`, `admin-ui-workstream`, `storage-executor-workstream`, `data-schema-workstream`, or `docs-revision-workstream`.

## Reasoning Effort Policy

Use reasoning effort as a task-risk control. When real subagents or APIs support it, `A-main-control` must set or request the selected level during assignment. When direct setting is unavailable, record the recommended level in the task assignment, workstream registry, or permission matrix.

- `A-main-control` defaults to high.
- Final decisions, cross-workstream conflicts, permission defects, and final audits use xhigh.
- Ordinary implementation workstreams default to medium.
- Simple lookup, formatting, organization, and low-risk docs cleanup use low or medium.
- Contract review, security review, progress audit, and high-risk Review use high; final closeout audit uses xhigh.

## Reuse Rules

Only one effective instance may exist for the same lifecycle and reuse scope.

- `persistent`: one global primary instance, such as the single A main control.
- `workstream`: reuse for the same module, directory, deliverable, or context chain.
- `one-shot`: use only for isolated investigation, validation, or Review. If later tasks need the same context, upgrade to `workstream`.
- Record every reuse: task, instance name, reason, latest output location.
- If duplicates exist, A pauses assignment, chooses the most complete instance as primary, merges other outputs into the registry/progress docs, then closes duplicates.
- Do not create a duplicate workstream because there are many tasks, a window is long, or switching seems convenient.

## Permission Matrix

Every output pack must include a permission matrix. Minimum columns:

| Column | Required content |
|---|---|
| Role / instance | exact name |
| Type | A main control, implementation-workstream, review, audit, docs, one-shot |
| Default agent | current session, worker, explorer, logical instance |
| Product-code write | yes, no, or restricted |
| Writable scope | directories, file types, module, or docs |
| Forbidden scope | shared contracts, other modules, secrets, build outputs, unrelated docs |
| Required evidence | tests, build, screenshot, log summary, Review, audit |
| Recommended reasoning effort | low, medium, high, or xhigh |
| Review owner | exact role |
| Close condition | objective proof |

Roles outside the matrix cannot modify product code.

## Phase Workflow

1. Requirements clarification: ask only questions that change the plan.
2. Collaboration design: offer 2-3 options, always with one A main control.
3. Plan generation: produce a decision-complete plan.
4. Documentation generation: create docs only after plan confirmation.
5. Main-control handoff: new session uses startup docs to execute.

## Plan Must Include

- Project summary and project shape.
- Frozen decisions and assumptions.
- Documentation set size: minimal, standard, or complete. Prefer references in `references/document-set.md` over repeating long lists.
- Docs to create.
- Workstream registry, reuse strategy, and permission matrix.
- Recommended reasoning effort in workstream registry, permission matrix, or task assignments.
- Contract-first rule for APIs/data.
- Startup environment checks.
- Code commit/push policy. Default: do not commit or push code during execution unless the project plan or a later user instruction explicitly authorizes it.
- Review and audit gates.
- Acceptance criteria.
- Mapping from human responsibility labels to workstreams or Review owners.

Implementation tasks in the plan must be assigned to workstreams, not A main control.

## Documentation Set Guidance

Use existing references and templates instead of copying long content into the skill:
- `references/workflow.md`
- `references/agent-roles.md`
- `references/document-set.md`
- `references/templates.md`
- `references/examples.md`
- `assets/doc-templates/`

The generated pack must include at least:
- startup prompt with a separate copyable `/goal` execution objective;
- startup environment checklist;
- main-control entry;
- requirements/spec;
- collaboration rules;
- A main-control rules;
- workstream rules;
- progress doc;
- human test checklist;
- docs acceptance checklist.

## Self-Check Before Completion

Before claiming docs are complete, verify:
- startup prompt exists and clearly enters main-control execution phase;
- startup prompt includes a separate copyable `/goal` execution objective;
- `/goal` objective states that code must not be committed or pushed by default unless the plan or user explicitly authorizes it;
- startup environment checklist exists;
- main-control entry exists;
- progress doc exists;
- there is exactly one A main control;
- project shape is documented before workstreams;
- each role has lifecycle, reuse scope, multi-instance rule, writable scope, forbidden scope, evidence, Review owner, and close condition;
- each real subagent or logical workstream has a recommended reasoning effort;
- permission matrix exists;
- product implementation tasks are assigned to workstreams, not A main control;
- human responsibility labels are not converted into A implementation tasks;
- reuse and duplicate-instance handling are explicit;
- A cannot do assigned workstream tasks without explicit user authorization, matrix update, and progress record;
- final audit is a required gate;
- human test checklist exists;
- docs acceptance checklist exists;
- no unfilled placeholder markers remain unless the user requested a blank template.

## Failure Handling

- Requirements too vague: do not generate plan; ask 1-3 material questions.
- User skips plan and asks for docs: give a short plan summary and ask for confirmation unless they explicitly insist.
- User asks for docs and code together: split docs phase first, development phase later.
- User does not want multi-agent: use A-only flow with Review/test checklists; do not create implementation workstreams.
- Tech stack unclear: offer 2-3 options and recommend one.
- Permission matrix missing or writable scope unclear: do not assign development tasks.
- Duplicate workstreams created: pause, select primary, merge outputs, close duplicates.
- Product-code task assigned to A main control: treat as doc defect; rewrite assignment to a workstream.
- A main control already did workstream work: stop, record violation, fix matrix/progress; without explicit user authorization, return work to the workstream.
