# Local Run, Verification, and Manual Hosting Guide

## Scope and prerequisites

This is a future implementation validation guide, not a claim that the feature exists.
The current app remains the workshop starter. The planning PR must merge and the instructor
must assign each implementation task before building. Use one task ID, branch, PR from
latest main; report actual check results in its PR, stop after opening it, and verify the
human merge before the next task. Do not execute the entire design as one unassigned task.

Use the existing package-lock.json and npm. Current observed tools are Node 24.15.0 and
npm 11.12.1. Keep dependency versions, Oxlint, Spec Kit/Codex integration and PowerShell
scripts. No added test framework or SQL CLI is required for the guide.

Have two prepared Supabase email/password accounts A and B, correct project URL and
publishable key, SQL Editor access for an explicitly assigned database task, and browser
access. Do not paste credentials into PRs, screenshots, logs, or committed files.

## Local environment and run

From the repository root, install exactly the locked dependency tree. This exits after
installation; expect npm's install summary, and investigate failures without updating versions.

```powershell
npm ci
```

Use the existing .env.example as a key-name reference. Configure ignored .env.local locally
with VITE_SUPABASE_URL and VITE_SUPABASE_PUBLISHABLE_KEY. Never place a secret/service-role
key in the frontend. Do not print these values as part of checks. Vite embeds VITE_ values
at build time; restart the development server after environment changes. Missing values
must produce a useful configuration error after implementation.

Start Vite at the root page. Expect a local URL; open it in a browser. Keep this terminal
open while testing and stop it with Ctrl+C when finished.

```powershell
npm run dev
```

## Manually apply assigned SQL

Only after the database implementation task is assigned and its SQL is reviewed, use the
correct Supabase project's Dashboard SQL Editor to apply versioned supabase/migrations
files in filename order during the session. Read each file first; apply only pending files,
prefer a transaction, and stop on error. Do not create a separate untracked schema by
clicking table settings. No SQL file is generated or applied by this plan.

Record project identifier (not credentials), migration filenames, application time, and
observed result in the task's progress report. Verify columns/defaults/constraints,
authenticated column grants, anonymous privilege revocation, RLS enabled, and all four
policies. A merged migration is not proof of live application. Do not test RLS only through
the SQL Editor's privileged connection; use A/B sessions and a signed-out request.

Database checks may create/delete only identified workshop test fixtures. Never use a
service-role key to prove browser ownership. Distinguish an explicit error from a zero-row
RLS result: either can deny access, but neither proves a successful mutation.

## Local checks

Run the existing Oxlint script. Expect its lint result and an exit status; it terminates
normally. Do not install ESLint or replace the script to make a check pass.

```powershell
npm run lint
```

Run TypeScript and Vite production checks. Expect a successful build and dist output;
the command terminates. This does not deploy or prove the remote schema is configured.

```powershell
npm run build
```

After the assigned validation module and focused tests exist, run the planned pure
validation checks using Node's built-in test runner. Expect passing/failing test counts;
the process terminates. This command is future guidance, not runnable against the starter.

```powershell
node --experimental-strip-types --test tests/validation.test.ts
```

Preview the production output locally after building. Expect a local URL; keep the terminal
open during browser checks and stop it with Ctrl+C afterward. This is not Render deployment.

```powershell
npm run preview
```

Check changes for whitespace errors before a PR. Expect no output on success; it exits.

```powershell
git diff --check
```

## Validation scenarios after implementation

Use spec.md plus contracts/resources.md and contracts/ui.md as the acceptance reference.

| Check | Procedure | Expected evidence |
| --- | --- | --- |
| Access | Signed-out page and direct table reads/writes; then sign in A | No signed-out data/access, prepared login works, no signup/recovery |
| Browse | Load 30 fixtures across topics and owners | All rows newest first with stable ties; edit does not reorder by creation |
| Filters | Search mixed-case title substring, select each topic, clear each independently | Exact intersection; title only; %/_ literal; correct no-match state |
| Required/boundary values | Title blank/space/1/100/101 after trim; description 0/300/301 | Correct acceptance/errors and preserved input; Unicode code-point counts |
| URL parity | Repeat accepted/rejected corpus from data-model.md in form and direct requests | Full HTTP(S) syntax enforced by database and form; https:// rejected |
| Topics | Missing/unsupported topic in form and direct writes | Rejected; existing rows unchanged after invalid edits |
| Persist/share | A creates, refreshes; B loads; repeat edit/delete and B reload | Same stored values; confirmed deletes remain absent |
| Duplicate URLs | A and B share the same URL | Separate owned resources allowed |
| Ownership | B targets A's id; A inserts B owner; A changes owner/id/created_at | No unauthorized data changes, including bypass attempts |
| Failure | Offline/block requests during save/delete/list; restore network and retry | No false success, no draft erasure, no silent local/mock fallback |
| Missing row | Delete same fixture in other owner session; submit stale edit | Unavailable message, no upsert/recreation or false success |
| Confirmation | Open delete, cancel/Escape; reopen and confirm | Cancel sends no write; confirm writes once; pending repeats disabled |
| Auth race | Expire session/switch accounts with request pending | Protected rows concealed; old responses/drafts never shown to B |
| Accessibility | Keyboard all flows; inspect 320px/1280px; long content | Focus entry/return, labels/errors, no page overflow, usable grid |
| External links | Activate Open resource, inspect target/rel | New tab, noopener noreferrer, hostname displayed |

Use browser network controls to simulate a lost response separately from a known rejected
write. The app must say the outcome is unconfirmed, preserve input, and offer reload before
manual retry. It must not promise exactly-once creation across uncertain separate requests.

Record pass/fail/not-run and reason for each applicable check in the assigned task PR.
Capture non-sensitive evidence; do not claim completion based on build/lint alone.

## Render deployment: only after a separate explicit request

Do not create a Static Site during planning or implementation verification. Creation starts
its first build/deployment. Before an authorized deployment, verify Dashboard access and
settings, reviewed main commit, manually applied database migrations and acceptance evidence.

In Render Dashboard choose Static Site and the connected repository, branch main. Set
Auto-Deploy Off and PR previews Disabled (not Manual). Verify the options before creation;
if the creation flow cannot ensure this, stop and report the actual blocker. No Blueprint,
GitHub Actions, deploy hook, or automatic job is part of this process.

Set the build command in the Dashboard to install the lockfile and build the frontend:

```text
npm ci && npm run build
```

Publish directory: dist. Set build-time VITE_SUPABASE_URL and
VITE_SUPABASE_PUBLISHABLE_KEY before building; never configure secrets/service-role keys.
Keep the app at `/`, with no client routes or SPA rewrite needed for this version.

Only after the instructor's deployment request, create the site (initial authorized deploy)
or use Manual Deploy for the reviewed main commit on an existing site. Verify the deployed
commit matches reviewed main, the root page, sign-in and authenticated access, and ensure
Auto-Deploy remains Off and previews disabled. Later pushes/merges must not deploy.
Changing environment variables requires another explicitly authorized manual build/deploy.

Official references are linked in research.md. Render access, creation options, and all
remote settings remain unverified until that deployment session.

## Evidence from this planning session

Verified PR #5 is merged and fetched latest main; created codex/student-resource-hub-plan
explicitly. Read starter/lockfile/scripts, resolved the plan template, reviewed official
documentation and retained full database URL validation by instructor answer. No schema,
Auth configuration, accounts, Render service, app source, dependency, or pipeline changed.
Current starter checks: npm run lint passed (Oxlint, exit 0); npm run build passed
(TypeScript and Vite, exit 0). These are starter checks, not feature acceptance.
future feature checks above have not been run.
