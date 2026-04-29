# Assets

Placeholder list of images referenced from the README and other docs. Replace each one with the real file before launching the public repo.

| Filename | What it should show | Recommended size |
|----------|---------------------|------------------|
| `hero.png` | Logo + tagline header. Pull from aiapplyd.com/og-image.png or render a centered AI Applyd wordmark on warm-orange-to-near-black gradient with the "AI that applies for you" tagline. | 1200 x 630 (OG-card friendly) |
| `dashboard.gif` | 8-12 second loop of the main app dashboard. Show the application list, status flow (saved -> applied -> interviewing), and a tailored resume preview. | 1280 x 720, < 8 MB |
| `ats-score.png` | A single ATS-score result page. Show the 0-100 dial, section breakdown, and missing-keyword list. | 1200 x 800 |
| `swipe-to-apply.gif` | The mobile / Chrome-extension flow. Card swipe or click-to-apply, then the auto-apply confirmation toast. | 720 x 1280 (mobile-portrait), < 6 MB |
| `integrations.png` | Logo grid of the supported ATS platforms: Greenhouse, Lever, Ashby, Workday, iCIMS, Personio, SmartRecruiters, Workable, Recruitee, Teamtailor, Pinpoint, Comeet, JazzHR, Breezy, plus LinkedIn, Indeed, Google Jobs. | 1200 x 600 |
| `mcp-claude.png` | Screenshot of Claude Desktop calling an AI Applyd MCP tool (e.g. score_resume) with the structured response visible. | 1200 x 800 |

## How to capture

- Browser screenshots: Cmd+Shift+5 on macOS, lock to a 16:9 region for hero / dashboard shots
- GIFs: use [Kap](https://getkap.co) or [Cleanshot X](https://cleanshot.com), keep under 8 MB so GitHub renders them inline
- ATS-score and dashboard screens: use a fake test account so no real user data leaks
- The MCP screenshot must use a free / unauthed tool (`score_resume` or `analyze_job_description`) so it works without leaking session state

## Anonymization rules

- No real user names, emails, or company names in any screenshot
- No real job URLs (use redacted or fake URLs)
- No Browserbase session IDs that map to real submits
- The submission wall data on aiapplyd.com/proof is already anonymised, screenshots from there are safe
