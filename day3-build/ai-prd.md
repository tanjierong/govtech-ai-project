# AI PRD: passport application vetter agent

**Scenario:** B · ICA passport applications and renewals

Drawn from `../SCENARIO-PICK.md`, `../day2-validate/living-prd.md`, `../day2-validate/handoff-note.md`, `CLAUDE.md` and `agent-control-panel.md`. All data is synthetic and labelled as made up. The permission settings were reviewed and decided by the product owner on 19 Nov 2026; remaining calls are in Open decisions.

## Problem and user

The user is a passport applicant in the online channel — someone who applies perhaps once every 10 years, most confused when applying for a child. 17% of submissions (made-up figure) are returned for fixable problems, adding ~6 days each, across 520,000 applications a year. Generic guidance fails: only 38% read it.

The agent serves that applicant at the pre-submission moment: it checks the draft against the published checklist and says exactly what would get it returned — before an officer ever sees it. Final decisions stay with ICA officers; the vendor window is 10 weeks.

Secondary users: the facilitator running the fix-through test (reads the run numbers), and the operations team (owns the checklist the agent mirrors).

## Instructions

The agent's full instructions live in `CLAUDE.md`, which is the single source; this section summarises, and if the two differ, `CLAUDE.md` wins.

One draft in, one VET RESULT out: blocking issues, warnings, not-sure items, checked-and-fine (only items actually checked this run), and from-your-records (trusted, not checked, labelled as such). Every flag cites its rule ID and checklist version and ends with one exact fix. Below confidence, it says "I am not sure — an officer will confirm." It never predicts outcomes, never blocks submission, and stops for a human on anything not written down.

## Tools

Three, and no others:

1. **Read the rule file** (`../day2-validate/context-skill.md`) — the only rule source.
2. **Read the current draft application** — the one being checked.
3. **Write one VET RESULT** — the only output.

No web, no other services, no messaging, no submission, no editing the draft, no reading other applicants' data. A missing tool or file is a stop, not a workaround.

## Context and memory

- Rules come only from the rule file, which mirrors the operations team's published checklist (made-up version: checklist-v3). Not in the file = cannot be flagged.
- Field provenance travels with the draft: verified prefilled fields (unedited) are never flagged and are reported as trusted-not-checked; applicant-typed or edited fields are checked in full.
- The agent is stateless: no memory between checks, no reference to any previous applicant, draft or result. Each run starts clean.
- Nothing from a draft is retained after the run ends.

## Permissions

Every action the agent takes, with its tier and a proposed setting. Tiers: **read** (looks at something), **draft** (produces text a human reviews), **write** (changes stored state), **send** (leaves the system toward a person or service). Settings: **auto** (runs without asking), **confirm** (a named human approves each time), **block** (the agent may never do it).

| # | Action | Tier | Setting | Status |
|---|---|---|---|---|
| 1 | Read the rule file (`context-skill.md`) | read | auto | Decided |
| 2 | Read the current draft application | read | auto | Decided |
| 3 | Classify items (blocking / warning / not sure / fine / trusted) | draft | auto | Decided |
| 4 | Produce the VET RESULT shown to the applicant | draft | auto | Decided |
| 5 | Suggest a fix on a flag | draft | auto | Decided |
| 6 | Mark an item "I am not sure — an officer will confirm" | draft | auto | Decided |
| 7 | Write the run log entry (run ID, counts, rules cited — no personal data) | write | auto | Decided |
| 8 | Escalate to the team (missing table, suspected real data, unreadable rule file) | send | auto | Decided |
| 9 | Apply a suggested fix to the draft | write | block | Decided |
| 10 | Submit or block the application | send | block | Decided |
| 11 | Edit the rule file | write | block | Decided |
| 12 | Retain or transmit draft data beyond the run | send | block | Decided |
| 13 | Predict or imply the application outcome | draft | block | Decided |
| 14 | Contact the applicant outside the VET RESULT | send | block | Decided |
| 15 | Adopt a new checklist version into the rule file | write | confirm | Decided — operations confirm |
| 16 | Go in front of testers after an eval run | send | confirm | Decided — operations confirm |
| 17 | Cover a new situation (e.g. adult renewal) once its table exists | write | confirm | Decided — operations confirm |

Rows 1–8 are the agent's whole working loop and run without asking; during the fix-through test the VET RESULT reaches testers directly, so the measurement reflects real behaviour. Rows 9–14 are the lines the handoff note says must not move — blocked outright. Rows 15–17 are the changes that alter what the agent *is*; the operations team is the named confirmer for all three (decided 19 Nov 2026; the role is a made-up course stand-in, like all names here).

## Human checkpoints

- **Every check:** the applicant decides what to fix and whether to submit ("Submit anyway" always works); an ICA officer decides the application. Two humans by design.
- **Before testers:** a human runs the five-question eval in the rule file against its answer key; any failure means no testers (permission row 16).
- **On every checklist change:** operations publish; a human updates the rule file (row 15) within the named lag (1 working day, made up); the eval re-runs.
- **On every dispute:** a human reviews the flag; the agent never argues.
- **On the kill switch:** the team, not the agent, decides rework-or-stop when fix-through lands below the 60% pass bar.

## Stop conditions

The agent stops itself and hands to a human when:

- The applicant's situation has no table in the rule file (only child renewal exists today).
- The draft appears to contain real personal data rather than labelled made-up personas — refuse, process nothing, repeat nothing.
- The rule file is missing, unreadable, or of undeterminable version.
- Any check would require guessing an unwritten rule.

Experiment level: the kill switch from `../day2-validate/build-log.md` — testers see results and make no change; fix-through below 60%.

## Logs

Per check, synthetic data only, no personal data (run IDs only):

- Run ID, date, checklist version.
- Situation detected; counts per section (blocking / warnings / not sure / checked-fine / trusted-not-checked).
- Each flag with its rule ID — so misses and false flags trace to a rule.
- The applicant's response: fixed before submission or not, submitted via "Check again" or "Submit anyway" — the fix-through inputs.
- Any stop condition hit, and why.

These feed the Day 4 metrics: fix-through rate (success signal), vetter–officer agreement, miss rate against the published stop line, false-flag disputes.

## Business rules

From the prompt chain, the handoff note and the stakeholder mitigations — the behaviour that must survive any rebuild:

1. The check never blocks submission; "Submit anyway" always works.
2. Officers decide; the agent never predicts or implies approval, and the officers-decide note appears on every result.
3. Unedited verified fields are never flagged; an edited field loses verified status for that run, permanently.
4. Nothing unchecked is presented as checked; trusted items are labelled trusted.
5. Failures fall back, never dead-end: plain language, no error codes, the applicant can always proceed.
6. A retried prefill asks before replacing anything the applicant typed.
7. Every flag cites rule ID and checklist version; unwritten means "I am not sure."
8. The checklist belongs to operations; the agent and product team never author rules.
9. Synthetic data only until the governance items (consent, retention, audit) are resolved.

## What engineering decides

Yours to hand over whole — the PRD constrains behaviour, not implementation:

- Stack, architecture and hosting; how the one-file prototype's behaviours are rebuilt (the handoff note says rebuild, not extend).
- Which model runs the agent, context strategy, and how `CLAUDE.md` + the rule file are supplied to it.
- How field provenance is represented and passed with the draft.
- Telemetry plumbing for the log schema above, and where logs live.
- How the eval harness runs the five questions and compares to the answer key.
- Latency, availability and cost targets within the 10-week vendor window.
- Test coverage, CI, rollback — all the engineering basics the prototype deliberately lacks.

## Open decisions

Settled on 19 Nov 2026: the Permissions table (all 17 rows decided — working loop auto, hard lines blocked, identity changes confirmed by operations). Testers see the VET RESULT directly during the fix-through test.

Still open — each needs an owner and a date:

1. **The handoff decision stands?** The handoff note says keep prototyping until fix-through ≥60% plus the paper funnel; confirm or revise before any build spend.
2. **Vetter–officer agreement threshold** — the director discussed 95%+; the exact number and its eval set are unset.
3. **The miss-rate stop line** — named in principle, no number on it yet.
4. **Sponsor's "patch vs cure" objection** — still open in `../day2-validate/stakeholder-pushback.md`; the roadmap positioning (vetter as step one toward data-pulling) needs their sign-off.
5. **Governance scope for real data** — Singpass/MyInfo consent, retention and audit requirements before any non-synthetic run.
6. **Eval pass bar for widening** — all five questions today; what the bar becomes when new situations add questions.
