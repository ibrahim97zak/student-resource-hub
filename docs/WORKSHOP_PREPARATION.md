# Workshop preparation

Prepared for Ibrahim Zakarneh on 2026-10-20 (Asia/Hebron).

## Foundation

Path: `C:\Users\ibrah\Downloads\SED-SPEC`
Repository: https://github.com/ibrahim97zak/student-resource-hub (public).
Bootstrap commit 086bb15 is pushed to main. This follow-up handoff is delivered through a setup PR; human review/merge is required.

Windows 10.0.26300, PowerShell 7.6.5, Node 24.15.0, npm 11.12.1,
uv 0.10.7, GitHub CLI 2.83.2, Vercel CLI 64.7.0.
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

None was run. Next live action after setup: create the planning branch from latest main,
explicitly transition AGENTS.md to live-workshop mode, and invoke `$speckit-constitution`.
Later each assigned implementation task gets its own ID, branch, PR, and human merge.

## Supabase: project, connection, and confirmed accounts verified

Dedicated project student-resource-hub verified in ibrahim97zak's Org (Free), region ap-northeast-1 (Tokyo), project ref zsenrarawfgnvlulvvbo. Database password was set privately by the instructor.
Dashboard access and project identity are verified. Email authentication is enabled. Auth settings endpoint returned HTTP 200 using the publishable key and confirmed email provider enabled. Account password sign-in/out are NOT VERIFIED.
Prepared account emails: lina@example.com and omar@example.com. Both accounts were verified in the authenticated dashboard with Confirmed at timestamps (Lina 00:26, Omar 00:27 on 2026-10-20). Neither had signed in at inspection.

Manual continuation if dashboard access remains unavailable:
1. Sign in at https://supabase.com/dashboard and select the intended Free organization.
2. Create project student-resource-hub on the free tier; choose a suitable region and set the database password privately. Do not accept a paid upgrade.
3. Under Authentication > Sign In / Providers, verify Email/password is enabled.
4. Under Authentication > Users > Add user > Create new user, create each demo email with its own strong password entered privately; enable Auto Confirm User. Use Create, not Invite, to avoid email delivery.
5. In the project's Connect/API settings, copy the project URL and publishable key into ignored `.env.local`, using only VITE_SUPABASE_URL and VITE_SUPABASE_PUBLISHABLE_KEY as shown in `.env.example`.
6. Run the prepared temporary C:\Users\ibrah\AppData\Local\Temp\student-resource-hub-auth-check.ps1 in PowerShell. It prompts with Read-Host -AsSecureString, signs in/out for each account, and writes only email/pass-fail booleans to a temporary results JSON. No account smoke test has run yet.

The publishable key is appropriate for browser code. Later resource-table RLS policies enforce row permissions.
Never use a secret or service-role key in VITE_ variables or frontend files.
No resources table, resource migrations, RLS policies, or app auth flow has been created.

## GitHub and Vercel

Saved GitHub keyring account ibrahim97zak is accessible. An invalid GITHUB_TOKEN environment override
was omitted only in setup command processes; no credentials were printed or committed.
Public repository supports branch protection on GitHub Free.
Bootstrap push completed. Effective protection was read back successfully through the GitHub API.
Verified: PRs for main including administrators, zero required approvals for solo presenter,
no status checks, no force pushes/deletion, no auto-merge. No CI workflows.

Vercel CLI installed persistently. Initial whoami reported logged out; normal device login flow was started and awaits user completion.
Account/team access is NOT VERIFIED. No project linked, repository imported, Git connection, hook, or deployment created.

## Official references

- https://github.github.com/spec-kit/reference/core.html
- https://github.github.com/spec-kit/reference/integrations.html
- https://github.github.io/spec-kit/installation.html
- https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-protected-branches/about-protected-branches
- https://supabase.com/docs/guides/auth/users

Local .env.local is configured with only the project URL and publishable key and is confirmed ignored by Git. Vercel whoami was rechecked and remains logged out.
