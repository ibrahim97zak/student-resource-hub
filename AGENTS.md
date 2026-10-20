# Repository instructions

- Before every terminal or shell command you ask the user to run, briefly explain in plain language what it does. Keep each command in its own code block. For long-running commands, say what output to expect and whether the terminal should remain open.

## Workshop rules

Presenter: Ibrahim Zakarneh.

- The current stage is live planning, explicitly authorized by the instructor. Reviewed Spec Kit planning steps are allowed. App implementation must wait until the planning PR merges and the instructor assigns a delivery task.
- Keep the constitution as a placeholder until the next explicit constitution skill call. Do not execute planning skills merely because live planning is now allowed.
- Keep planning document changes on the planning branch for a human-reviewed planning PR against main.
- Every later implementation task uses its own task ID, branch, and PR against main.
- Start from the latest main after the previous task PR merges.
- Implement only the assigned task and stop after opening its PR.
- Human review and merge are required; no auto-merge.
- Do not push future implementation changes directly to main.
- Run meaningful local checks and report what actually happened.
- Keep credentials out of commits and PR descriptions.
- Host the frontend on Render Static Sites; Supabase remains the backend. A Render source-repository connection is allowed, with Auto-Deploy Off and PR previews disabled. Do not create CI/CD, automatic deployments, or deploy hooks.
- Render access and all deployment settings are currently unverified. Do not create or deploy a Static Site during planning. Deploy manually only after an explicit deployment request.
- The initial starter commit/push before protection is the bootstrap exception. It does not waive the later planning/task PR workflow.
- Manage task branches explicitly. Do not enable Spec Kit automatic Git branch management.
- Stack: React, Vite, TypeScript, plain CSS, Supabase Auth/Postgres. Preserve the npm lockfile, Oxlint, persistent Spec Kit 1.2.3.dev0, Codex integration, and PowerShell scripts. Auth UI, feature client, queries, forms, resource schema, migrations, and RLS require the approved planning PR and an assigned implementation task.
