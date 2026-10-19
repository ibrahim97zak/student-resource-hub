# Repository instructions

- Before every terminal or shell command you ask the user to run, briefly explain in plain language what it does. Keep each command in its own code block. For long-running commands, say what output to expect and whether the terminal should remain open.

## Workshop rules

Presenter: Ibrahim Zakarneh.

- The current stage is foundation preparation only. Do not implement features or generate feature specifications, plans, tasks, or the final constitution yet.
- The instructor will explicitly transition this file from foundation-only to live-workshop mode when beginning the constitution step.
- Later planning changes use a planning branch and PR.
- Every later implementation task uses its own task ID, branch, and PR against main.
- Start from the latest main after the previous task PR merges.
- Implement only the assigned task and stop after opening its PR.
- Human review and merge are required; no auto-merge.
- Do not push future implementation changes directly to main.
- Run meaningful local checks and report what actually happened.
- Keep credentials out of commits and PR descriptions.
- Do not create CI/CD, automatic deployments, or a Vercel Git connection.
- Deploy manually only after an explicit deployment request.
- The initial starter commit/push before protection is the bootstrap exception. It does not waive the later planning/task PR workflow.
- Manage task branches explicitly. Do not enable Spec Kit automatic Git branch management.
- Stack: React, Vite, TypeScript, plain CSS, Supabase Auth/Postgres. Leave auth UI, feature client, queries, forms, resources schema, migrations, and RLS for the live session.
