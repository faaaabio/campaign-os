---
name: retro-doc
description: Generate the D7 retrospective document and trigger prompt-version updates across the agent system. Always use this skill when invoked via /retro at the end of every sprint, when the Analyst is reading performance data and translating it into prompt improvements, or when documenting what worked vs. didn't for the OS to learn from. The retro is the loop that turns each sprint into an upgrade — skipping it means the OS stops improving.
---

# Retro Document

The retro is run by the Analyst on D7 of every sprint. It is the single mechanism by which the OS learns. Without it, prompts don't improve and the system stays at its initial quality forever.

## When to run

D7 of every sprint. Non-negotiable. If you're tempted to skip because "we're too busy with the next campaign," that's exactly when the retro matters most — busy without learning compounds the wrong patterns.

## Inputs

1. `measurement/reports/{sprint-id}-d6.md` (in-flight diagnostic)
2. `measurement/reports/{sprint-id}-final.md` (end-of-sprint metrics)
3. `campaigns/{id}/outputs/manifest.json` (asset tags)
4. `campaigns/{id}/beats/*.md` (render logs with rejection counts)
5. `prompts/{agent}/v{n}.md` (current prompt versions in flight)

## Template

Save at `campaigns/{id}/retro.md`:

```markdown
# Retro: {campaign-id}

> Sprint ID: {YYYY-WW}
> Duration: D1 {date} → D7 {date}
> Owner: Analyst
> Status: [in-progress | published | prompts-updated]

## Sprint summary

- Hero films shipped: {n}
- Hub pieces shipped: {n}
- Help pieces shipped: {n}
- Hygiene variants shipped: {n}
- Total Higgsfield credits spent: {n}
- Total HyperFrames renders: {n}
- KPI vs. target: {hit / missed / partial — name the specific metric}

## Three best assets

### 1. `{tag}`
- Channel/format: {tiktok 9x16}
- Performance vs. benchmark: {+34% hook retention, +12% CTR vs sprint avg}
- Hypothesis for why: {2 sentences}
- What to keep doing: {1-2 specific things}
- Screen reference: {path or link}

### 2. ...
### 3. ...

## Three worst assets

### 1. `{tag}`
- Channel/format: {meta-feed 4x5}
- Underperformance: {-40% CTR vs sprint avg}
- Diagnosis: was this creative, distribution, or audience?
  - Creative: {specific element — e.g., "hook landed at 4s instead of 1.5s"}
  - Distribution: {e.g., "audience targeting too broad"}
  - Audience: {e.g., "wrong persona for this beat"}
- Cause attribution: {primary cause}
- What to stop doing: {1-2 specific things}

### 2. ...
### 3. ...

## By-agent performance

### Showrunner (prompt v{n})

- Approval rate (story bible accepted as-is): {%}
- Avg iterations to lock: {n}
- AI-native justification quality: {strong / weak — was the AI-native angle real or rationalized?}
- What worked: {specific patterns from this sprint's bible}
- What to update for v{n+1}: {bullet list}

### DP (prompt v{n})

- Approval rate per beat (renders shipped without re-prompt): {%}
- Avg attempts per hero beat: {n}
- Face-filter rejection count: {n}
- Model mix used: {Seedance 2.0: %, Soul 2.0: %, Veo: %, etc.}
- Which prompt mode performed best: {Reference-Based / Continuation / etc.}
- What worked: {specific MCSLA patterns}
- What to update for v{n+1}: {bullet list}

### Motion (prompt v{n})

- Channel adaptations shipped on time: {y/n + delay if any}
- HyperFrames template reuse rate: {how much of this sprint reused prior templates}
- What worked: ...
- What to update for v{n+1}: ...

### CGI (prompt v{n})
{same structure if invoked this sprint}

### Art Director (prompt v{n})

- Visual system adherence: {how often did renders need re-grading}
- Brand safety flags raised: {n}
- What worked: ...
- What to update for v{n+1}: ...

### Channel Strategist (prompt v{n})

- Channel plan accuracy: {did the planned KPIs match reality?}
- In-flight optimization speed: {time from data → decision}
- What worked: ...
- What to update for v{n+1}: ...

### Analyst (prompt v{n})

(self-reflection)
- Did the measurement framework miss anything?
- Were the tags joined correctly?
- What to update for v{n+1}: ...

## A/B prompt tests

If any prompts were A/B tested this sprint:

| Agent | A version | B version | Variable changed | Winner | Δ KPI |
|---|---|---|---|---|---|
| DP | v1.0 | v1.1 | added "anti-slop" negatives section | v1.1 | +18% approval rate |

Winners get promoted. Losers are documented as "tested and rejected" in `prompts/{agent}/_archive/`.

## Open questions

Things the data couldn't answer. Queued for next sprint:

```
- [ ] Does Seedance Reference-Based outperform Continuation on hero beats, or is the difference within noise?
- [ ] Are PT-BR hooks outperforming EN hooks on Reels because of language or because the EN cut had a worse hook line?
- [ ] Is hygiene-tier content actually paying back its production cost?
```

## Prompt update recommendations

Each agent receives this section and authors their own `v{n+1}.md`. Don't write the updates here — point each agent at the data and let them make the creative call.

### To Showrunner
- {data point} → consider {direction}
- ...

### To DP
- {data point} → consider {direction}
- ...

(repeat per agent)

## Cross-sprint patterns

Look back at the last 3 retros. What recurring patterns emerge?

- {pattern 1}: e.g., "Hero beats consistently underperform when we use text-to-video without keyframe"
- {pattern 2}: e.g., "TikTok hooks landing in first 1.5s are 40% more effective than 2-3s"

These patterns should be promoted into the methodology playbook itself if they hold across sprints.

## Sign-offs

- [ ] Analyst (author)
- [ ] Showrunner (read + acknowledged)
- [ ] DP (read + acknowledged)
- [ ] Motion (read + acknowledged)
- [ ] CGI (read + acknowledged if invoked this sprint)
- [ ] Art Director (read + acknowledged)
- [ ] Channel Strategist (read + acknowledged)

## Next-version commits

After all agents acknowledge:

```bash
git add prompts/showrunner/v{n+1}.md
git add prompts/dp/v{n+1}.md
# ... etc.
git commit -m "retro({sprint-id}): prompt versions updated"
```

The OS is now updated for the next sprint.
```

## Quality bar

A good retro:
- Names specific assets with specific numbers, never vague impressions
- Distinguishes creative vs. distribution vs. audience for every failure
- Produces concrete prompt edits, not "be better at hooks"
- Is honest when an idea didn't work, including when it was the senior creative's favorite

A bad retro:
- "Mixed results, learnings noted"
- Praises everything to avoid uncomfortable conversations
- Hand-waves causation ("the algo was weird this week")
- Skipped because the team was busy

## Method drift

Once per quarter (every ~12 sprints), do a meta-retro: re-read the last 12 retros, identify patterns that became methodology, and update `playbooks/methodology.md` directly. The OS is supposed to evolve.
