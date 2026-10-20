# Resource Data Contract

## Client boundary

Use the installed Supabase JS client, project URL and publishable key; authenticated
requests carry the student's session. No custom REST server, RPC write service, or secret
key. Public browser configuration does not authorize resource access by itself.

| Operation | Input | Expected result |
| --- | --- | --- |
| List | Authenticated session | Select id,owner_id,title,url,topic,description,created_at; order created_at DESC then id DESC |
| Create | owner_id from session plus validated content fields | Insert with returned full row; exactly one row required |
| Edit | Resource id and title/url/topic/description only | Update by id with returned full row; exactly one row required |
| Delete | Confirmed resource id | Delete by id with returned id; exactly one returned id required |

Do not include id/created_at in insert or owner_id/id/created_at in updates. Do not use
upsert. Duplicate URLs are permitted. Browser ownership checks decide which controls to
show; grants/RLS and constraints enforce requests independently.

## Search and topic

Read the collection only after sign-in. Lowercase trimmed search and resource titles,
then apply literal substring matching and exact selected topic together. All topics and
empty search remove only their own constraint. Symbols such as % and _ stay literal.
Derive matches from the ordered list without sorting by title or updated time. Filter state
stays when editing/creating; a saved item can validly disappear from filtered results.

## Result and failure semantics

A request succeeds only when no error is present and exactly one requested resource is
returned for a mutation. Zero-row update/delete is unavailable/denied, never success.
RLS may intentionally hide distinctions between missing and unauthorized rows.

Map validation errors to field or form errors without logging credentials. Transport,
authorization, and constraint failures preserve draft fields and leave the dialog open
unless authentication must conceal it. No optimistic success or silent local fallback.
After lost responses say the result cannot be confirmed and offer list reload before
another manual attempt. Never automatically repeat a mutation.

After confirmed writes update local rows from the returned representation. A failed list
refresh after a successful mutation must not present the mutation as failed or invite
recreation. Ignore responses from a prior account/session. Sign-out clears resource state.

## Verification matrix

| Actor/request | Read | Insert self | Insert as other | Edit/delete self | Edit/delete other | Change owner/id/time |
| --- | --- | --- | --- | --- | --- | --- |
| Signed out | Denied | Denied | Denied | Denied | Denied | Denied |
| Student A | All | Allowed | Denied | Allowed | Denied | Denied |
| Student B | All | Allowed | Denied | Allowed | Denied | Denied |

For each invalid title, URL, topic, and description case, repeat direct INSERT and UPDATE
with the legitimate owner's token: both reject, with no new row or changed row. Successful
HTTP transport or an empty returned row list alone does not pass a mutation check.
