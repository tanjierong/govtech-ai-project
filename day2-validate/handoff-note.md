# Handoff note: passport vetter prototype

**Scenario:** B · ICA passport applications and renewals

For the engineer or team inheriting `prototype/`. Written from what the code does today. Companion to `living-prd.md` (the full eight-block PRD); this note is the short version with the decisions and the call.

## What it does

A one-file click-through of the pre-submission moment, and a test instrument for our riskiest assumption:

- Simulated Singpass login → pick what you are renewing → pick the child from the linked list (or type one, or skip Singpass entirely).
- Prefilled fields are locked with a "From Singpass (simulated)" badge; unlock-to-edit removes the badge for good and puts the field back under full checking.
- One planted child-renewal case with three problems (missing birth certificate, 1 of 2 consent signatures under shared custody, photo over 12 months old).
- "Check and submit" shows a scripted VET RESULT — blocking / warnings / not sure / checked and fine — with every flag fixable inline, and "Submit anyway" always available.
- Prefill failures are simulatable: full failure, partial failure, retry (asks before overwriting typed fields), session expiry (keeps the draft).
- A facilitator panel reports each run's fix-through rate — the number the whole experiment turns on.

## What is synthetic

Everything. Made-up personas (the Tan family, fake identity and passport numbers), a pretend Singpass button that authenticates nothing and retrieves nothing, a scripted vet result (no model call), simulated file pickers (no file is read), a typed name as a "signature", a made-up fee and checklist version. Nothing is stored or sent anywhere. No real Singpass, MyInfo or NRIC data was used at any point, and none may be added to this prototype.

## What is missing for production

- **The vetter itself.** The AI does not exist; the result is hard-coded for one case. The behaviour is specified in `PROMPTS.md` (prompt chain) and `context-skill.md` (rules, examples, five test questions with an answer key) — build and eval that first.
- **Everything real:** authentication and MyInfo consent flows, actual document upload and validation, storage, submission, payment method, officer-side view.
- **Coverage:** adult and first-time applications, multiple children cases beyond the one planted scenario.
- **Engineering basics:** a real stack (the file is 1,300 lines of hand-toggled screens and `innerHTML` — do not extend it), tests, telemetry (the facilitator panel's numbers, emitted properly), accessibility and mobile (prompt chain stage 3, Refine, has not been run).
- **Governance:** checklist ownership and the rule-change update lag formalised with operations; Singpass/MyInfo consent, retention and audit requirements.

## Decisions an engineer must not change without asking

1. **The check never blocks submission.** "Submit anyway" always works. This is the control trust principle and the kill-switch measurement; removing it breaks both.
2. **Officers decide.** The vetter never predicts or implies approval. The officers-decide note stays on every result.
3. **Unedited prefilled fields are never flagged;** an edited field loses its verified status permanently for that run. Verified data is not re-questioned, and nothing unverified is presented as verified.
4. **Failures fall back, never dead-end.** A prefill failure costs convenience, not the ability to apply; plain-language messages only, no error codes.
5. **A retried prefill asks before replacing anything the applicant typed.**
6. **Below-confidence items say "I am not sure — an officer will confirm"** and stay out of the blocking list.
7. **The checklist belongs to operations.** The vetter mirrors their published rules (see `context-skill.md`); the product team owns the format, never the content.
8. **Fix-through rate stays measurable.** Whatever is built must emit the facilitator panel's numbers from day one.

## Handoff decision

**Keep prototyping — do not move to a production spec yet.**

Why: the riskiest assumption is still untested. The fix-through test (5–10 users, pass bar ≥60% of blocking issues fixed, named in `build-log.md` before any run) has not been run. Writing a production spec now would spend the 10-week vendor window building before knowing whether applicants act on flags at all — exactly the kill-switch mistake the build log warns against. The head of product's condition (behavioural evidence first) and the paper funnel on real return reasons are also still outstanding.

What flips this decision: a passed fix-through test (≥60%) plus the paper funnel showing the vetter would catch a meaningful share of real return reasons. Then move straight to a production spec, starting from `living-prd.md` section 8 — build the vetter from `PROMPTS.md` + `context-skill.md` and eval it against the answer key before any UI work.

If the test fails: the kill switch fires — rework the result card (fewer flags, stronger wording, different moment) or stop, per the build log.
