# Operating model: passport vetter agent

**Scenario:** B · ICA passport applications and renewals

Who decides what for the vetter agent, by role — no names. Drawn from `../one-pager.md` (constraints and stakeholder mitigations), `agent-control-panel.md` (rules of engagement) and `ai-prd.md` (permissions, decided 19 Nov 2026). All roles are made-up course stand-ins, consistent with the synthetic-data rule.

## Roles

- **Product owner** — owns the problem, the one-pager and the PRD; runs the experiment; accountable for the kill switch being honoured.
- **Operations lead** — heads the frontline team that owns the published checklist; the named confirmer for the agent's identity changes (PRD permission rows 15–17).
- **Engineering lead** — owns the build, the stack and everything in the PRD's "What engineering decides"; runs incident response.
- **Executive sponsor** — funds the work; owns the strategy position (vetter as step one toward data-pulling); the voice on reputation risk.
- **Facilitator** — runs user tests, records the run numbers by hand, reports fix-through against the pass bar.
- **ICA officer** — decides every application. Not a project role; named here because every decision below must leave this one untouched.

## Decision rights

| Harness part | Decision | Who decides | Who is consulted | Who is told |
|---|---|---|---|---|
| Instructions + rule file (`CLAUDE.md`, `context-skill.md`) | **Scope** — what the agent covers: which situations, which checks, what the VET RESULT contains | Product owner | Operations lead (rules must exist and be theirs), engineering lead (feasibility) | Executive sponsor, facilitator |
| Permissions table (`ai-prd.md`) | **Permissions** — each action's tier and setting; any move between auto, confirm and block | Product owner | Operations lead (anything touching the checklist or applicant trust), engineering lead (enforceability) | Executive sponsor |
| Model + platform the agent runs on | **Vendor and model** — which model, which stack, within the 10-week vendor window | Engineering lead | Product owner (cost and behaviour trade-offs), operations lead (if behaviour changes are visible to applicants) | Executive sponsor |
| Eval gate + release switch | **Go-live** — in front of testers, then any wider step | Operations lead (per PRD rows 16–17: their confirm) | Product owner (the experiment design), facilitator (test readiness), engineering lead (eval results) | Executive sponsor |
| Logs, stop conditions + the running agent | **Incident** — a miss complaint, a false-flag dispute, suspected real data, or the kill switch firing | Engineering lead (stop the agent, immediately, no consultation needed); product owner (what happens next) | Operations lead (frontline impact), facilitator (if mid-test) | Executive sponsor, and the operations lead's frontline team |
| The whole harness | **Retire** — switch the agent off for good, or after a failed kill-switch rework | Executive sponsor | Product owner (evidence from the logs), operations lead (frontline handover), engineering lead (decommission) | Facilitator, frontline team |

Three lines that keep the table honest:

- **Anyone can stop; only the sponsor can retire.** An incident stop is reversible and needs no meeting. Retiring is permanent and sits with the role that owns the strategy.
- **Operations hold the go, product holds the what.** The product owner can widen scope on paper, but nothing reaches a tester or a new situation without the operations lead's confirm — the director's trust condition, made structural.
- **No row touches the officer.** Every decision above is about the agent; the officer's decision on each application is outside this table and stays that way.
