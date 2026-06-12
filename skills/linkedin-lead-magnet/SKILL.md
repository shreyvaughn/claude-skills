---
name: linkedin-lead-magnet
description: Draft or sharpen a LinkedIn lead-magnet post - one that ends with "comment <KEYWORD> to receive <ASSET>". Two modes - generate 3 variants from an asset description, or sharpen a draft. Keeps the author's voice while applying the proven comment-to-receive structure.
---

# linkedin-lead-magnet

The user wants to write or sharpen a LinkedIn post whose explicit goal is harvesting comments and connections in exchange for a delivered asset.

The lead-magnet format is one of the strongest published patterns on LinkedIn. The job of this skill is to scale a proven formula across new topics, not to invent a new style.

## Two modes - decide first

- **Mode A - generate from asset description.** The user gave you the asset, trigger word, and pain. You produce 3 fresh variants.
- **Mode B - sharpen a draft.** The user pasted a lead-magnet post they wrote and wants it sharpened. You diagnose it against the patterns, then output a tightened version plus an alternative angle.

If ambiguous, ask one line: "Are you giving me an asset to draft a magnet from, or a draft you already wrote that needs sharpening?"

## Step 0 - Optional: study current top performers

Before drafting, offer to look at current top-performing lead-magnet posts in the user's niche (ask them to name a few creators, or paste examples). This refreshes the opener shapes and trigger-word patterns that are landing right now.

If the user is impatient, fall back to the static shapes in this file, flagged as "using the built-in shapes, not live examples".

## What qualifies as a lead magnet (vs a regular post)

- It ends with an instruction to comment a specific trigger word in exchange for receiving an asset.
- The asset is named: a system, swipe file, prompts pack, walkthrough video, blueprint, checklist, template.
- The CTA usually requires connecting plus commenting (2-step), sometimes plus liking (3-step).

If any of those is missing, the user wants a regular post. Use the `linkedin-post` skill instead.

## Step 1 - Intake

### Mode A - generate from asset description

Ask all five in one message:

```
Five quick answers and I will draft 3 variants:

1. The asset - what are you giving away? (e.g. "a Notion doc + workflow exports", "an audit checklist", "a walkthrough video")
2. Trigger word - what should they comment to claim it? 1-2 words, easy to type, not a common English word.
3. Topic - what is this about in one phrase?
4. Pain - what is the painful status quo this asset replaces?
5. Proof point - at least one specific number you can stand behind (a count, a percentage, a timeframe, a money figure).
```

If they already gave enough, skip and confirm.

### Mode B - sharpen a draft

```
Two quick checks before I edit:
1. Trigger word + asset - confirm these from the draft so I do not change them accidentally.
2. Goal - keep your voice intact and just tighten, or open to swapping the opener / structure if a better shape would land harder?
```

If clear from context, confirm in one line and proceed.

## Step 2 - Pick the pattern for each variant

Three working lead-magnet shapes. Generate one of each across the 3 drafts.

### Shape A - death-of-old-way
- Opener: `R.I.P [old thing].` or `Breaking: [tool/system] just killed [common practice].`
- Body: name the failing status quo, bullet-list its symptoms, reveal the new system, bulleted feature list of what it does, numbered CTA.
- Length: long (300 to 500 words).
- Best fit: sales, outreach, agent or automation topics.

### Shape B - stack-reveal
- Opener: `I just shipped [specific N-component system] that [outcome with a number].`
- Body: short context line, arrow-bulleted limitations of the alternative, arrow-bulleted what the asset includes, numbered CTA.
- Length: medium to long (200 to 400 words).
- Best fit: stacks, replaced-role narratives.

### Shape C - gap-name
- Opener: `[Specific stat]. [Counter-stat]. [Question that names the gap].`
- Body: name the gap, 3-bullet diagnosis of why it exists, reveal the asset that closes it, 2-bullet outcome teaser, numbered CTA.
- Length: medium (180 to 300 words).
- Best fit: strategy, governance, adoption topics.

## Step 3 - Voice rules

### Banned

- **Em dashes.** Anywhere. Use commas, periods, semicolons, colons, or parentheses.
- **"Not X, not Y" positioning phrasing.** (Arrow-bulleted negative lists in the body are fine, see below. The ban is on using that pattern to define what your offer IS.)
- **Staccato period-fragment-period rhythm.** This is the number one AI-tell. Never write recurring 1 to 3 word fragments separated by periods (`no chaos. no work. no guessing.`). The fix: write it as one flowing sentence with commas and "and". One isolated fragment for a single moment of emphasis is fine. Stacking three in a row is the tell.
- **Corporate filler verbs:** leverage, synergize, unlock, disruptive, game-changer, revolutionize.
- **AI-tell phrases:** "in today's fast-paced world", "navigate the complexities of", "ever-evolving", "at the end of the day", "delve into", "harness the power of".
- **Vague magnitudes:** "tons of leads", "massive results", "huge ROI". Use specific numbers or skip the claim.

### Required

- **Mobile rhythm.** 1 to 3 line paragraphs. Single-line paragraphs are the norm.
- **At least 2 hard numbers** in the body. Numbers do the persuasion.
- **One negative-trio, arrow-bulleted form only.** `→ can't X / → can't Y / → can't Z`. Exactly three lines, not four. Do NOT use the period-separated prose form, which triggers the staccato tell.
- **Arrow bullets for capability lists, dot bullets for feature lists.** Match what works.
- **First-person directness.** "I shipped X." "I built Y." Never "We are excited to share".
- **Trigger word in quotes and capitalized.** `Comment "Keyword"` not `comment keyword`.
- **Numbered CTA at the end.** 2-step (connect + comment) or 3-step (connect + comment + like).

### Style notes

- A `R.I.P [old thing].` opener is a proven shape. Keep it as an option, rotate it so it does not get stale.
- Heavy emoji is fine on the hook if the topic supports it (`R.I.P`, `Breaking`). Do not sprinkle emoji through the body.

## Step 4 - Output

### Mode A - generate 3 variants

Produce 3 variants in a single response. One per shape (A, B, C). Each variant:

```
### Draft N - Shape [A|B|C] - [length] - [topic]

[post text, copy-pasteable, with the CTA at the bottom]

Trigger word: "[word]"
Asset implied: [one-line description]
Why this should work: [1 sentence]
Tradeoff vs the others: [one line]
```

End with: `Which draft do you want to refine, or want me to remix with a different angle?`

Three drafts is the spec. Do not output a fourth.

### Mode B - sharpen a draft

Principle: the user's voice stays, the magnet mechanics get fixed. You are not rewriting them into someone else. Keep their sentences and rhythm, sharpen only where the structure leaves engagement on the table.

```
Diagnosis of your draft:
- Opener: [classify against the three shapes. Punchy in the first 2 lines? Earns the scroll-stop?]
- Pain framing: [names a specific painful status quo? If vague, flag.]
- Proof points: [count hard numbers. If fewer than 2, flag.]
- Negative-trio: [present? exactly 3 lines? if 4+, suggest cutting. If 0, suggest adding if the topic fits.]
- Feature list: [bulleted? arrow vs dot? teaches the reader or just hypes?]
- Trigger word: [confirm exactly. Flag if it is a common English word (collision risk) or longer than 2 words.]
- CTA mechanics: [numbered? 2-step or 3-step?]
- Length: [word count + bucket. Flag if under 150 words.]

Sharpened version (your voice, sharper magnet):
[Full edited post, copy-pasteable, CTA included. Keep the user's vocabulary and rhythm. Only change: weak hook, missing or vague proof points, negative-trio length, feature-list specificity, colliding trigger word, non-standard CTA. Do NOT introduce vocabulary the user does not already use.]

What I changed and why:
- [change → reason]

Voice deltas:
- [if any. If none, say "none, voice intact".]

Alternative angle (optional, only if a different shape would land much harder):
[1 paragraph pitch for a structural recast. Do not write the rewrite yet.]
```

End with: `Want me to tighten further, run the alternative angle, or save this as the final draft?`

## Step 5 - Refinement

When the user picks one, tighten it:
- Cut at least 10 percent of word count unless they ask to expand.
- Sharpen the opener, give 1 to 2 alternates if it helps.
- Verify the trigger word is unique enough to be detectable in comments. Avoid common English words like "AI", "Go", or "Now" alone. Prefer something with a consonant cluster or a compound.

## Annotated shapes (built-in reference)

Worked structures, voice-neutral. Fill with the user's own receipts.

- **Shape A example skeleton:** `<bold money-or-outcome line> ‼️ <parenthetical second hook>. <Common failing alternative>. → can't X / → can't Y / → can't Z. <reveal>. <feature bullets>. <numbered CTA>.` Why it works: the opener carries a number and a comparison, the parenthetical creates a second hook for skimmers, the arrow-bulleted negative-trio is portable, then it pivots to a positive reveal.
- **Shape B example skeleton:** `Breaking: I just shipped <thing>. Here is what <specific number> <unit> of <medium> actually looks like. <reframe of how the reader thinks about the tool>. <named stack components>. What this replaces: <comparison rows>. <CTA>.` Why it works: specific numbers replace vague magnitudes, naming the actual stack does the persuasion, the "what this replaces" framing lands the value.
- **Shape C example skeleton:** `<specific stat>. <counter-stat or counter-claim>. <question that names the gap>. <3-bullet diagnosis>. <asset reveal>. <2-bullet outcome teaser>. <numbered CTA>.` Why it works: stat-first earns the scroll-stop, the gap question creates tension, the asset is the resolution.

## Anti-patterns

- Lead magnets vague about what the asset actually is. ("Comment SYSTEM to learn how I do this" is bad. Name the artifact.)
- Multiple competing CTAs. One trigger word per post.
- A trigger word that collides with common conversation in comments.
- Asset promises the user cannot deliver. The skill assumes the asset is real and ready to DM within 24 hours.
- Generic slop wrapped around a CTA. The lead magnet should be a strong post even without the CTA.
- External link CTAs. Always replace with a comment trigger.
- 4+ line negative-trios. Cap at 3.
