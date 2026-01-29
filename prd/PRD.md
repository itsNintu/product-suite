# Retell Feedback -> Roadmap -> Changelog PRD

## Overview
Retell will ship a unified feedback -> roadmap -> changelog application that captures customer input, turns it into prioritized roadmap items, and closes the loop with transparent updates. The app blends public community feedback with internal product planning, and it is delivered only via embedded views inside Retell's Discourse community. If the user is logged into the app, the embed auto-authenticates via app SSO; if not, the embed is read-only.

## Goals
- Centralize feedback intake and avoid losing signals across channels.
- Increase transparency on what Retell is building and why.
- Improve prioritization by combining internal strategy with community votes.
- Close the loop with timely changelog updates mapped to roadmap delivery.
- Drive higher engagement and retention in the Retell community.

## Non-goals
- Full project management replacement (e.g., Jira, Linear).
- Real-time chat or support ticketing.
- Public API v1 for external integrations (future work).
- Advanced experimentation/feature-flag management.

## Personas
- Public Visitor (Read-only): Wants to browse ideas, roadmap, and changelog without logging in.
- Community Member (Authenticated): Wants to submit ideas, vote, comment, and track progress.
- Customer Champion: Account manager who needs to represent a customer's needs.
- Product Manager: Curates ideas, manages roadmap, and publishes changelog.
- Engineering Lead: Needs clarity on prioritized items and release notes.
- Community Moderator: Ensures discussions are civil and organized.
- Executive: Looks for strategic alignment and visibility into demand.

## User Journeys
- Submit Idea -> Vote -> Roadmap -> Changelog
- User views ideas publicly; if logged into Discourse the embed auto-auths, otherwise login is required to submit, vote, or comment.
  - Idea is enriched, categorized, and visible to the community.
  - Users vote and comment; PM adjusts priority.
  - Idea becomes a roadmap item with a target quarter.
  - When shipped, it appears in changelog and auto-updates the idea.

- Internal Intake -> Publish
  - PM adds an internal idea with linked customer requests.
  - PM moves item to roadmap and sets owner/status.
  - PM publishes update and notifies watchers.

- Changelog Consumption
- User views changelog in the embedded view.
  - Login is required to react or subscribe to updates.
  - Related roadmap items link back to delivered features.

## Functional Requirements

### Ideas
- Create, edit, and merge ideas.
- Mandatory fields: title, description, category, submitter.
- Optional fields: tags, related customers, attachments.
- Status workflow: New -> Reviewing -> Planned -> In Progress -> Shipped -> Archived.
- Duplicate detection with suggested merges.
- Commenting and threaded discussion.
- Idea ownership and internal notes (hidden from public).
- Public users can view ideas read-only; creating/editing/commenting requires authentication.

### Voting
- Upvote and downvote (single vote per user).
- Weighted votes by plan tier and customer ARR.
- Anti-gaming controls: rate limits, suspicious activity flags.
- Vote visibility: total score and breakdown by segment (public).
- Voting requires authenticated users; public users can only view vote totals.

### Roadmap
- Create roadmap items from ideas or standalone.
- Views: Now/Next/Later, Quarterly, Kanban.
- Fields: title, summary, status, target window, owner, linked ideas.
- Public and internal visibility flags.
- Updates feed with change history.
- Dependency and risk notes (internal-only).
- Public users can view public roadmap items read-only; interactions (follow/subscribe) require authentication.

### Changelog
- Publish release notes with rich text and media.
- Link changelog entries to roadmap items and ideas.
- Reactions: like, celebrate, insightful (authenticated only).
- Subscribe to changelog notifications (authenticated only).
- Filters by product area and release type (feature, fix, announcement).

## Growth Loop & Notifications
- Submitting an idea prompts sharing within Discourse and email, initiated from the embedded view (authenticated only).
- Voting prompts subscribe-to-updates for that idea/roadmap item (authenticated only).
- Changelog updates notify watchers and related idea voters (authenticated only).
- Weekly digest email of trending ideas and recent releases.
- In-app nudges to upvote or comment on relevant ideas.

## Auth/SSO (App SSO)
- App session is the source of truth for embeds.
- IdP remains behind the app (not used directly by embeds).
- Support SSO for customers with enterprise connections.
- Map app session user to Discourse account for seamless embedding.
- JWT-based session exchange between Retell app and Discourse.
- Roles derived from app/session metadata.
- Embed auto-authenticates when the app session exists; otherwise it is read-only and requires login for interactions.

## Embed Strategy (Embeds Only)
- Embed ideas/roadmap/changelog views directly inside Discourse.
- Use Discourse theme component and iframe with app SSO-driven auth.
- Avoid direct IdP usage in the embed; rely on app session + SSO exchange.
- All interactions remain in the embedded view; only auth uses a top-level app login when needed.
- Styling uses basic shadcn defaults for now.
- Embedded views do not add Discourse navigation or back-navigation.
- Embedded views auto-auth when app session exists; otherwise show read-only and prompt login on interaction.

## Data Model
- User: id, name, email, org_id, role, plan, sso_user_id.
- Organization: id, name, plan, arr, sso_enabled.
- Idea: id, title, description, status, category, tags, submitter_id.
- Vote: id, idea_id, user_id, weight, type, created_at.
- RoadmapItem: id, title, summary, status, target_window, owner_id.
- ChangelogEntry: id, title, body, release_type, published_at.
- Comment: id, parent_type, parent_id, author_id, body.
- Notification: id, user_id, type, entity_id, delivered_at.

## Permissions
- Public (read-only): view public ideas, roadmap, and changelog; no interactions.
- Authenticated: submit ideas, vote, comment, react, subscribe.
- Moderators: edit/merge ideas, moderate comments, feature items.
- Product Managers: create roadmap, publish changelog, manage status.
- Admins: manage org settings, roles, SSO, analytics.

## Admin Tools
- Merge ideas with audit trail.
- Bulk status updates.
- Tag management and taxonomy editor.
- Org-level settings: vote weights, visibility, SSO requirements.
- Export data (CSV) for internal analysis.

## Moderation
- Spam detection with rate limiting.
- Report and flag system for inappropriate content.
- Auto-hide content after threshold flags.
- Moderation queue with approve/reject actions.

## Analytics/Metrics
- Idea submissions per week.
- Vote participation rate by segment.
- Time-to-planned and time-to-shipped.
- Changelog open rate and engagement.
- Conversion rate from idea to roadmap to shipped.

## Risks/Mitigations
- Low engagement: seed content and nudge via email digests.
- Vote manipulation: weighted votes and anomaly detection.
- Scope creep: keep roadmap data minimal and integrate later.
- Trust gaps: public visibility and clear status definitions.
- Discourse embed failures: show an in-embed error state (no full-page fallback).
- Auth gating reduces interaction: use lightweight login prompts and preserve view-only access.

## Milestones
- M1: Ideas + voting MVP embedded in Discourse.
- M2: Roadmap views + status workflow.
- M3: Changelog + notifications.
- M4: Admin tools + analytics dashboards.

## Open Questions
- Should enterprise customers have private idea boards? No
- What are the default vote weights by plan tier? They are all equal. There is only one tier
- Which Discourse events need webhook integration? No idea
- How strict should moderation auto-hide thresholds be? This can be a future functionality
- What data retention policy is required for archived ideas? Nil
