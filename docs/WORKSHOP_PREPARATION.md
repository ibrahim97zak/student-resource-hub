# Workshop preparation

Prepared for Ibrahim Zakarneh on 2026-10-20 (Asia/Hebron).

## Foundation

Path: `C:\Users\ibrah\Downloads\SED-SPEC`
Repository: https://github.com/ibrahim97zak/student-resource-hub (public).
Setup PR #1 is merged: https://github.com/ibrahim97zak/student-resource-hub/pull/1 (merge commit a45ac848748ef6df5fa1f231874c2aee583f2996). GitHub merge status was verified on 2026-10-20.

Windows 10.0.26300, PowerShell 7.6.5, Node 24.15.0, npm 11.12.1,
uv 0.10.7, GitHub CLI 2.83.2. Hosting target: Render Static Sites; Supabase remains the backend.
Specify CLI was upgraded persistently with uv from official github/spec-kit:
1.2.3.dev0, source commit f68ffcab37a253d42a226bc9bcf4fef451efa38c,
managed Python 3.14.3. The WindowsApps `python` alias does not run; Specify's own managed Python works.
React 19.3.1, Vite 8.3.4, TypeScript 7.0.2, Supabase JS 2.118.0.
npm is the package manager; package-lock.json is committed with the baseline.

## Startup and verification

`npm ci` installs locked dependencies. `npm run dev` starts Vite; keep its terminal open.
`npm run build` checks TypeScript and builds. `npm run lint` runs Oxlint.

Dependency installation, production build, and lint passed.
Browser verification passed at http://127.0.0.1:5174/: the page renders exactly
“Student Resource Hub - Workshop starter.” No app features or permanent auth client exist.
`.env.local`, `.env`, other `.env.*` files, and `.vercel/` are ignored; `.env.example` is trackable.

## Spec Kit: installed, not executed

Initialized using `specify init --here --integration codex --script ps --force --non-interactive`.
The workspace had only the freshly scaffolded starter; merge paths were checked before initialization.
`specify integration status --json` reports ok, Codex only, no missing or modified managed files.
No Git extension was installed. Task branch management stays explicit.
The generated constitution is an untouched placeholder template, not workshop principles.

Verified local skill directories under `.agents/skills` expose these live invocations:

- `$speckit-constitution`
- `$speckit-specify`
- `$speckit-clarify`
- `$speckit-plan`
- `$speckit-checklist`
- `$speckit-tasks`
- `$speckit-analyze`
- `$speckit-implement`
- `$speckit-converge`

No Spec Kit workflow skill has run. The instructor has explicitly authorized live planning in AGENTS.md. Keep the constitution as a placeholder until the next explicit `$speckit-constitution` call. Implementation must wait until the planning PR merges and the instructor assigns a delivery task.
Later each assigned implementation task gets its own ID, branch, PR, and human merge.

## Supabase: project, connection, and confirmed accounts verified

Dedicated project student-resource-hub verified in ibrahim97zak's Org (Free), region ap-northeast-1 (Tokyo), project ref zsenrarawfgnvlulvvbo. Database password was set privately by the instructor.
Dashboard access and project identity are verified. Email authentication is enabled. Auth settings endpoint returned HTTP 200 using the publishable key and confirmed email provider enabled. The redacted temporary test-results JSON was inspected: both prepared accounts report signIn=true and signOut=true. These are results of the instructor-run local smoke test; it was not rerun during this documentation update.
Prepared account emails: lina@example.com and omar@example.com. Both accounts were verified in the authenticated dashboard with Confirmed at timestamps (Lina 00:26, Omar 00:27 on 2026-10-20). That dashboard inspection preceded the later successful sign-in/sign-out test.

Evidence: the redacted results file at `C:\Users\ibrah\AppData\Local\Temp\student-resource-hub-auth-results.json` contains only the account emails and sign-in/sign-out booleans. Accounts were not recreated, and passwords were not inspected.

The publishable key is appropriate for browser code. Later resource-table RLS policies enforce row permissions.
Never use a secret or service-role key in VITE_ variables or frontend files.
No resources table, resource migrations, RLS policies, or app auth flow has been created.

## GitHub and Render

Saved GitHub keyring account ibrahim97zak is accessible. An invalid GITHUB_TOKEN environment override
was omitted only in setup command processes; no credentials were printed or committed.
Public repository supports branch protection on GitHub Free.
Bootstrap push completed. Effective protection was read back successfully through the GitHub API.
Verified: PRs for main including administrators, zero required approvals for solo presenter,
no status checks, no force pushes/deletion, no auto-merge. No CI workflows.

Render Static Sites is the frontend hosting target. Supabase remains the backend.
Render source-repository connection is allowed, but Auto-Deploy must be Off and PR previews disabled.
Render account/workspace access and all deployment settings are currently UNVERIFIED. These are requirements, not confirmed settings.
No Render Static Site has been created or deployed by this setup. Do not create/deploy one during planning.
No CI/CD or deploy hooks. Manual deployment requires an explicit later request.

## Live planning transition and branch verification

AGENTS.md now permits reviewed Spec Kit planning. Keep these changes for the planning PR; human review and merge remain required.
Each later assigned delivery task uses its own branch and PR from latest main after the previous merge.
Before this planning branch was created, the two document edits were temporarily preserved, local main was fast-forwarded to origin/main at `a45ac848748ef6df5fa1f231874c2aee583f2996`, and `docs/live-planning-transition` was created from that synchronized main. The document edits were then restored without conflicts; existing history was preserved.
These documentation changes are delivered through the planning transition PR for human review and merge.
No implementation task IDs were created; no planning skill, account smoke test, build, or lint was rerun for this documentation-only update.

## Official references

- https://github.github.com/spec-kit/reference/core.html
- https://github.github.com/spec-kit/reference/integrations.html
- https://github.github.io/spec-kit/installation.html
- https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches
- https://supabase.com/docs/guides/auth/users

Local .env.local is configured with only the project URL and publishable key and is confirmed ignored by Git. Render access remains unverified.
