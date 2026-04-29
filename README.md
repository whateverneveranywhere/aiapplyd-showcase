<div align="center">

<img src="./assets/hero.png" alt="AI Applyd" width="640" />

# AI Applyd

**AI that actually gets your job.**

The job market is rigged. The ATS kills your resume in 6 seconds, recruiters ghost, and the tools that promise to fix it either spam, hallucinate, or hide a $150 paywall. AI Applyd is the cheat code: real per-job tailoring, real submissions on Greenhouse, Lever, Ashby, SmartRecruiters, and join.com via direct API, an AI browser agent for Workday, LinkedIn Easy Apply, iCIMS, and the long tail. Every submit gets a receipt or a screenshot. Tokens refund when the run isn't your fault.

[![Live](https://img.shields.io/badge/live-aiapplyd.com-orange)](https://aiapplyd.com)
[![License](https://img.shields.io/badge/license-MIT-blue)](./LICENSE)
[![Pricing](https://img.shields.io/badge/free%20tier-20K%20tokens-green)](https://aiapplyd.com/pricing)
[![Submission Wall](https://img.shields.io/badge/proof-screenshot%20verified-lightgrey)](https://aiapplyd.com/proof)

[**Try it free**](https://aiapplyd.com) - [**Pricing**](https://aiapplyd.com/pricing) - [**Submission Wall**](https://aiapplyd.com/proof) - [**MCP server**](https://mcp.aiapplyd.com) - [**Blog**](https://aiapplyd.com/blog)

</div>

---

## Why this exists

I lost my job in February. Three months of runway, a working laptop, and 47 hand-written applications by Sunday afternoon. By the 30th cover letter I was a zombie typing "I am excited to apply for this opportunity at your great company" while a real human read none of it.

So I wrote a Chrome extension that filled in forms based on a JSON resume. The 20 jobs the bot applied to got 3 callbacks. The 47 I'd done by hand got 1. That ratio is what AI Applyd is now.

Every day you wait, someone less qualified gets your job. The applying-while-you-sleep part is the gimmick. The real product is a filter: you say yes to everything that fits, and only spend time on the conversations that come back.

\- Ava

---

## Demo

![dashboard demo](./assets/dashboard.gif)

Live product: **[aiapplyd.com](https://aiapplyd.com)** - free signup, 20K tokens funds one full end-to-end auto-apply.

---

## What it does

Application tracker (Teal Plus). ATS score (Jobscan $50). AI cover letters (Simplify $80). Resume tailoring (Rezi). All here. Free tier covers it.

- [x] **ATS resume scoring** - paste a job description or URL, get a 0-100 compatibility score with section-level feedback in 30 seconds
- [x] **AI resume tailoring** - per-job rewrite grounded in your real history, no invented skills, no hallucinated metrics. PDF + DOCX export
- [x] **Auto-apply on 5 direct-API platforms** - Greenhouse, Lever, Ashby, SmartRecruiters, join.com. Structured submission receipt with the ATS-side application ID on every send
- [x] **AI browser agent for everything else** - Workday, LinkedIn Easy Apply, iCIMS, Workable, Breezy, Recruitee, Indeed, custom career pages. Captcha auto-solve via Browserbase. 2FA escalates to a session live-view link emailed to you
- [x] **Cover letter generation** - tailored per job, references the actual posting, written in your voice
- [x] **Interview prep** - company-specific questions pulled from real interview reports, practice answers grounded in your resume
- [x] **List-view application tracker** - receipts list, not a kanban. Stages: Applied, Callback, Phone Screen, Interview, Offered, Rejected, No Response. Every row has the screener answers we submitted, the match score at the time, the proof screenshot or ATS receipt, and a clear failure reason if a run didn't complete
- [x] **Submission proof on every apply** - browser-driven runs save a screenshot of the confirmation page. Direct-API runs save a structured receipt. Public anonymised wall at [/proof](https://aiapplyd.com/proof)
- [x] **Public MCP server** - connect Claude Desktop, Claude Code, Cursor, or ChatGPT and use the tools directly in your AI client ([mcp.aiapplyd.com](https://mcp.aiapplyd.com))
- [x] **Token refunds on non-user-fault failures** - if a run dies because the page was unreadable, the ATS was down, or bot detection blocked the session, tokens go back to your balance automatically
- [x] **Per-session cost guard** - per-platform spend caps (Workday $1.50, Lever / Ashby $0.80, generic $1.00). Stuck sessions get killed before they bleed money
- [x] **Anti-fabrication guardrails** - the system refuses to invent job titles, certifications, metrics, or skills not on your resume. If a JD demands something you don't have, the tailor flow surfaces it as a gap instead of forging it

---

## How it works

```
1. Upload resume     -> AI parses experience, skills, dates into a profile
        |
        v
2. Score against     -> 0-100 ATS compat score, missing keywords called out
   job description
        |
        v
3. Tailor + generate -> Per-job resume + cover letter, you preview before send
        |
        v
4. Auto-apply        -> Direct-API submission on Greenhouse / Lever / Ashby /
                        SmartRecruiters / join.com returns a STRUCTURED RECEIPT
                        with the ATS-side application ID. Every other site
                        (Workday, LinkedIn Easy Apply, iCIMS, Workable, Breezy,
                        custom career pages) routes through the AI browser
                        agent on Browserbase, which fills the form, handles
                        captchas, escalates 2FA, and saves a PROOF SCREENSHOT
                        of the confirmation page.
        |
        v
5. Track + follow up -> Every application saved, status updates, recruiter
                        emails matched back to the application
```

A bad auto-apply tool keeps clicking buttons in a loop until it submits something broken. A good one knows when to stop. AI Applyd has a per-session cost guard, a stuck-loop fingerprint detector, an off-origin navigation guard, and a confirmation verifier that needs at least 2 of 3 success signals (URL change, success keyword, hidden CSRF state) before it marks an apply as sent.

---

## Pricing

Monthly only. No annual lock-in. No upsell on auto-apply credits hidden behind the subscription.

| Plan | Price | AI applies | ATS scores | Resume tailors | Tokens |
|------|-------|------------|------------|----------------|--------|
| **Job Seeker** | Free | 1 (taste-test) | 10 / month | 5 / month | 20K signup |
| **Hired in 30** | $39 / mo | 100 / month, 15 / day | Unlimited | Unlimited | 7M / mo |
| **Hired Yesterday** | $79 / mo | 300 / month, 50 / day | Unlimited | Unlimited | 15M / mo |

Per-application math at full utilization: **$0.39** on Hired in 30, **$0.26** on Hired Yesterday.

[Full pricing](https://aiapplyd.com/pricing)

---

## Tech stack

Built solo, on Cloudflare's edge.

- **Frontend**: TanStack Start, TanStack Router, Tailwind, shadcn/ui, Zustand
- **Backend**: Hono on Cloudflare Workers
- **Database**: Cloudflare D1 (SQLite at the edge), Drizzle ORM
- **Auth**: Better Auth + Google OAuth
- **AI**: OpenRouter routed through Vercel AI SDK; Gemini 2.5 Flash Lite for high-volume work, Claude Haiku for tool-calling
- **Browser automation**: Browserbase + Stagehand for auto-apply, Cloudflare Browser Rendering for scraping
- **Email**: React Email + Resend
- **Payments**: Stripe
- **Analytics + replay + flags**: PostHog
- **MCP**: public Cloudflare Worker at mcp.aiapplyd.com

No SaaS bills above the Cloudflare base. The whole platform runs on the free tiers of every dependency I could keep on a free tier.

---

## How it compares

Five competitors, real review data, no pricing-only swipes. Sources: Trustpilot, Reddit, BBB, Chrome Web Store. Full breakdown in [docs/launch/competitor-honest-audit.md](https://github.com/aiapplyd/aiapplyd/blob/main/docs/launch/competitor-honest-audit.md).

|  | AI Applyd | LazyApply | Simplify | Teal | JobCopilot | Huntr |
|--|-----------|-----------|----------|------|------------|-------|
| **Price (monthly)** | $39 | $99-249 lifetime upfront | $39.99 | $29 | $29.90-39.90 | $40 |
| **Real auto-apply submission** | Yes (5 direct-API + browser agent) | Indeed plugin fails captcha. Only applies to job 1 per page | Autofill only, you click apply | None | Yes, but surfaces fraudulent postings | Autofill only |
| **ATS score before applying** | Yes | No | No | Yes | No | Yes |
| **Resume hallucination guards** | Yes (anti-fabrication) | Spams same resume | Heavy editing needed on senior roles | Misspells last names, hallucinates skills | Reddit-flagged fabrications | Misses industry keywords |
| **Open-ended screener answers** | Yes, in your voice | Drops or fakes | No | No | Hit or miss | No |
| **Proof of submission** | Screenshot + ATS receipt | No | N/A | N/A | No | N/A |
| **LinkedIn ban risk** | Low (residential proxies, no script injection) | High (raw browser inside your session) | Low (autofill) | None | Lower (cloud agent) | None (autofill) |
| **MCP / Claude Desktop** | Yes | No | No | No | No | No |
| **Refund / billing trust** | Case-by-case, real human reviews | Trustpilot 2.4, refunds nearly impossible | No documented refund path | 7-day trial, denies refunds after day 2 | Duplicate billing complaints | Silent auto-renewal complaints |

---

## FAQ

**Will I get banned from LinkedIn?**
Unlikely. The tools that get accounts banned inject scripts into the LinkedIn page from a Chrome extension. AI Applyd does not do that. When a job lives on the company's own ATS we route through Greenhouse, Lever, Ashby, SmartRecruiters, or join.com directly. LinkedIn Easy Apply runs through the AI browser agent on Browserbase with residential proxies and human pacing.

**Is there really a free tier?**
Yes. Free Job Seeker plan, no credit card. 20K tokens at signup is enough for one full auto-apply end-to-end so you can see what it actually does before paying. 10 ATS scores, 5 resume tailors, 5 cover letters per month on top.

**Does it apply to jobs without me knowing?**
Only if you flip Autopilot on. Default mode is manual approval, you preview each application before it sends. About 60% of users stay on manual for the first week then turn it off.

**Where do you store my resume?**
Cloudflare D1, encrypted in transit (TLS / HTTPS) and at rest using Cloudflare D1's default at-rest encryption. We do not sell your data. Your resumes, jobs, and application content are never used to train AI Applyd's own models. One-click delete from your dashboard wipes everything.

**Why monthly only, no annual?**
Because the worst version of this product would be one that gets you locked in for a year and then doesn't have to keep delivering. If we stop being useful you should be able to leave next month.

**Can I use it without giving you my LinkedIn password?**
Yes. We never ask for LinkedIn credentials. When you connect LinkedIn for Easy Apply support, OAuth handles auth and we never see your password.

**Do I need a Chrome extension?**
No. The web app does everything. Submissions run on our infrastructure (Browserbase + direct-API), not in your browser tab, so the application looks like a careful human applicant and your laptop doesn't need to stay open all night.

[**See all 30+ FAQs**](./FAQ.md)

---

## Links

- Live product: [aiapplyd.com](https://aiapplyd.com)
- Pricing: [aiapplyd.com/pricing](https://aiapplyd.com/pricing)
- Submission wall: [aiapplyd.com/proof](https://aiapplyd.com/proof)
- MCP server: [mcp.aiapplyd.com](https://mcp.aiapplyd.com)
- Blog: [aiapplyd.com/blog](https://aiapplyd.com/blog)
- Twitter / X: [@aiapplydHQ](https://x.com/aiapplydHQ)
- LinkedIn: [linkedin.com/company/aiapplyd](https://www.linkedin.com/company/aiapplyd/)
- Discord: [discord.gg/9PZ2V8DNM5](https://discord.gg/9PZ2V8DNM5)
- Founder: [@firstexhotic](https://x.com/firstexhotic)

---

<sub>Built solo by Ava Bagherzadeh on Cloudflare. 2026.</sub>
