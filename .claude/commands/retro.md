---
description: Run the D7 retrospective — measure final performance, generate retro doc, prompt agent updates
argument-hint: [campaign-id, optional]
---

# /retro $ARGUMENTS

Run the D7 retro that turns this sprint into next sprint's prompt upgrades.

## Steps

1. Resolve campaign.

2. Verify the campaign is at end-of-sprint:
   - `manifest.json` shows ≥6 days since `launched_at`
   - At least one `/measure` report exists

3. Load the **Analyst** agent (`agents/analyst.md`) and the retro skill (`skills/retro-doc/SKILL.md`).

4. Pull final performance via Meta Ads MCP:
   - Full sprint window (launch → end-of-sprint)
   - All creatives with `utm_campaign={campaign-id}`
   - Both leading (hook, hold) and lagging (CVR, CPA) metrics

5. Produce `measurement/reports/{sprint-id}-final.md` (the data foundation for the retro).

6. Produce `campaigns/{id}/retro.md` per `skills/retro-doc/SKILL.md` template. Sections:
   - Sprint summary
   - Three best assets (with hypothesis)
   - Three worst assets (with diagnosis)
   - By-agent performance
   - A/B prompt test results (if any)
   - Open questions
   - Prompt update recommendations (per agent)
   - Cross-sprint patterns (look back at last 3 retros)

7. Notify each agent in turn — they read their section of the retro and author the next version of their prompt:
   ```
   For each agent (showrunner, dp, motion, cgi, art-director, channel-strategist, analyst):
     - Read their section in retro.md
     - Read prompts/{agent}/v{n}.md (current)
     - Author prompts/{agent}/v{n+1}.md with edits informed by the data
     - Keep changes focused — don't rewrite the whole prompt, edit surgically
   ```

8. Commit the new prompt versions:
   ```bash
   git add prompts/*/v*.md campaigns/{id}/retro.md measurement/reports/{sprint-id}-final.md
   git commit -m "retro({sprint-id}): {campaign-id} → prompt versions updated"
   ```

9. Update `manifest.json`:
   - `status: "closed"`
   - `retro_at: {timestamp}`
   - `next_prompt_versions: { showrunner: "v{n+1}", dp: "v{n+1}", ... }`

10. Tell the user:
    ```
    Retro complete for {campaign-id}.

    Top finding: {one-line takeaway}
    Prompts updated: {list of agents with new versions}
    Open for next sprint: {n} questions queued

    The OS just learned. Next /sprint-start will use v{n+1} prompts.
    ```

## Quarterly meta-retro

Every 12 sprints (roughly quarterly), prompt the user to run a meta-retro:

```
You've completed 12 sprints since the last meta-retro. Consider promoting
recurring patterns from the last 12 retros into playbooks/methodology.md.

Run: claude > "Read the last 12 retros and propose updates to methodology.md"
```

This is how the OS evolves at the methodology level, not just the prompt level.

## Anti-skip discipline

If the user tries to start a new sprint without running the previous retro, refuse and remind them:

```
Cannot run /sprint-start while {previous-campaign-id} is still status: active.
Run /retro first. The retro is non-negotiable — skipping it means the OS stops improving.
```
