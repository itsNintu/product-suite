# Resend Setup (Product Suite)

This doc covers email setup using Resend.

## Prerequisites
- Resend account
- Verified sending domain

## Domain Verification (Resend)
- Add the sending domain in Resend.
- Add the required DNS records (SPF, DKIM) to your DNS provider.
- Wait for verification to pass in Resend before sending.

## Environment Variables
Create an API key in Resend and add to `.env.local` (already present in this repo):

```
RESEND_API_KEY=YOUR_RESEND_API_KEY
```

## Usage (High‑Level)
- Use Resend API for transactional notifications:
  - Idea status changes (planned/in‑progress/shipped)
  - Roadmap → shipped
  - Changelog published

## Recommended Sender Setup
- From: `updates@YOUR_DOMAIN` (or a verified subdomain)
- Reply‑to: `support@YOUR_DOMAIN`

## Integration Hook (Placeholder)
- Add the email send hook in the app where notifications are triggered.

## Notes
- Keep templates simple for MVP (plaintext + minimal HTML).
- Marketing emails must include unsubscribe footer and list‑unsubscribe headers.
- Transactional emails do not require unsubscribe, but avoid mixing marketing content.
