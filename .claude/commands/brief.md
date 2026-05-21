---
description: Invoke the Showrunner to produce the campaign story bible from the brief
argument-hint: [campaign-id, optional — defaults to active campaign]
---

# /brief $ARGUMENTS

Invoke the Showrunner to produce `story-bible.md` from the brief.

> **Path resolution**: every `campaigns/{id}/...` reference below resolves to `$CAMPAIGN_OS_CAMPAIGNS_DIR/{id}/...` if the env var is set, otherwise to the literal `./campaigns/{id}/...` in this repo. See `CLAUDE.md → Workspace configuration`.

## Steps

1. Determine the target campaign:
   - If `$ARGUMENTS` is provided, use `campaigns/$ARGUMENTS/`
   - Else, find the campaign in `campaigns/*/manifest.json` with `status: "active"`
   - If multiple, ask the user which one

2. Verify `campaigns/{id}/brief.md` is filled (not still the template stub). If empty, stop and ask the user to populate it.

3. Load the **Showrunner** agent: read `agents/showrunner.md` into context.

4. Load supporting skills:
   - `skills/story-bible/SKILL.md`
   - `skills/soul-character/SKILL.md` (for character cards)
   - `playbooks/ai-native-principles.md`
   - `playbooks/methodology.md` (for the 6 methods + CAL spec)

5. Pass the Showrunner:
   - The brief at `campaigns/{id}/brief.md`
   - Any reference materials in `campaigns/{id}/world/_inbox/` and `campaigns/{id}/characters/_inbox/`
   - The current Showrunner prompt version (`prompts/showrunner/v{latest}.md`)

6. The Showrunner produces:
   - `campaigns/{id}/story-bible.md` (full bible per `skills/story-bible/SKILL.md`)
   - `campaigns/{id}/world/world.md`
   - `campaigns/{id}/characters/*.md` (one card per character — invoke `soul-character` skill to produce Soul ID briefs)
   - DP handoff section appended to story-bible.md

7. Before publishing, run the **AI-native justification check**: the Showrunner must answer in writing whether this campaign needs AI to exist. If the answer is unconvincing, push back on the brief rather than ship the bible.

8. Save outputs. Update `manifest.json` with `story_bible_locked_at: {timestamp}`.

9. Tell the user:
   ```
   Story bible locked for {campaign-id}.
   AI-native justification: {one-line summary}
   Beats planned: {n} hero, {n} hub, {n} help
   Characters: {names}

   Next step (D2): run /shot-list to have the DP convert beats into Seedance prompts.
   Also recommended D2: have the Channel Strategist finalize channel-plan.md now that beats are known.
   ```

## Quality gate

Do not let the user proceed past `/brief` if:
- The AI-native justification is missing or hand-waving
- The Big Idea exceeds one sentence
- Characters lack Want ≠ Need
- No beat uses Transformation mode (probably just an ad)

These are caught by `skills/story-bible/SKILL.md`'s quality checks. Refuse to lock the bible until they pass.
