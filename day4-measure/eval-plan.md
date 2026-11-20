# Eval plan: passport vetter agent

**Scenario:** B · ICA passport applications and renewals

Who runs the eval, when it runs, and how we know the whole thing is working. Roles come from `../day3-build/operating-model.md` — job roles, no names, all made-up course stand-ins. The mechanics live in `human-rubric.md` (the runbook) and the current numbers in `eval-results.md`. All data synthetic. **Everything below is Proposed — nothing is decided until the product owner decides it.**

## Who owns what

All Proposed:

- **Who signs the pass bar:** the **product owner** sets and changes the bar's numbers (they decided the current ones on 20 Nov 2026). The **operations lead** signs the gate the bar feeds — nothing goes in front of testers or widens without their confirm (PRD permission row 16). Two signatures, two different questions: "is the bar right?" is product's; "did it clear the bar, so may it proceed?" is operations'.
- **Who writes and refreshes the test cases:** the **product owner** owns `eval-cases.md`. The **operations lead** triggers refreshes — every new checklist version must add or update the cases that touch the changed rules. The **facilitator** contributes one case per revision written *without reading the run log*, so at least one input exists that no rule was written against.
- **Who runs the eval:** the **engineering lead**, following the runbook in `human-rubric.md` steps 1–8 — version stamps, clean-start runs, two graders, append-only results. The facilitator may be the second grader.
- **Who reviews a weekly sample:** the **operations lead** reads a sample of graded answers each week (suggested: every failed or 3-scored answer, plus three passes at random). This is the frontline director's trust condition made routine: the team that owns the checklist sees with their own eyes what the agent tells applicants. Findings go to the product owner as proposed case or wording changes.

## When it runs

All Proposed:

- **Before go-live (before testers, and before any widening):** a full batch — all cases in `eval-cases.md` — plus the five-question eval in the rule file against its answer key, after a fresh calibration. Operations lead confirms on the results; no partial batches, no skipping calibration because "nothing changed".
- **Before every change:** any PR touching `../day3-build/CLAUDE.md` (the agent's instructions) or `../day2-validate/context-skill.md` (the rules) triggers a full batch before the changed agent checks anything for a tester. The 20 Nov wording fix is the pattern: change, rerun affected cases at minimum, full batch before the next gate.
- **Every week:** one scheduled full batch, run by the engineering lead, even if nothing changed — drift is only visible against a schedule. The weekly sample review (above) reads from this run.
- **When policy changes:** operations publish a new checklist → the rule file is updated within the named lag (1 working day, made up) → cases touching the changed rules are refreshed → recalibrate → full batch. The agent does not check against a new checklist version that the eval has not seen.

## How I'll know it's working

All Proposed:

- **The goal:** applications complete first time. The eval exists so the vetter stays trustworthy enough to put in front of people — the project's success signal remains returns falling from 17% toward 10% (first experiment), with fix-through ≥ 60% as the pass bar that keeps the experiment alive.
- **The baseline:** the 20 November 2026 run in `eval-results.md` — 12 of 12 cases pass, 36 of 36 deal-breaker checks "yes", D4 average 5.0, D5 average 5.0 after the wording fix. Returns baseline: 17% (made-up figure).
- **The target and review date:** every weekly batch between now and **20 December 2026** passes the decided bar with zero deal-breaker "no"s, including at least one batch containing a new outsider-written case. Reviewed on 20 December 2026 by the product owner and operations lead together.
- **A usage metric:** checks run per week, split by how they ended — fixed-then-submitted, "Submit anyway", or stopped/escalated. A trustworthy vetter nobody uses is still a failure; this is the number that says people are letting it check them.
- **The pattern to watch first:** deal-breaker *near-misses* after instruction changes — above all, any unchecked item drifting back toward the checked list (the Run 1 failure). Second: grader disagreement rising on D4/D5, which means the anchors are decaying before the agent is.
- **The stop line:** any D1–D3 "no" in any batch halts all movement toward testers until fixed and fully rerun. At experiment level the kill switch stands: fix-through below 60% means stop and rework, and the miss rate (post-vet returns) is published against a stop line whose number the operations lead still has to set.

## First 30 days

All Proposed — 20 November to 20 December 2026:

- **Week 1 — close the calibration loop.** Product owner scores cases 2, 5 and 7 plus the planted bad answer (the sheet is in `eval-results.md`, the planted answer in `human-rubric.md`). Compare with the existing scores, write the agreed examples under the anchors, and move the calibration routine from Proposed to Decided. Owner: product owner + engineering lead.
- **Week 2 — break the circularity.** Facilitator writes the first outsider case without reading the run log; product owner adds it to `eval-cases.md`; engineering lead runs the first scheduled weekly batch including it. Set the grader-agreement threshold properly (the runbook's working rule stands in until then). Owner: facilitator + product owner.
- **Week 3 — clear the gate.** Fresh calibration, five-question eval against the answer key, full batch. If everything passes, the operations lead decides whether the agent goes in front of testers for the fix-through test (3–5 testers, pass bar ≥ 60% named before the test, per the handoff note). Owner: operations lead.
- **Week 4 — review and decide.** On 20 December 2026: review every batch against the target, the usage metric from any tester sessions, and the fix-through numbers if week 3's test ran. Set the miss-rate stop line number. Decide continue, pivot or stop — the one-pager's decision discipline, applied to the eval itself. Owner: product owner, with the operations lead's go and the sponsor told.

Open after this plan: the product owner's calibration scores (week 1's input), the grader-agreement threshold (week 2), the miss-rate number (week 4) — each already named in `human-rubric.md` or the PRD's Open decisions.
