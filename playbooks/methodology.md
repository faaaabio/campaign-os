# Methodology

This is the operating philosophy of Campaign OS. Everything else in the repo — agents, skills, commands, templates — is downstream of these principles.

## The 6 AI-native methods

The strongest current approach to AI-native cinematic campaigns treats the campaign as a **system of reusable pieces**, not a monolithic film. The methods below are inherited from the industry's leading practice (Google's NotebookLM → Gemini → Whisk → Veo flow; Runway's character + storyboard workflows; OpenAI's video API for image-reference and clip continuation; Adobe Firefly for brand-safe production) and adapted to our specific Higgsfield + HyperFrames stack.

### 1. Concept-to-film prototyping

Generate roteiro, moodboard, and storyboard with AI before any finished piece exists. The Showrunner produces story-bible.md on D1. The DP produces shot list on D2. Both happen before a single Seedance render is paid for.

In our stack:
- Showrunner uses Claude itself for narrative drafting
- Moodboard via Soul 2.0 + Nano Banana Pro (Higgsfield) for hero stills
- Storyboard via Seedance text-to-video at lower resolution for animatic
- Only after the animatic reads do we burn credits on hero generation

### 2. Character / world consistency

Build characters and environments as **reusable assets**, not per-shot. Soul ID is the single source of truth. The world.md and visual-system.md are the second.

In our stack:
- Soul 2.0 produces character sheets registered with Higgsfield Soul ID hash
- Every Seedance shot references the Soul ID image
- Cross-campaign recurring characters live in `/cast/` and are referenced from campaign folders
- The world's lighting/color script is locked at D2 in `brand/visual-system.md`

### 3. Shot modular + AI editorial

Generate short scenes separately, then assemble and adjust. Don't try to ship a single perfect 60-second master.

In our stack:
- Seedance 2.0 caps at 15s per shot. We don't fight this — we use Continuation mode to chain
- Edit Shot mode produces day/night, EN/PT variants without re-rendering everything
- Expand Shot stretches the working clips
- HyperFrames wraps and adapts

### 4. Brand-safe production

Use governance — not opaque models — to keep brand control. Every output passes `brand-safety-check` before launch.

In our stack:
- Higgsfield models are commercial-rights-cleared
- Soul ID prevents accidental celebrity resemblance
- Brand safety gate runs at D4
- Disclosure policies tracked per jurisdiction (EU AI Act, FTC, LGPD)

### 5. Scale of variations

Produce many versions per audience, format, country — not one master. This is where HyperFrames earns its keep.

In our stack:
- The `/adapt` command + batch-spec.json + HyperFrames batch mode produces N variants per master
- A 1-week sprint typically ships 1 hero + 5-10 hub + 2-5 help + 10-30 hygiene variants
- Tag schema (campaign × beat × channel × format × variant × lang × model × prompt-v) is the join key for the Analyst

### 6. AI-native narrative (the hard one)

The campaign is **conceived from the start to be AI-native** — scenes designed to be reused, remixed, and adapted; ideas that AI is part of the meaning of, not just the means.

This is the method that most agencies skip because it requires creative judgment rather than tooling. The Showrunner's AI-native justification check (in `skills/story-bible/SKILL.md`) is the enforcement mechanism.

The Reuters "Pure News" campaign is the canonical example: they didn't use AI to make traditional advertising cheaper — they made AI the metaphor for distorted information, contrasted with their real footage. The campaign couldn't exist without AI being the subject.

If your campaign's AI-native answer is weak, you're doing method 1-5 with method 6 missing — which is technically AI-native but creatively empty.

---

## CAL — Campaign Asset Library

Every campaign is a folder structured as a CAL. This is the artifact that makes methods 2, 3, and 5 actually work.

```
campaigns/{campaign-id}/
├── manifest.json     # metadata (client, dates, prompt versions in flight, status)
├── brief.md          # client input
├── story-bible.md    # Showrunner output (D1)
├── channel-plan.md   # Channel Strategist output (D1-D2)
├── world/            # environmental refs, lighting, color script
│   ├── world.md
│   ├── _inbox/       # uploaded references before processing
│   └── *.png         # generated environment refs
├── characters/       # one .md per character + Soul ID assets
│   ├── _inbox/
│   ├── {name}.md
│   ├── {name}_soul-id-hero.png
│   └── {name}_soul-id-variants/
├── beats/            # numbered scene files (b01.md, b02.md, ...)
│   ├── {beat-id}.md  # MCSLA prompt, render log, status
│   └── {beat-id}/    # per-beat assets (keyframe.png, audio refs)
├── product/          # 3D models, hero stills, technical
│   ├── spec.md
│   ├── refs/
│   └── 3d/
├── wrapper/          # HyperFrames templates for this campaign
│   ├── package.json
│   ├── hyperframes.config.json
│   ├── shared/       # tokens.css, logo.svg
│   └── *.html        # one template per channel
├── brand/            # visual system, colors, type, logo behavior
│   ├── visual-system.md
│   ├── colors.json
│   ├── type.css
│   ├── logo.svg
│   └── _inbox/       # client brand guidelines uploads
├── outputs/          # all rendered MP4s + manifest.json
│   ├── manifest.json # tag → file + metadata + status
│   └── *.mp4         # tagged final files
├── brand-safety/     # sign-off docs per date
│   └── {YYYY-MM-DD}.md
└── retro.md          # D7 retrospective
```

Required properties:
- Every file has a single owner (named in the file or implied by directory)
- Every output is tagged per `playbooks/tagging-schema.md`
- Every manifest entry is updateable but the original tag never changes
- Soul IDs are append-only — never overwrite a character's ID without bumping the version

## The moat

Tooling stack (Higgsfield, Seedance, HyperFrames, MCP) is not a moat. Within 12 months, every competitor will have it.

The moat is what compounds:

1. **A mature CAL** — characters, worlds, prompts trained on YOUR performance history
2. **Showrunners who think AI-native** — scarcity of creative directors who can author for this medium
3. **Loop speed** — how fast retros become prompt updates. The team that iterates 50 sprints in a year beats the team that iterates 12.

Treat the OS as a living system that gets sharper every retro, not a tool you install once.

## Reading order for new team members

1. This file (`playbooks/methodology.md`)
2. `playbooks/ai-native-principles.md`
3. `playbooks/sprint-1w.md`
4. `playbooks/tagging-schema.md`
5. `CLAUDE.md` (the kernel)
6. `agents/showrunner.md` (the spine)
7. The rest as needed
