# Feature Specification: Student Resource Hub

**Feature Branch**: `codex/student-resource-hub-planning`

**Created**: 2026-10-20

**Status**: Draft

**Input**: Students with prepared email/password accounts share learning links, browse and
search all resources, filter by topic, and manage only their own resources. Signed-out
visitors cannot access resources. This version excludes registration, password recovery,
uploads, comments, votes, notifications, real-time updates, and duplicate-link detection.

## Clarifications

### Session 2026-10-20

- Q: Must invalid resource data also be rejected when someone bypasses the form and submits it directly? → A: Reject invalid data from both forms and direct requests.

## User Scenarios & Testing *(mandatory)*

### User Story 1 - Sign In and Browse Resources (Priority: P1)

As a student, I can sign in with my prepared account and browse shared learning links.

**Why this priority**: Students need protected access and useful resources before other
journeys have value.

**Independent Test**: Use a prepared account and existing sample resources to verify sign-in,
listing, link opening, and denial of signed-out access without using creation or editing.

**Acceptance Scenarios**:

1. **Given** a signed-out visitor, **When** they open the app or attempt direct resource
   access, **Then** the app displays sign-in, and resource reads and changes are denied.
2. **Given** valid prepared credentials, **When** the student signs in, **Then** they see
   resources from all students, newest creation first, showing title, link, topic, and any
   supplied description.
3. **Given** invalid credentials or a failed sign-in request, **When** sign-in is attempted,
   **Then** a clear failure message appears and resource access remains denied.
4. **Given** resources are loading, **When** the student waits, **Then** loading is indicated;
   an empty successful result is distinguished from a failed load, which offers retry.
5. **Given** a listed resource, **When** its link is activated, **Then** the destination opens
   in a new tab while the resource list remains available.
6. **Given** a signed-in student, **When** they sign out or their session expires,
   **Then** resource content is removed from view, sign-in is shown, and subsequent reads
   and changes are denied until sign-in succeeds again.

---

### User Story 2 - Share a Learning Link (Priority: P1)

As a signed-in student, I can add a valid learning link for other students to discover.

**Why this priority**: Sharing creates the hub's learning content.

**Independent Test**: With prepared accounts A and B, create a resource as A, refresh, and
sign in as B to verify the saved resource is visible without testing editing or deletion.

**Acceptance Scenarios**:

1. **Given** student A and a valid title, absolute HTTP/HTTPS link, allowed topic, and optional
   description, **When** A saves, **Then** one resource is created with A as its owner,
   success is reported after confirmation, and the resource is first in an unfiltered list.
2. **Given** a saved resource, **When** A refreshes and B separately signs in and loads the
   list, **Then** both see the same saved field values.
3. **Given** a blank or spaces-only title, a title over 100 characters after trimming,
   an absent/invalid link, a missing/unsupported topic, or a description over 300 characters,
   **When** save is attempted, **Then** the invalid fields are explained and nothing is saved.
4. **Given** otherwise valid fields, **When** the trimmed title is exactly 100 characters
   and the description is exactly 300 characters or empty, **Then** the resource is accepted.
5. **Given** entered form values, **When** saving fails, **Then** a clear failure message
   appears, no success is reported, all entered values remain, and the student can retry.
6. **Given** a resource URL already shared by A, **When** B shares that same URL with valid
   fields, **Then** B's resource is accepted as a separate resource owned by B.
7. **Given** a save is pending, **When** the student activates save again, **Then** no second
   simultaneous submission is initiated and the pending state is visible.
8. **Given** a creation request that names a different owner, **When** A submits it,
   **Then** it cannot create a resource owned by another student.
9. **Given** a signed-in student bypassing the form, **When** they directly submit a create
   or update with a blank/overlong title, invalid link, unsupported/missing topic, or overlong
   description, **Then** the same validation rules reject the request, no invalid resource
   is stored, and an existing resource remains unchanged after a rejected update.

---

### User Story 3 - Find Resources (Priority: P2)

As a signed-in student, I can combine title search and topic selection to find useful links.

**Why this priority**: Search and filtering make a growing collection easier to use.

**Independent Test**: Use existing resources across all three topics, including titles
containing differently cased matching text; compare results to the expected intersection.

**Acceptance Scenarios**:

1. **Given** titles containing `React Basics` and `Advanced REACT`, **When** the student
   searches `react`, **Then** both match regardless of case, with newest matching first.
2. **Given** matching titles across different topics, **When** a search and React topic are
   selected, **Then** only resources matching both the title search and React topic appear.
3. **Given** active filters, **When** search is cleared, **Then** only the topic constraint
   remains; choosing All topics removes only the topic constraint; clearing both shows all.
4. **Given** a query appearing only in a description or link, **When** searching that query,
   **Then** it does not match unless the title also contains it.
5. **Given** no matching resources, **When** filtering completes, **Then** an explicit
   no-matches state appears and search/topic controls remain usable.

---

### User Story 4 - Maintain My Resources (Priority: P2)

As a student, I can correct or remove my own resources while others' resources stay protected.

**Why this priority**: Owners need to maintain useful content without altering others' work.

**Independent Test**: With existing resources owned separately by A and B, verify owner
editing/deletion, cancellation, failure handling, and attempted unauthorized changes.

**Acceptance Scenarios**:

1. **Given** A's resource, **When** A edits its fields and saves valid values, **Then** the
   update persists after refresh, B sees it on the next load, and owner/creation time stay
   unchanged. The list still follows active search and topic filters.
2. **Given** an edit form, **When** invalid fields are submitted or a save request fails,
   **Then** no success is shown, entered values remain available, and field validation
   follows the same rules as creation. Failed requests offer retry.
3. **Given** A's resource, **When** B views it, **Then** edit/delete controls are unavailable;
   attempts to bypass those controls and update or delete it are also denied.
4. **Given** A's resource, **When** A or B attempts to change its owner to another student,
   **Then** the ownership change is denied and the original owner remains unchanged.
5. **Given** A requests deletion, **When** confirmation appears and A cancels, **Then** the
   resource remains unchanged; **When** A confirms and deletion succeeds, **Then** success
   is reported and the resource stays absent after refresh and B's next list load.
6. **Given** a confirmed deletion, **When** the request fails, **Then** no success is shown,
   the resource is not presented as deleted, and a clear error allows another attempt.
7. **Given** a resource was deleted by its owner in another session, **When** an old edit or
   delete action is submitted, **Then** the app explains that it is unavailable, does not
   report success, and allows reloading the list without silently recreating the resource.

---

### Edge Cases

- Validation covers title lengths 0, 1, 100, and 101 after trimming and description lengths
  0, 300, and 301; surrounding title spaces do not consume the title limit.
- Links such as `https://example.org/lesson?q=react#start` and `http://example.org` are valid;
  relative paths, bare domains without a scheme, malformed URLs, and non-HTTP(S) schemes
  are rejected. Reachability of the destination is not a save requirement.
- Empty collections, zero filter matches, loading, and failed loading have distinct messages.
- Resources with equal creation times have a stable order; edits do not make a resource new.
- A successful create/edit may fall outside active filters. Report success accurately even
  if that resource is not in the current filtered results.
- Authentication loss during an action denies the action and requests sign-in. Unsaved
  values must not be erased by the failed save while the page stays open, but resource data
  must not be displayed to a signed-out visitor.
- Failed saves do not imply that the student can assume a resource was saved. After a lost
  response, reloading may be needed to establish the persisted outcome before retrying.
- All sign-in, browse, filter, form, and confirmation flows are operable by keyboard with
  visible focus, including at narrow viewport sizes.

## Requirements *(mandatory)*

### Functional Requirements

- **FR-001**: Students MUST sign in using prepared email/password accounts and be able to
  sign out. Signed-out and expired-session users MUST see sign-in and MUST NOT read,
  create, update, or delete resources, including through direct access attempts.
- **FR-002**: Every signed-in student MUST be able to read all saved resources regardless
  of owner. Each resource MUST show title, link, topic, and its description when supplied.
- **FR-003**: Resources MUST be sorted by creation time descending, including filtered
  results. Editing MUST NOT change creation time. Equal-time ordering MUST be stable.
- **FR-004**: Title search MUST match case-insensitive substrings of titles only. Trim
  surrounding search whitespace; an empty search MUST impose no title constraint.
- **FR-005**: Topic selection MUST offer React, JavaScript, AI, and All topics. Search and
  topic MUST combine as an intersection; All topics MUST impose no topic constraint.
- **FR-006**: Signed-in students MUST be able to add a resource owned by themselves.
  Creation MUST NOT assign ownership to another student.
- **FR-007**: A title MUST be trimmed before validation and saving, contain at least one
  character after trimming, and contain at most 100 characters after trimming.
- **FR-008**: A link MUST be required and, after trimming surrounding whitespace, be a
  valid absolute URL with an HTTP or HTTPS scheme and a hostname. Other schemes and
  relative links MUST be rejected. Destination reachability MUST NOT be required.
- **FR-009**: A resource topic MUST be exactly one of React, JavaScript, or AI.
- **FR-010**: Description MUST be optional and limited to 300 characters as entered.
- **FR-011**: Only the owner MUST be able to update or delete a resource. Enforcement MUST
  protect stored data even when the visible interface is bypassed; hiding controls alone
  MUST NOT grant protection. Ownership MUST remain immutable after creation.
- **FR-012**: Editing MUST allow changing title, link, topic, and description under the same
  validation rules as creation, and MUST leave owner and creation time unchanged.
- **FR-013**: Deletion MUST require explicit confirmation identifying the resource. Cancel
  MUST leave it unchanged. Only a confirmed, successful deletion MUST report success.
- **FR-014**: Successful creates, edits, and deletions MUST persist after refresh and be
  visible to another signed-in student on their next list load. Different students MUST
  be allowed to create separate resources with the same URL.
- **FR-015**: Resource links MUST open in a new tab.
- **FR-016**: Forms MUST explain invalid fields. Failed requests MUST show clear errors
  with a retry or sign-in recovery path as applicable. Failed saves MUST NOT show success
  or erase entered values. Missing resources MUST NOT be silently recreated by an edit.
- **FR-017**: The app MUST distinguish loading, empty collection, no matches, request
  failure, and successful actions. Success MUST follow confirmed persistence. Pending
  saves/deletes MUST prevent simultaneous repeat submissions of the same action.
- **FR-018**: All primary flows MUST support keyboard operation, visible focus, identifiable
  field labels and errors, and usable layouts at 320px and 1280px viewport widths without
  horizontal page scrolling.
- **FR-019**: Registration, password recovery, uploads, comments, votes, notifications,
  real-time updates, and duplicate-link detection MUST remain outside this version.
- **FR-020**: Resource validation rules in FR-007 through FR-010 MUST apply to both form
  submissions and direct create/update requests that bypass the form. Invalid requests
  MUST NOT create a resource or change an existing resource. Form validation alone MUST
  NOT be the only protection against storing invalid data.

### Key Entities *(include if feature involves data)*

- **Student**: A person identified by a prepared account who may sign in and own resources.
  Account preparation is external to the app; public profiles are not required.
- **Resource**: One learning link with a unique identity, title, link, topic, optional
  description, immutable owning student, and immutable creation time. Distinct resources
  may use the same URL.
- **Resource view criteria**: A student's current title search and selected topic, applied
  together to the shared collection; these are not shared resource attributes.

## Success Criteria *(mandatory)*

### Measurable Outcomes

- **SC-001**: With two prepared accounts, 100% of signed-out read/change attempts,
  cross-owner update/delete attempts, and ownership-transfer attempts in the acceptance
  checks are denied without unauthorized persisted changes.
- **SC-002**: A student can sign in and share one valid resource within two minutes in a
  workshop walkthrough, without instructor intervention; a second student can find and
  open it after loading their list.
- **SC-003**: With 30 sample resources spanning all three topics, every tested combination
  of case-insensitive title search and topic selection returns exactly the expected
  intersection in newest-first order.
- **SC-004**: Every validation boundary and failed-save acceptance check gives an
  understandable error, reports no false success, and preserves entered form values.
  Direct create/update attempts using invalid resource data MUST also be rejected in
  every tested validation case without creating or changing a stored resource.
- **SC-005**: Every successfully confirmed create, edit, and delete in the two-account
  walkthrough remains reflected after refresh and the other student's next list load.
- **SC-006**: A student can complete sign-in, search/filter, create, edit, confirmation
  cancellation, confirmed deletion, and sign-out using only a keyboard with visible focus;
  the same controls remain usable at both 320px and 1280px viewport widths.

## Assumptions

- The instructor supplies at least two usable prepared accounts and sample resources for
  verification. Account provisioning, registration, and password recovery are not app flows.
- Sign-out is included as the minimal way to end an authenticated session. Valid sessions
  survive ordinary refresh until sign-out or expiry; sign-in is required again after expiry.
- Search uses literal substrings, not fuzzy matching, regular expressions, or full-text
  ranking. Empty search and All topics are the initial view. Filter preferences need not
  survive refresh. These defaults resolve unspecified search details without adding scope.
- Description whitespace is preserved and counts toward its limit. Character limits count
  Unicode code points. Titles and links trim surrounding whitespace before validation/save.
- Equal creation times may use resource identity as a stable tie-breaker; no particular
  tie order is visible product behavior beyond being stable.
- Changes by another student appear after an explicit reload/refresh; automatic refresh,
  live updates, pagination, and sophisticated concurrent-edit conflict handling are not
  required. For simultaneous valid edits by an owner, the last successful save may win.
- Unsaved form preservation applies while the current page remains open; draft recovery
  after a reload, tab close, or sign-out is not required. Authentication loss may conceal
  retained input until the same student signs in again.
- A repeated URL is not a validation error, including for the same student; no duplicate
  detection is required. Retry after an uncertain network outcome does not promise
  exactly-once creation across separate requests.
- Workshop evaluation uses a working network and a small collection (30 sample resources);
  production-scale traffic targets and offline use are outside this version.
- Remaining ambiguity: no blocking clarification remains. The defaults above, especially
  substring search, character counting, session persistence, and concurrency behavior,
  remain explicit review points for the instructor.
- This specification is planning only. Implementation requires the reviewed planning PR
  to merge and the instructor to assign a task; deployment requires a separate explicit
  request under the project constitution.
