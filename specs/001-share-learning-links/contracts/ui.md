# UI Contract: Student Resource Hub

## Screens and visual system

Root URL `/` renders either auth checking, sign-in, or collection; no additional routes.
Configuration failure shows a useful setup error, not a blank page or dummy collection.
Use a system font stack, warm off-white background, white cards, dark text, indigo actions,
subtle neutral borders, and restrained shadows. Shared CSS custom properties define color,
spacing (4/8/12/16/24/32px), radii, focus ring, and content width. Verify readable contrast;
never convey topic/error/selection only by color. Avoid decorative dashboards.

Sign-in has email/password labels, password masking, pending state, clear error, and submit.
Do not trim passwords; no registration/recovery flows. After sign-in header shows Resource
Hub, short purpose text, current account email, and sign-out. Toolbar has labeled title
search, All topics/React/JavaScript/AI controls, and prominent Add resource.

Cards show topic badge, title, optional description, parsed website hostname and Open
resource. Links use target=_blank and rel=noopener noreferrer, visibly communicate new-tab
behavior, and render user values as text (never injected HTML). Edit/delete appear for
owned cards and remain easy to find without hover. Wrap long titles/URLs without overflow.

Use a one-column narrow grid and additional columns as width permits. Toolbar wraps at
320px; dialogs fit viewport width/height with internal scrolling. No horizontal page scroll.

## Forms and dialogs

Shared create/edit form fields: title, link, topic, optional description. Associate labels,
limits and errors with fields. First invalid field receives focus after rejected submit;
announce request failures and success using appropriate status/alert regions.

Use native dialog showModal with a labeled heading, focus inside on opening, keyboard
focus containment, and restore focus to opener on close (or next sensible control if a
deleted card no longer exists). Add/edit have a visible Cancel action and Escape cancel
while idle. Cancel discards that draft without writing. No accidental backdrop dismissal.
While a request is pending disable repeated submit and dismissal/cancel, show Saving or
Deleting, and restore actionable controls on failure. Auth loss always conceals the dialog.

Delete confirmation identifies the resource title, offers Cancel and Delete resource,
and initially focuses Cancel. Only Delete resource starts the write. Cancellation/Escape
while idle makes no request. Failed deletion keeps resource and confirmation available
with an error; zero affected rows asks the student to reload rather than report success.

## Collection states

- Checking session/loading collection: explicit status, no premature empty-state message.
- Initial empty collection: explain no resources exist and offer Add resource.
- No matching search results: explain zero matches and keep search/topic controls usable.
- Load failure: recoverable error and Retry; do not mislabel as empty or show mock resources.
- Validation failure: field-specific messages, all entered values retained.
- Save failure: form stays available with unchanged values; no success announcement.
- Confirmed action: clear success; do not reset active search/topic.
- Session lost: sign-in replaces protected content. In-memory failed-save draft is scoped
  to original account and may reappear only for that account; deliberate sign-out discards it.

## Browser acceptance

Use prepared A/B accounts and 30 sample rows to exercise spec stories and SC-001..SC-006.
Check keyboard-only operation, focus return, Escape/Cancel, screen-readable errors/status,
external link attributes, long content, 320px/1280px layout, failed requests, expired session,
and account switching during an in-flight list request. Browser checks supplement direct
request/database ownership checks; neither substitutes for the other.
