# Sprint Cadence: 1 Week

The aggressive sprint. Brief in, retro out, in 7 days. Built for agencies operating at high tempo where the campaign volume per quarter is what matters.

## Day-by-day

### D1 (Monday) — Intake & narrative lock

**09:00** Sprint kickoff
- Client brief uploaded to `campaigns/{id}/brief.md`
- Run `/sprint-start {campaign-id}`
- Channel Strategist produces initial channel-plan.md skeleton

**10:00 – 16:00** Showrunner D1 block
- Run `/brief`
- Showrunner produces story-bible.md
- AI-native justification reviewed by senior creative
- Character cards drafted
- World.md sketched
- DP handoff appended

**16:00** Quality gate
- AI-native justification: PASS / push back to client
- Big Idea: one sentence confirmed
- Character Want ≠ Need: confirmed
- Transformation mode used in at least one beat: confirmed

**17:00** Lock the bible. Tag commit: `git tag {sprint-id}-bible-locked`

If the bible can't lock by EOD D1, the sprint is at risk. Add a half-day buffer or reduce scope (drop hub pieces).

---

### D2 (Tuesday) — Visual system + shot list

**09:00 – 12:00** Art Director D2 block
- Reads story-bible.md (Story World, Tone, Characters)
- Produces `brand/visual-system.md`
- Generates initial palette, type, logo behavior
- Specifies camera language for DP (default lens, lighting, color script)

**09:00 – 14:00** Channel Strategist D2 block (in parallel)
- Locks `channel-plan.md` with full beat × channel × format × language matrix
- Briefs Motion on required wrapper templates

**13:00 – 18:00** DP D2 block
- Run `/shot-list`
- DP produces beat files in `beats/` with MCSLA prompts
- Soul 2.0 keyframes generated for hero beats and characters
- Soul ID hashes registered in character cards

**18:00** D2 checkpoint
- Visual system locked
- Channel plan locked
- Shot list complete (all beats have MCSLA prompts)
- Keyframes for hero beats generated and approved

---

### D3 (Wednesday) — Hero generation

**09:00 – 18:00** DP execution block
- Run `/generate` per hero beat (typically 1-3 hero beats)
- 3-5 attempts per beat with one variable changed
- Face-filter rejections handled per skill workaround
- Continuation mode for chained shots
- Soul ID maintained across all character appearances

**16:00** Mid-sprint check-in
- Hero beats: green / yellow / red status
- Higgsfield credit burn rate vs budget
- Any face-filter or quality blockers escalated

If a hero beat can't land by EOD D3, the sprint may need to drop or compress. Have a "good enough" version ready as fallback.

---

### D4 (Thursday) — Hub/help + adaptation + safety

**09:00 – 13:00** DP block (continued)
- Hub and help beats generated
- Lower attempt budget than hero (1-3 per beat)
- Adopt Continuation/Edit Shot modes for efficiency

**11:00 – 14:00** Brand safety gate
- Art Director + Channel Strategist run `brand-safety-check` on all approved masters
- Sign-off document created at `brand-safety/{date}.md`
- Any flag returns to DP for re-render or to Showrunner for rewording

**14:00 – 19:00** Motion adaptation block
- Wrapper templates approved by AD
- Run `/adapt` per approved beat with full channel list
- Batch render across channel × format × language matrix
- Manifest.json populated with all variants

**19:00** D4 checkpoint
- All masters: approved + brand-safety-signed-off
- All adaptations: rendered and tagged
- Ready for launch tomorrow

---

### D5 (Friday) — Launch

**08:00** Final QA pass
- Spot-check ~20% of adaptations at random
- Verify tags match `playbooks/tagging-schema.md`
- Verify captions reviewed by native speakers (NOT just AI translation)
- Verify all UTM parameters present

**09:00** Upload + scheduling
- Push to Google Drive deliverable folder (via MCP)
- Upload to ad platforms with utm_content matching tags
- Schedule per channel-plan.md release calendar

**12:00 BRT** Launch (or per channel plan timing)
- Hero films go live on owned channels
- Paid social rotation starts
- OOH (if applicable) ships to vendors
- Update manifest.json with `launched_at` timestamp

**Afternoon** Monitoring setup
- Verify pixels firing
- Verify Meta Ads dashboard showing creative-level data
- Channel Strategist on call for in-flight escalations

---

### D6 (Saturday) — In-flight optimization

**Morning** First measurement pass
- Run `/measure`
- Analyst reads 18-24h of data
- Diagnostic report at `measurement/reports/{sprint-id}-d6.md`

**Afternoon** Channel Strategist decisions
- Scale top performers (move spend)
- Kill underperformers
- Iterate on variants with specific tweaks
- Update channel-plan.md with `## In-flight Updates` section

**EOD** Iteration spec
- If new variants needed, brief the Motion agent
- New variants typically use Edit Shot (cheap) not full re-render

---

### D7 (Sunday) — Retro

**Morning** Final data pull
- Run `/retro`
- Analyst produces `measurement/reports/{sprint-id}-final.md`
- Cross-reference with D6 diagnostics

**Afternoon** Retro doc
- Analyst produces `campaigns/{id}/retro.md`
- Three best / three worst with hypotheses
- By-agent performance breakdown
- A/B prompt test results (if any)

**EOD** Prompt updates
- Each agent reads their retro section
- Each agent authors `prompts/{agent}/v{n+1}.md`
- Commit:
  ```bash
  git commit -m "retro({sprint-id}): prompt versions updated"
  ```
- Campaign manifest status → "closed"

The OS is now updated. Next sprint starts Monday with v{n+1} of every agent prompt.

---

## Risk and buffers

The 1-week sprint has no slack. If anything slips by half a day, scope must compress. Default compression order:

1. Drop hygiene variants first (lowest creative value, can be added next sprint)
2. Drop one hub piece (preserve the hero)
3. Reduce language variants (ship primary language only, swap later)
4. As a last resort: extend the sprint to 8-9 days and rename it as a learning point in the retro

Never compress:
- The retro (the OS stops learning)
- Brand safety (legal/reputational cost)
- Hero beat quality (the hero IS the campaign)

## Calendar template

```
Mon  D1  ████░░░░  Bible
Tue  D2  ████░░░░  Visual + Shot list
Wed  D3  ██████░░  Hero generation
Thu  D4  ████████  Hub + adapt + safety
Fri  D5  ██████░░  Launch
Sat  D6  ████░░░░  Measure + iterate
Sun  D7  ████░░░░  Retro + prompt updates
```

Block these days as deep-work focus. The sprint doesn't survive open-ended meetings.
