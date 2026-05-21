# Campaign OS

AI-native creative agency operating system. Runs on [Claude Code](https://claude.com/claude-code) via terminal. Produces cinematic omnichannel campaigns in 1-week sprints using Higgsfield (Seedance 2.0, Soul 2.0) + [HyperFrames](https://www.npmjs.com/package/hyperframes) + Meta Ads + Google Drive via MCP.

> Think of it as a creative studio that fits in a repo: agents that hold the brief, skills that hold the craft, slash commands that move the sprint forward, and a Campaign Asset Library (CAL) layout that keeps every render traceable from prompt → channel cut → performance.

## Quick start

```bash
git clone https://github.com/faaaabio/campaign-os.git
cd campaign-os
cp .env.example .env                     # then fill in your API keys
git lfs install                          # required for asset versioning
npm install -g hyperframes               # video renderer
claude                                   # boots Claude Code with this OS
```

Inside Claude Code, the kernel (`CLAUDE.md`) loads automatically. Then:

```
/sprint-start cliente-x-launch-q3
```

That's it. The OS scaffolds `campaigns/cliente-x-launch-q3/` from `_template/`, opens the brief, and hands off to the agents.

## What's in the box

- **7 agents** in `agents/` — Showrunner, DP, Motion, CGI, Art Director, Channel Strategist, Analyst
- **7 skills** in `skills/` — Seedance prompting (MCSLA), Soul character consistency, HyperFrames templates, story bible, retro doc, brand safety, MCSLA examples
- **7 slash commands** in `.claude/commands/` — `/sprint-start`, `/brief`, `/shot-list`, `/generate`, `/adapt`, `/measure`, `/retro`
- **Methodology playbooks** in `playbooks/` — sprint mechanics, AI-native principles, tagging schema, brand safety gate
- **Campaign Asset Library template** in `campaigns/_template/`
- **Versioned agent prompts** in `prompts/{agent}/v{n}.md` — the retro at D7 forks the next version
- **Measurement scaffolding** in `measurement/`

## Repository layout

```
.
├── CLAUDE.md                  # the kernel — auto-loaded by Claude Code
├── .mcp.json                  # Higgsfield + Meta Ads + Google Drive MCP wiring
├── .claude/
│   ├── commands/              # slash commands (the sprint verbs)
│   └── settings.json          # permissions + model pin (claude-opus-4-7)
├── agents/                    # 7 agent system prompts
├── skills/                    # 7 SKILL.md crafts (loaded on demand)
├── prompts/{agent}/v{n}.md    # versioned agent prompts (retro bumps the n)
├── playbooks/                 # methodology, sprint, principles, tagging, safety
├── campaigns/
│   └── _template/             # CAL skeleton copied on /sprint-start
└── measurement/               # dashboards + reports
```

## The CAL `_template`

`/sprint-start <id>` copies `campaigns/_template/` to `campaigns/<id>/`. The skeleton:

```
_template/
├── manifest.json    # campaign id, sprint id, status timestamps, prompt versions used, KPIs
├── brief.md         # filled in by the account lead, ingested by the Showrunner
├── world/           # ambient refs, lighting, color script (drop refs in _inbox/)
├── characters/      # Soul ID sheets, voice samples (drop refs in _inbox/)
├── beats/           # numbered scene prompts + keyframes (DP writes these)
├── product/         # 3D/hero shots; reference photos in refs/
├── wrapper/         # HyperFrames templates for this campaign's branded wrappers
├── brand/           # logo behavior, type, paleta (drop guidelines in _inbox/)
└── outputs/         # final renders + outputs/manifest.json for analytics joins
```

`brief.md` is structured — sections for client, audience, product truth, mandatories, channels, KPIs, references uploaded, and open questions for the client. The Showrunner reads it whole on D1.

`manifest.json` tracks status timestamps (`started_at`, `story_bible_locked_at`, `visual_system_locked_at`, `launched_at`, `retro_at`), the exact agent prompt versions used in this sprint, channels, languages, and KPI targets. This is what `/measure` joins against ad performance.

## Sprint workflow

| Day | Command | Owner | What happens |
|---|---|---|---|
| D1 | `/sprint-start` + `/brief` | Channel Strategist + Showrunner | CAL scaffolded; brief ingested; story bible drafted; channel plan written |
| D2 | `/shot-list` | DP | Story beats → MCSLA-formatted Seedance prompts; first keyframes via Soul 2.0 |
| D3 | `/generate` (per beat) | DP | Hero beats generated via Higgsfield MCP; outputs tagged and stored |
| D4 | `/adapt --channels=...` | Motion + Art Director | HyperFrames produces channel variants; brand-safety-check runs (non-negotiable gate) |
| D5 | Launch (manual) | — | Instrumented from minute 0 |
| D6 | `/measure` | Analyst | Meta Ads MCP joined to asset tags; in-flight cut/scale |
| D7 | `/retro` | Analyst + all agents | Retro doc; each agent forks `prompts/{agent}/v{n+1}.md` |

## Tagging convention (mandatory)

Every output file is named:

```
{campaign-id}_{beat-id}_{channel}_{format}_{variant}_{model}_v{prompt-version}.mp4
```

Example: `nike-q3-launch_b03_tiktok_9x16_a_seedance2_v1.2.mp4`

This is what makes `/measure` work — performance data joins back to the exact prompt version that produced the winning cut.

## Required setup

### 1. MCP servers

Copy `.env.example` to `.env` and fill in:

```bash
HIGGSFIELD_API_KEY=          # or OAuth on first connect
META_ADS_ACCESS_TOKEN=       # Meta Business app, long-lived token
GOOGLE_DRIVE_OAUTH_TOKEN=    # OAuth for the workspace you sync to
```

The `.mcp.json` in this repo picks these up.

### 2. Git LFS

Asset binaries (mp4, mov, webm, png, jpg, wav, mp3, psd, ai, fbx, glb) are tracked via Git LFS — see `.gitattributes`. Run `git lfs install` once per machine.

### 3. HyperFrames

```bash
npm install -g hyperframes
npx hyperframes --version
```

## Privacy: real campaigns are git-ignored

`.gitignore` excludes everything under `campaigns/` except `_template/`. Real client work stays local by default. If you want to publish a sample campaign, allowlist it explicitly:

```gitignore
campaigns/*
!campaigns/_template/
!campaigns/my-public-demo/
```

## Documentation

- `CLAUDE.md` — the kernel (read this to understand the system)
- `playbooks/methodology.md` — the 6 AI-native methods + CAL spec
- `playbooks/sprint-1w.md` — day-by-day operations
- `playbooks/ai-native-principles.md` — narrative philosophy
- `playbooks/tagging-schema.md` — file naming and metadata
- `playbooks/brand-safety-gate.md` — pre-launch checklist

## License

MIT. See `LICENSE`.
