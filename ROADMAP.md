# Roadmap

What's done, what's in flight, what's coming, what's parked.

This is the public-facing roadmap. For bug reports and feature requests see [CONTRIBUTING.md](./CONTRIBUTING.md).

---

## Shipped (last 90 days)

- **Public submission wall** at [/proof](https://aiapplyd.com/proof). Browser-driven submissions get a proof screenshot, direct-API submissions (Greenhouse, Lever, Ashby, SmartRecruiters, join.com) get a structured submission receipt. All entries are anonymised at the SQL level (no user ID, no company, no job URL leak), then streamed to a public page. First competitor doing this.
- **Public MCP server** at mcp.aiapplyd.com. ATS scoring, job analysis, cover letter generation, auto-apply, resume translation, all callable from Claude Desktop / Claude Code / Cursor.
- **Cost-aware auto-apply guards**. Per-session cost ceiling that's progress-aware (kills only when cost > soft ceiling AND 3+ consecutive zero-progress steps), per-platform soft caps (Workday $1.50, Lever $0.80, generic $1.00), absolute hard cap at 2x soft. Replaced flat $0.50 ceiling that was killing legitimate runs mid-fill.
- **Stuck-loop fingerprint detector**. Auto-apply agent now recognizes when it's clicking the same button in a loop and bails with a clean error code instead of burning tokens.
- **Confirmation verifier**. Every successful submit needs at least 2 of 3 success signals (URL change, success keyword, hidden CSRF state) before it counts as sent. No more false positives.
- **Off-origin navigation guard**. Agent refuses to submit on a domain that wasn't the original ATS. Stops aggregator redirects from being mis-counted as applies.
- **Resume PDF + DOCX export**. ATS-aware formatting, proper section headings, no images that break parsers.
- **Chrome extension** published on Web Store. One-click apply from any job page.
- **20K free signup tokens**. Reduced from 50K after abuse. Free user gets exactly 1 full auto-apply end-to-end.
- **Direct-API auto-apply submitters (5 platforms)**: Greenhouse, Lever, Ashby, SmartRecruiters, join.com. Each returns a structured submission receipt with the ATS-side application ID. AI browser agent fallback (with Stagehand fastpath selectors) covers Workday, iCIMS, Workable, Breezy, Recruitee, custom career pages, LinkedIn Easy Apply, Indeed.
- **PostHog session replay** on all dashboards. Helps me debug user reports faster than they can describe them.
- **Better Auth + Google OAuth**. Sign in with Google works, OAuth flow opens in browser, MCP picks up the token automatically.

---

## Building now

- **Interview prep, personalized**. Pull actual interview questions from Glassdoor + Reddit threads about that exact company, generate practice answers using your resume. Currently testing on internal users. Eerily accurate at predicting which questions get asked.
- **Mobile experience**. Runs in any mobile browser as a responsive web app today (iOS Safari and Android Chrome, add to home screen for native-like UX). No native app yet, no mobile-first claim. A native shell with push notifications for "you got a callback" is on the table but not committed.
- **Application response tracker**. Match recruiter emails back to the application that triggered them, so the dashboard knows which apps converted.
- **Apply via direct ATS API where possible**. Skip the browser entirely on Greenhouse / Ashby / Lever where their public APIs accept submissions. Faster, cheaper, more reliable.

---

## Next 90 days

- **Salary negotiation copilot**. The moment after you get an offer and have to write the email saying "I'd like to discuss the compensation". That email is the highest-stakes 100 words a job seeker writes all year. Most people write it badly. I want to fix that.
- **Localized job search for non-US markets**. Today Indeed / LinkedIn / Google Jobs work in any market. The matching is US-centric. Non-US users want titles, salary bands, and seniority levels normalized for their geography.
- **Resume A/B testing**. Send two resume variants to similar roles, compare callback rates after 2 weeks. The data already lives in the tracker, just needs the UI.
- **Recruiter outreach mode**. Cold-email mode for the inverse direction: when a recruiter sees your profile and reaches out, draft a reply.
- **Public roadmap voting**. Ship a /roadmap page on aiapplyd.com where users can upvote upcoming features. Right now it's just my Notion.

---

## Considering

These are open questions. Tell me what you'd want.

- **Team / agency mode**. Career coaches and recruiters using AI Applyd on behalf of their clients. Multi-tenant, billing per seat. Real demand exists. Building it would mean a different product.
- **Insider-contact email finder**. JobRight ships this and it's the one feature their users genuinely praise. For roles where one warm intro beats 50 cold applications, this is the right tool.
- **Recruiter outreach mode**. Cold-email mode for the inverse direction: when a recruiter sees your profile and reaches out, draft a reply.
- **Job alerts via Telegram / Discord / SMS**. Right now alerts go via email. Could route them to chat channels for the people who treat their inbox like a graveyard.
- **Open-source the resume parser**. The PDF -> structured profile parser is good. Could open-source it. Would help the community, doesn't help me close a deal. Tradeoff.
- **API for resume scoring**. Some users have asked. Could be a paid endpoint. Not sure the volume justifies the support burden.
- **Voice mock interviews**. Record yourself answering generated questions, get AI feedback on tone + content. Fun. Not sure it's better than what users already get from text mock interviews.

---

## Not doing

- **Annual plans**. Monthly only by design. If we stop being useful you should be able to leave next month.
- **Free unlimited auto-apply**. Costs real money per submit. Would attract spammers and ruin the ban-risk math for legitimate users.
- **Script-injection inside LinkedIn's UI**. That's the pattern that triggers bans. AI Applyd handles LinkedIn Easy Apply via the AI browser agent on Browserbase with residential proxies and human-pacing, not by injecting scripts into LinkedIn's page.
- **Selling user data**. Ever.
- **Training AI Applyd's own models on your resumes**. Ever. We do not sell your data.
