## Discourse Session-Aware Embed (App SSO Flow)

States
- Unauthenticated (no app session, no Discourse session)
- App session present (source of truth, not yet mapped to Discourse)
- Discourse session present (user mapped)
- Session sync in progress
- Error (session sync failed or token invalid)

Flow Steps
1. Embed loads and checks for existing app session (cookie or `/session` endpoint).
2. If app session present, ensure Discourse session is established via SSO exchange; set Discourse session.
3. Re-check Discourse session; if present, render authenticated embed content.
4. If no app session, render read-only embed state.

Fallback Login
- If no app session, render read-only content and show an inline login entry point (button or link) that opens the full app login (top-level).
- After interactive login completes, resume at Discourse SSO step and re-check session.

Session Sync Failure Handling
- Treat session sync errors as non-fatal.
- Clear any stale session state and show fallback login entry point.
- For network or token validation errors, show retry action and allow fallback login.

Return-to-Embed Behavior
- Always preserve embed return URL in state before redirecting to app login.
- After interactive login, redirect back to the embed URL and run the flow from step 1.
- If Discourse session is established, render content; otherwise keep fallback login visible.
