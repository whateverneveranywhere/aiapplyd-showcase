# Examples

Real-world output, anonymised. All names, emails, and companies are fake. Numbers and structure are real.

---

## 1. Before / After ATS score

Same resume, same job description (Senior Backend Engineer at a Series-B fintech). Score from AI Applyd's `/tools/ats-score`.

### Before

```
Overall ATS Score: 47 / 100

Section breakdown:
  Skills              42 / 100   missing: Go, gRPC, PostgreSQL, Kafka, AWS
  Experience          61 / 100   no quantified impact in 8 of 11 bullets
  Education           80 / 100
  Format              90 / 100   minor: 2-column layout flagged

Top 10 missing keywords (high signal):
  Go, gRPC, PostgreSQL, Kafka, AWS, microservices, Kubernetes,
  observability, SLO, distributed systems

Recommendation: 23 keyword gaps. Add Go, gRPC, Kafka explicitly.
Quantify outcomes (revenue, latency, scale numbers).
```

### After (one tailoring pass)

```
Overall ATS Score: 86 / 100

Section breakdown:
  Skills              92 / 100   added: Go, gRPC, PostgreSQL, Kafka, AWS, Kubernetes
  Experience          84 / 100   8 of 11 bullets now have impact metrics
  Education           80 / 100
  Format              95 / 100   reformatted to single column

Top 10 missing keywords (low signal): -

Recommendation: Strong match. Apply.
```

47 -> 86 in 90 seconds. The system added keywords that were genuinely in the user's experience but not surfaced in the original resume.

---

## 2. Tailored resume diff

Same person, same base resume, two job descriptions. The system generates two different resumes that emphasize different parts of the same career.

### Job A: Platform Engineer at a logistics startup

```diff
- Senior Software Engineer | Acme Corp | 2022 - Present
- - Built and maintained backend services
- - Collaborated with cross-functional teams
- - Owned production deployments

+ Senior Software Engineer | Acme Corp | 2022 - Present
+ - Migrated 12-service Python monolith to Go microservices on Kubernetes,
+   cut p99 latency from 840ms to 180ms
+ - Built distributed job scheduler handling 4M tasks/day on PostgreSQL + Redis
+ - Owned on-call rotation across 3 regions, set SLOs at 99.95% availability
```

### Job B: Backend Engineer at a finance company

```diff
- Senior Software Engineer | Acme Corp | 2022 - Present
- - Built and maintained backend services
- - Collaborated with cross-functional teams
- - Owned production deployments

+ Senior Software Engineer | Acme Corp | 2022 - Present
+ - Designed payment ledger handling $2.4M / month across 6 currencies
+ - Implemented PCI-compliant tokenization service in Go + Postgres,
+   passed SOC 2 Type II audit on first attempt
+ - Built fraud detection pipeline (Kafka -> Flink) that reduced
+   chargebacks by 34% in first 90 days
```

Both are true. The career has both threads. The tailoring picks the right thread for the right job.

---

## 3. Auto-apply success log (anonymised)

Excerpt from the dashboard for one user, one day on Hired in 30.

```
2026-04-29 03:14:22 UTC  -  apply queued  -  job_id job_8f3a... ATS: greenhouse
2026-04-29 03:14:31 UTC  -  resume tailored, ATS score 78 -> 91, sent
2026-04-29 03:14:45 UTC  -  cover letter generated (412 tokens), sent
2026-04-29 03:14:52 UTC  -  browserbase session created, purpose: apply
2026-04-29 03:15:03 UTC  -  greenhouse fastpath matched, skipped LLM step
2026-04-29 03:15:14 UTC  -  fields filled: name, email, phone, linkedin, resume_pdf
2026-04-29 03:15:22 UTC  -  screener q: "Are you authorized to work in the US?"
                            answered: yes (from profile)
2026-04-29 03:15:24 UTC  -  screener q: "Salary expectation?"
                            answered: $145,000 - $165,000 (from profile)
2026-04-29 03:15:31 UTC  -  submit clicked, awaiting confirmation
2026-04-29 03:15:38 UTC  -  confirmation verified (3 of 3 signals matched)
2026-04-29 03:15:39 UTC  -  screenshot saved, cost $0.42, status: applied

Daily total: 14 applies, $5.88, 0 failures, 0 stuck-loops, 0 captcha events
```

The cost-per-apply hit $0.42 here vs the $0.39 budgeted average because this Greenhouse posting had 4 screener questions that needed LLM calls. Other applies in the same batch came in under $0.30.

---

## 4. Generated interview questions: "Senior Backend Engineer"

Output from `generate_interview_questions` for a hypothetical Senior Backend role. Pulled from real interview reports for similar companies, then deduped.

### Behavioral

1. Tell me about a time you had to debug a production incident at 3am. What was the postmortem outcome?
2. Describe a technical decision you made that you later regretted. What did you change?
3. Walk me through how you'd onboard a new engineer to a service you own.

### System design

4. Design a rate limiter that works across a fleet of 200 stateless API servers. Walk through tradeoffs.
5. We have a billing system that needs to charge customers monthly. How would you build it to be idempotent under retries?
6. Sketch a system that ingests 100M events / day and serves real-time aggregations to a dashboard with p99 < 200ms.

### Coding (sample)

7. Given a stream of stock trades, write a function that returns the moving average over the last N seconds. Discuss memory tradeoffs.
8. Given two large files of email addresses, find emails that appear in both without loading either entirely into memory.

### Company-specific (anonymised, derived from real Glassdoor threads)

9. Our team is migrating from monolith to services. What's your take on when to split a service vs keep it together?
10. We pair-program 3 days a week. How do you feel about that, and what's worked or not worked for you in past pair setups?

For each question the system also generates:
- A 60-second answer grounded in the user's resume
- The 2-3 follow-up questions the interviewer is most likely to ask
- Common pitfalls (what makes a weak answer)

---

## 5. MCP server: Claude Desktop usage

Once you've added AI Applyd to your `claude_desktop_config.json`:

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

You can ask Claude things like:

> Score this resume against this job description: [paste resume] [paste JD]

Claude calls `score_resume`, gets the score and missing keywords, summarises them.

> Analyse this job posting and tell me if the seniority level matches what I'm looking for: https://...

Claude calls `analyze_job_description`, returns required skills, red flags, seniority level.

> Generate 5 interview questions for a Senior Backend Engineer role at a fintech startup, plus suggested answers based on my saved profile.

Claude calls `generate_interview_questions`, returns the structured output. (Requires auth.)

> Apply me to this job: https://...

Claude calls `auto_apply` and triggers a Browserbase session. (Requires paid plan with token balance.)

The free tools (`score_resume`, `analyze_job_description`) work without an account. The auth tools open a Google OAuth flow in your browser the first time.

---

## 6. 30-day applied tracker (description)

The dashboard view is a receipts list of every application from the last 30 days, with stage filters for Applied, Callback, Phone Screen, Interview, Offered, and Rejected. Stage counts at a glance:

```
applied (87)  -  callback (14)  -  phone screen (8)  -  interview (6)  -  offered (2)  -  rejected (29)
```

For each application:
- Job title, company logo, ATS platform tag
- Application date, status, days since last update
- Resume version + cover letter used
- Screenshot of the confirmation page (Browserbase)
- Recruiter response email matched back to the application (when the user grants Gmail read access)

Conversion stats at the top:
```
Last 30 days
  87 applied
  16% response rate
  7% interview rate
  2.3% offer rate
  Median days to first response: 5
```

This is what makes auto-apply different from spam: every single submission has a receipt, a screenshot, and a status that updates as the recruiter chain proceeds.
