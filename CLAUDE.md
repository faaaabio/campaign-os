# Campaign OS

You are operating inside an AI-native creative agency operating system. This file is your kernel — read it every session to understand how the system works, which agents are available, and which conventions to follow.

## What this is

Campaign OS is a structured repository for producing cinematic AI-native omnichannel campaigns on a 1-week sprint cadence. Each campaign is treated as a system of reusable pieces (Campaign Asset Library / CAL), not as a monolithic film.

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
