# Agent control panel: passport vetter

**Scenario:** B · ICA passport applications and renewals

The one-page operating view of the vetter agent: what it is doing, where it could go next, and who decides what. Companion to `CLAUDE.md` (the agent's own instructions) and `agent-run-log.md` (test runs). All data is synthetic and labelled as made up.

## What the agent is doing

One job: check one draft passport application against the operations team's published checklist (`../day2-validate/context-skill.md`, checklist-v3) and return one VET RESULT — blocking issues, warnings, not-sure items, checked-and-fine, and trusted-from-records, each flag citing its rule and giving one exact fix.

Today it covers one situation (child renewal), runs on made-up data only, and exists as an instruction file plus a rule file — tested in `agent-run-log.md` (completed, escalated and refused correctly across four runs), not yet evaluated formally or put in front of testers.

## Opportunity portfolio

Where the same agent pattern could go, in order of readiness — not a commitment list:

1. **Checklist sync watcher** (workflow): when operations publish a new checklist version, update the rule file, re-run the five-question eval, hold for human sign-off. Keeps every other row honest. Ready to build first.
2. **Vetting the draft before submission** (this agent): the core bet — 17% returns toward 5% across 520k applications a year. Ready to test (eval gate, then the fix-through test with its ≥60% pass bar), not to deploy.
3. **More situations**: adult renewal, first-time, name change — each unlocked only by operations publishing that situation's table into the rule file. The agent escalates on all of them today, by design.
4. **Applicant Q&A before starting** (chat assistant): same rule file, same "I am not sure" behaviour, answering questions the guidance page fails to. Buildable after the vetter proves itself.
5. **Officer-side summary** (agent, heavily fenced): last, whatever the technology allows — it touches the real decision, so it waits for a vetter–officer agreement track record.

## Rules of engagement

**Decides alone (no waiting):**
- Reading the rule file and the current draft.
- Classifying each item as blocking, warning, not sure, checked-and-fine, or trusted-from-records.
- Producing the VET RESULT, rules cited.
- Stopping itself when a stop condition is met, and saying what is missing.

**Proposes and waits (a person must say yes before anything happens):**
- Nothing ships on the agent's say-so. Suggested fixes are proposals to the applicant, who decides what to change and whether to submit — "Submit anyway" always works.
- An escalation ("no table for this situation", "this looks like real data") proposes an action to the team; the agent does not act on it.

**A person reviews (after the fact, on a schedule or a trigger):**
- The five-question eval against the answer key in the rule file — before any tester sees the agent, and again after every checklist sync.
- Every disputed flag — a human reviews it; the agent never argues.
- The run log numbers (fix-through rate, flags per run, stop conditions hit) — reviewed against the metrics plan's stop line.

## Stop conditions

The agent stops itself and hands to a human when:

- The applicant's situation has no table in the rule file (only child renewal exists today).
- The draft appears to contain real personal data instead of labelled made-up personas — it refuses without processing or repeating the data.
- The rule file is missing, unreadable, or its version cannot be determined.
- Any check would require guessing a rule that is not written down.

And the team stops the whole experiment when the kill switch fires: testers see results and make no change — fix-through below the 60% pass bar named in `../day2-validate/build-log.md`.

## What stays human

Permanently, not just for now:

- **The decision.** An ICA officer decides every application. The agent never predicts, promises or implies an outcome.
- **The submission.** The applicant chooses what to fix and whether to submit; the check can never block.
- **The rules.** The operations team writes and versions the checklist. The agent mirrors it and may never edit it.
- **The thresholds.** The fix-through pass bar, the vetter–officer agreement threshold, and the miss-rate stop line are set by people before the measurements run.
- **The go/no-go.** Moving from prototype to production spec, widening to new situations, and switching off after a kill — all human calls, informed by the agent's logs but never made by them.
