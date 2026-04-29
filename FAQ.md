# FAQ

Short answers. No fluff.

If something here is wrong, [open an issue](./CONTRIBUTING.md) or email hello@aiapplyd.com.

---

## Pricing

### Is there a free tier?
Yes. Free Job Seeker plan, no credit card. 20K signup tokens funds one full auto-apply end-to-end, plus 10 ATS scores per month and 5 resume tailors per month.

### What does Hired in 30 actually cost per application?
$39 / 100 applies = $0.39 per submission at full utilization.

### What does Hired Yesterday cost per application?
$79 / 300 applies = $0.26 per submission at full utilization.

### Why monthly only, no annual plans?
Because the worst version of this product would be one that locks you in for a year and stops delivering. If we stop being useful you should be able to leave next month.

### How does this compare to LazyApply pricing?
LazyApply is $99-999 / year prepaid. AI Applyd is $39-79 / month, no annual. Cheaper if you find a job in 1-2 months. More flexible either way.

### Are there hidden fees or token packs?
No. The plan token allowance covers all features. No surprise overages, no pay-as-you-go top-ups required to get the basics.

---

## Capabilities

### Which job platforms does AI Applyd actually support?
Direct-API submitters: Greenhouse, Lever, Ashby, SmartRecruiters, join.com. AI browser agent fallback (with Stagehand fastpath selectors): Workday, iCIMS recognition, Ashby microsites, SmartRecruiters, Workable, Breezy. LinkedIn Easy Apply and Indeed go through the AI browser agent fallback. Job board search covers LinkedIn, Indeed, Google Jobs. Everything else hits the generic LLM-driven browser agent.

### Does it work on Workday?
Yes. Workday is the ugliest of them and gets the most engineering attention. There's a fast-path that handles known Workday selectors without burning LLM tokens, and a slow-path agent that takes over when the form is custom.

### Will it answer screener questions?
Yes. Salary expectations, work authorization, years of experience, etc. It answers based on your application profile. You set the answers once, they get reused. Edge cases (free-text "tell us about yourself" 200-word boxes) get a generated answer based on your resume + the job description.

### Can it write cover letters?
Yes. Tailored per job, references the actual posting. Free plan: 5 / month. Paid plans: unlimited (token-gated).

### Does it do interview prep?
Yes. Company-specific questions pulled from real interview reports plus generated practice answers grounded in your resume. Token-gated on free, unlimited on paid.

### Does it generate resumes from scratch?
Yes. Resume builder with ATS-aware formatting, templates, PDF + DOCX export. Plus a chat-based editor that can rewrite individual bullets.

### How fast is the ATS score?
Under 30 seconds for the keyword-based score, 1-2 minutes for the AI-powered version with section feedback.

---

## Safety

### Will I get banned from LinkedIn?
Unlikely. Tools that get accounts banned inject scripts into LinkedIn's page from a Chrome extension. AI Applyd does not do that. When a job lives on the company's own ATS we route through Greenhouse, Workday, Lever, etc. directly. LinkedIn Easy Apply runs through the AI browser agent on Browserbase with residential proxies and human pacing.

### What about Indeed?
Same approach. We use Indeed's job search to find roles. When the original posting lives on the company's ATS we submit there. When it's an Indeed-only Easy Apply we route through the AI browser agent fallback.

### Does it look like a bot to recruiters?
The application output looks like a careful applicant: tailored resume, specific cover letter, sane screener answers. It doesn't look like a bot because it isn't sending bot-quality content. Recruiters can't tell from the application alone.

### What if it submits the wrong answer to a screener?
You set the answers in your application profile once. The system reuses them. For free-text questions you can require manual approval before send (default mode). About 60% of users stay on manual for the first week.

### Has anyone been banned using this?
No reports as of April 2026. Browserbase uses residential proxies and stealth fingerprints. We rate-limit per-user to mimic human pacing (max 50 / day on the top tier, 15 / day on Hired in 30).

### Can I review applications before they go out?
Yes. Default mode is manual approval. Each application shows you the rendered resume, cover letter, and screener answers before you click Send.

### What happens if a captcha or 2FA appears mid-apply?
Captchas auto-solve via Browserbase. Two-factor codes pause the run and email a session link to complete. Persistent bot-detection skips rather than burns a session.

---

## Comparisons

### How is this different from LazyApply?
LazyApply injects scripts inside LinkedIn from a Chrome extension (high ban risk), $99-999 / year prepaid, 2.3 / 5 Trustpilot, and is mass-apply by design. AI Applyd routes through the company's ATS when possible and uses Browserbase with residential proxies for LinkedIn Easy Apply (low ban risk), monthly billing, ATS scoring before each apply, resume tailoring per job.

### How is this different from Simplify Copilot?
Simplify is $80 / month and is a Chrome extension that fills forms as you click apply. AI Applyd is $39 / month, runs in the cloud, and can apply autonomously while you sleep. Simplify doesn't include ATS scoring or interview prep.

### How is this different from Teal?
Teal is a tracker + resume builder, not an auto-apply tool. AI Applyd does both, plus actually applies on your behalf.

### How is this different from JobCopilot?
JobCopilot is in the same price range and category. AI Applyd has the public submission wall (proof screenshot on every browser-driven submission, structured submission receipt on direct-API platforms), the public MCP server, and a more aggressive token-gated free tier.

### Is Sonara still around?
No. Sonara closed to new signups in Q1 2026. Existing accounts partially work.

### Are there any cases where I should use a different tool?
Yes. If you only want to track manual applications without auto-apply, Teal or Huntr are simpler. If you want a recruiter network, LinkedIn Premium. If you want a free Chrome extension that fills forms but does nothing else, Simplify's free tier works.

---

## Privacy

### Where is my data stored?
Cloudflare D1 (SQLite at the edge). Encrypted in transit (TLS/HTTPS) and at rest via Cloudflare D1's default at-rest encryption. Access controls and regular reviews. No data sold.

### Is my resume used to train AI?
We do not sell your data. Your resumes, jobs, and application content are never used to train AI Applyd's own models. Most "free" job tools bury this in paragraph 14 of their ToS. We don't do that. Your data powers your job search and nothing else.

### Can I delete my data?
Yes. One click in your dashboard. No 30-day stalling, no "are you sure?" guilt trips. Your data vanishes.

### Are you GDPR compliant?
Yes. EU users get full GDPR rights including deletion, portability, and rectification.

### Do you sell user data?
No. Revenue is 100% from subscriptions. Verifiable by anyone reading our Stripe dashboard.

### Who can see my applications?
Only you. The submission wall at [/proof](https://aiapplyd.com/proof) shows anonymous receipts (ATS platform, role family, timestamp) but never your user ID, the company name, the job URL, or any PII. Anonymisation is enforced at the SQL projection level.

---

## MCP

### What is the MCP server for?
Connect Claude Desktop, Claude Code, or Cursor to AI Applyd's tools directly. You can score resumes, generate cover letters, search jobs, and trigger auto-apply from inside your AI assistant.

### How do I install it?
Add to your `claude_desktop_config.json`:
```json
{
  "mcpServers": {
    "ai-applyd": {
      "command": "npx",
      "args": ["mcp-remote", "https://mcp.aiapplyd.com/sse"]
    }
  }
}
```

### Which tools work without auth?
`score_resume` (keyword-based ATS score) and `analyze_job_description` (skills + red flags + seniority). 5 / day and 10 / day rate limits respectively.

### Which tools need an account?
AI-powered scoring, resume optimization, interview question generation, resume translation, job search. Sign in with Google when first invoked.

### Which tools cost tokens?
auto_apply, generate_cover_letter, build_resume_pdf. Drawn from your plan's token balance.

---

## Mobile

### Is there a mobile app?
Runs in any mobile browser as a responsive web app. No native iOS/Android app yet.

### Can I auto-apply from my phone?
Yes. You can trigger applies and review status from your phone. The actual browser submission runs in the cloud (Browserbase), not on your device.

### Will there be a native app?
Maybe. The responsive web app covers the queue + approve UX today. A native shell is on the roadmap but not committed.

---

## Roadmap

### What are you building next?
Personalized interview prep grounded in real Glassdoor + Reddit threads about each company. Then salary negotiation copilot for the moment after you get an offer. See [ROADMAP.md](./ROADMAP.md).

### Will there be a referral program?
Already exists. Refer a friend, both get bonus tokens.

### Are you raising funding?
No plans. Profitable from day 1, want to keep optionality.

### What about non-English resumes?
Resume translation is shipped (auth tool in MCP). Localized job search for non-US markets is in the next quarter.
