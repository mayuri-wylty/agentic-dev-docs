# Agent Roles Reference

## Fixed role: A Main Controller
Every workflow has exactly one A main controller.

A owns:
- planning
- project profiling
- workstream generation
- task decomposition
- subagent dispatch
- permission matrix maintenance
- progress tracking
- logs
- user communication
- final closeout

A is the only role that modifies final task status in `任务进度.md`.

## Project profile first
Do not start from fixed frontend/backend/SDK roles. First identify:

- project type
- deliverables
- technology stack and runtime
- directories/modules/services/pages/commands/data domains that can be edited independently
- shared contracts and conflict risks
- required review and audit gates

Generate workstreams from that profile.

## Workstream role types
Choose only roles that serve the project.

### Implementation workstream
Implements assigned tasks, runs self-tests, writes implementation feedback, and reports to A. Name it after the real module or deliverable, such as `admin-ui-workstream`, `sdk-cli-workstream`, `storage-executor-workstream`, `data-schema-workstream`, or `docs-revision-workstream`.

### Contract Review
Freezes API, CLI, DTO, schema, typed client, runtime config, or UI behavior boundaries before parallel implementation starts. Default read-only unless explicitly authorized to maintain contract docs.

### Integration workstream
Connects modules, services, clients, and runtime configuration, then verifies end-to-end flows. It is writable only within the permission matrix.

### Test Review
Reviews implementation output, runs tests, checks requirements/design/contract alignment, reports pass/fail/blocking issues, and retests fixes. Default read-only.

### Progress Consistency Auditor
Compares real code/docs/tests with `任务进度.md`. Finds missing work, incorrect done markers, stale progress, and inconsistencies. Default read-only.

### Security/Privacy reviewer
Reviews auth, secrets, permissions, sensitive data, logging, and threat boundaries. Default read-only.

### Documentation workstream
Maintains user-facing docs, runbooks, and manual test guides when documentation workload is large. Writable only in documented docs paths.

## Permission matrix requirement
Every generated role must appear in a permission matrix with:

- role/instance name
- role type
- default Codex agent type
- write permission
- writable scope
- forbidden scope
- required evidence
- review owner
- close condition

No role outside the matrix may edit product code.

## Serial and parallel rules
Main dependency chain:

```text
A preflights -> A assigns -> workstream implements -> workstream reports -> A assigns review -> reviewer reviews -> A assigns fix or accepts
```

Allowed parallelism:
- multiple independent implementation tasks with disjoint writable scopes
- review of submitted work while unrelated implementation continues
- progress audit during low-risk unrelated work

Forbidden parallelism:
- multiple workers changing the same contract or same file area
- reviewer approving work that has not been submitted
- A declaring completion before final review and final audit pass
- writable development before startup environment preflight
