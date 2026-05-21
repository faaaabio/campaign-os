---
description: Run HyperFrames adaptations for an approved beat across target channels and languages
argument-hint: <beat-id> --channels=<list> [--langs=<list>]
---

# /adapt $ARGUMENTS

Run channel adaptations via HyperFrames for an approved master.

> **Path resolution**: every `campaigns/{id}/...` reference below resolves to `$CAMPAIGN_OS_CAMPAIGNS_DIR/{id}/...` if the env var is set, otherwise to the literal `./campaigns/{id}/...`. See `CLAUDE.md → Workspace configuration`.

## Steps

1. Parse arguments:
   - First positional: beat ID (e.g., `b03`)
   - `--channels=tiktok,reels,yt-shorts,...` (required)
   - `--langs=pt,en,es` (optional, defaults to campaign primary language)

2. Resolve active campaign.

3. Verify the beat is approved:
   - Read `campaigns/{id}/beats/{beat-id}.md`
   - Confirm `status: approved`
   - If not approved, stop and tell user to approve the master first

4. Run **brand-safety-check** if not already passed for this beat:
   - Load `skills/brand-safety-check/SKILL.md`
   - Run the checklist on the master video
   - If any flag, halt and surface to Art Director
   - If pass, document at `campaigns/{id}/brand-safety/{date}.md`

5. Load the **Motion** agent (`agents/motion.md`) and HyperFrames skill (`skills/hyperframes-template/SKILL.md`).

6. Verify the campaign's `wrapper/` directory has templates for each requested channel. If missing, Motion produces them first using the channel-specific patterns from the skill. Templates must have AD approval (signed off in `brand-safety/`).

7. Build the `batch-spec.json` from the request:
   ```json
   {
     "beat_id": "{beat-id}",
     "master": "outputs/{master-tag}.mp4",
     "common_vars": { ... from campaigns/{id}/brand/colors.json ... },
     "variants": [
       { "template": "tiktok.html", "channel": "tiktok", "format": "9x16", "lang": "pt", ... },
       { "template": "tiktok.html", "channel": "tiktok", "format": "9x16", "lang": "en", ... },
       ...
     ]
   }
   ```

   Channel × language × variant matrix per the channel-plan.md from Channel Strategist.

8. Execute:
   ```bash
   cd campaigns/{id}/wrapper
   npx hyperframes batch ../batch-spec.json
   ```

9. HyperFrames produces tagged MP4s in `campaigns/{id}/outputs/`. Naming per `playbooks/tagging-schema.md`:
   ```
   {campaign-id}_{beat-id}_{channel}_{format}_{variant}-{lang}_seedance2-hf_v{prompt-v}.mp4
   ```

10. Update `campaigns/{id}/outputs/manifest.json` with every new variant.

11. Show the user a summary table:
    ```
    Adaptations complete for {beat-id}:

    | Channel    | Format | Lang | File                                         |
    |------------|--------|------|----------------------------------------------|
    | tiktok     | 9x16   | pt   | nike-q3_b03_tiktok_9x16_a-pt_seedance2-hf... |
    | tiktok     | 9x16   | en   | nike-q3_b03_tiktok_9x16_a-en_seedance2-hf... |
    | reels      | 9x16   | pt   | nike-q3_b03_reels_9x16_a-pt_seedance2-hf...  |
    | ...        |        |      |                                              |

    Total variants: {n}
    Total HyperFrames render time: {n}s
    All UTMs will use utm_content={full-tag} for analytics joins.

    Next (D5): ship to media platforms. Instrumentation handled per channel-plan.md.
    Then (D6): run /measure to read in-flight performance.
    ```

## Hygiene volume mode

If the request is hygiene-tier (no beat master, pure HyperFrames generation — e.g., promo countdowns, price overlays):

1. Skip the master-approval check
2. Use Motion's volume mode (see `agents/motion.md` → Volume mode section)
3. Brand safety still runs but is faster — verify wrapper templates and copy only

Tag with `_hygiene_` in the variant slot.
