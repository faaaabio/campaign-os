# Campaign OS

AI-native creative agency operating system. Runs on Claude Code via terminal. Produces cinematic omnichannel campaigns in 1-week sprints using Higgsfield (Seedance 2.0, Soul 2.0) + HyperFrames + Meta Ads + Google Drive via MCP.

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

That's it. The OS scaffolds the campaign and the agents take over.

## What's in the box

- 7 agents (Showrunner, DP, Motion, CGI, Art Director, Channel Strategist, Analyst)
- 7 skills (Seedance prompting, Soul character, HyperFrames templates, story bible, MCSLA, retro, brand safety)
- 7 slash commands wrapping the 1-week sprint
- Methodology playbooks
- Campaign Asset Library template
- Measurement scaffolding

## Required setup

### 1. MCP servers

Configure API keys in your shell environment before booting Claude Code:

```bash
export HIGGSFIELD_API_KEY="hf-..."
export META_ADS_ACCESS_TOKEN="EAA..."
export GOOGLE_DRIVE_OAUTH_TOKEN="ya29..."
```

The `.mcp.json` in this repo will pick them up.

### 2. Git LFS

Asset binaries (mp4, png, jpg, mov, wav) are tracked via Git LFS. See `.gitattributes`. Run `git lfs install` once per machine.

### 3. HyperFrames

```bash
npm install -g hyperframes
```

Test it: `npx hyperframes --version`

## Sprint workflow

| Day | Command | Owner |
|---|---|---|
| D1 | `/sprint-start` + `/brief` | Channel Strategist + Showrunner |
| D2 | `/shot-list` | DP |
| D3 | `/generate` (per beat) | DP |
| D4 | `/adapt --channels=...` + brand safety | Motion + AD |
| D5 | Launch (manual) | — |
| D6 | `/measure` | Analyst |
| D7 | `/retro` | Analyst + all agents |

## Documentation

- `CLAUDE.md` — the kernel (read this if you want to understand the system)
- `playbooks/methodology.md` — the 6 AI-native methods + CAL spec
- `playbooks/sprint-1w.md` — day-by-day operations
- `playbooks/ai-native-principles.md` — narrative philosophy
- `playbooks/tagging-schema.md` — file naming and metadata
- `playbooks/brand-safety-gate.md` — pre-launch checklist

## License

MIT. See `LICENSE`.
