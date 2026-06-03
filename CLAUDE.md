# Campaign OS

You are operating inside an AI-native creative agency operating system. This file is your kernel — read it every session to understand how the system works, which agents are available, and which conventions to follow.

## What this is

Campaign OS is a structured repository for producing cinematic AI-native omnichannel campaigns on a 1-week sprint cadence. Each campaign is treated as a system of reusable pieces (Campaign Asset Library / CAL), not as a monolithic film.

## Workspace configuration

The framework (this repo) holds agents, skills, playbooks, and the `_template`. Actual client campaigns can live anywhere — typically in a private per-client repo.

- **`CAMPAIGN_OS_CAMPAIGNS_DIR`** (env var) — absolute path to where new campaigns should be scaffolded. If set, every `campaigns/<id>/` path in this OS resolves to `$CAMPAIGN_OS_CAMPAIGNS_DIR/<id>/`. If unset, paths resolve to `./campaigns/<id>/` (in-repo, the framework default).

When a command needs to read or write a campaign, follow this resolution order:
1. If `CAMPAIGN_OS_CAMPAIGNS_DIR` is set and the campaign exists there → use it.
2. Otherwise, fall back to `./campaigns/<id>/`.

The template is always sourced from this repo's `./campaigns/_template/` regardless of where the new campaign is scaffolded.

Production stack:
- **Higgsfield MCP** — cinematic image/video generation (Seedance 2.0 is the primary model for narrative shots, Soul 2.0 for character consistency, Nano Banana Pro for product hero stills)
- **HyperFrames** — deterministic HTML-to-MP4 rendering for branded wrappers, channel adaptations, and structured/data-driven video
- **Meta Ads MCP** — campaign performance data
- **Google Drive MCP** — client deliverables and asset sync

## The 7 agents

Each agent lives in `agents/{name}.md` as a system prompt. Load the relevant agent prompt as context when invoking it via slash command or natural language.

| Agent | Role | Invoked by |
|---|---|---|
| **showrunner** | Owns the campaign narrative + story bible. Writes AI-native scripts where AI is part of the meaning, not just production. | `/brief` |
| **dp** | Director of Photography. Converts story beats into MCSLA-formatted Seedance prompts. | `/shot-list`, `/generate` |
| **motion** | Editorial + HyperFrames specialist. Builds wrappers, transitions, channel adaptations at scale. | `/adapt` |
| **cgi** | 3D/CGI specialist for product viz, environments, anything diffusion still mishandles. | direct mention |
| **art-director** | Visual system: KV, type, color, logo behavior. Guards visual coherence across the CAL. | direct mention |
| **channel-strategist** | Maps story to channels using Hero/Hub/Help/Hygiene. Owns cadence and sequencing. | `/sprint-start` |
| **analyst** | Pulls performance data via Meta Ads MCP, attributes results to prompt versions, runs retro. | `/measure`, `/retro` |

## Skills

Skills are loaded on demand. View `skills/{name}/SKILL.md` when the trigger applies.

- `seedance-prompt` — MCSLA formula, prompt modes (Reference-Based, Continuation, Expand, Edit, Transformation), audio sync rules
- `soul-character` — character consistency via Soul 2.0 (face filter workarounds, Soul ID sheets)
- `hyperframes-template` — HTML composition templates per channel
- `story-bible` — story bible structure + AI-native narrative checklist
- `mcsla-formula` — concrete examples of Model · Camera · Subject · Look · Action
- `retro-doc` — D7 retro template + prompt versioning rules
- `brand-safety-check` — pre-launch checklist (face clearance, IP, claims, tone)

## Slash commands

Commands live in `.claude/commands/`. They wrap workflow steps in single invocations.

| Command | What it does |
|---|---|
| `/sprint-start [campaign-id]` | Scaffolds `campaigns/{id}/` from `_template/`, opens brief.md, invokes channel-strategist |
| `/brief` | Showrunner reads brief.md + ingests references, produces story-bible.md and character cards |
| `/shot-list` | DP reads story-bible.md, converts beats into Seedance prompts, writes to `beats/` |
| `/generate [beat-id]` | DP invokes Higgsfield MCP with the beat's prompt, saves render to `outputs/`, applies tagging |
| `/adapt [beat-id] --channels=tiktok,reels,yt-shorts,...` | Motion runs HyperFrames templates to produce channel variants |
| `/measure` | Analyst connects Meta MCP, cross-references asset tags with performance, writes to `measurement/reports/` |
| `/retro` | Analyst generates retro doc; each agent updates its prompt to `prompts/{agent}/v{n+1}.md` |

## Sprint cadence (1 week — aggressive)

```
D1  Brief intake + Showrunner story bible + channel-strategist plan
D2  DP shot list + AD visual system + first keyframes (Soul 2.0)
D3  Hero beats generated (Seedance 2.0)
D4  QA + brand-safety-check + channel adaptations (HyperFrames)
D5  LAUNCH (tracking instrumented from minute 0)
D6  In-flight optimization (cut/scale variants via /measure)
D7  Retro → prompt versions updated → commit
```

## CAL (Campaign Asset Library) structure

Every campaign lives at `campaigns/{id}/` with this layout:

```
{campaign-id}/
├── manifest.json     # metadata (client, dates, prompts versions used)
├── brief.md          # client brief
├── story-bible.md    # Showrunner output
├── channel-plan.md   # channel-strategist output
├── world/            # ambient refs, lighting, color script
├── characters/       # Soul ID sheets, voice samples
├── beats/            # numbered scene prompts + keyframes
├── product/          # 3D/hero shots
├── wrapper/          # HyperFrames templates for this campaign
├── brand/            # logo behavior, type, paleta for this campaign
├── outputs/          # final renders, tagged
└── retro.md          # D7 retrospective
```

Read `playbooks/methodology.md` for the full CAL specification.

## Tagging convention (mandatory)

Every output file MUST be named according to this pattern:

```
{campaign-id}_{beat-id}_{channel}_{format}_{variant}_{model}_v{prompt-version}.mp4
```

Example: `nike-q3-launch_b03_tiktok_9x16_a_seedance2_v1.2.mp4`

Tag metadata also lives in `outputs/manifest.json` per campaign for analytics joins.

## No burned-in text or logos (non-negotiable)

Diffusion models (Seedance, Soul 2.0, Nano Banana Pro — any Higgsfield model) **never render the final text or brand elements**. They produce clean plates only. Every glyph and every mark is composited in **HyperFrames**, deterministically, from the locked design system.

- **No baked-in text.** No headlines, CTAs, captions/subtitles, lower-thirds, price tags, legal lines, or any reading text inside a model-generated frame. Diffusion text is unreliable, unversioned, and not localizable.
- **No baked-in logos.** No brand mark, wordmark, lockup, or product logo painted by the model — not in the hero, not on the product, not as background signage.
- **All typography and brand elements come from HyperFrames**, pulling every value (font, weight, size, color, logo file, clearspace, safe area, motion) from `campaigns/{id}/brand/visual-system.md`. Same inputs → same output → on-brand every time. Never hardcode brand values in a wrapper — reference the design system so a single change re-renders everything.
- **Generation prompts request clean plates**: leave deliberate negative space where copy/logo will be composited, and carry `no text, no logos, no watermark, no UI, no signage copy` in NEGATIVES. If a beat calls for "a sign that reads X" or an on-product logo, render it blank and hand the element to Motion.
- **The gate enforces it.** `brand-safety-check` is a HARD FAIL for any output with model-rendered text or a logo — the asset returns to DP for a clean re-render.

Why: text and logos are the brand's contract — they must be exact, legible, localizable, and versioned, which only the structural layer (HTML/CSS over the design system) guarantees. The diffusion layer owns cinematic imagery; HyperFrames owns every glyph and mark.

## Brand safety gate (non-negotiable)

No asset ships without `brand-safety-check` passing. The gate runs at D4 before adaptations begin. See `skills/brand-safety-check/SKILL.md`. If risk is flagged, the asset returns to DP for regeneration.

## Versioning rules

- **Agent prompts**: every retro produces `prompts/{agent}/v{n+1}.md`. Old versions stay. The current pointer is the latest `v{n}.md`.
- **Campaigns**: each campaign is a folder. Use git branches per campaign for parallel work (`campaign/{id}`).
- **Assets**: tracked via git-lfs (see `.gitattributes`).

## When in doubt

- Read `playbooks/methodology.md` for the 6 AI-native methods + CAL spec
- Read `playbooks/sprint-1w.md` for day-by-day operations
- Read `playbooks/ai-native-principles.md` for the narrative philosophy

This OS is a living system. The retro at D7 is not optional — it's how the OS learns.
