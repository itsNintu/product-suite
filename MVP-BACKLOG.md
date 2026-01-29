# MVP Backlog

Concise epics, user stories, and acceptance criteria for MVP.

## Ideas

**Epic: Capture and triage ideas**

- User story: As a signed-in user, I can submit an idea with a title and description so it can be reviewed.
  - Acceptance criteria:
    - An idea requires a title; description is optional.
    - Ideas list is available as a public, read-only embed.
    - Only signed-in users can create ideas.
    - Ideas show status (New, Under Review, Planned, Done).

- User story: As a team member, I can tag and categorize ideas to organize the backlog.
  - Acceptance criteria:
    - At least one tag can be applied to an idea.
    - Tags are filterable in the embedded ideas list.
    - Category is optional and single-select.

## Voting

**Epic: Measure demand**

- User story: As a signed-in user, I can upvote an idea to signal interest.
  - Acceptance criteria:
    - Each user can cast one vote per idea.
    - Vote count is displayed on each idea card.
    - Users can remove their vote.
    - Voting controls are hidden or disabled for signed-out viewers.

- User story: As a team member, I can sort ideas by votes to prioritize.
  - Acceptance criteria:
    - Embedded ideas list supports sorting by votes.
    - Vote-based sorting is stable and deterministic.

## Roadmap

**Epic: Communicate plans**

- User story: As a team member, I can group ideas into a roadmap column to show status.
  - Acceptance criteria:
    - Columns: Now, Next, Later.
    - An idea can be in only one column at a time.
    - Column changes are reflected in the idea detail view.

- User story: As a viewer, I can see a public, read-only roadmap.
  - Acceptance criteria:
    - Roadmap is publicly viewable as an embed.
    - Roadmap shows idea title, status, and vote count.

## Changelog

**Epic: Share updates**

- User story: As a team member, I can publish a changelog entry.
  - Acceptance criteria:
    - Entry has title, summary, and date.
    - Entries are listed newest-first.
    - Draft entries are not visible to the public.

- User story: As a viewer, I can read past changelog entries.
  - Acceptance criteria:
    - Changelog is publicly viewable as an embed.
    - Changelog lists all published entries.
    - Each entry has a permalink.

## Auth/Embed

**Epic: Secure access and sharing**

- User story: As a user, I can sign in via app SSO (IdP behind the app) to access interactive features.
  - Acceptance criteria:
    - App SSO supports the configured identity providers.
    - App session persists across browser refresh.
    - Signed-out users can view embeds in read-only mode.
    - Signed-in users can interact (submit ideas, vote, comment if enabled).

- User story: As a team member, I can embed the product suite inside a Discourse theme component.
  - Acceptance criteria:
    - Embed is iframe-based and works within a Discourse theme component.
    - Public embeds are read-only by default.
    - Authentication gates all interactions within embeds.
    - Embeds render responsively on mobile and desktop.

## Notifications

**Epic: Keep users informed**

- User story: As a user, I can opt in to email updates for ideas I voted on.
  - Acceptance criteria:
    - Opt-in is per idea.
    - Users can opt out at any time.
    - Updates include status change and changelog publish.

- User story: As a team member, I can trigger a notification when an idea moves to Now.
  - Acceptance criteria:
    - Notification is sent to all opted-in voters.
    - Notification includes idea title and link.

## Admin/Moderation

**Epic: Maintain quality**

- User story: As an admin, I can edit, merge, or archive ideas.
  - Acceptance criteria:
    - Admin can update title, description, tags, and status.
    - Merge combines votes into the target idea.
    - Archived ideas are hidden from public lists.
