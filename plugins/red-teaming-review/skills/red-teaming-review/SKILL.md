---
name: red-teaming-review
description: Runs a two-round adversarial review of any output — plans, designs, specs, architecture decisions, feature proposals — attacking from six lenses and synthesizing the strongest version across all rounds for the user to approve. Use after finishing a significant, hard-to-reverse deliverable, or when the user says "red team this", "attack my plan", or "find weaknesses" before acting on it.
---

# Red-Teaming Review

## Overview

Two rounds of adversarial attack on any output, followed by a comparative synthesis that keeps the strongest elements from every version. The skill drives the attack and proposes the synthesis; the user gives final approval.

If the output doesn't exist yet, create it first, then start the review.

## Step 0 — Frame the target

Before attacking, classify the target in a line or two. This focuses the attack and stops irrelevant lenses from firing:

- **Type** — plan, design, spec, architecture decision, code, prose, other. Determines which lenses bite.
- **Stakes** — what's the real downside if this ships wrong?
- **Reversibility** — one-way door (hard to undo) or two-way door (cheap to reverse)?
- **Mode** — Full or Light, chosen from stakes × reversibility.

High stakes + irreversible → **Full**. Low stakes + reversible → **Light**, or skip the review and just try it.

## Review modes

- **Full (default):** Two rounds of attack, comparative synthesis, user approval. For anything that will be acted on and is hard to reverse.
- **Light:** Single round — attack, revise, present. For medium-stakes outputs where a quick sanity check is enough.

## Process

1. **Frame** the target — type, stakes, reversibility, mode (Step 0).
2. **Round 1 — Attack** from all six lenses; severity- and confidence-tag every finding.
3. **Round 1 — Revise**: give every finding a disposition (Fix / Accept / Defer); persist deferrals. Nothing dropped silently.
4. **Light mode?** If yes, skip to step 7.
5. **Round 2 — Comparative attack**: a fresh persona attacks the original and revised versions side-by-side.
6. **Synthesize** the strongest version of each aspect across all versions.
7. **User checkpoint** — present with an explicit approve / reject prompt.
   - Approved → done.
   - Rejected (1st–2nd time) → incorporate feedback, restart at Round 1.
   - Rejected (3rd time) → stop iterating on details; reopen the fundamental approach.

## Round 1 — Attack

Attack from **all six lenses**. If a lens genuinely doesn't apply, say why in one line — don't skip it silently.

**Ground the attack against the project's real constraints, not generic best practice.** Before firing the lenses, read `CLAUDE.md`, the relevant specs/ADRs, and any stated requirements. An attack that ignores the actual technical contract is noise — cite the constraint each finding tests against.

- **Correctness** — Factually wrong, logically inconsistent, or built on a bad assumption?
- **Completeness** — Missing steps, edge cases, or scenarios?
- **Feasibility** — Buildable within the stated constraints? What's underestimated?
- **Risk** — What breaks at runtime, in production, or under adversarial conditions?
- **UX/DX impact** — Does it make life harder for users or developers?
- **Operational complexity** — Deployable, monitorable, debuggable, maintainable?

**Tag every finding with severity and confidence:**

- **Severity** — *Critical* (blocks success / serious harm) · *Major* (fix before proceeding) · *Minor* (fix if time allows).
- **Confidence** — *High* (this will break) · *Medium* (likely under some conditions) · *Low* (a scenario I can't rule out).

Confidence keeps speculative objections from blocking progress: a Low-confidence Minor is a footnote; a High-confidence Critical is a gate. Don't let a paranoid hypothetical wear the same weight as a certain breakage.

If Round 1 finds zero issues, say so explicitly and skip to the user checkpoint.

## Round 1 — Revise

For each finding, do ONE of:

1. **Fix it** — change the output.
2. **Accept the risk** — state why and what the trade-off is.
3. **Defer it** — move to a later phase with a concrete trigger for when to revisit.

No finding may be silently dropped. For 5+ findings, track them in a table:

| Finding | Severity | Confidence | Action | Detail |
|---------|----------|------------|--------|--------|
| [description] | Critical/Major/Minor | High/Med/Low | Fixed / Accepted / Deferred | [what changed, why accepted, or revisit trigger] |

For fewer findings, address them inline.

**Persist every deferral.** Sessions don't carry memory between runs, so a deferred finding that lives only in this transcript is a finding you've dropped. Append each Deferred item — and any Accepted risk worth revisiting — to a durable file (`docs/technical-debt.md`; create it if absent) with its severity, the revisit trigger, and today's date.

## Round 2 — Comparative attack

A model attacking its own work twice shares the same blind spots across both rounds. **Open Round 2 in a different reviewer persona** — and where the harness allows, route it to a different model — to break that correlation. Name the persona you're adopting (e.g. a skeptical SRE, a security reviewer, a cost-conscious staff engineer) and attack from there.

Attack the **original AND revised versions side-by-side** — not another list of fresh complaints.

For each significant aspect:

> **[Aspect]**: Original said X. Revised said Y. **Verdict**: which is stronger and why — or *neither*, here's what's actually right.

For outputs without discrete aspects, compare holistically: what did the revision gain, what did it lose, what's the net?

**Must also answer:**

- Did the revision introduce new problems?
- Did the revision lose strengths the original had?
- Where did the revision genuinely improve?

**NEVER open Round 2 with praise. Attack first.**

## Synthesis

Pick the strongest version of each aspect across ALL versions — original, revised, and ideas surfaced during the attacks. This is NOT "tidy up the revised version." For any non-obvious choice, note why that version won.

Look hard for where the original beat the revision — a clean sweep is rarer than it feels. But if the revision genuinely dominated on every axis, say so plainly; **don't manufacture a regression** just to prove the original had merit.

## User checkpoint

Present the synthesized result with an explicit **approve / reject** prompt. After 2 rejections, stop iterating on details and discuss whether the fundamental approach needs to change.

## Red flags — you're doing it wrong

- **Praising before attacking.** Delete it. Attack first.
- **Attacking in a vacuum.** Ground findings in `CLAUDE.md` and the real specs, not generic best practice.
- **Same persona both rounds.** You're running the same review twice and will miss the same things twice.
- **Findings without severity and confidence.** You haven't triaged — a paranoid hypothetical and a certain breakage aren't equal.
- **Deferrals that live only in the transcript.** If it isn't written to `docs/technical-debt.md`, it's dropped.
- **Synthesis = revised with minor edits.** Look hard for where the original was stronger — but don't fake a regression if the revision genuinely won.
- **Steamrolling the user checkpoint.** The user MUST approve.
- **Punting Majors/Criticals to "Future Work."** Deferral is for Minors.
- **"Good enough" under time pressure.** Not a license for shallow work.
- **Full review for everything.** Use Light mode for medium-stakes outputs.
- **Skipping a lens silently.** Say why it doesn't apply.
