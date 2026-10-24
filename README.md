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

The demo is a single static `index.html` with plain HTML, CSS and JavaScript, so there is no framework or build step. A small script engine plays timed dialogue word by word. It drives the captions, transcripts, speaking indicators and the notes that appear as the call goes on. Each scene has a call clock that starts at its point in the 30-minute timeline.

- **Avatars**: the AI avatar is built from the Halftime Report mark, and its two halves pulse while it speaks. The VC's memoji-style avatar and the founder's portrait are hand-drawn SVGs, animated with CSS to breathe, blink and talk. **Use camera** swaps in a live webcam feed.
- **Charts**: the post-meeting charts are hand-built inline SVG.
- **Brand**: colors and logos come from the Halftime Report brand kit.
- **Demo data**: the story uses Adonis, an AI revenue-recovery company for healthcare providers, as the portfolio company. All people and financials are fictional.

## Challenges we ran into

- **Making a scripted demo feel live.** Typing speed, pauses before hard answers and notes that arrive just after the founder says something all had to be timed so the scenes play naturally on camera.
- **The handoff.** The moment the AI leaves and the human arrives is the core of the product. It had to be clear to the founder that the partner is now there, and also that notes keep running.
- **Sensitive signals.** Founder wellbeing is the most valuable thing a partner can learn and the easiest to get wrong. We kept it private to the partner, phrased it around support ("intro two fractional engineering leads"), and avoided anything that reads as surveillance.
- **Realistic avatars without stock assets.** We drew the founder and VC as layered, animated SVGs instead of relying on third-party images.

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
