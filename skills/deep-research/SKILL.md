---
name: deep-research
description: Run a deep, multi-source, fact-checked research pass. Fan out searches across angles, fetch and score sources, adversarially verify each claim, then synthesize a cited report with explicit confidence levels.
---

# deep-research

A harness for research you can actually trust. Most AI "research" is one search and a confident summary. This skill forces breadth, then verification, then citation, so the output is defensible instead of plausible.

Use it when the answer needs to be correct, multi-source, and quotable: market and competitor scans, technical comparisons, due diligence, "what is the current state of X," or any question where being wrong is expensive.

## The loop

1. **Scope first.** If the question is underspecified, ask 2 to 3 sharp clarifying questions before searching (budget, region, time window, use case). A narrow question returns a useful answer; a vague one returns mush.
2. **Fan out, do not tunnel.** Generate 4 to 8 distinct search angles, not rephrasings of one query. Cover the topic by container (who/what/where), by counter-claim (search for the opposite of your hypothesis), by time (recent vs established), and by primary source (filings, docs, repos, datasets, not just blogs).
3. **Fetch and score.** Open the actual sources. Score each on recency, authority (primary > reputable secondary > anonymous), and independence (two outlets citing the same press release are one source, not two).
4. **Extract claims, not vibes.** Pull the specific factual claims that matter. Tag each with its source.
5. **Adversarially verify.** For every load-bearing claim, look for an *independent* corroborating source, and actively search for one that contradicts it. A claim with one source is a lead, not a fact. Mark single-source claims as such.
6. **Synthesize with citations.** Write the report. Every non-obvious claim carries an inline source link. Separate what is well-supported from what is contested or thin.

## Verification rules (the part that makes it trustworthy)

- **No assertion without a source.** If you cannot link it, label it as inference or estimate, not fact.
- **Two independent sources** for anything material. Note when you only found one.
- **Prefer primary.** A company's own filing or a project's own repo beats a summary of it.
- **Distrust round numbers and viral stats.** Trace them to origin; aggregators routinely inflate or mis-attribute figures.
- **Date everything.** "Current" decays. State the as-of date for time-sensitive claims.
- **Surface disagreement.** When good sources conflict, present both and say which is better supported and why. Do not average them into a fake consensus.

## Scaling depth

For a heavy question, run sub-questions in parallel: split the topic into independent threads, research each thread on its own (fan out, fetch, verify), then merge and dedupe findings before writing. A final pass asks one question: what is still missing, unverified, or single-sourced? That gap list becomes the next round or an explicit "open questions" section.

## Output format

```
# <Question>

## Bottom line
2 to 4 sentences. The answer, stated plainly, with the confidence level.

## Findings
- <claim> [source](url) <confidence: high | medium | low>
- <claim> [source](url) <confidence: ...>
  - contested: <the opposing view> [source](url)

## What is thin or contested
- <single-source or disputed claims, called out honestly>

## Sources
Numbered list of every source used, with what each contributed.

## Open questions
What a follow-up round should chase.
```

## Anti-patterns

- Searching once and summarizing. That is a guess with footnotes.
- Treating the first page of results as the truth.
- Citing a source you did not open.
- Hiding uncertainty to sound authoritative. Confidence levels are a feature, not a weakness.
- Padding the report. Length is not rigor; verified claims are.
