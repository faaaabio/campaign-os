# Tagging Schema

Every asset produced by this OS — every render, every variant, every adaptation — is tagged. The tag is the join key that lets the Analyst attribute performance back to creative variables. Without it, the retro is blind.

## The full schema

```
{campaign-id}_{beat-id}_{channel}_{format}_{variant}-{lang}_{model}_v{prompt-version}.{ext}
```

Example:
```
nike-q3-launch_b03_tiktok_9x16_a-pt_seedance2-hf_v1.2.mp4
```

Decomposes to:

| Field | Value | Notes |
|---|---|---|
| `campaign-id` | `nike-q3-launch` | kebab-case, matches folder under `campaigns/` |
| `beat-id` | `b03` | matches beat file under `beats/` |
| `channel` | `tiktok` | see channel list below |
| `format` | `9x16` | aspect ratio with `x` instead of `:` |
| `variant` | `a` | letter for A/B variants (a, b, c, ...) |
| `lang` | `pt` | ISO 639-1 code (pt, en, es, zh, ...) |
| `model` | `seedance2-hf` | model that produced it (suffix `-hf` if HyperFrames-wrapped) |
| `prompt-version` | `1.2` | the DP's prompt version at time of render |
| `ext` | `mp4` | extension |

## Channel codes

| Code | Channel |
|---|---|
| `tiktok` | TikTok |
| `reels` | Instagram Reels |
| `yt-shorts` | YouTube Shorts |
| `yt-instream` | YouTube in-stream |
| `meta-feed` | Meta Feed (Facebook + Instagram) |
| `meta-story` | Meta Story |
| `twitter` | X / Twitter |
| `linkedin` | LinkedIn |
| `ooh` | Out-of-home digital |
| `web-hero` | Website hero |
| `email` | Email banner |
| `master` | The Seedance master, pre-adaptation |
| `hygiene` | Always-on hygiene variants |

If a new channel emerges, add the code to this table and update `agents/motion.md`.

## Format codes

| Code | Aspect |
|---|---|
| `9x16` | 9:16 vertical |
| `1x1` | 1:1 square |
| `4x5` | 4:5 social |
| `16x9` | 16:9 horizontal |
| `21x9` | 21:9 ultrawide |
| `var` | variable (responsive web) |

## Model codes

| Code | Model |
|---|---|
| `seedance2` | Seedance 2.0 (default for narrative video) |
| `soul2` | Soul 2.0 (character keyframes) |
| `nano-banana` | Nano Banana Pro (product hero) |
| `veo3` | Veo 3.1 |
| `kling3` | Kling 3.0 |
| `sora2` | Sora 2 |
| `flux2` | Flux 2 |
| `seedream` | Seedream |

Suffix `-hf` indicates the asset was wrapped by HyperFrames after generation. Example: `seedance2-hf` means Seedance master + HyperFrames adaptation.

## Variant letter convention

- `a` = base variant (the default approved master)
- `b`, `c`, `d`, ... = A/B/C variants with one variable changed
- `hygiene` = always-on hygiene variant (replaces letter)

When the Channel Strategist requests A/B testing, the DP renders `a` and `b` of the same beat with one variable difference (lighting, lens, action timing). The Analyst attributes the win to that variable.

## Prompt version convention

- `v{n}` = major prompt version (matches `prompts/{agent}/v{n}.md`)
- `v{n}.{m}` = minor revision within a major version (m = 0 default, increment for mid-sprint tweaks)

Example: if the DP's prompt is `prompts/dp/v3.md` and they made a small adjustment mid-sprint, renders made under the adjustment are tagged `v3.1`.

The Analyst uses this to attribute performance to specific prompt versions in the retro.

## UTM parameter mapping

When uploading to ad platforms, the tag becomes the `utm_content` parameter:

```
utm_campaign=nike-q3-launch
utm_content=nike-q3-launch_b03_tiktok_9x16_a-pt_seedance2-hf_v1.2
utm_medium=paid-social
utm_source=tiktok
```

This is what makes the Meta Ads MCP join the performance data back to specific creative tags.

## Manifest entry

Every tagged file has a corresponding entry in `campaigns/{id}/outputs/manifest.json`:

```json
{
  "assets": [
    {
      "tag": "nike-q3-launch_b03_tiktok_9x16_a-pt_seedance2-hf_v1.2",
      "file": "outputs/nike-q3-launch_b03_tiktok_9x16_a-pt_seedance2-hf_v1.2.mp4",
      "campaign_id": "nike-q3-launch",
      "beat_id": "b03",
      "channel": "tiktok",
      "format": "9x16",
      "variant": "a",
      "lang": "pt",
      "model": "seedance-2.0",
      "hyperframes_wrapped": true,
      "prompt_version": "v1.2",
      "duration_seconds": 15,
      "rendered_at": "2026-05-21T14:33:00Z",
      "approved_by": ["art-director", "channel-strategist"],
      "shipped_at": "2026-05-22T12:00:00Z",
      "status": "shipped"
    }
  ]
}
```

The Analyst reads this manifest and joins it with Meta Ads performance via `utm_content`.

## Common mistakes

1. **Using spaces or colons.** Always underscores and `x` for separators.
2. **Forgetting the `-hf` suffix** when HyperFrames wrapped the asset. The Analyst can't separate diffusion-layer from structural-layer performance without it.
3. **Reusing variant letters across beats.** Variant `a` of beat `b01` is unrelated to variant `a` of beat `b02`. The full tag is what's unique.
4. **Bumping prompt version mid-render.** If you're rendering variants of one beat, freeze the prompt version. Bumping versions mid-batch makes attribution noisy.
5. **Renaming files after the fact.** Tags are immutable. If something needs to change, render a new file with the corrected tag.

## Validation script

To verify all outputs match the schema, run:

```bash
ls campaigns/{id}/outputs/*.mp4 | while read f; do
  basename "$f" | grep -qE '^[a-z0-9-]+_b[0-9]+_[a-z-]+_[0-9]+x[0-9]+_[a-z]+-[a-z]+_[a-z0-9-]+_v[0-9]+(\.[0-9]+)?\.mp4$' \
    || echo "MALFORMED: $f"
done
```

Add this to the D5 pre-launch QA pass.
