# FAQ

Short answers. No fluff.

If something here is wrong, [open an issue](./CONTRIBUTING.md) or email hello@aiapplyd.com.

---

## Pricing

### Is there a free tier?
Yes. Free Job Seeker plan, no credit card. 20K signup tokens funds one full auto-apply end-to-end, plus 10 ATS scores per month, 5 resume tailors per month, and 5 cover letters per month.

### Can I cancel anytime?
Yes. One click. Access stays until the end of the month you already paid for. No retention call, no exit survey, no "are you sure" five times.

### How does usage work?
Every AI action spends tokens from your monthly balance. Free gets 20K tokens on signup, enough for one full auto-apply so you can see it actually submit a job. Hired in 30 refills 7M tokens every month, Hired Yesterday refills 15M. Failed runs that aren't your fault auto-refund the tokens. You only get charged for stuff that worked.

### How many tokens does each action cost?
Approximate cost per action (depends on job description length and model):
- Full auto-apply (prepare + submit + proof screenshot when applicable): ~30,000 tokens
- ATS resume score: ~1,200 tokens
- Cover letter: ~1,200 tokens
- Tailored resume rewrite: ~1,600 tokens
- Interview prep: ~1,100 tokens

Unused tokens stay in your balance until the next refill. We bill at roughly 2x the underlying compute cost, so the margin is honest, not 50x like the prompt-wrapper "AI" tools that charge $30/mo to call the same Gemini API you could call yourself.

### What happens when I run out of tokens?
Buy a one-time top-up from the sidebar or upgrade so the next refill resets your balance. There is no pay-as-you-go meter ticking in the background. You either top up or wait for the cycle, and you always know what the next bill looks like.

### Do you offer refunds?
No blanket 30-day refund policy. If something legitimately broke on our side, email support and we'll fix it or refund the cycle. We do not have a script for hiding behind "all sales final" while the service is down. Token refunds on non-user-fault failures (page unreadable, ATS down, bot detection block) are automatic.

### Is there a contract?
No. Monthly billing, cancel from the billing portal, no annual lock-in, no discount-for-soul-trade.

### Can I switch plans?
Yes. Upgrade or downgrade anytime. Stripe prorates it. No "wait until next cycle" dance.

### What's the difference between Hired in 30 and Hired Yesterday?
Hired in 30 ($39/mo) gives you 7M tokens, ~100 AI auto-applies (15/day pace), unlimited ATS scores, tailoring, cover letters, interview prep. Hired Yesterday ($79/mo) gives you 15M tokens, ~300 applies (50/day), priority queue. Pick based on how fast last week was the deadline.

### What does AI Applyd do for free that competitors charge for?
Jobscan charges $50/month to score one resume against one job. AI Applyd does 10 of those a month for $0. Teal charges $29/month for resume building. Free tier here builds and tailors 5 resumes a month for $0. Plus you get one full auto-apply submission, free, so you can confirm we actually submit jobs before you pay anything.

### How do I know my application actually went through?
Every submission lands in your dashboard with proof. On Workday, LinkedIn, iCIMS, and similar sites you get a confirmation screenshot. On Greenhouse, Lever, Ashby, SmartRecruiters, and join.com you get a structured receipt with the ATS-side application ID. Either way, every application has a paper trail. If a run fails, you get a clear error code on the dashboard, not a "something went wrong" shrug.

---

## Capabilities

### Which job platforms does AI Applyd actually support?
Direct-API submitters (5 platforms): Greenhouse, Lever, Ashby, SmartRecruiters, join.com. AI browser agent fallback (with Stagehand fastpath selectors) handles Workday, iCIMS, Workable, Breezy, Recruitee, custom career pages, LinkedIn Easy Apply, Indeed. Job board search covers LinkedIn, Indeed, Google Jobs.

### Does it work on Workday?
Yes, via the AI browser agent. Workday is the ugliest of them and gets the most engineering attention. There's a fast-path that handles known Workday selectors without burning LLM tokens, and a slow-path agent that takes over when the form is custom.

### Will it answer screener questions?
Yes. Salary expectations, work authorization, years of experience, etc. It answers based on your application profile. You set the answers once, they get reused. Free-text "tell us about yourself" boxes get a generated answer based on your resume + the job description, written in your voice. Anti-fabrication rules block invented job titles, fake certifications, fake metrics, and skills you don't have.

### Why does AI Applyd actually answer the open-ended screener questions when other tools leave them blank?
Because most "AI apply" tools are wrappers around a form-filler. They map "name" to your name, "email" to your email, and skip the box that says "Tell us about a time you led a project." We treat that box as the actual application. Your resume, work history, and persona profile feed a system prompt with hard anti-fabrication rules. The answer comes back in your voice, grounded in your real experience, and gets reviewed by you in Copilot mode before submission. The reason Sonara, LazyApply, and AIApply skip these questions is that doing them well is the hard part. Doing them badly gets you auto-rejected.

### Can it write cover letters?
Yes. Tailored per job, references the actual posting. Free plan: 5 / month. Paid plans: unlimited (token-gated).

### Does it do interview prep?
Yes. Company-specific questions pulled from real interview reports plus generated practice answers grounded in your resume. Token-gated on free, unlimited on paid.

### Does it generate resumes from scratch?
Yes. Resume builder with ATS-aware formatting, templates, PDF + DOCX export. Plus a chat-based editor that can rewrite individual bullets.

### How fast is the ATS score?
Under 30 seconds for the keyword-based score, 1-2 minutes for the AI-powered version with section feedback.

### How accurate is the AI at filling applications?
It only writes from data already in your profile. The system prompt has explicit anti-fabrication rules that block invented job titles, made-up certifications, fake metrics, and skills you don't have. If the application asks for a salary number you never gave us, it asks you instead of guessing. Copilot mode lets you review every answer before it submits. Autopilot mode submits without review, but the same anti-fabrication rules apply.

---

## Safety

### Will I get banned from LinkedIn?
Honest answer: any tool that touches LinkedIn carries some risk, and any tool that says "zero risk" is lying. What we do to keep that risk low: per-user pacing (15 applies/day on Hired in 30, 50/day on Hired Yesterday), residential proxy IPs, no scraping LinkedIn search results, no automated connection spam, no bulk profile views. We submit to Easy Apply forms only, the same way you would on your phone. The accounts that get banned are the ones running 200 applications a day from a datacenter IP. We do not do that and we will not let you do that.

### What about Indeed?
Same approach. We use Indeed's job search to find roles. When the original posting lives on the company's ATS we submit there. When it's an Indeed-only Easy Apply we route through the AI browser agent fallback.

### Why doesn't AI Applyd have a Chrome extension?
Because extension-based auto-apply is a bad architecture. It runs in your browser, on your machine, using your fingerprint, against ATS sites that ban accounts based on browser behavior. When LazyApply triggers a captcha loop on a Workday job, that loop is happening in your browser tab and getting fingerprinted to your LinkedIn account. We run submissions on our infrastructure with rotating residential IPs, captcha solvers, and per-platform pacing. Your browser never touches the ATS. This is also why we can answer screener questions without your laptop needing to stay open all night.

### What happens when an application hits a captcha or 2FA?
Captchas (hCaptcha, reCAPTCHA, Turnstile) auto-solve in the background via Browserbase. You don't see them, the run keeps going. Two-factor codes and unfamiliar identity checks pause the run and email you a session live-view link so you can punch in the code. Persistent bot-detection challenges (DataDome, PerimeterX) are classified as blocked, the session is killed, and your tokens get refunded. We do not waste your tokens on a fight we can't win.

### How is this different from mass-apply bots?
Mass-apply bots (LazyApply, JobCopilot in spam mode) blast the same resume at 100+ jobs a day. Your last 200 applications didn't fail because your resume is bad. They failed because the ATS rejected you in 6 seconds and a recruiter never saw the file. AI Applyd scores your resume against each job before applying. If you're below the threshold, we tell you instead of burning a token on a job you have no shot at. When we do apply, the resume is tailored to that specific JD and the screener questions are answered in your voice. This is why response rates exist at all.

### Can I review applications before they go out?
Yes. Default mode is Copilot (manual approval). Each application shows you the rendered resume, cover letter, and screener answers before you click Send.

### Will AI Applyd make up fake skills or experience?
No. Anti-fabrication rules in the system prompt block invented job titles, fake certifications, fake metrics, and skills not on your resume. If a JD demands something you don't have, the tailor flow surfaces it as a gap instead of forging it.

---

## Comparisons

### How is this different from LazyApply?
LazyApply injects scripts inside LinkedIn from a Chrome extension (high ban risk), $99-249 lifetime upfront, Trustpilot 2.4 with 56% one-star, refunds nearly impossible. AI Applyd routes through the company's ATS when possible and uses Browserbase with residential proxies for LinkedIn Easy Apply (low ban risk), monthly billing, ATS scoring before each apply, resume tailoring per job, anti-fabrication guardrails. LazyApply Indeed plugin can't pass captcha. Only applies to job 1 per page. Documented across Reddit and Trustpilot.

### How is this different from Simplify?
Simplify is autofill via Chrome extension with 1M+ installs, the free tier is genuinely loved. Simplify+ at $39.99/mo has no documented refund path, AI output needs heavy editing on senior roles, privacy policy not updated since 2021. AI Applyd is real submission (Simplify is autofill, you still click apply), $39/mo with anti-fabrication guardrails, MCP integration, screenshot or receipt on every send.

### How is this different from Teal?
Teal is a tracker + resume builder, no auto-apply at all. Best-in-class kanban tracker, generous free tier. Cover letters frequently misspell user last names, Reddit reports skills hallucinated from JDs, 7-day trial requires cancelling within 2 days or refunds get denied. AI Applyd does both tracking and real auto-apply, plus per-job tailoring without the hallucination issues.

### How is this different from JobCopilot?
JobCopilot is in the same price range. Trustpilot 4.2 with heavy invited-review skew. Reviewers report being connected to fraudulent postings (5 scam attempts in 45 applications per one Trustpilot reviewer). Duplicate billing complaints. Cancellation friction. AI Applyd has the public submission wall (proof on every browser-driven send + structured receipt on direct-API), the public MCP server, off-origin navigation guard that prevents scam-redirect submissions, and aggregator preflight that skips bad URLs before a session is paid for.

### How is this different from Huntr?
Huntr is autofill-only Chrome extension with a polished tracker. Trustpilot 4.0 with recurring auto-renewal complaints, AI keyword extraction misses industry-specific terms. AI Applyd is real submission, MCP integration, cheaper at $39/mo vs $40/mo Pro tier, no silent auto-renewal complaints in our refund history.

### Is Sonara still around?
No. Sonara closed to new signups in February 2024. Existing accounts partially work. Anyone listing Sonara on a 2026 best-of list is reading old content.

### Are there any cases where I should use a different tool?
Yes. If you only want to track manual applications without auto-apply, Teal or Huntr are simpler. If you want a recruiter network, LinkedIn Premium. If you want a free Chrome extension that fills forms but does nothing else, Simplify's free tier works. If you want a draggable kanban board specifically, pair AI Applyd with Huntr.

### How does AI Applyd compare to LazyApply, Simplify, Teal, JobCopilot, LoopCV, AIApply, JobRight, and Massive?
AI Applyd is the only platform that ships real auto-apply, ATS scoring, anti-fabrication tailoring, open-ended screener answers, and an MCP server in one place. LazyApply, AIApply, and LoopCV are volume bots with high LinkedIn ban risk or hidden paywalls. Simplify is autofill that breaks on Workday. Teal and Huntr skip auto-apply. JobCopilot leaves open-ended fields blank and surfaces fraudulent postings. JobRight has documented Reddit hallucinations. Massive invents credentials by copying words from JDs.

---

## Privacy

### Where is my data stored?
Cloudflare D1 (SQLite at the edge). Encrypted in transit (TLS / HTTPS) and at rest using Cloudflare D1's default at-rest encryption. Access controls and regular reviews. No data sold.

### Is my resume used to train AI?
No. We do not run our own model and we do not feed your resume into anyone else's training pipeline. AI calls go through providers configured to opt out of training on your data. Your resume is used to apply to jobs, not to make a smarter chatbot for someone else.

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
Connect Claude Desktop, Claude Code, Cursor, or any MCP-aware AI assistant to AI Applyd's tools directly. You can score resumes, generate cover letters, search jobs, and trigger auto-apply from inside your AI client. Nobody else in the category ships this.

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
Runs in any mobile browser as a responsive web app. iOS Safari and Android Chrome both work, add to home screen and it opens like a native app. No App Store / Play Store install required.

### Can I auto-apply from my phone?
Yes. You can trigger applies and review status from your phone. The actual browser submission runs in the cloud (Browserbase), not on your device, so your phone is just the tap-to-approve queue UI.

### Why do LazyApply, Simplify, and JobCopilot not work on a phone?
They ship as desktop Chrome extensions. Mobile Safari and mobile Chrome do not load that extension model, so none of them can fire autofill on a phone. AI Applyd avoids this trap by handling submission on our own infrastructure instead of through a browser extension.

### Will recruiters know I applied from a phone?
No. The ATS records the same fields whether you submitted from a phone or a laptop. Recruiters see the resume, the screener answers, and the cover letter, not the device.

### Will there be a native app?
Maybe. The responsive web app covers the queue + approve UX today. A native shell with push notifications for "you got a callback" is on the table but not committed.

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
