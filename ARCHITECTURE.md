# Retell Feedback -> Roadmap -> Changelog
# Technical Plan (Architecture)

## 1) Summary
Build a standalone app (modern stack) for ideas → roadmap → changelog, embedded inside Discourse via iframe/theme component. If the user is logged into the Retell app, the embed auto-authenticates via app SSO (matching Discourse session); otherwise the embed is read-only. Styling uses basic shadcn defaults for now.

## 2) Constraints (Confirmed)
- Discourse is hosted on **community.retell.ai** (paid tier) with a custom theme.
- We cannot modify Discourse navigation; embeds only (no nav changes).
- The new app should live on a subdomain (e.g., **feedback.retell.ai**).
- App SSO is the source of truth for sessions; Auth0 remains behind the app.
- Public read‑only access; auth required for interactions.

## 3) Assumptions / TBD
- Web app stack: Next.js (App Router) + shadcn UI (proposed).
- Database: Convex (proposed) with Convex schema and server functions (TBD).
- Email provider: Resend (confirmed).

## 4) High‑Level Architecture
**Web App:** Next.js (App Router, proposed) + shadcn UI (baseline) on **feedback.retell.ai**.
**Backend:** Next.js API routes or server actions (TBD) to serve data and auth‑gated actions.
**Database:** Convex (proposed) with Convex schema and server functions (TBD).
**Auth:** App SSO (Auth0 behind the app; not used directly by embeds).
**Email:** Resend (confirmed).

## 5) Embed Strategy (Embeds Only)
- Discourse theme component embeds iframe pointing to:
  - `/ideas`
  - `/roadmap`
  - `/changelog`
- Each view is self‑contained and has **no app navigation** (Discourse owns nav).
- Iframe is styled minimally; shadcn defaults only for MVP.

## 6) Auth Model
**Public:** read‑only access to Ideas/Roadmap/Changelog when not logged into the app.
**Authenticated:** app session is source of truth; embed auto-auths via app SSO and establishes Discourse session for interactions.

### In‑embed interaction flow
1. User loads embedded view (auto-auth if app session exists; read-only if not).
2. On interaction → trigger app login.
3. After login → return to the same embedded route and continue action.

### App SSO considerations
- App session cookies should support embedded flows (`SameSite=None; Secure`).
- Avoid direct IdP usage inside the embed; rely on app session + SSO exchange.
- If iframe third‑party cookie restrictions block session reuse, use top-level app login and then resume in-embed.

### Auth-embed findings (Jan 2026)
- Discourse does not support full-page iframe embedding; only theme components are allowed on the hosted paid tier.
- App session is the source of truth; embeds should not call Auth0 directly.
- Iframe remains third-party; session reuse can break due to browser privacy blocks.
- Provide a fallback “Log in” button inside the embed when session sync fails.
- Token handoff from Discourse is likely infeasible on hosted Discourse without plugins.

## 7) Data Model (v1)
- **User**: id, sso_user_id, email, name, org_id, role
- **Organization**: id, name, plan, arr
- **Idea**: id, title, description, status, category, tags, submitter_id
- **Vote**: id, idea_id, user_id, type (up/down), weight
- **RoadmapItem**: id, title, summary, status, target_window, owner_id, linked_idea_ids[]
- **ChangelogEntry**: id, title, body, release_type, published_at, linked_roadmap_ids[]
- **Comment**: id, parent_type, parent_id, author_id, body
- **Notification**: id, user_id, type, entity_id, delivered_at

## 8) API Surface (v1)
**Public (read‑only)**
- `GET /ideas`
- `GET /roadmap`
- `GET /changelog`

**Authenticated**
- `POST /ideas`
- `POST /ideas/:id/vote`
- `POST /ideas/:id/comment`
- `POST /subscriptions` (idea/roadmap/changelog)

**Admin**
- `POST /roadmap`
- `POST /changelog`
- `PATCH /ideas/:id/status`

## 9) Growth Loop & Notifications
**Auto‑subscribe:**
- Idea submitter
- Anyone who votes

**Trigger emails:**
- Idea → Planned/In Progress
- Roadmap → Shipped
- Changelog published

**Recipients:** submitter + all voters/subscribers.

## 10) Roles & Permissions
- Public: read‑only view.
- Authenticated: submit, vote, comment, react, subscribe.
- Moderator: edit/merge ideas, moderate comments.
- PM/Admin: manage roadmap + changelog.

## 11) Risks & Mitigations
- **Iframe auth limitations** → app session + top-level login; avoid direct Auth0 in embeds.
- **Auth gating reduces interaction** → lightweight prompts, preserve view‑only access.
- **Spam** → rate limits + moderation queue.

## 12) Open Questions
- Email provider choice (SendGrid/Postmark/Resend?).
- Database choice & hosting (Convex).
- Need for token handoff from Discourse if app session sync fails.
