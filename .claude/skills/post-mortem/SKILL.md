---
name: post-mortem
description: Write the canonical engineering record of a fixed bug — root cause, mechanism, fix, validation, and how it slipped through. Engineer-audience, code identifiers welcome. Use after a debug session lands a fix, before closing the ticket. Trigger on /post-mortem, when the user says "write the post-mortem / postmortem / RCA / root cause analysis", "document this fix", "write up the root cause", "close out this bug with a writeup", or hands you a fixed-and-validated bug and asks for the writeup.
---

# Post-mortem

The canonical engineering record of a bug fix. Written **after** debugging lands a real fix, **for** other engineers (and future-you, who will have forgotten everything in 6 months).

## When to invoke

- "/post-mortem"
- "write the post-mortem / postmortem / RCA / root-cause analysis"
- "document this fix" / "write up the root cause" / "close out this bug with a writeup"
- After a debug session has clearly landed a fix, proactively offer to draft one.

## When NOT to use

- **Bug not fixed yet, or fix not validated.** Refuse and tell the user what's missing.
- **Customer-visible outage / incident.** Those need a separate incident report.
- **Trivial fix** (typo, obvious one-liner). The PR description is the record.

## Required inputs — refuse to draft without these

Before writing a single line, confirm all four:

- [ ] **Reliable repro** exists (deterministic or high-rate-flake repro the next person can run).
- [ ] **Root cause is known** (the mechanism is identified, not a hypothesis).
- [ ] **Fix is identified** (PR / commit / branch pointer).
- [ ] **Fix is validated** (the original repro now passes).

## Structure

Use these blocks in this order. **Summary, Root cause, Fix, and Validation are mandatory.**

### 1. Summary _(mandatory)_
One paragraph. What broke, in user/workload terms. What fixed it, in one sentence. PR number, owner.

### 2. Symptom
What was actually observed. Test output, error message, log line, perf number. Concrete identifiers.

### 3. Root cause _(mandatory)_
The actual bug mechanism. **Code identifiers welcome and expected** — function names, file paths, struct fields, branch conditions. Walk the cause chain end-to-end.

### 4. Why it produced the symptom
Link the root cause to the symptom. Walk the chain so a reader can connect it back to the cause without re-deriving it.

### 5. Fix _(mandatory)_
What changed and **why this change addresses the root cause** rather than hiding the symptom. Link to PR / commit.

### 6. How it was found
The debugging path: what repro made it deterministic, what tools cracked it, hypotheses tried and rejected, the single experiment that confirmed the cause.

### 7. Why it slipped through
What allowed this bug to reach the branch / release / customer. Pick the real reason:
- CI gap
- Latent code
- Workload gap
- Incomplete prior fix
- Review miss

**Blameless** — describe the gap, not the person.

### 8. Validation _(mandatory)_
How we know the fix works. Concrete: test names, run links, numbers before/after. State coverage honestly — don't imply broader coverage than you actually have.

### 9. Action items / follow-ups
Concrete next-steps. Each item: what + owner + tracking artifact. If none: *"None — the fix is sufficient."*

## Tone

- **Code identifiers are first-class.** Keep function names, file paths, commit SHAs.
- **Mechanism over narrative.** Walk the actual cause chain.
- **No hedging.** "We believe" / "appears to" — drop. State it or don't write it.
- **Blameless.** Describe the bug, the gap, and the fix. Never "X should have caught this."

## Output flow

1. Confirm all four required inputs are satisfied. If any are missing, list them and stop.
2. Confirm where it goes (default: comment on the source ticket, or `docs/postmortems/<ticket>.md`).
3. Produce the draft as a single block.
4. Get sign-off before posting anywhere.
5. Offer the management summary handoff: *"Want a leadership-flavored version?"*
