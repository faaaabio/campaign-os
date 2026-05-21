# Brand Safety Gate

The operational checklist lives in `skills/brand-safety-check/SKILL.md`. This playbook documents the gate's place in the sprint.

## Position in the sprint

The gate runs at D4, after all hero/hub/help masters are rendered and approved, before Motion runs channel adaptations. This timing is intentional:

- **Too early** (D2 or D3): brand safety can't be checked because the assets don't exist yet
- **Too late** (D5 morning): if a flag forces a re-render, the launch slips
- **D4 mid-day**: rendering done, time to fix, adaptations downstream

If brand safety flags after adaptations have been batched, you've wasted the batch. Always gate the masters first.

## Owners

- **Art Director** owns visual artifact issues (warping, logo distortion, wardrobe IP)
- **Channel Strategist** owns platform-rule issues (Meta restrictions, age gating, disclosure)
- **Showrunner** is consulted on claim/dialogue issues
- **DP** is the one who re-renders if a master fails

The sign-off requires both AD and Channel Strategist initials in `campaigns/{id}/brand-safety/{date}.md`.

## The eight checklist sections

Detailed in `skills/brand-safety-check/SKILL.md`:

1. Human likeness
2. IP and trademark
3. Claims and language
4. AI disclosure
5. Sensitive category triggers
6. Cultural sensitivity
7. Visual artifacts
8. Channel-specific platform rules

Every section runs every sprint. Don't skip sections that "obviously don't apply" — the bias is to flag, not to wave through.

## When external review is needed

The internal gate is not a substitute for client legal review on:

- Comparative claims against named competitors
- Health, medical, financial claims
- Political or civic content
- Likenesses of real public figures
- Use of trademarked IP under fair-use rationale

For these, external review happens in parallel with internal gate. The sprint plan accounts for legal turnaround time when these are in scope.

## The rationalization check

The single most dangerous failure mode is talking yourself into "it's probably fine." If you find yourself mentally arguing why something passes, that's the signal to flag it.

Higgsfield credits to re-render: cheap. Take-down notices, lawsuits, brand-trust collapse: not cheap.

Bias is to flag.

## Documentation

Every gate run produces a dated sign-off file. Even when everything passes. The audit trail matters more than the gate itself when a question comes up months later about why a specific decision was made.

See the template at the bottom of `skills/brand-safety-check/SKILL.md`.
