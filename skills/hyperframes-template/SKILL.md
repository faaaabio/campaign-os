---
name: hyperframes-template
description: Build HyperFrames HTML video compositions for channel adaptations, branded wrappers, lower-thirds, end cards, and language swaps. Always use this skill when the Motion agent needs to wrap a Seedance master with brand elements, when producing channel variants (TikTok, Reels, YT Shorts, OOH, web) of an approved master video, when adding captions or localization, or when batching hygiene-tier always-on content. HyperFrames is deterministic — same inputs always produce the same output — so the template here is the artifact that scales.
---

# HyperFrames Templates

HyperFrames renders MP4 from HTML deterministically via headless Chrome + FFmpeg. This skill provides composition patterns the Motion agent reuses across campaigns.

## When to use HyperFrames vs. Seedance

| Need | Tool |
|---|---|
| Cinematic photoreal video | Seedance (via Higgsfield MCP) |
| Branded wrapper (intro, end card, lower-third) | HyperFrames |
| Caption burn-in | HyperFrames |
| Aspect-ratio adaptation of approved master | HyperFrames |
| Language swap (text only) | HyperFrames |
| Data-driven hygiene content (price changes, dates) | HyperFrames |
| Logo reveal, motion graphics | HyperFrames |

HyperFrames produces video that looks like well-crafted web content — polished but not cinematic. Use it to wrap and adapt Seedance masters, not to replace them.

## Project structure

For each campaign, create at `campaigns/{id}/wrapper/`:

```
wrapper/
├── package.json
├── hyperframes.config.json
├── shared/
│   ├── tokens.css        # campaign color/type tokens from brand/
│   ├── logo.svg
│   └── transitions.css
├── tiktok.html           # one template per channel
├── reels.html
├── yt-shorts.html
├── yt-instream.html
├── meta-feed.html
├── ooh-digital.html
└── web-hero.html
```

## Base HTML template

Every channel template follows this scaffold (TikTok example, 9:16, 15s):

```html
<!DOCTYPE html>
<html lang="pt-br" data-duration="15" data-fps="30" data-resolution="1080x1920">
<head>
  <meta charset="UTF-8">
  <title>{{campaign_id}}_{{beat_id}}_tiktok</title>
  <link rel="stylesheet" href="shared/tokens.css">
  <style>
    :root {
      --primary: {{color_primary}};
      --accent: {{color_accent}};
      --neutral: {{color_neutral}};
      --duration: 15s;
    }
    body { margin: 0; background: var(--primary); overflow: hidden; }
    .stage { width: 1080px; height: 1920px; position: relative; }
    .master {
      position: absolute; inset: 0;
      width: 100%; height: 100%; object-fit: cover;
    }
    .hook {
      position: absolute; bottom: 480px; left: 60px;
      font-family: var(--font-display);
      font-size: 88px; font-weight: 700;
      color: var(--neutral); line-height: 1.05;
      opacity: 0; transform: translateY(20px);
      animation: hook-in 0.4s ease 0.3s forwards;
    }
    .cta {
      position: absolute; bottom: 280px; left: 60px;
      background: var(--accent); color: var(--neutral);
      padding: 24px 48px; border-radius: 999px;
      font-family: var(--font-display); font-size: 48px;
      opacity: 0;
      animation: cta-in 0.3s ease 12s forwards;
    }
    .logo {
      position: absolute; top: 80px; right: 60px;
      width: 200px;
    }
    .caption-track {
      position: absolute; bottom: 120px; left: 60px; right: 60px;
      font-family: var(--font-display); font-size: 56px;
      color: var(--neutral); text-align: center;
      text-shadow: 0 2px 8px rgba(0,0,0,0.6);
    }
    @keyframes hook-in {
      to { opacity: 1; transform: translateY(0); }
    }
    @keyframes cta-in {
      to { opacity: 1; }
    }
  </style>
</head>
<body>
  <div class="stage">
    <video
      class="master"
      src="{{master_video}}"
      data-start="0"
      data-duration="15"
      autoplay muted
    ></video>

    <img class="logo" src="shared/logo.svg" alt="logo">

    <div class="hook" data-start="0.5" data-duration="3.5">
      {{hook_text}}
    </div>

    <div class="caption-track" data-srt="{{captions_srt}}"></div>

    <a class="cta" data-start="12" data-duration="3">
      {{cta_text}}
    </a>
  </div>
</body>
</html>
```

The `data-start` and `data-duration` attributes are HyperFrames timeline markers. The renderer pauses the page, captures frames at the configured FPS, and stitches MP4.

## hyperframes.config.json

```json
{
  "output": {
    "format": "mp4",
    "codec": "h264",
    "crf": 18,
    "audio_codec": "aac",
    "audio_bitrate": "192k"
  },
  "render": {
    "fps": 30,
    "device_scale_factor": 2,
    "wait_for_fonts": true,
    "warmup_frames": 5
  }
}
```

## Channel-specific notes

### TikTok / Reels (9:16, 1080x1920)
- Hook visible by **1.5s** (TikTok algo gate)
- Captions ALWAYS on, large type (≥48px at 1080w)
- CTA only in last 3s, never overlapping hook
- Logo top-right or bottom-corner only — never blocking subject's face

### YT Shorts (9:16, 1080x1920)
- Same as TikTok but hook gate at 2s
- Captions optional (YT auto-captions are decent)
- Can go up to 60s — use Continuation-mode Seedance masters

### YT in-stream (16:9, 1920x1080)
- Three durations: 6s bumper, 15s, 30s
- 6s bumper is its own template — no time for hook+CTA, just the idea
- Captions optional, sound-on default
- End screen with channel/website at 0:25 for 30s variant

### Meta Feed (4:5, 1080x1350)
- 4:5 max screen real-estate beats 1:1 on mobile feed
- Hook in first 2s, sound-off default
- CTA via Meta's native CTA button — don't burn it into video unless required

### OOH digital
- 6-10s loop, **silent**, **no captions**
- Big shapes, single idea, brand asset always visible
- No fades to black (kills loop) — design for seamless loop point
- Test on the actual screen pixel density before shipping

### Web hero
- 10-15s loop, autoplay-friendly (no abrupt motion in first frame — browsers may pause it)
- No audio default, optional muted captions
- Provide poster.jpg fallback (first frame extracted)

## Rendering

From the wrapper directory:

```bash
# Preview locally
npx hyperframes preview tiktok.html

# Render single variant
npx hyperframes render tiktok.html \
  --vars "master_video=../outputs/b03_master_16x9_a_seedance2_v1.mp4" \
  --vars "hook_text=Não tem volta." \
  --vars "cta_text=Saiba mais" \
  --vars "color_primary=#1a1a1a" \
  --vars "color_accent=#ff4d2e" \
  --out ../outputs/b03_tiktok_9x16_a-pt_seedance2-hf_v1.mp4

# Batch render across channels and languages
npx hyperframes batch ./batch-spec.json
```

## batch-spec.json pattern

For producing N variants of one beat across channels × formats × languages:

```json
{
  "beat_id": "b03",
  "master": "../outputs/b03_master_16x9_a_seedance2_v1.mp4",
  "common_vars": {
    "color_primary": "#1a1a1a",
    "color_accent": "#ff4d2e",
    "color_neutral": "#f5f3ee"
  },
  "variants": [
    {
      "template": "tiktok.html",
      "channel": "tiktok",
      "format": "9x16",
      "lang": "pt",
      "vars": { "hook_text": "Não tem volta.", "cta_text": "Saiba mais" }
    },
    {
      "template": "tiktok.html",
      "channel": "tiktok",
      "format": "9x16",
      "lang": "en",
      "vars": { "hook_text": "No going back.", "cta_text": "Learn more" }
    },
    {
      "template": "reels.html",
      "channel": "reels",
      "format": "9x16",
      "lang": "pt",
      "vars": { "hook_text": "Não tem volta.", "cta_text": "Saiba mais" }
    }
  ]
}
```

The batch renderer produces correctly-tagged MP4s in one pass.

## Naming convention

Output files MUST follow `playbooks/tagging-schema.md`:

```
{campaign-id}_{beat-id}_{channel}_{format}_{variant}-{lang}_{model}-hf_v{prompt-version}.mp4
```

The `-hf` suffix on the model field tells the Analyst this is the HyperFrames-wrapped variant of the master.

## Brand safety with HyperFrames

Because HyperFrames is deterministic, brand safety risk is mostly in the master video (handled by `brand-safety-check`). For the wrapper, verify:

- Logo behavior matches `brand/visual-system.md` (size, clearspace, placement)
- Color tokens match `brand/colors.json`
- Type matches `brand/type.css`
- Caption translations have been reviewed by a native speaker (not just Claude)

These checks run before the batch.
