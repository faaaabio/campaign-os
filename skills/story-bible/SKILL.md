---
name: story-bible
description: Produce the campaign story bible — the complete narrative document the Showrunner publishes on D1 of every sprint. Always use this skill when invoked via /brief, when the Showrunner is structuring a campaign world, when writing characters/world/beats for a campaign, or when defining the AI-native narrative angle of a campaign. The story bible is the single source of truth all other agents pull from for the rest of the sprint.
---

# Story Bible

The story bible is the Showrunner's deliverable for D1 of every sprint. Everything downstream reads from it: the DP builds shots from it, the Art Director builds the visual system from it, the Channel Strategist builds the channel plan from it.

## Template

Save at `campaigns/{id}/story-bible.md`:

```markdown
# Story Bible: {campaign-id}

> Owner: Showrunner
> Locked: D1 end-of-day
> Version: v1
> Status: [draft | locked | revised-mid-sprint]

## AI-Native Justification

Answer in 100 words or less: **Does this campaign NEED AI to exist?**

If the campaign would be better executed as traditional production with bigger budget, the answer is "no — we're using AI to make traditional advertising cheaper." That's a valid reason to use the tools but not a valid story. Push back on the brief and find the version where AI is part of the meaning.

If yes, name the AI-native element specifically:
- Are we using AI as a metaphor in the story (Reuters)?
- Are we producing assets impossible to film conventionally (mass character variation)?
- Are we letting the audience interact with the story dynamically?
- Are we collapsing a production timeline that the message itself depends on (real-time response to a news cycle)?

This section is not optional. If you can't write it, the campaign isn't ready.

---

## The Big Idea

One sentence. If you can't reduce it to one sentence, the idea isn't ready.

Example:
> "A water company tells the story of clean information by showing what happens when AI distorts what we drink." (Reuters)

---

## Story World

The universe rules. What's true in this campaign's world that isn't true in literal reality?

- **Time/place**: when and where does this take place?
- **Tone**: comedic / dramatic / melancholic / surreal / documentary / fantastical
- **Visual logic**: what does this world look like? (the AD turns this into a visual system)
- **Sonic logic**: what does this world sound like? (the DP and Motion use this for audio)
- **The rule**: name one thing that's true in this world that the audience will recognize as the campaign's signature

---

## Characters

For each character (typically 1-3 per campaign), write:

### {Character name}

- **Want** (what they pursue in the story)
- **Need** (what they actually need — often different)
- **Voice** (1-2 sentence sample of how they speak)
- **Visual key** (the first detail you'd notice — see Soul ID for full sheet)
- **Soul ID reference** → `characters/{name}.md`

Repeat per character.

---

## Hero Film Beat Sheet

The hero film is the cinematic anchor (typically 30-60s, may extend to 90s for category-defining campaigns). Use Save the Cat adapted for advertising:

| # | Beat name | Duration | Description | Seedance mode |
|---|---|---|---|---|
| 1 | Opening image | 0:00–0:03 | Establishes world, tone, central question | Reference-Based |
| 2 | Catalyst | 0:03–0:07 | The change that forces character to act | Continuation |
| 3 | Debate / setup | 0:07–0:15 | Establishes stakes and product role | Continuation |
| 4 | Break into 2 | 0:15–0:20 | Character commits | Transformation |
| 5 | Midpoint | 0:20–0:30 | A reversal or revelation | Reference-Based |
| 6 | All is lost | 0:30–0:40 | The dark night of the brand promise | Continuation |
| 7 | Break into 3 | 0:40–0:50 | The product delivers | Transformation |
| 8 | Final image | 0:50–0:60 | The world transformed | Reference-Based |

Adjust beat count by total duration. For 15s social hero, compress to 4 beats. For 90s, expand to 12.

For each beat write:
- **Visual**: what the audience sees (1-2 sentences)
- **Audio**: dialogue (if any) and ambient texture
- **Function**: what story job this beat does
- **Seedance mode**: suggested mode for the DP

---

## Social Cuts (Hub)

3-5 pieces that live in the same world but stand alone. These aren't trimmed hero films — they're fresh story moments that expand the world.

### Cut 1: {title}
- Length: 15s
- Channel target: TikTok + Reels
- Story job: introduce {character} from their POV
- Visual: ...
- Hook (first 2s): ...
- Payoff: ...

Repeat per cut.

---

## Help Pieces

1-3 utility pieces that serve product or audience need without breaking world.

### Help 1: {title}
- Length: 20-30s
- Channel: YouTube + Meta
- Story job: show how the product works inside the world's logic
- Visual: ...

---

## Tone of Voice

- **Words we use**: 5-10 specific words/phrases that anchor the campaign voice
- **Words we don't use**: 5-10 specific words/phrases to avoid
- **Sample dialogue**: 3-5 lines that demonstrate the voice
- **Cadence**: short and punchy / measured / long-form / conversational

---

## Taglines

Never propose a single tagline. Propose 5-7 variations. The Channel Strategist + AD pick the working set.

1. ...
2. ...
3. ...

---

## DP Handoff

For each beat in the hero film and each social cut, summarize:

```
Beat b01-opener:
  Mode: Reference-Based
  Mood: contemplative, golden hour interior
  Camera intent: slow push-in, 50mm, shallow DOF
  Audio: ambient + dialogue PT-BR
  Character: Maria (see characters/maria.md)
  Soul ID required: yes

Beat b02-catalyst:
  Mode: Transformation
  Mood: shift from interior calm to exterior urgency
  ...
```

The DP reads this and produces the MCSLA prompts in `beats/`.

---

## Channel Strategist Handoff

Pass-through intent for the channel strategist:

- Primary audience: ...
- Critical channels (must-have): ...
- Optional channels: ...
- Language priorities: ...
- Cultural moments to ride or avoid: ...

---

## Open questions

Things you couldn't resolve from the brief that need client input or D2 follow-up. Don't hide these — write them down and surface them at the Monday standup.

```
- [ ] Confirm whether {character} can speak the tagline on-camera or VO-only
- [ ] Approve the AI-native framing with client legal
- [ ] Verify product claim wording for help pieces
```
```

## How to use this template

1. Copy the entire structure into `campaigns/{id}/story-bible.md`
2. Fill every section in order — don't skip ahead
3. Stop and verify the AI-native justification before writing beats
4. Save as draft, share with team, lock at end of D1
5. Any mid-sprint revision bumps the version (v2, v3) and is logged in the retro

## Quality checks before locking

- [ ] AI-native justification answers "yes" with specifics
- [ ] Big Idea fits in one sentence (literally — count it)
- [ ] Every character has Want ≠ Need
- [ ] Every hero beat has Visual + Audio + Function + Mode
- [ ] At least one beat uses Transformation mode (because if no beat does, this is probably just an ad)
- [ ] Tone of Voice has both "use" and "don't use" words
- [ ] At least 5 tagline variations
- [ ] DP handoff is explicit per beat
- [ ] Open questions are written down, not buried
