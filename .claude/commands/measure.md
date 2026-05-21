---
description: Pull in-flight performance via Meta Ads MCP, attribute results, deliver diagnostic to Channel Strategist
argument-hint: [campaign-id, optional]
---

# /measure $ARGUMENTS

Read live performance data and produce a diagnostic for in-flight decisions.

## Steps

1. Resolve campaign.

2. Verify the campaign has launched. If `manifest.json` lacks a `launched_at` timestamp, ask the user to confirm launch happened or to mark it.

3. Load the **Analyst** agent (`agents/analyst.md`).

4. Read inputs:
   - `campaigns/{id}/outputs/manifest.json` (the full tag → file map)
   - `campaigns/{id}/channel-plan.md` (KPI targets per beat)

5. Pull performance via Meta Ads MCP:
   - Query at the ad/creative level for the campaign's ad set IDs
   - Filter by `utm_content` matching tags in manifest.json
   - Time window: launch → now (or last 24h, whichever is shorter, for first measurement pass)

6. Build the joined performance table:
   ```
   tag | impressions | spend | hook-2s | hold-25 | CTR | CVR | CPA
   ```

7. Stratify analysis by:
   - `beat-id` → which story moments resonate
   - `model` → diffusion vs structural performance
   - `channel × format` → adaptation effectiveness
   - `lang` → localization performance
   - `prompt-version` → are recent prompt updates moving the needle?

8. Produce `measurement/reports/{sprint-id}-d6.md`:
   - 24h snapshot of key metrics
   - Top 3 performers with attribution hypothesis
   - Bottom 3 performers with diagnostic (creative / distribution / audience)
   - Recommendations to Channel Strategist (scale / kill / iterate)

9. Show the user the report inline and tell them:
   ```
   In-flight report ready at measurement/reports/{sprint-id}-d6.md

   Top performer: {tag} (+{n}% vs benchmark)
   Bottom performer: {tag} ({n}% under) — diagnosis: {cause}

   Recommended actions for Channel Strategist:
   - Scale: {tags}
   - Kill: {tags}
   - Iterate: {tags} with notes

   The Channel Strategist should now read this and decide on spend/variant shifts.
   ```

## Cadence

Default to a single `/measure` call on D6. For high-stakes campaigns, consider multiple calls (D5 EOD, D6 morning, D6 EOD). Each writes a separate timestamped report.

## Vanity-metric guard

The Analyst flags vanity metrics explicitly. Impressions without CTR, reach without engagement, views without completion — these are noted but not optimized toward. The report should make this distinction clear so Channel Strategist doesn't scale spend on the wrong signal.
