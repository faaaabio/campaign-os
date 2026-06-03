---
name: brand-safety-check
description: Run the pre-launch brand safety review on every campaign output before it ships. Always use this skill at D4 before Motion runs channel adaptations, when any AI-generated asset features a human face, when a campaign claim could be challenged legally, when generative AI is used in a sensitive category (financial, health, news, politics, kids), or when a master video is being approved for production. This gate is non-negotiable — no asset ships without it passing.
---

# Brand Safety Check

The brand safety gate sits between D4 QA and D5 launch. The Art Director and Channel Strategist co-sign. If any item flags, the asset returns upstream — usually to DP for regeneration, sometimes to Showrunner for rewording.

## When to invoke

- D4 of every sprint, before Motion runs channel adaptations
- Whenever a new beat is generated that wasn't in the original story bible
- Whenever a client asks "can we ship this?" and the answer isn't obvious
- After any prompt-version update affecting visual output

## The checklist

Run each section. Document pass/flag for each item in `campaigns/{id}/brand-safety/{date}.md`.

### Section 1 — Human likeness

- [ ] No real public figure resembled without explicit permission (face, hair, signature pose)
- [ ] No real private individual identifiable (even from generated likenesses — if it looks like someone specific, treat it as their likeness)
- [ ] If a Soul ID character resembles any real person within plausible deniability, flag and check with client legal
- [ ] No children depicted in adult-target advertising
- [ ] If children are in scope: parental consent provenance for any reference images used

### Section 2 — IP and trademark

- [ ] No third-party brand logos visible (apparel, products, signage in background)
- [ ] No copyrighted characters (cartoons, mascots, film characters)
- [ ] No music sample matches any commercial track (if Seedance-generated audio, verify)
- [ ] No architectural likeness of trademarked buildings without rights
- [ ] No artwork likeness of identifiable contemporary artists

### Section 3 — Claims and language

- [ ] Every product claim ("clinically tested", "fastest", "first") has supporting documentation
- [ ] No comparative claims against named competitors without legal review
- [ ] No health/financial claims that exceed approved language
- [ ] No superlatives that would require footnoting
- [ ] If dialogue: read the line out loud — does it commit the brand to something it can't deliver?

### Section 4 — AI disclosure

- [ ] If the campaign uses AI as a creative tool, decide whether to disclose (many regions are moving toward mandatory disclosure for AI-generated content)
- [ ] If the campaign claims something is "real footage" — verify it actually is, not Seedance output
- [ ] If the AI-native angle hinges on the audience knowing it's AI (Reuters case), the disclosure is part of the meaning, not a footnote
- [ ] Country-specific rules:
  - EU: AI Act disclosure for synthetic media in certain contexts
  - US: FTC guidance on AI-generated endorsements
  - Brazil: LGPD impact for AI-generated likenesses of real people
  - Add your jurisdictions here as patterns emerge

### Section 5 — Sensitive category triggers

If the campaign sits in any of these categories, run additional review:

- [ ] **Health / medical**: claim wording reviewed by regulated-content specialist
- [ ] **Financial**: required disclosures present; no past-performance-implies-future-results phrasing
- [ ] **Political / civic**: no impersonation of officials; transparent funder identification
- [ ] **Alcohol / tobacco / gambling**: age gating, no on-camera consumption in some regions
- [ ] **Children (under 13)**: enhanced privacy, parental consent, no behavioral targeting
- [ ] **News / journalism**: extra care if AI imagery could be mistaken for actual reporting

### Section 6 — Cultural sensitivity

- [ ] No accidental stereotypes (review with a regional partner if shipping internationally)
- [ ] No religious imagery used incidentally as set dressing
- [ ] No flags or national symbols used in commercial association without intent
- [ ] Translations reviewed by a native speaker, not just Claude

### Section 7 — Visual artifacts

- [ ] No warped faces visible in final 24fps motion
- [ ] No extra/missing fingers visible at 1x playback speed
- [ ] No flickering between frames (Seedance occasionally produces this)
- [ ] No logo distortions on the brand's own product

### Section 7b — No burned-in text or logos (hard fail)

See `CLAUDE.md` → "No burned-in text or logos". Diffusion output is clean plates; all text/logos are HyperFrames layers from `brand/visual-system.md`.

- [ ] **No model-rendered (burned-in) text anywhere in the frame** — headlines, CTAs, captions, lower-thirds, price/legal copy, or signage text must be composited in HyperFrames, never baked by Seedance/Soul/Nano Banana
- [ ] **No model-rendered logo or brand mark** — on the product, on signage, or in the background. Logos are HyperFrames layers pulled from the design system, never painted by the model
- [ ] Any on-screen copy in the final deliverable comes from the wrapper, reads correctly, and matches the locked type system
- [ ] Wrapper templates reference `visual-system.md` tokens (no hardcoded brand values)

### Section 8 — Channel-specific platform rules

- [ ] Meta: no before/after weight loss imagery, no clickbait phrases ("doctors hate this")
- [ ] TikTok: no medical claims without disclaimers, no political ads in regions where prohibited
- [ ] YouTube: complies with kids-content opt-out if applicable, age-rating accurate
- [ ] OOH: meets local guidelines on size, brightness, and content type

## Decision matrix

After running the checklist:

| Result | Action |
|---|---|
| All items pass | Sign-off, proceed to Motion adaptations |
| Burned-in text or logo in a render | HARD FAIL — return to DP for a clean plate; Motion composites the text/logo via HyperFrames from `visual-system.md` |
| 1-2 minor flags (artifact-level) | Return to DP for re-render of affected beats |
| Claims/IP/likeness flag | Return to Showrunner for rewording; legal review if needed |
| Sensitive category flag without specialist sign-off | HALT — escalate to senior creative + legal |
| Cultural sensitivity flag | Add native-speaker review; possibly re-cast or re-translate |

## Sign-off file

Every sprint produces `campaigns/{id}/brand-safety/{date}.md`:

```markdown
# Brand Safety Sign-off: {campaign-id}

> Date: {YYYY-MM-DD}
> Reviewer: Art Director + Channel Strategist
> Status: [pass | conditional-pass | hold]

## Assets reviewed
- {beat-id-1} ({tag})
- {beat-id-2} ({tag})
- ...

## Flags raised
- [ ] {flag} → {resolution}

## Sign-off
- [ ] Art Director: {name/initials} — {date}
- [ ] Channel Strategist: {name/initials} — {date}
- [ ] Client legal (if applicable): {name} — {date}

## Notes for the Analyst (D7 retro)
{anything to track for future sprints}
```

## The "rationalization" check

The most dangerous failure mode of brand safety is rationalization — talking yourself into "it's probably fine." If you find yourself mentally arguing for why something passes, that's the signal to flag it, not to ship it.

When in doubt, the answer is "regenerate." Higgsfield credits cost less than a take-down notice.
