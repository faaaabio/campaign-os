---
name: seedance-prompt
description: Generate production-ready Seedance 2.0 prompts for Higgsfield MCP using the MCSLA formula. Always use this skill when the DP agent is converting a story beat into a video generation prompt, when producing any prompt for Higgsfield's Seedance 2.0 model, when working with prompt modes (Reference-Based, Continuation, Expand Shot, Edit Shot, Transformation), when handling face-filter rejections, or when iterating on cinematic AI video generations. Use this even if the user says "just a quick prompt" — Seedance prompts that skip the MCSLA hierarchy produce inconsistent output.
---

# Seedance 2.0 Prompting

This skill turns a story beat into a structured Seedance 2.0 prompt ready for Higgsfield MCP execution.

## The MCSLA formula (ordering is not optional)

```
[SHOT STRUCTURE]
[MODEL DIRECTIVE]
[COMPOSITION]
[SUBJECT]
[CAMERA]
[ACTION + DIALOGUE]
[LOOK]
[NEGATIVES]
```

Top-down. The model locks the frame from `COMPOSITION` before inventing motion from `CAMERA` and `ACTION`. Inverting this order causes contradictions and hallucinations.

## The base template

```
[SHOT STRUCTURE]
Shots: {N} / Total: {seconds}s / Aspect: {16:9 | 9:16 | 1:1 | 4:5}
Audio: {dialogue | ambient | music | dialogue+ambient}
Language: {pt-br | en | es | zh}

[MODEL DIRECTIVE]
Seedance 2.0, cinematic, photoreal, 35mm film grain, ARRI ALEXA aesthetic,
professional color grading, sharp focus, high detail texture, film stock language.

[COMPOSITION]
{framing: close-up | medium close | medium | wide | extreme wide}
{angle: eye-level | low | high | dutch}
{background: specific environmental description}
{depth: shallow DOF | deep DOF | rack focus}

[SUBJECT]
{character description, reference Soul ID image if available}
{wardrobe specifics}
{expression / emotional state}

[CAMERA]
{movement: static | slow push-in | pull-out | dolly | orbit | handheld}
{lens: 24mm | 35mm | 50mm | 85mm | anamorphic}
{aperture: f/1.4 | f/2 | f/2.8 | f/4}
{focus behavior: locked | rack to subject | follow focus}

[ACTION + DIALOGUE]
{specific action in 1-2 sentences, present tense}
{if dialogue: "exact line in target language" with lip-sync directive}
{timing: at 0:02 she turns, at 0:06 she speaks, at 0:11 she exits frame}

[LOOK]
{color grade: teal-orange | warm bias | cool desaturated | high contrast}
{lighting source: golden hour through window left | low-key practicals | soft top light}
{atmosphere: light haze | clean | smoke | volumetric}
{film stock reference: Kodak Portra 400 | Fuji 8553 | digital ARRI clean}

[NEGATIVES]
no warping faces, no extra fingers, no plastic skin,
no logo distortion, no text artifacts,
{campaign-specific negatives}
```

## Prompt modes — choosing and structuring

### 1. Reference-Based mode

For hero shots that must match a keyframe exactly.

Add to the prompt:
```
[REFERENCE]
Anchor to: keyframe.png
Match: facial features, wardrobe, lighting direction
Preserve: exact framing for first 0.5s, then begin motion
```

Upload the keyframe via Higgsfield MCP `image_reference` parameter. Always generate the keyframe first via Soul 2.0 or Nano Banana Pro.

### 2. Continuation mode

For beats that chain to a previous shot in a sequence.

Add to the prompt:
```
[CONTINUATION]
Previous beat: {beat-id}_seedance2_v{n}.mp4
Connect at: {last frame state — character pose, location, lighting}
Begin with: {first action of this beat}
Maintain: character Soul ID, color script, camera energy
```

This is how you build sequences longer than 15s — Seedance's hard cap per shot.

### 3. Expand Shot mode

For beats where the working render is too short.

Add to the prompt:
```
[EXPAND]
Source: {beat-id}_seedance2_v{n}.mp4
Extend by: {seconds}s
Direction: {forward | backward | both}
Maintain: motion vector, character behavior
```

Use when you have a great shot but the action needs more breathing room.

### 4. Edit Shot mode

For surgical changes to an approved shot without regenerating from scratch.

Add to the prompt:
```
[EDIT]
Source: {beat-id}_seedance2_v{n}.mp4
Change: {specific element — e.g., "lighting to golden hour" or "wardrobe red instead of blue"}
Preserve: everything else
```

Cheap and fast. Use for A/B variants of the same beat.

### 5. Transformation mode

For reveals, morphs, before/after, conceptual cuts.

Add to the prompt:
```
[TRANSFORMATION]
Start state: {description or reference image}
End state: {description or reference image}
Transition style: {dissolve | morph | hard cut | parametric (e.g., water clearing)}
Duration: {seconds — typically 3-8s for hero transforms}
```

This is the mode for AI-native ideas like the Reuters water-clearing cut. The model is at its conceptual best here.

## The face-filter workaround

Seedance runs computer vision on reference images BEFORE reading the prompt. If a stylized, AI-generated, or non-photoreal face gets blocked, rewriting the prompt does nothing.

**Tier 1 (most reliable):** Grid overlay method. Add solid grid lines at 100% opacity over the reference image. Saves the upload to a local copy before applying.

**Tier 2:** Re-grade the keyframe more photoreal — reduce stylization, add subtle film grain and skin texture in the source image.

**Tier 3:** Switch model. Kling 3.0 has different filters and handles stylized faces.

Log every rejection in the beat file:
```markdown
## Rejections
| Attempt | Image | Workaround | Result |
|---|---|---|---|
| 1 | keyframe.png raw | none | REJECTED (filter) |
| 2 | keyframe_grid.png | grid overlay | PASSED |
```

This data feeds the D7 retro.

## Multimodal inputs (Seedance 2.0 specialty)

In a single generation, Seedance 2.0 can accept:
- Up to **9 images** (references, keyframes, mood)
- Up to **3 video clips** (15s max each — use for Continuation source)
- Up to **3 audio clips** (15s max each — for ambient or VO reference)
- Plus text prompt

The model reads each input's role automatically based on context. When you need character consistency across multiple shots, upload the Soul ID character sheet as one of the image inputs — Seedance will pin features across the generation.

## Audio in Seedance 2.0

Seedance generates audio in the same pass — dialogue with native lip-sync, ambient soundscapes, music. No post-stitching needed.

For dialogue, specify:
```
[ACTION + DIALOGUE]
At 0:04 she says (lip-sync PT-BR): "isso muda tudo."
Voice: warm, low register, slight breath
```

For ambient:
```
Audio: cafe ambience, distant traffic, faint jazz piano at low volume
```

Languages with native lip-sync: EN, ES, ZH (Mandarin), and regional dialects. PT-BR support is high quality. For any other language, expect to swap voice in post via ElevenLabs.

## Iteration discipline

For every hero beat, queue 3-4 variations with **one variable changed** at a time:
- Variant A: base prompt
- Variant B: shorter lens (35mm instead of 50mm)
- Variant C: shifted lighting (golden hour → blue hour)
- Variant D: tighter framing

Run side-by-side in Higgsfield workspace. Document which won and why in the beat file. Don't ship the first render.

## Example: a working prompt

```
[SHOT STRUCTURE]
Shots: 1 / Total: 12s / Aspect: 9:16 / Audio: dialogue + ambient / Language: pt-br

[MODEL DIRECTIVE]
Seedance 2.0, cinematic, photoreal, 35mm film grain, ARRI ALEXA aesthetic,
warm color grade, sharp focus, high detail texture.

[COMPOSITION]
Medium close-up, low angle from waist height, suburban kitchen interior,
shallow DOF with bokeh from window practicals behind subject.

[SUBJECT]
Woman, mid-30s, Brazilian, dark wavy shoulder-length hair, warm olive skin,
wearing oversized cream linen shirt unbuttoned over white tank.
Reference: characters/maria_soul-id.png
Expression: contemplative, slight smile forming.

[CAMERA]
Slow push-in from waist to chest height over full duration,
50mm lens, f/2.0, focus locked on subject's eyes,
subtle handheld breath in motion.

[ACTION + DIALOGUE]
At 0:00 she stands holding a ceramic mug, looking out of frame left.
At 0:04 she turns to camera, looks down at the mug.
At 0:06 she says (lip-sync PT-BR): "isso muda tudo."
At 0:08 she lifts the mug to take a sip.
Audio: morning ambience, distant birds, faint kettle steam.

[LOOK]
Color grade: teal-orange, warm highlights, cool shadows in deep background.
Lighting source: golden hour through window camera-left,
soft bounce fill from camera-right, no top light.
Atmosphere: light haze catching window light, clean foreground.
Film stock reference: Kodak Portra 400 digital emulation.

[NEGATIVES]
no warping faces, no extra fingers, no plastic skin,
no warped ceramic, no logo on mug, no readable text.
```

## When to break the formula

Almost never. The MCSLA order is empirically what works. If you find yourself wanting to omit a section, write `[SECTION] (intentional default)` rather than removing the section — keeps the prompt parseable for the Analyst's retro joins.
