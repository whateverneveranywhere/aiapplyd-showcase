# Changelog

User-facing product updates. Internal infra and refactors are not listed.

---

## April 2026

- **2026-04-29** Public submission wall at /proof. Every successful auto-apply gets a Browserbase screenshot, anonymised at the SQL level, streamed to a public page.
- **2026-04-26** Per-session cost guard rewritten to be progress-aware. Replaced flat $0.50 ceiling that was killing legitimate Greenhouse runs mid-fill. Soft caps now per-platform (Workday $1.50, Lever $0.80, generic $1.00).
- **2026-04-25** Free signup tokens reduced from 50K to 20K plus a free-first-apply bypass. New free user gets exactly one full auto-apply end-to-end before paywall.
- **2026-04-24** Auto-apply retry cap dropped from 3 to 1 (2 total attempts). A single stuck Lever / Greenhouse job was burning 4 Browserbase sessions, this caps the blast radius.
- **2026-04-23** Stuck-loop fingerprint detector live. Auto-apply agent now bails with a clean error when it spots a click loop instead of burning tokens.
- **2026-04-20** Confirmation verifier shipped. Every successful submit needs at least 2 of 3 success signals (URL change, success keyword, hidden CSRF state) before it counts as sent.
- **2026-04-15** Resume PDF + DOCX export with ATS-aware formatting. No images or 2-column layouts that break parsers.
- **2026-04-10** Public MCP server at mcp.aiapplyd.com. Connect Claude Desktop, Claude Code, or Cursor and use the tools directly.
- **2026-04-05** Direct ATS API integrations for Personio, SmartRecruiters, Workable, Recruitee, Teamtailor, Pinpoint, Comeet, JazzHR, Breezy. Now 14 direct ATS integrations total.
- **2026-04-01** Chrome extension published on Web Store. One-click apply from any job page.

---

## Earlier

- **2026-03** Hired in 30 / Hired Yesterday tier rename from previous Pro / Premium naming. Same caps, more honest tagline.
- **2026-03** Application response tracker beta. Match recruiter emails back to the application that triggered them.
- **2026-02** First public version. Greenhouse + Lever + Workday auto-apply, ATS scoring, resume builder.
