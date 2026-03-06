# Report Writing

Transform a data analysis README or research document into a publication-ready blog post for casruta.github.io. The post should read like long-form data journalism — serious, precise, and engaging.

## Workflow

1. **Gather source material** — Fetch the README/report (via `gh api` or `WebFetch`), identify all plots/images, and fetch an existing post from `casruta.github.io/src/posts/` for front-matter format reference.

2. **Launch research agent (parallel with drafting)** — Spawn a dedicated research agent (Sonnet-class) that investigates the topic from a journalistic lens. This agent runs in the background while drafting agents work. See **Research Agent** section below.

3. **Parallel drafting** — Launch 2–3 background agents, each responsible for a distinct section of the post. Each agent receives:
   - The exact data tables and numbers (never fabricated)
   - The image URLs to embed
   - Tone guidance: serious but engaging, no emojis, long-form data journalism
   - Instruction to use footnotes (`[^n]`) for methodology asides (rendered as sidenotes on wide screens)

4. **Integrate research findings** — When the research agent returns, weave its findings into the draft: footnotes for context that enriches, inline references where connections strengthen the argument, and new paragraphs where the research reveals something the data alone cannot show. Do NOT add research findings that lack sourcing or that contradict the primary data.

5. **Apply Incentive Weighted Reasoning** — Before finalizing, run the narrative through the IWR framework (see below). Flag any claims that rest on low-reliability domains. Strip any "balanced" caveats that don't follow from the analysis.

6. **Combine and polish** — Merge agent outputs into a single post. Ensure consistent voice, remove redundancy, and incorporate the strongest phrasing from each draft.

7. **Write the file** — Save to `casruta.github.io/src/posts/<slug>.md` with proper front matter.

---

## Research Agent

Launch a background agent with `WebSearch` and `WebFetch` tools. The agent's job is investigative — not summarization. It thinks like a journalist: who benefits, who is connected, what is the broader context, and what adjacent stories intersect with this one.

### Research Agent Prompt Template

```
You are a research agent investigating the topic: [TOPIC SUMMARY].

Your job is investigative journalism research. Think like a reporter, not a summarizer. For every claim, policy, or trend in the source material, probe deeper:

**People and institutions:**
- Who held decision-making authority during the period? (Premier, relevant ministers, Treasury Board chair — names, titles, dates in office)
- Which institutions shaped this — and what are their incentive structures?
- Who benefits from the current arrangement? Who bears the cost?
- What public statements, budget speeches, or policy announcements did officeholders make about this topic during their tenure? (Direct quotes with dates preferred)
- NOTE: The goal is establishing the public record of who was in charge and what they said — not assigning blame or motive. Report the facts of authority.

**Connected ideas and adjacent stories:**
- What other policy areas, economic trends, or demographic shifts intersect with this topic?
- Has this pattern played out in other provinces or countries? What happened there?
- What recent developments (last 12 months) have changed the landscape?
- Are there academic papers, auditor reports, or investigative pieces that examined similar questions?

**Structural context:**
- What is the institutional machinery behind these numbers? (e.g., how does Alberta's education funding formula actually work? Who sets it?)
- What political or economic pressures were active during the period in question?
- What data exists that the source analysis did NOT use but could inform the story?

**Incentive analysis:**
- For each major actor (provincial government, school boards, universities, federal government), what do they gain from different framings of this data?
- Where do incentives align across institutions in ways that might produce coordinated messaging?
- What would a critic of the findings say, and is that critique grounded in high-reliability evidence?

Return your findings as structured notes:
1. **Key people and institutions** — names, roles, relevance
2. **Connected stories** — adjacent topics with brief explanation of the connection
3. **Contextual facts** — verified background that enriches the narrative
4. **Incentive map** — who benefits from what framing
5. **Contradictions or tensions** — anything that complicates the simple narrative
6. **Sources** — URLs or citations for everything you found

Do NOT fabricate sources. If you cannot verify something, say so. Prefer government data, academic papers, auditor reports, and established journalism over opinion pieces.
```

### What to do with research output

- **Footnotes**: Context about institutions, people, or adjacent policies belongs in footnotes (sidenotes on wide screens). Example: a footnote about Alberta's school construction approval pipeline, or about federal immigration policy changes that drove the population surge.
- **Inline enrichment**: Where research reveals a mechanism behind a number (e.g., *why* post-secondary grew faster), weave it into the prose. Attribute clearly.
- **Incentive map**: Use this to pressure-test the framing. If the analysis happens to align with what every powerful institution wants people to believe, note the alignment explicitly and verify through high-reliability domains.
- **Discard**: Research that is speculative, unsourced, or that would weaken the argument without being grounded in data. The post's authority comes from restraint.

---

## Incentive Weighted Reasoning (IWR)

Before finalizing the post, route the core claims through this framework. This is a structural check, not a political one.

### For each major claim in the post:

1. **Domain reliability** — Is this claim grounded in high-reliability evidence (math, primary data, replicated findings, working code) or low-reliability evidence (policy consensus, institutional messaging, expert predictions)?

2. **Incentive scan** — Which institutions or actors have a stake in this claim being framed a particular way? Rate the incentive pressure:
   - **Low**: No powerful actor benefits from a specific framing
   - **Medium**: Some institutional preference exists but dissent is tolerated
   - **High**: Aligned incentives across multiple institutions, high cost of dissent, coordinated messaging

3. **Scaffolding check** — If incentive pressure is high, ask:
   - Does the claim feel "obvious" or like "common knowledge"? (Suspect — may reflect training data contamination)
   - Can the claim be reformulated mechanistically? (Prefer: "spending per capita declined 9.5% in real terms" over "the government underfunded education")
   - Does the evidence survive if you strip the narrative framing?

4. **Contradiction audit** — List any high-reliability evidence that doesn't fit the emerging narrative. If it exists with probability > 0.3 of undermining a core claim, STOP and restructure around it. Do not bury inconvenient data.

5. **Answer discipline** — For each sentence in "What It Means":
   - Does it follow from the analysis, or does it "sound like what you should say"?
   - Remove the conclusion — does what remains sound like institutional messaging? If yes, rewrite.
   - Did you add caveats beyond what was asked? Why? Flag any additions explicitly.

### Trust hierarchy for claims in the post:

```
MOST TRUSTED:  Derived from primary data, math, formal models
               ↓
               Verified through independent sources (auditor reports, StatsCan)
               ↓
               Contextual from high-quality journalism with named sources
               ↓
LEAST TRUSTED: Intuitions, "common knowledge," "balanced" talking points
```

### IWR in practice for this blog:

- State what the data shows. Do not editorialize about intent unless the evidence supports it.
- "K-12 real per-capita spending declined 9.5%" is a high-reliability claim (derived from primary data + CPI).
- "The government chose to underfund K-12" is a low-reliability claim (requires intent evidence not in the data).
- When institutional framings conflict with primary data, lead with the data. Note the framing conflict in a footnote if relevant.
- Be direct, not diplomatic. If uncertain, say specifically why — not with generic hedges.

---

## Front Matter Template

```yaml
---
title: <Descriptive title — lead with the finding, not the topic>
subtitle: <One-sentence summary with the key numbers>
date: <today's date, YYYY-MM-DD>
tags: post
layout: post.njk
hashtags:
  - <relevant>
  - <tags>
---
```

## Post Structure

1. **Abstract** — Wrap in `<div class="abstract"><p>...</p></div>`. Summarize the entire analysis in one paragraph: the question, the nominal answer, the inflation-adjusted (or otherwise corrected) answer, and the key takeaway. Front-load the surprise.

2. **Context section** — Set up the background (e.g., population growth, market conditions). Use tables for key metrics. Embed and discuss relevant figures. Establish the "so what?" question the analysis answers. Integrate research agent context here — institutional background, adjacent policy, demographic drivers.

3. **Data section** — Present the raw findings. Tables of source data. Embed all relevant charts. Discuss structural patterns, anomalies, and data gaps. Note caveats inline.

4. **Nominal / surface-level story** — Present what the headline numbers appear to show. Build the case that looks straightforward. This section deliberately sets up the reveal.

5. **Adjusted / deeper story** — Introduce the correction (inflation, normalization, per-capita, etc.). Show how the headline transforms. This is the dramatic pivot — use contrast language ("collapses," "dissolves," "razor-thin margin"). Embed the most policy-relevant charts here.

6. **What it means** — 3–4 paragraphs. Summarize the gap between headline and reality. Identify the most consequential finding. Acknowledge limitations honestly. End with the open questions — what remains unanswered. Run this section through IWR before finalizing.

7. **Data attribution** — Italic paragraph at the end citing all sources with links.

8. **Footnotes** — Use `[^n]` syntax. Content: methodology notes, context from research agent that enriches but would interrupt the narrative, incentive/institutional context, caveats on specific numbers. These render as margin sidenotes on wide screens.

## Writing Rules

- **Every number must be real.** No approximations, no placeholders, no rounding beyond what the source provides. If a number is needed and unavailable, say so explicitly.
- **Lead with findings, not process.** "K-12 real per-capita spending declined 9.5%" not "We calculated the CPI-adjusted per-capita figure."
- **Use contrast for impact.** Place nominal and real figures side by side. Let the gap speak.
- **Tables for precision, prose for narrative.** Don't narrate what a table already shows — interpret it.
- **Embed every chart.** Each figure gets a markdown image tag and 1–3 sentences of interpretation. Use raw GitHub URLs: `https://raw.githubusercontent.com/casruta/<repo>/main/plots/<filename>.png`
- **Acknowledge data gaps.** Never interpolate or speculate about missing years. State the gap, note what it prevents you from concluding.
- **No hedging language.** Don't say "it appears that" or "it seems." State what the data shows. Use "cannot be determined from this data" when genuinely uncertain.
- **Mechanistic over normative.** Prefer "spending did not keep pace with population" over "the government failed to invest." The data shows the former; the latter requires evidence of intent.
- **Accountability through juxtaposition.** State the data. State who was in charge. Stop. "Under Premier Smith's government, K-12 real per-capita spending declined 9.5%" — the sentence does not need "which means she neglected schools." The reader is not stupid.
- **Transitions should advance the argument.** Each section ending should set up the next section's question.
- **Research enriches, not inflates.** Add research findings only where they make the data story more complete or more honest. Never add context just to seem thorough.

## Tone Calibration

- Think: The Economist meets Gwern. Institutional confidence, analytical precision, zero decoration.
- Sentence length: vary. Short sentences for emphasis. Longer sentences for nuance and qualification.
- Occasional dry wit is acceptable ("roughly the cost of a single university textbook"). Sarcasm is not.
- Write for a reader who understands percentages and per-capita math but may not know the policy context.
- Be direct, not diplomatic. If the data says something uncomfortable, say it plainly.

### Editorial Stance: Data and Accountability, Not Blame

The purpose of these reports is to present the data and establish who held decision-making authority during the period in question. The reader connects the two. The writer does not.

- **Name the officeholders.** When a policy, budget, or structural shift occurred during a specific government's tenure, name the premier, minister, or governing party as a matter of public record — not as an accusation. Example: "Under Premier [X]'s government, K-12 operating expense grew 59.9% in nominal terms between 2012 and 2025" — not "Premier [X] failed to fund K-12."
- **Never assign motive.** Do not speculate about why decisions were made. "Post-secondary's share of the education budget rose from 32% to 40%" is a finding. "The government chose to prioritize universities over schools" is an editorial.
- **Never assign blame.** The analysis shows what happened and under whose watch. That is sufficient. If the data is damning, it does not need help.
- **Let juxtaposition do the work.** Place the data next to the decision-maker's public record and let the contrast speak. The power is in the restraint.
- **Research agent accountability context.** The research agent should identify who held relevant cabinet positions (Education Minister, Advanced Education Minister, Premier, Treasury Board chair) during the periods covered. These names appear in footnotes or inline factual references — never in accusatory framing.

## Image Handling

- All images use raw GitHub URLs from the source repository
- Every image referenced in the README must appear in the post
- Alt text should be descriptive: `![Real Per-Capita Education Spending (K-12 vs Post-Secondary)](...)`
- Group related charts together when they tell a combined story

## Quality Checks Before Committing

- [ ] Every number in the post matches the source README exactly
- [ ] All charts from the source are embedded
- [ ] Front matter is valid YAML with correct date and layout
- [ ] Footnotes are numbered sequentially and all referenced in text
- [ ] No orphaned sections — every section connects to the narrative arc
- [ ] Abstract contains the key finding, not just the topic
- [ ] The "reveal" section actually reveals something the nominal section set up
- [ ] Research agent findings are attributed and sourced (no unsourced context)
- [ ] IWR pass complete: no claims rest solely on low-reliability domains
- [ ] No normative claims smuggled in as data conclusions
- [ ] "What It Means" section survives the scaffold leak test
