# Student Resource Hub Constitution

## Core Principles

### I. Small, Readable Implementation

Implementation MUST remain small, readable, and easy to explain during the live workshop.
Code MUST follow the approved specification and existing project conventions. Dependencies
MUST be added only for a concrete app need explained in the PR. This keeps the teaching
app understandable and avoids unnecessary maintenance.

### II. Clear User Feedback

Forms and actions MUST provide clear validation. Data-driven views and asynchronous actions
MUST communicate applicable loading, empty, error, and success states so students can
understand what happened and what to do next.

### III. Accessible, Responsive Interaction

Interactive flows MUST support keyboard use and visible focus. Layouts MUST remain usable
at narrow and wide viewport sizes. Local verification MUST cover interactions and layouts
affected by a change.

### IV. Database-Enforced Ownership and Credential Safety

Data ownership MUST be enforced through Supabase database policies, including row-level
security for owned data. Hidden buttons or client checks alone MUST NOT authorize access.
Passwords, private keys, credentials, and local environment files MUST NOT be committed.
Credentials MUST also remain out of PR descriptions and progress reports.

### V. Evidence and Honest Progress

Each task MUST run appropriate, meaningful local checks and report actual results,
including failures, skipped checks, and limitations. Progress updates MUST be included in
that task's PR. Unfinished work MUST NOT be marked complete; main reflects completion only
after the PR merges. Reviewers need evidence rather than assumed success.

## Technology and Deployment Constraints

- The stack MUST remain React, Vite, TypeScript, plain CSS, and Supabase Auth/Postgres.
  Preserve the npm lockfile, Oxlint, persistent Spec Kit 1.2.3.dev0, Codex integration,
  and PowerShell scripts.
- The frontend MUST use a Render Static Site; Supabase remains the backend.
- GitHub Actions, CI/CD, deployment hooks, and automatic deployments MUST NOT be created.
- Connecting the Render source repository is allowed only with Auto-Deploy Off and
  PR previews disabled. Pushes and merges MUST NOT trigger deployment.
- A Static Site MUST NOT be created or deployed during planning. Deployment MUST be manual
  and occur only after an explicit deployment request from the instructor.
- Render access and settings are currently unverified. Before an explicitly requested
  deployment, verify access and required settings rather than assume them.

## Delivery Workflow

- Presenter and instructor: Ibrahim Zakarneh. The Step 3.1 live-planning transition is
  explicitly authorized. Reviewed Spec Kit planning steps may proceed when requested;
  this authorization does not automatically invoke other planning commands.
- Planning documents MUST remain on the planning branch and be reviewed in one planning
  PR against main before implementation begins.
- Implementation MUST wait until the planning PR merges and the instructor assigns a
  delivery task. This includes auth UI, feature clients, queries, forms, resource schema,
  migrations, and RLS implementation.
- Every implementation task MUST have one ID, one explicitly managed branch, and one PR
  against main. Spec Kit automatic Git branch management MUST remain disabled.
- Before beginning each task, verify the preceding task PR has merged and start from the
  latest main. Implement only the assigned task, one task at a time. Unrelated tasks
  MUST NOT be combined.
- Implementation changes MUST NOT be pushed directly to main. The initial starter
  commit/push before protection was a bootstrap exception, not a continuing exemption.
- After opening a task PR, stop. The instructor reviews and merges it in GitHub.
  Human review and merge are required; auto-merge MUST NOT be enabled.
- Verify the merge before beginning the next task. Opening a PR alone does not complete
  the task on main.

## Governance

This constitution governs project work. AGENTS.md MUST remain aligned with it. Every
planning and task PR MUST be reviewed for compliance. Amendments MUST document the rule
changes, rationale, and required guidance updates in a human-reviewed PR approved and
merged by the instructor. Amendments do not waive task-assignment or deployment-request
gates unless the instructor explicitly changes those requirements.

Versions MUST follow semantic versioning: MAJOR for incompatible principle or governance
redefinitions/removals, MINOR for new principles or materially expanded guidance, and PATCH
for non-semantic clarifications. Amendments MUST update the last-amended date and preserve
the original ratification date. This initial constitution is adopted on 2026-10-20 under
the instructor's explicit request; implementation remains gated by the planning PR merge
and an assigned task.

**Version**: 1.0.0 | **Ratified**: 2026-10-20 | **Last Amended**: 2026-10-20
