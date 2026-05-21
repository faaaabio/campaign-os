---
name: soul-character
description: Build and maintain character consistency across a campaign using Higgsfield's Soul 2.0 model and Soul ID system. Always use this skill when creating a new campaign character, when generating a character keyframe before a Seedance video shot, when a character appears across multiple beats and must look identical, or when troubleshooting face-filter rejections in Seedance 2.0. The Soul ID sheet is the single source of truth for any character — generate it once and every subsequent shot references it.
---

# Soul ID Character Consistency

This skill produces the Soul ID character sheet used by all downstream shots (Seedance, Kling, Sora) to maintain visual continuity.

## When to invoke

- New character is introduced in story-bible.md
- A character appears in 2+ beats (mandatory consistency)
- A keyframe is needed before a Seedance beat
- Face-filter is rejecting a stylized character in Seedance

## The Soul ID sheet structure

Save at `campaigns/{id}/characters/{character-name}.md`:

```markdown
# Soul ID: {character-name}

## Identity card
- Age range: {28-35}
- Gender presentation: {woman}
- Ethnicity / origin: {Brazilian, Afro-descendant, São Paulo}
- Build: {medium, athletic}
- Distinctive features: {gap in front teeth, small scar above left eyebrow, septum piercing}

## Hair
- Color: {natural black, sun-touched warm tips}
- Texture: {3B curls}
- Length / style: {shoulder-length, often loose, sometimes half-up}

## Wardrobe range
- Default: {oversized cream linen shirt, white tank, vintage jeans}
- Hero film: {same as default}
- B-roll alt: {black turtleneck for night scenes}
- Color palette: warm neutrals, cream, terracotta, indigo

## Voice (for dialogue beats)
- Register: warm low alto
- Pace: measured
- Texture: slight breath
- Reference (if available): voice-sample.wav

## Soul ID image set
Generated via Soul 2.0 at D2. Files:
- soul-id-front.png   (eye-level, neutral expression, 50mm equiv)
- soul-id-three-quarter.png  (3/4 view)
- soul-id-profile.png
- soul-id-hero.png    (the locked KV pose)

## Higgsfield Soul ID
Soul ID hash: {hf-soul-id-abc123}
(returned by Higgsfield MCP when the character is registered)

## Usage rules
- Every Seedance shot featuring this character MUST upload `soul-id-hero.png` as a reference image
- For non-photoreal stylizations (illustration, anime), generate a separate stylized Soul ID set rather than fighting the face filter
```

## Generation workflow

### Step 1 — Define before generating

Don't generate first and rationalize after. Write the identity card fully BEFORE invoking Soul 2.0. The text description is what makes the character feel like a person rather than a stock photo.

### Step 2 — Generate the hero pose

Call Higgsfield MCP with Soul 2.0:

```
[SOUL 2.0 PROMPT]
{full identity card description}
Pose: 3/4 view, neutral expression, looking just off-camera-right
Framing: medium close-up, shoulders included
Lighting: soft north window, no direct sun, low-key warm fill
Lens: 85mm equivalent, f/2.8
Background: out-of-focus warm neutral interior
Output: 4K, photoreal, no stylization
```

Generate 4-6 variations. Pick the one that feels like a person you'd want to follow for 30s of film.

### Step 3 — Generate the variant set

Once the hero pose is locked, use Soul 2.0's character consistency feature to generate:
- Front view
- Profile
- Different wardrobe (for variant beats)
- Different lighting (day/night)

These all become reference inputs for Seedance later.

### Step 4 — Register the Soul ID

Higgsfield's Soul Character tool returns a Soul ID hash when you register a character. Save the hash in the .md file. Future Seedance shots can pass the Soul ID directly instead of re-uploading the image.

## Face-filter triage (Seedance side)

If Seedance rejects the Soul ID image:

**Tier 1 — Grid overlay**

```bash
# Apply grid via ImageMagick locally
convert soul-id-hero.png \
  -gravity center -draw "stroke '#00000040' stroke-width 1 \
  line 0,0 0,1024 line 256,0 256,1024 ..." \
  soul-id-hero-grid.png
```

Use the grid version as the upload.

**Tier 2 — Re-grade more photoreal**

Re-run Soul 2.0 with explicit photoreal cues: "real skin texture, pores visible, natural color, no smoothing."

**Tier 3 — Switch model**

If filter persists on a stylized character, switch downstream model to Kling 3.0 for that beat. Document in the beat file.

## Multi-character scenes

Seedance 2.0 accepts up to 9 image references. For scenes with 2-3 characters, upload all Soul IDs as separate references and label them in the prompt:

```
[SUBJECT]
Character A: {description}, reference image 1
Character B: {description}, reference image 2
Spatial relationship: A camera-left, B camera-right, facing each other
```

For >3 characters, generate group keyframes first via Soul 2.0 (with all characters in frame) and use that as a single Reference-Based input to Seedance.

## Cross-campaign reuse

If a character appears in multiple campaigns (recurring brand mascot, ongoing series), promote their Soul ID to a top-level location:

```
/cast/
  /{character-name}/
    {character-name}.md
    soul-id-hero.png
    soul-id-variants/
```

Campaigns symlink or reference these instead of duplicating. This is the moat from the methodology doc — recurring AI-native cast is a brand asset.
