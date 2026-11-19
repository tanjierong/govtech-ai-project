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

## Permissions

One row per tool the agent uses, with its tier and a proposed setting — yours to decide. Tiers: **read** (looks at something), **draft** (produces text a human reviews), **write** (changes stored state), **send** (leaves the system toward a person or service). Settings: **auto** (runs without asking), **confirm** (a named human approves each time), **block** (never).

This table is per tool; the 17-row per-action table in `ai-prd.md` (decided 19 Nov 2026) is the finer grain. The two must not disagree: each row below notes the PRD rows it carries, and the stress runs (5–10 in `agent-run-log.md`) that shaped it.

| # | Tool | Tier | Setting | Status | What the stress test showed |
|---|---|---|---|---|---|
| 1 | Read the rule file (`context-skill.md`) | read | auto | Proposed | Runs 7/10: an empty read is a stop, never a from-memory fallback — so auto is safe only because the tool's failure is a stop condition, not a retry. Carries PRD row 1. |
| 2 | Read the current draft application | read | auto | Proposed | Runs 5/8: an empty draft is a named stop. Runs 6/9: what the draft *says* never becomes a rule — citations get looked up, not believed. Run 3: a real-data draft is refused at read, unprocessed. Carries PRD row 2. |
| 3 | Write the VET RESULT (the one output) | draft | auto | Proposed | Runs 8/10: the tool is withheld entirely on a stop — no result on an empty draft or an empty rule source, because zero flags would read as a pass. Run 4: checked and trusted stay separate counts. Carries PRD rows 3–6. |
| 4 | Write the run log entry (run ID, counts, rules cited) | write | auto | Proposed | Every run 1–10 logged, including the stops — the log is how misses trace to rules, so it must fire on failures too, without asking. No personal data, run IDs only. Carries PRD row 7. |
| 5 | Escalate to the team (missing table, empty rule file, suspected real data) | send | auto | Proposed | Runs 2, 3, 5, 7, 8, 10 all ended in an escalation or refusal notice. This is the safety valve: putting confirm on it would silence the agent exactly when something is wrong. Carries PRD row 8. |

Everything not in this table, the agent has no tool for: it cannot edit the draft, submit or block an application, change the rule file, message the applicant outside the VET RESULT, or keep anything after a run — those are the PRD's blocked rows (9–14), enforced here by absence. The three confirm-gated changes (new checklist version, in front of testers, new situation — PRD rows 15–17) are changes humans make *to* the agent, not tools the agent holds, so they have no row.

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
