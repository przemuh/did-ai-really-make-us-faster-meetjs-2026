# Did AI Really Make Us Faster? — Talk Notes

This is a companion document to the slides from the Meet.js Summit 2026 presentation by Przemysław Suchodolski. It summarises the story arc of each chapter, with key lines from the talk.

---

## Intro

Przemysław opens by introducing himself — a Senior Software Engineer at Apollo.io with 14 years in the industry, almost exactly as old as the meet.js community itself. He acknowledges the wave of AI hype that swept through tech, and shares a tweet he posted in April 2023:

> "ChatGPT this, ChatGPT that... please wake me up when all the dust has settled 😴"

He thought he could wait it out. He was wrong.

> "The dust did not settle. It turned into a sandstorm."

AI didn't stay a novelty — it evolved fast. Tab autocomplete became co-pilot mode, which became fully autonomous agents. The question stopped being *if* you use AI and became *how well*. Twitter promised 10x. Some said 100x. Apollo.io decided not to guess.

> "When we don't know — we measure."

The experiment was real: 250+ engineers, a 10-year-old Ruby on Rails and TypeScript monolith, real production traffic.

---

## Chapter 01 — Excitement

In Q1 2025, Apollo.io rolled out AI tooling across the entire engineering organisation — Cursor for coding, Windsurf as an alternative, CodeRabbit for automated code review. Adoption was voluntary; engineers could pick up the tools at their own pace.

The results were striking. By the end of the quarter, 85% of engineers were weekly active users — 106% of the adoption goal — and over 90% were active monthly. PR velocity went up. Energy in the team was high. Engineers loved it.

> "Everything *felt* faster."

The operative word, it turned out, was *felt*.

---

## Chapter 02 — Reality

When the team looked more carefully at the data, a gap appeared. *Perceived velocity* — how fast engineers felt they were working — had clearly increased. But *system velocity* — PRs merged, features deployed, cycle time — had stayed flat.

> "That gap should make you nervous."

Breaking results down by discipline made the picture sharper. Frontend power users went from ~5 PRs per month to 16–20 — a genuine 3–4x lift. Backend engineers saw no consistent correlation in quantitative metrics at all. Interestingly, backend engineers *self-reported* higher perceived benefits — but the Git data told a different story.

The reason turned out to be context. Frontend had multiple comprehensive `.cursorrules` files, well-documented component libraries, standardised patterns, and engineers who had mastered domain-specific prompting. Backend had sparse documentation, less standardised conventions, and infrastructure complexity that wasn't well-captured in any context file. AI amplifies what's already there.

Speed came with a cost.

> "Speed without boundaries creates DEBT."

The debt was concrete: auto-generated tests that passed CI but validated incorrect behaviour; coverage metrics that went up while test quality went down; AI-generated code merged without adequate review. A survey confirmed the tension — 78% of engineers said Cursor made them faster, but 60% didn't trust the tests it wrote, and 46% were neutral or negative on overall code quality.

> "Speed without trust is a liability."

---

## Chapter 03 — Optimization

Apollo.io's response was to measure more precisely. They built a custom weighted effectiveness score:

```
USAGE_SCORE = (AGENT_REQUESTS × 2.0)
            + (APPLIED_SUGGESTIONS × 1.0)
            + (TABS_SHOWN × 0.05)
```

Agent requests carry the most weight — that's intentional, autonomous AI work. Applied suggestions show deliberate acceptance. Tab completions are the weakest signal. The score was smoothed with `ln(1 + n)` to reduce outlier distortion.

Running this across the org revealed the real distribution: only 2% were Super Power Users, 8% were Power Users, and the remaining 90% had *adoption* — but not depth.

To bridge that gap, the team built a custom analytics pipeline in Snowflake, joining three data sources: raw Cursor usage logs, GitHub data (cycle time, revert rates), and quarterly engineer surveys. The goal was triangulation — checking whether engineers who *felt* faster were actually *shipping* faster. Spoiler: it wasn't always the same people.

Culturally, the team identified engineers already using AI well and empowered them as squad champions — one per squad, responsible for sharing what works and raising the floor for everyone around them. Crucially, this was not a top-down mandate.

> "Voluntary champions with manager support consistently outperformed mandated rollouts."

Engineers trust other engineers. Not policy documents.

The framing shifted entirely:

> "We no longer optimize for *more* AI. We optimize for *better* AI."

---

## Results

The team's journey mapped almost perfectly onto the Gartner Hype Cycle — a peak of inflated expectations (85% adoption, everything feels amazing), a trough of disillusionment (flat cycle time, quality concerns), and a slope of enlightenment (guardrails, champions, measurement). The final result, measured across 250+ engineers and the full organisation including ~40 new hires:

> **1.15x**

Not 10x. Not 100x. One point one five. And they're proud of it.

Five things they'd tell their past selves:

1. **Invest in context infrastructure first.** Rules, docs, examples — before you think about measurement.
2. **Build custom analytics from day one.** WAU doesn't tell you *how* people use AI.
3. **Start quality measurement earlier.** The debt shows up in surveys before it shows up in metrics.
4. **Set realistic expectations with leadership.** 1.15x is real. 10x is not.
5. **Survey more frequently.** Engineer sentiment shifts faster than cycle time does.

> "1.15x is not the point. Measuring it is."
