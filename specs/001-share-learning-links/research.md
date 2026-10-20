# Research: Student Resource Hub

Date: 2026-10-20. Research is design evidence, not proof of remote configuration.

## Existing stack and small application structure

Decision: retain the lockfile versions listed in plan.md, npm, Oxlint, local React state,
and plain CSS. No routing or component dependency is needed for two screens at `/`.
Rationale: the starter already contains React, Vite, TypeScript, and Supabase JS; additions
would expand workshop explanation and maintenance without a demonstrated need.
Alternatives considered: Express/ORM, global state, component libraries, and framework
replacement are outside the instructor's constraints.

## Authentication, keys, and configuration

Decision: SDK password sign-in and sign-out, session bootstrap, and auth-state events;
frontend configuration uses only VITE_SUPABASE_URL and VITE_SUPABASE_PUBLISHABLE_KEY.
Rationale: the publishable key identifies the application; user sessions and database
policies decide access. Vite embeds prefixed values in the browser build, so secrets
cannot be placed there. Fail clearly when configuration is missing, never use mock data.
Alternatives considered: signup/recovery and server-managed credentials are out of scope.
Sources: [password sign-in](https://supabase.com/docs/reference/javascript/auth-signinwithpassword),
[API keys](https://supabase.com/docs/guides/getting-started/api-keys),
[Vite environment variables](https://vite.dev/guide/env-and-mode).

## Database authorization and immutable attributes

Decision: four authenticated-only RLS policies, revoke broad grants before narrow grants,
and permit updates only to title/url/topic/description. INSERT cannot supply id/created_at.
Rationale: RLS must check both the original and resulting owner on UPDATE; column grants
protect database-generated identifiers/timestamps from direct client writes. A UI check
alone cannot protect the table. Do not test ownership using privileged SQL-editor identity.
Alternatives considered: hiding buttons, service-role frontend keys, permissive policies,
and table-wide updates are rejected.
Sources: [RLS](https://supabase.com/docs/guides/database/postgres/row-level-security),
[column privileges](https://supabase.com/docs/guides/database/postgres/column-level-security).

## Validation parity and full URL syntax

Decision: retain full database URL validation, confirmed explicitly by the instructor
on 2026-10-20 during planning. A scheme check is only one necessary check, not the full
FR-020 contract. Plan a pure SQL/PLpgSQL validation helper called by CHECK constraints,
with matching browser validation cases and no network requests or optional extensions.
Rationale: a prefix such as https:// still permits missing or malformed hosts. Validate
absolute scheme, authority/host, port, escapes and illegal characters, including IPv6
and international-host handling, against a shared accepted/rejected fixture corpus.
Browser URL parsing alone is not evidence about direct database writes. Parser compatibility
must be demonstrated before accepting the assigned schema task; do not claim a casual
prefix regular expression meets the contract.

Trim the exact ECMAScript whitespace/line-terminator set consistently in frontend and SQL;
count Unicode code points (Postgres char_length and frontend code-point iteration).
Descriptions preserve whitespace. A DB normalization trigger may trim title/url before
CHECK constraints; it must not change owner, id, or creation time.
Alternatives considered: scheme-only database checks contradict the approved clarification;
adding an application server or requiring an unverified database extension is unnecessary.
Sources: [Postgres string functions](https://www.postgresql.org/docs/current/functions-string.html),
[URL standard](https://url.spec.whatwg.org/).

## Search, confirmed writes, and failure behavior

Decision: load the small ordered collection and apply literal lowercased substring plus
exact topic locally, avoiding SQL wildcard interpretation of search text. No realtime.
Request representations for writes, require exactly one returned ID, and distinguish
zero affected rows from success. Never use upsert for editing missing rows.
Rationale: success without a matched row misleads students; optimistic mutation and silent
fallback would complicate failed-save and ownership guarantees.
Alternatives considered: backend full-text search, automatic retries, and mock/local-storage
resource fallback add complexity or violate requirements.
Sources: [update](https://supabase.com/docs/reference/javascript/update),
[delete](https://supabase.com/docs/reference/javascript/delete),
[maybeSingle](https://supabase.com/docs/reference/javascript/maybesingle).

## Dialogs and visual presentation

Decision: native modal dialogs with explicit labels, focus entry/return, keyboard cancel,
and a shared form. System fonts, warm neutral backgrounds and one indigo accent.
Rationale: native dialogs provide a foundation for modal behavior without dependencies;
keyboard and responsive checks still must verify the actual implementation.
Alternatives considered: component libraries and decorative dashboard scaffolding are rejected.
Source: [HTML dialog](https://developer.mozilla.org/en-US/docs/Web/HTML/Reference/Elements/dialog).

## Hosting without automation

Decision: Render Dashboard manual deployment of reviewed main only after explicit request;
build npm ci followed by npm run build; publish dist. Auto-Deploy Off and PR previews disabled.
Rationale: creation triggers the first deployment, so even creation must wait for approval.
Environment values are set before build; changing them requires a new authorized build.
Alternatives considered: GitHub Actions, deploy hooks, Blueprints, automatic previews, and
creating a placeholder site during planning are prohibited.
Sources: [Static Sites](https://render.com/docs/static-sites),
[deploy controls](https://render.com/docs/deploys),
[service previews](https://render.com/docs/service-previews).
