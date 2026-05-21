---
description: Generate the Seedance render for a specific beat via Higgsfield MCP
argument-hint: <beat-id> [variant-letter, optional, defaults to a]
---

# /generate $ARGUMENTS

Execute the Seedance 2.0 generation for a single beat through Higgsfield MCP.

> **Path resolution**: every `campaigns/{id}/...` reference below resolves to `$CAMPAIGN_OS_CAMPAIGNS_DIR/{id}/...` if the env var is set, otherwise to the literal `./campaigns/{id}/...`. See `CLAUDE.md → Workspace configuration`.

## Steps

1. Parse arguments:
   - First arg: beat ID (required, e.g., `b03`)
   - Second arg: variant letter (optional, defaults to `a`)

2. Resolve the active campaign from `campaigns/*/manifest.json`.

3. Read the beat file `campaigns/{id}/beats/{beat-id}.md`. If missing, stop and ask user to run `/shot-list` first.

4. Pre-pass check — does this beat need a keyframe?
   - If the beat file specifies Reference-Based mode, the keyframe at `campaigns/{id}/beats/{beat-id}/keyframe.png` must exist
   - If missing, generate it first using Soul 2.0 or Nano Banana Pro via Higgsfield MCP (consult `skills/soul-character/SKILL.md` for character keyframes)

5. Load the **DP** agent context (`agents/dp.md`) and the Seedance skill (`skills/seedance-prompt/SKILL.md`).

6. Extract the MCSLA prompt from the beat file. Verify all 8 sections present.

7. Call Higgsfield MCP with:
   - Model: `seedance-2.0` (or whatever the beat file specifies)
   - Mode: from beat file
   - Prompt: the full MCSLA prompt
   - Image references: keyframe + Soul ID sheet (if applicable)
   - Audio inputs: any reference clips in the beat file
   - Aspect ratio: from beat file's `[SHOT STRUCTURE]`

8. Handle the async job:
   - Higgsfield returns a job handle
   - Poll periodically (typically 30s–3min depending on duration and model)
   - When complete, download the render

9. Apply tagging per `playbooks/tagging-schema.md`:
   ```
   {campaign-id}_{beat-id}_master_{aspect}_{variant}-{lang}_seedance2_v{prompt-v}.mp4
   ```

10. Save to `campaigns/{id}/outputs/{tagged-filename}.mp4`.

11. Append to the beat file's `## Render log`:
    ```
    | Attempt {n} | {variant-description} | {OK/needs-iteration} | {notes} |
    ```

12. Update `campaigns/{id}/outputs/manifest.json` with the new asset entry.

13. Show the user the result inline (a frame preview if possible) and ask:
    ```
    Beat {beat-id} variant {letter} rendered.
    File: outputs/{tag}.mp4
    Duration: {n}s | Model: seedance-2.0 | Mode: {mode}

    Next: review the render.
    - If approved: mark `status: approved` in beat file, ready for /adapt
    - If iterate: run /generate {beat-id} {next-letter} with one variable changed
    - If face-filter rejection: see skills/seedance-prompt → workaround tiers
    ```

## Face-filter rejection handling

If Higgsfield rejects the keyframe pre-flight:

1. Log the rejection in the beat file
2. Apply Tier 1 workaround (grid overlay) — see `skills/soul-character/SKILL.md` and `skills/seedance-prompt/SKILL.md`
3. Retry
4. If still rejected, escalate per the tiered workaround

## Iteration discipline

Don't ship the first render. Default attempt budget per beat:
- Hero beat: 3-5 attempts
- Hub beat: 2-3 attempts
- B-roll: 1-2 attempts

Change ONE variable per attempt. Track in the render log so the retro can attribute what worked.
