# Halftime Report: demo UI

A clickable, scripted web UI for the product demo video. AI runs the first half of a portfolio check-in, and the partner takes over for the second half.

## Inspiration

Pre-seed funds write many small checks and end up with huge portfolios. One fund we talked to has 3 people covering about 300 companies across 30 countries, roughly 90 companies per person. At that ratio, check-ins get skipped, red flags surface late, quiet companies get overlooked, and founders arrive at their follow-on raise without audit-ready numbers.

Interviews shaped the product. Founders told us they want real time with their investor, not a bot. Investors told us that faces and live conversation tell them more than written updates. Funds told us their portfolio companies need clean financials to raise from institutional investors. So we split every check-in like a game: AI plays the first half, and the human partner plays the second.

## What it does

Halftime Report runs a 30-minute portfolio check-in that one partner can repeat across a whole portfolio.

1. **Before the call**, it pulls the company's books (accounting, billing and payroll), standardizes the KPIs and keeps them audit-ready.
2. **Minutes 0 to 15**: an AI persona of the covering partner runs the call. It asks questions tailored to the latest financials, past check-ins and earlier commitments, such as why September growth slowed or where the engineering hiring plan stands.
3. **Minute 10**: the partner opens a side-by-side view. On the left is the live call, and on the right are live notes with a running summary, KPIs reconciled to the books, timestamped key points and flags. One flag: payroll shows a senior engineer left, but the founder hasn't mentioned it.
4. **Minute 15**: the partner joins and the AI avatar leaves the call. From the founder's side, it becomes a normal conversation with their investor, while notes keep running in the background.
5. **Minute 25**: a private copilot shows the partner live cues (talk time, sentiment, eye contact) and questions that haven't come up yet. For example: "A senior engineer left on Oct 3. Who's covering the EHR integrations?"
6. **After the call**, it produces a report with financial health, red flags, commitments compared with last time, founder wellbeing signals (visible only to the partner) and action items for both sides.

The founder gets something back for showing up: audit-ready books and an investor who acts on what they need.

## How we built it

**The demo:** a single static web page in plain HTML, CSS and JavaScript, hosted on GitHub Pages. A small script engine plays timed dialogue, captions and live notes. Jordan AI is an animated, memoji-style avatar, the founder and partner are real video clips, and the example portfolio company is Adonis, with illustrative data.

**The real thing:**

- **[Nicrron](https://nicrronninoo.ai/): the AI brain.** The Nicrron API puts every major model behind one endpoint, with routing, fallbacks and caching. It would power the AI interviewer, the live notes, the missed-question nudges and the post-meeting report, and let us use the best model for each job without being locked in to one provider. Once NiNI Cloud opens, its serverless inference could host the agent pipeline.
- **[akta.pro](https://akta.pro/): outside signals.** Its company data and news API covers 20M+ companies, with signals for hiring, launches, partnerships and expansion. It would add what's happening around each portfolio company to the founder's own books, so the AI can ask about news the founder didn't mention, and the fund gets portfolio-wide red-flag alerts.
- **[Roundtable](https://roundtable.bot/): shared memory.** It gives the AI interviewer, note-taker, copilot and report writer one shared workspace for briefs, transcripts and notes on each founder. That memory carries over between quarters, so the AI follows up on last quarter's commitments, and the partner's own assistant can read the briefing before the call.
- **[PRISM by Block Convey](https://blockconvey.com/prism): trust and quality.** Before launch, PRISM would test the AI interviewer against simulated founders, including evasive answers and missing data. After launch, it would trace every call and score whether the AI asked the right questions and caught the red flags. That's how we'd show partners the AI is reliable enough to run the first half of a check-in.

## Challenges we ran into

- **Making a scripted demo feel live.** Typing speed, pauses before hard answers and notes that arrive just after the founder says something all had to be timed so the scenes play naturally on camera.
- **The handoff.** The moment the AI leaves and the human arrives is the core of the product. It had to be clear to the founder that the partner is now there, and also that notes keep running.
- **Sensitive signals.** Founder wellbeing is the most valuable thing a partner can learn and the easiest to get wrong. We kept it private to the partner, phrased it around support ("intro two fractional engineering leads"), and avoided anything that reads as surveillance.
- **Real video in a browser call.** iPhone clips are HEVC, which Chrome doesn't reliably play, so we converted them to H.264 and cropped portrait footage to fit landscape call tiles.

## Accomplishments that we're proud of

- The full story runs end to end in about five minutes of screen time, from the AI opening the call to the post-meeting report.
- The "missed question" moment works. The AI notices a mismatch between the founder saying "engineering is fine" and a payroll record. It hands the partner the exact question to ask, and the answer changes the partner's view of the company.
- Each view is tailored to its viewer. The founder sees a friendly call, and the partner sees a briefing and a coaching tool.
- It has no dependencies. One HTML file runs anywhere and can be shared over a local network for live demos.

## What we learned

- Founders accept AI in a check-in when it saves them time and a human clearly owns the relationship. The handoff matters more than the AI.
- The most valuable insight usually isn't in the numbers the founder volunteers. It comes from comparing what they say with what the books show.
- Partners don't want more dashboards. They want a short list of things to ask right now.

## What's next for Halftime Report

- Replace the scripted data with real integrations: QuickBooks, Stripe, Gusto and other accounting, billing and payroll systems.
- Add a live, real-time video avatar of the partner that can read facial cues, and real-time nudges from tone and visual cues.
- Use call memory across quarters, so the AI follows up on last quarter's commitments automatically.
- Add portfolio-level red-flag alerts and LP-ready reporting across all companies.
- Pilot with a pre-seed fund. Planned pricing is about $50 per portfolio company per month, which comes to about $180K a year for a 300-company fund.

## Run

```bash
python3 -m http.server 4173
```

Open http://localhost:4173. It's a single static page (`index.html`) with no build step.

## Scenes

| Key | Call time | View | What happens |
| --- | --- | --- | --- |
| `#1` | 0:00 | Founder | The partner's AI avatar asks about Q3 financials and hiring |
| `#2` | 10:00 | Partner | Side by side: the live call on the left, live notes and flags on the right. **Join call** goes to scene 3 |
| `#3` | 15:00 | Founder | The partner joins and the AI leaves. Notes keep running in the background |
| `#4` | 25:00 | Partner | Copilot suggests missed questions. Click **Ask now** on the senior-engineer card |
| `#5` | Post | Partner | Post-meeting analytics covering financial health and founder wellbeing |

## Presenter controls

- `←` / `→` switch scenes, `R` replays the current scene, `H` hides the top bar for clean recording.
- `?speed=1.5` speeds up the scripted dialogue.
- **Use camera** on a self-view tile shows your real webcam in place of the placeholder.

All companies, people and numbers in the demo are fictional.
