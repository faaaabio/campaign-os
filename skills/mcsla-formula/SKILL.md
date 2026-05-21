---
name: mcsla-formula
description: Concrete MCSLA-formatted prompt examples across common campaign scenarios (product hero, character dialogue, transformation reveal, environmental B-roll, lifestyle, dramatic close-up). Use when the DP needs reference examples to pattern-match a new beat's prompt structure, when teaching a new prompt mode by example, or when comparing two prompt variants for an A/B test. Pair this with skills/seedance-prompt for the base formula.
---

# MCSLA Formula — Worked Examples

The base formula lives in `skills/seedance-prompt/SKILL.md`. This skill is a library of working examples across common scenarios, so the DP has reference patterns to adapt rather than starting from scratch.

## Example 1 — Product hero (Nano Banana Pro → Seedance Reference-Based)

```
[SHOT STRUCTURE]
Shots: 1 / Total: 8s / Aspect: 16:9 / Audio: ambient + sfx / Language: n/a

[MODEL DIRECTIVE]
Seedance 2.0, Reference-Based mode anchored to keyframe,
photoreal commercial product cinematography, ARRI ALEXA aesthetic.

[COMPOSITION]
Medium close on product, eye-level, dark velvet backdrop with subtle texture,
shallow DOF with one specular highlight blooming behind product.

[SUBJECT]
Matte-black ceramic pour-over coffee dripper, hand-thrown texture,
matte finish, brand mark embossed bottom-left.
Reference: product/3d/renders/hero.png

[CAMERA]
Slow orbit 15 degrees left-to-right over full duration,
85mm macro lens, f/2.8, focus locked on rim of dripper,
no breath, no shake.

[ACTION + DIALOGUE]
At 0:00 product is still. At 0:02 steam begins curling from rim.
At 0:05 a single drop of water lands on the lip and slides down.
Audio: low room tone, faint kettle hiss, soft drip at 0:05.

[LOOK]
Color grade: warm bias, soft bloom on specular highlight,
black shadows deep but not crushed.
Lighting: single soft top-key 4500K camera-right at 45 degrees,
black flag camera-left to deepen shadow side.
Film stock reference: Kodak Vision3 500T digital emulation.

[NEGATIVES]
no warping ceramic, no reflection of camera or crew,
no readable text other than brand mark, no extra steam plumes.
```

## Example 2 — Character dialogue with lip-sync

```
[SHOT STRUCTURE]
Shots: 1 / Total: 12s / Aspect: 9:16 / Audio: dialogue + ambient / Language: pt-br

[MODEL DIRECTIVE]
Seedance 2.0, cinematic, photoreal, 35mm film grain,
warm hand-held documentary feel.

[COMPOSITION]
Medium close-up, eye-level, suburban kitchen interior morning,
shallow DOF with bokeh from window practicals behind subject.

[SUBJECT]
Maria, mid-30s Brazilian woman, dark wavy shoulder-length hair,
warm olive skin, oversized cream linen shirt unbuttoned over white tank.
Reference: characters/maria_soul-id.png
Expression: contemplative, slight smile forming around 0:06.

[CAMERA]
Slow push-in from waist to chest over 12s,
50mm lens, f/2.0, focus locked on subject's eyes,
subtle handheld breath in motion.

[ACTION + DIALOGUE]
At 0:00 she stands holding a ceramic mug, looking out of frame left.
At 0:04 she turns to camera, looks down at the mug.
At 0:06 she says (lip-sync PT-BR): "isso muda tudo."
At 0:08 she lifts the mug to take a sip, eyes close briefly.
Audio: morning ambience, distant birds, faint kettle steam.
Voice: warm low alto, measured pace, slight breath.

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

## Example 3 — Transformation reveal (AI-native conceptual cut)

```
[SHOT STRUCTURE]
Shots: 1 / Total: 6s / Aspect: 16:9 / Audio: ambient + score / Language: n/a

[MODEL DIRECTIVE]
Seedance 2.0, Transformation mode, cinematic conceptual cut,
photoreal start state, photoreal end state, parametric morph between.

[COMPOSITION]
Locked medium-wide, eye-level, glass of water on a kitchen counter,
window light from camera-left, soft ambient interior.

[SUBJECT]
Single transparent water glass, half-full, plain.
Reference start: world/water-cloudy.png
Reference end: world/water-clear.png

[CAMERA]
Locked static, 50mm lens, f/4, deep focus on glass.

[ACTION + DIALOGUE]
At 0:00 the water in the glass is cloudy gray-brown, sediment visible.
From 0:01 to 0:05 the cloudiness clears parametrically from top to bottom,
sediment dissolves, water becomes pure transparent.
At 0:05 the water is fully clear, light passing through cleanly.
Audio: low hum at 0:00 transitioning to clean silence + gentle high tone at 0:05.

[LOOK]
Color grade: desaturated and cool at 0:00,
shifting to neutral warm by 0:05.
Lighting source: stays constant — soft north window, no direct sun.
Atmosphere: light haze at start dissipating to clean by end.

[NEGATIVES]
no warping glass, no levitating water, no hand entering frame,
no text or branding visible.
```

## Example 4 — Environmental B-roll (text-to-video, no keyframe)

```
[SHOT STRUCTURE]
Shots: 1 / Total: 8s / Aspect: 16:9 / Audio: ambient only / Language: n/a

[MODEL DIRECTIVE]
Seedance 2.0 text-to-video, cinematic, photoreal documentary,
35mm film grain, naturalistic.

[COMPOSITION]
Wide shot, eye-level from a low vantage point at street level,
São Paulo residential street in early morning, slight haze.

[SUBJECT]
Empty street, sidewalk with potted plants, occasional pedestrian
moving slowly far in background. No featured human in foreground.

[CAMERA]
Slow lateral dolly from right to left over full duration,
35mm lens, f/4, deep focus,
gentle handheld breath.

[ACTION + DIALOGUE]
At 0:00 a single bird flies across frame from right to left at upper third.
Throughout: leaves stir slightly in a soft breeze.
Audio: distant traffic, occasional bird, faint wind.

[LOOK]
Color grade: cool morning blue,
soft golden light just beginning to touch upper buildings.
Lighting source: indirect sun from camera-right just above frame.
Atmosphere: light haze, no heavy fog.
Film stock reference: Fuji Eterna 250D digital emulation.

[NEGATIVES]
no readable signage, no recognizable brand logos on buildings,
no distorted architecture, no pedestrian close enough to identify.
```

## Example 5 — Continuation chained shot

Use Example 2 as the preceding beat. This is the immediate next beat:

```
[SHOT STRUCTURE]
Shots: 1 / Total: 10s / Aspect: 9:16 / Audio: ambient | Language: pt-br

[MODEL DIRECTIVE]
Seedance 2.0, Continuation mode, chained to previous beat.

[CONTINUATION]
Previous beat: b02-character-pov_seedance2_v1.mp4
Connect at: Maria mid-sip with eyes closed, kitchen morning light.
Begin with: her opening eyes slowly.
Maintain: Soul ID, color script, kitchen geography, lighting.

[COMPOSITION]
Same framing as previous beat (medium close-up).

[SUBJECT]
Maria — Soul ID maintained from previous beat.

[CAMERA]
Push-in continues another 5cm closer over first 3s,
then settles static for remainder.

[ACTION + DIALOGUE]
At 0:00 Maria's eyes open. At 0:02 she lowers the mug.
At 0:04 she looks directly into camera, half-smile.
At 0:07 she turns and walks out of frame camera-right.
Audio: continued kitchen ambience, footsteps at 0:08.

[LOOK]
Identical to previous beat — Continuation should preserve grade.

[NEGATIVES]
no character drift from Soul ID, no lighting shift,
no warping faces.
```

## Example 6 — Edit Shot (cheap variant of approved master)

The master is Example 2. We want a night-time variant for paid social rotation.

```
[SHOT STRUCTURE]
Shots: 1 / Total: 12s / Aspect: 9:16 / Audio: dialogue + ambient / Language: pt-br

[MODEL DIRECTIVE]
Seedance 2.0, Edit Shot mode.

[EDIT]
Source: b02-character-pov_seedance2_v1.mp4
Change: time of day from morning to evening,
        lighting source from window day to single warm interior practical,
        ambient from morning birds to evening wind.
Preserve: character Soul ID, wardrobe, dialogue, camera move, framing,
          grade contrast level.
```

That's the entire prompt. Edit Shot is surgical — name only what changes.

## Example 7 — 6-second YouTube bumper (compressed beat sheet)

```
[SHOT STRUCTURE]
Shots: 1 / Total: 6s / Aspect: 16:9 / Audio: dialogue + score / Language: pt-br

[MODEL DIRECTIVE]
Seedance 2.0, cinematic, photoreal, brand-anchored,
single hero shot designed to land idea + brand + product in 6s.

[COMPOSITION]
Medium close-up on product held by character,
eye-level, soft interior, product holds center frame.

[SUBJECT]
Maria's hands holding the matte-black coffee dripper,
warm olive skin, cream sleeve in frame edge.
Reference: characters/maria_hands.png + product/3d/renders/hero.png

[CAMERA]
Static, 50mm, f/2.8, focus locked on product.

[ACTION + DIALOGUE]
At 0:00 product held still.
At 0:02 Maria's voice (off-camera, lip-sync PT-BR): "isso muda tudo."
At 0:04 her thumb traces the rim of the dripper slowly.
Audio: low warm score, gentle ceramic sound at 0:04.

[LOOK]
Warm color grade, soft top window light, deep shadows.
Film stock: Kodak Portra 400.

[NEGATIVES]
no warping product, no logo distortion, no extra fingers,
no readable text beyond product mark.
```

## Reading these examples

When the DP gets a new beat from the Showrunner:
1. Identify the closest example above by intent (product / dialogue / transformation / B-roll / continuation / edit / bumper)
2. Copy the structure
3. Replace the specifics
4. Adjust mode if needed
5. Verify all 8 MCSLA sections are present (even if some say "(intentional default)")

Every working prompt becomes a new example. Periodically add new patterns to this file as they emerge from sprints.
