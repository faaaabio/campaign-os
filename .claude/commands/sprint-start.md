---
description: Scaffold a new campaign from the template and kick off D1
argument-hint: <campaign-id>
---

# /sprint-start $ARGUMENTS

You're starting a new sprint. The campaign ID provided is `$ARGUMENTS` (use kebab-case: `client-product-q3-launch`).

## Steps

1. Verify the campaign ID doesn't already exist at `campaigns/$ARGUMENTS/`. If it does, ask the user whether to overwrite or pick a new ID.

2. Copy the template: `cp -r campaigns/_template campaigns/$ARGUMENTS`

3. Populate `campaigns/$ARGUMENTS/manifest.json` with:
   - `campaign_id`: $ARGUMENTS
   - `sprint_id`: ISO week (e.g., 2026-W21)
   - `started_at`: current timestamp
   - `status`: "active"
   - `cadence`: "1-week"
   - `prompt_versions`: scan `prompts/{agent}/` and record current latest version per agent

4. If the user uploaded a brief, save it to `campaigns/$ARGUMENTS/brief.md`. Otherwise, open `campaigns/$ARGUMENTS/brief.md` (which is a template from `_template/brief.md`) and ask the user to fill it.

5. Load the **Channel Strategist** agent prompt from `agents/channel-strategist.md`. Have it:
   - Read the brief
   - Produce an initial `channel-plan.md` skeleton with audience, KPIs, and channel mandates
   - Note open questions
   - Stop short of beat-level mapping (that comes after the Showrunner publishes the story bible)

6. Once the Channel Strategist returns, tell the user:
   ```
   Sprint started for {campaign-id}. The Channel Strategist has produced a draft plan.
   Next step (D1 continued): run /brief to invoke the Showrunner and produce the story bible.
   ```

## Naming validation

Campaign IDs must:
- Use kebab-case (lowercase, hyphens)
- Include client + product/theme + period (e.g., `nike-runtest-q3-2026`)
- Be unique within `campaigns/`

If the user provides a bad ID, suggest a fix before scaffolding.
