# Security

How AI Applyd handles your data and how to report a vulnerability.

---

## What we store

The minimum needed to run the product.

- **Account**: email, name, hashed Google OAuth identity (Better Auth)
- **Resume + profile**: PDF / DOCX uploads, parsed structured data (skills, experience, dates, etc.)
- **Application data**: jobs you've saved, jobs you've applied to, screener answers you've configured
- **Application results**: timestamps, status, ATS platform, proof screenshots from Browserbase on browser-driven submissions, structured submission receipts on direct-API platforms
- **Subscription state**: Stripe customer ID + subscription status (no card numbers, those live with Stripe)
- **Telemetry**: anonymised PostHog events for product analytics + session replays (replays exclude form inputs by default)

We do not store:
- Your LinkedIn / Indeed / job platform passwords (we never ask for them)
- Your raw OAuth refresh tokens for third-party services beyond what Better Auth needs for sign-in
- Your Stripe card details
- Anything you delete after you click delete

---

## Encryption

- **At rest**: Cloudflare D1's default at-rest encryption (managed by Cloudflare).
- **In transit**: TLS/HTTPS for all connections (browser, API, Browserbase, Stripe webhooks)
- **Auth tokens**: signed with HS256 + a 32+ char `BETTER_AUTH_SECRET` rotated on incident
- **OAuth**: standard Google OAuth flow, tokens stored in D1 by Better Auth

Access controls and regular reviews. No data sold.

---

## Retention

- **Active accounts**: data kept as long as the account is active
- **Deleted accounts**: hard-deleted within 24 hours of clicking delete. Cascades to all related rows (jobs, resumes, applications, billing history, telemetry events).
- **Cancelled subscriptions**: account stays active read-only for 90 days, then hard-deleted unless re-subscribed
- **Backups**: Cloudflare D1 daily snapshots, retained 30 days, encrypted at rest

We do not soft-archive. When you delete, the row is gone.

---

## Deletion path

One click. Dashboard -> Settings -> Delete account.

What happens:
1. Your Stripe subscription is cancelled immediately
2. Your D1 rows are hard-deleted
3. PostHog identify is reset, future replays are not associated with you
4. Browserbase screenshots are purged from R2
5. You receive a confirmation email
6. The whole flow takes under 30 seconds

No 30-day stalling. No "are you sure?" guilt trips. No buried setting. No retention email.

---

## Data isolation

- D1 rows are scoped by `user_id` foreign key
- Every API endpoint validates the session before reading user data
- The submission wall at /proof uses a SQL projection that literally cannot return user_id, company, or job URL to the client (enforced at the schema level, not just the application layer)
- Workers run in isolated V8 contexts, no shared memory between requests

---

## AI training

We do not sell your data. Your resumes, jobs, and application content are never used to train AI Applyd's own models.

- OpenRouter (our AI gateway) is called via API, not fine-tuned on your data
- Models we use (Gemini 2.5 Flash, Claude Haiku, etc.) are called via API, not fine-tuned on your data

Most "free" career AI tools bury training rights in paragraph 14 of their ToS. Read the competition's. Then read ours.

---

## Compliance

- **GDPR**: full rights including access, rectification, deletion, portability
- **CCPA**: California users have the same rights as GDPR users
- **SOC 2**: not yet certified, planned for 2026
- **HIPAA**: not in scope (we don't handle health data)

For data subject requests: privacy@aiapplyd.com.

---

## Reporting a vulnerability

Email **security@aiapplyd.com** with:
- A description of the vulnerability
- Steps to reproduce
- Optional: a proof-of-concept

We will:
- Acknowledge within 48 hours
- Triage and confirm within 7 days
- Patch and disclose within 90 days for non-critical, faster for critical
- Credit you in the changelog (with your permission)

Please do not:
- Publicly disclose before we've patched
- Test against real user accounts (use your own)
- Run scans that degrade service for other users

We don't run a paid bounty program yet, but if you find something serious we'll send merch and a public credit.

---

## Past incidents

None reported as of April 2026. If that changes, this section gets updated within 24 hours of disclosure.
