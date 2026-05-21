---
description: Invoke the DP to convert story beats into MCSLA-formatted Seedance prompts
argument-hint: [campaign-id, optional]
---

# /shot-list $ARGUMENTS

Have the DP read the story bible and produce per-beat prompt files.

> **Path resolution**: every `campaigns/{id}/...` reference below resolves to `$CAMPAIGN_OS_CAMPAIGNS_DIR/{id}/...` if the env var is set, otherwise to the literal `./campaigns/{id}/...`. See `CLAUDE.md → Workspace configuration`.

## Steps

1. Resolve campaign per `$ARGUMENTS` or active campaign.

2. Verify `campaigns/{id}/story-bible.md` exists and is locked. If not, stop and ask the user to run `/brief` first.

3. Load the **DP** agent: read `agents/dp.md`.

4. Load supporting skills:
   - `skills/seedance-prompt/SKILL.md` (MCSLA + prompt modes)
   - `skills/mcsla-formula/SKILL.md` (worked examples)
   - `skills/soul-character/SKILL.md` (consistency techniques)

5. Pass the DP:
   - The story bible
   - The visual system from `campaigns/{id}/brand/visual-system.md` (if AD has produced it yet — D2 ideally has)
   - Character Soul ID sheets from `campaigns/{id}/characters/`
   - The current DP prompt version (`prompts/dp/v{latest}.md`)

6. For each beat in the story bible (hero film + hub cuts), the DP produces:
   - `campaigns/{id}/beats/{beat-id}.md` — beat file with full MCSLA prompt
   - A keyframe generation plan (which beats need Soul 2.0 or Nano Banana Pro pre-pass)

7. The DP does NOT execute generations yet. Shot list is the planning artifact. Execution is `/generate`.

8. Tell the user:
   ```
   Shot list ready for {campaign-id}.
   Total beats: {n}
   Hero beats: {n} (require keyframe pre-pass)
   Continuation chains: {x} sequences
   Estimated Higgsfield credit cost: {n} (rough — based on attempt budget)

   Next step (D2-D3): run /generate <beat-id> for each beat. Start with hero beats.
   ```

## Validation

The DP must produce a complete MCSLA prompt for every beat — all 8 sections present, even if some say `(intentional default)`. Reject incomplete prompts.

If the story bible's DP handoff is missing or vague, the DP can push back to the Showrunner before producing prompts.
