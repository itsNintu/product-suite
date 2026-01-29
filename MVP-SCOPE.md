# MVP Scope Summary

## Purpose
Deliver a minimal, reliable embedded experience that can be placed inside a Discourse theme component iframe and supports app SSO (auto-auth when app session exists; read-only when not).

## Requirements (Fixed)
- Embeds only (no standalone web app experience)
- Embed auto-auth when app session exists
- Embed is read-only when user is not logged in
- App SSO for any user interactions (IdP behind the app)
- Discourse theme component iframe as the host surface

## In Scope
- A single embedded experience that renders correctly inside a Discourse theme component iframe
- Public, unauthenticated read-only viewing
- App SSO login flow for interaction permissions
- Basic authenticated interactions (e.g., actions that change state or submit input)
- Minimal embed configuration parameters and documented constraints
- Basic analytics or logging for load, auth, and interaction events
- Accessibility and responsive behavior inside the iframe

## Out of Scope
- Standalone site or direct navigation entry points
- Private or paid content gating beyond public read-only
- Alternative identity providers or custom SSO flows
- Full Discourse plugin development or deep platform integration
- Advanced personalization, recommendations, or moderation tooling
- Multi-tenant admin consoles or extensive role management
- Offline support, background sync, or complex caching strategies

## Key Flows
- Embed load: Discourse theme component loads iframe; embed auto-auths when app session exists, otherwise renders read-only
- Interaction: user attempts an action, is prompted to authenticate via app login
- Authenticated action: user returns to the embed and completes the interaction successfully
- Session handling: user remains authenticated for subsequent interactions within the embed

## Milestones
1. Scope lock: confirm embed constraints, read-only behavior, and app SSO requirements
2. Prototype: embed renders inside Discourse iframe with public read-only view
3. Authenticated interactions: app SSO works end-to-end for actions
4. MVP hardening: accessibility, responsive behavior, and analytics/logging
5. Launch readiness: documentation, support checklist, and rollout plan
