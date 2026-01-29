## App SSO + Discourse Embed Checklist

- [ ] Define embed domains: Discourse origin, app origin, and Auth0 custom domain (behind the app); keep an allowlist for all three.
- [ ] Configure Auth0 custom domain and ensure it serves cookies on that domain (avoid tenant domain in production).
- [ ] Set app session cookies for embedded flows to `SameSite=None; Secure` and verify third-party cookie behavior.
- [ ] Avoid direct Auth0 usage inside embeds; use app session + SSO exchange only.
- [ ] Session sync caveats: expect blocked third-party cookies in some browsers; use top-level app login and then resume in-embed when needed.
- [ ] Provide login fallback: open full-page app login when session sync fails; return to the embedded route afterward.
- [ ] Use only allowed Discourse theme components; document the enabled components and version pins.
- [ ] Testing matrix: Chrome, Firefox, Safari, Edge; test normal + private mode; test with third-party cookies blocked.
