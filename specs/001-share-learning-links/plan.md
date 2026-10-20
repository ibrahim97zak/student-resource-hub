# Implementation Plan: Student Resource Hub

**Branch**: `codex/student-resource-hub-plan` | **Date**: 2026-10-20 | **Spec**: [spec.md](spec.md)

**Input**: Approved specification in `specs/001-share-learning-links/spec.md`, including
FR-020 and the instructor's planning direction. This document ends at design, not delivery.

## Summary

Extend the existing Vite single-page starter with Supabase email/password sign-in and a
shared resource collection at `/`. Use React local state, plain CSS, one resources table,
database constraints and RLS. Add/edit use a shared modal form; deletion uses an explicit
confirmation dialog. No new application dependencies or dependency upgrades are planned.

Fetch the small collection only after authentication, order by created_at descending and
id descending, and derive literal case-insensitive title search AND exact topic filtering
locally. Update local collection state only after confirmed writes. Never fall back to
mock resources or local-storage resource persistence.

## Technical Context

**Language/Version**: TypeScript 7.0.2; local Node 24.15.0 and npm 11.12.1 observed.

**Primary Dependencies**: Lockfile resolves React/React DOM 19.3.1, Supabase JS 2.118.0,
Vite 8.3.4, Oxlint 1.89.0. Keep package.json ranges and package-lock.json unchanged.

**Storage**: Supabase Postgres `public.resources`; Supabase Auth users supply ownership.
Supabase SDK session persistence is allowed; resources and form drafts are not persisted
as a substitute for database writes.

**Testing**: Existing `npm run build` (TypeScript + Vite) and `npm run lint` (Oxlint).
Plan focused validation checks with Node's built-in test runner and TypeScript stripping
for a pure, erasable-TypeScript validation module; browser and authenticated direct-write
checks are manual. No test framework, ESLint, or browser-test package is needed initially.

**Target Platform**: Modern desktop/mobile browsers; responsive checks at 320px and 1280px.

**Project Type**: Single frontend with managed Auth/Postgres, no application server.

**Performance Goals**: Meet SC-002's two-minute sharing walkthrough and exact results for
SC-003's 30-resource fixture. No invented production latency or traffic SLA.

**Constraints**: Root URL only; one sign-in and one collection screen; no router, ORM,
Express server, global state framework, component library, subscriptions, CI/CD, deploy
hooks, or automatic deployments. Credentials never enter commits or PR text.

**Scale/Scope**: Small live-workshop collection, three topics, two prepared test accounts.
No pagination or production-scale optimization in this version.

## Constitution Check

Pre-research gate: PASS. Post-design gate: PASS; no exceptions requested.

| Gate | Design evidence |
| --- | --- |
| Small readable code and dependency restraint | Existing stack, local state, narrow modules, no new dependencies |
| Approved specification and honest states | Contracts cover FR-001 through FR-020; pending/error/success are explicit |
| Keyboard and responsive access | Native dialogs, focus restoration, labels, visible focus, responsive grid |
| Database ownership and safe credentials | RLS plus column grants, publishable key only, direct-request tests |
| Honest progress and checks | quickstart separates current starter checks from future acceptance checks |
| Planning/task review gates | This planning branch needs human-reviewed PR merge; no feature implementation now |
| One-task delivery and human merge | Later assigned task gets one ID/branch/PR from latest verified main; stop on PR |
| Manual hosting | Render creation/deploy deferred to explicit request; Auto-Deploy Off, previews disabled |

The earlier specification and clarification PRs are verified merged. The implementation
plan still needs its planning PR reviewed and merged, then explicit task assignment.
Spec Kit's setup output used the feature identifier `001-share-learning-links` as BRANCH;
the actual explicitly managed Git branch is the one in this document's header. Automatic
Git branch management remains disabled; no template/script edits are needed.

## Project Structure

### Documentation (this feature)

```text
specs/001-share-learning-links/
  spec.md
  plan.md
  research.md
  data-model.md
  quickstart.md
  contracts/resources.md
  contracts/ui.md
  checklists/requirements.md
```

`tasks.md` is future speckit-tasks output, not created by this planning invocation.

### Source Code (repository root)

Proposed future layout; none of these additions are implemented by this plan:

```text
src/
  App.tsx                  # auth bootstrap and screen selection
  main.tsx                 # existing entry
  index.css                # reset, system font, shared tokens, visible focus
  App.css                  # screens, toolbar, grid and dialog layouts
  lib/supabase.ts          # one configured client; useful config failure
  features/auth/SignIn.tsx
  features/resources/
    types.ts
    validation.ts          # pure normalization and validation
    resources.ts           # explicit select/insert/update/delete operations
    ResourceCollection.tsx # request state, filters, modal orchestration
    ResourceCard.tsx
    ResourceForm.tsx        # shared create/edit fields
  components/Modal.tsx     # small native-dialog wrapper
  components/ConfirmDelete.tsx
  components/StatusMessage.tsx
supabase/migrations/
  <timestamp>_create_resources.sql
  <timestamp>_resource_validation_and_policies.sql
  # May be one atomic initial migration if assigned together; task scope decides.
tests/validation.test.ts
```

**Structure Decision**: Keep the root starter and scripts. Components stay small but avoid
one-line abstraction layers. Business validation is separate from rendering; one data
module is enough. Migration SQL, tests, and app files are future implementation artifacts.

### State and request design

App owns auth status (checking, signed out, signed in, error); initialize SDK session and
subscribe only to auth events, unsubscribing on cleanup. Prevent stale bootstrap results
from overriding newer events. Collection owns rows, loading/error, search, topic, modal
selection, and action status; use local state/hooks rather than global stores.

Invalidate outstanding list responses when account/session changes. Immediately conceal
collection and pending dialogs on auth loss; retain a failed-save draft only in memory,
keyed to its originating account, reveal only after that same account returns. Discard
on deliberate sign-out or account switch. Do not store drafts or passwords in logs.

Writes await returned row identity. A successful write changes local rows and closes the
form; refresh failure afterward is a distinct refresh error, not a failed write to retry.
A network-uncertain result keeps input and says outcome could not be confirmed; do not
retry automatically. List reload resolves uncertainty. An edited/new item outside active
filters still reports saved success. Missing-row mutations report unavailable/denied.

### Delivery sequencing and blockers

Design sequence for later task generation: database constraints/RLS and verification;
auth/configuration boundary; collection and filtering; create/edit/confirmation flows;
focused browser and ownership verification. This is dependency guidance, not assigned
implementation tasks or permission to build multiple tasks together.

No unresolved product/design blocker remains after the instructor retained full database
URL validation. Scheme-only validation is explicitly insufficient. Full syntax validation
must be implemented and tested in database SQL as described in data-model.md; inability
to match the supported URL contract is a delivery blocker, not permission to weaken FR-020.

Operational prerequisites remain unverified: prepared account usability, live schema and
policy state, and Render access/settings. Existing preparation notes report ignored local
environment configuration, but this plan did not inspect values or authenticate remotely.
Live acceptance requires manual SQL application and two working accounts. Deployment
requires separate explicit approval and verified Dashboard settings. None blocks writing
or reviewing these design artifacts.

## Complexity Tracking

No constitution violations or complexity exceptions. Full database validation is required
by FR-020; use a focused SQL helper, not a new server, ORM, or unverified extension.
