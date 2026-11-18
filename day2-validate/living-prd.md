# Living PRD: passport application vetter

**Scenario:** B · ICA passport applications and renewals

This PRD is extracted from the working prototype in `prototype/index.html` — it describes what the code actually does today, not what a slide says it should do. Every name, number and rule in the prototype is synthetic and labelled as made up. Update this file whenever the prototype changes; if the two disagree, the code is the truth and this file is stale.

## 1. Product overview

A pre-submission check for passport applications. The applicant logs in with Singpass (simulated), the service prefills everything the government already knows, the applicant adds only what cannot be pulled, and before submission a vetter checks the draft against the published checklist and flags what would get it returned — each flag fixable on the spot. The check can never block submission, and an ICA officer still makes every final decision.

One sentence: take the applicant out of the loop where possible, and where not, tell them exactly what to fix before an officer ever sees the application.

## 2. Problem and riskiest assumption

**Problem (from `../day1-discover-define/opportunity.md`):** 17% of applications (made-up figure) are returned for fixable problems, costing about 6 days each across ~520k applications a year. Applicants do this once every ten years; they cannot know the pitfalls.

**Riskiest assumption (from `build-log.md`):** applicants will actually change their application when prompted with a list of suggested fixes. This is a market risk, not a technical one — the model finding issues is worthless if people skim past the flags and click "Submit anyway".

- **Kill switch:** testers see the vet result and make no change.
- **Pass bar, set before testing:** at least 60% of blocking issues fixed across testers.

The prototype is the instrument that tests this, not a draft of the product.

## 3. User flow and screen map

Four screens, one page, as the code implements them:

1. **Login** (`#screen-login`) — "Renew a passport". Singpass button (simulated, 900ms delay) or "Continue without Singpass". A facilitator-only selector can force a prefill failure for the run.
2. **Renewal type** (`#screen-type`) — "What are you renewing?" My own passport (not built; the screen says so) or a child's passport, the test case.
3. **Child picker** (`#screen-picker`, Singpass path only) — the children linked in the records. Two made-up children with names one letter apart (wrong-child trap), plus "My child is not in this list" (typed child = warning asking for proof of relationship, never a block).
4. **Application** (`#screen-app`) — three labelled steps:
   - Prefilled fields are locked with a "🔒 From Singpass (simulated)" badge and an **Unlock to edit** link. An edited field loses its badge for good and is fully checked.
   - Documents: parent ID (pulled), child's photo (over 12 months old — new one asked for), birth certificate (missing). Payment shows the fee only; method comes at a later step.
   - **Check and submit** opens the scripted VET RESULT: blocking issues / warnings / not sure / checked and fine (split into verified-from-Singpass and checked-by-hand). Every flag has an inline fix UI. "Check again and submit" re-counts; "Submit anyway, without fixing" always works.

Failure paths (all reachable from the facilitator selector): full prefill failure → plain-language banner, everything manual, never a dead end; partial failure → parent prefilled, child manual; retry → asks before replacing anything typed; session expiry at the check step → draft kept, re-login or continue without badges.

## 4. Success metrics

What the facilitator panel actually measures per run (nothing is stored or sent; the facilitator records by hand):

- **Fix-through rate (the success signal):** share of the 2 planted blocking issues fixed before submission. Pass bar ≥60% across 5–10 testers.
- Flow used (Singpass / guest / which failure was simulated) and prefilled fields edited — to see whether prefill changes fixing behaviour.
- Items fixed at step 2, before the check ever runs.
- Warning photo replaced (yes/no) — warnings must persuade without blocking.
- Submitted via "Check again" or "Submit anyway" — the kill-switch observation.

Metrics the real service would add later: post-vet return rate (misses), false-flag disputes, vetter–officer agreement.

## 5. Technical reality

What an engineer inherits, stated plainly:

- One file, `prototype/index.html`, ~1,300 lines: ~430 of CSS, ~400 of HTML (four screens toggled by CSS classes), ~470 of JavaScript. No build step, no server, no dependencies.
- **Everything is faked.** No backend, no storage, no model call, no real Singpass/MyInfo/authentication. The vet result is hard-coded for one child-renewal case; the "AI" does not exist yet. The real vetting logic is specified in `PROMPTS.md` and `context-skill.md`, not implemented.
- The planted case's three issues map one-to-one to rules CR-1, CR-2 and CR-3 in `context-skill.md`, so the prototype doubles as acceptance criteria for the real vetter.
- Known shortcuts that must never ship: UI built by string concatenation into `innerHTML`; consent "signed" by typing a name; a pretend file picker; blocker counting hard-codes the two issue IDs; one global state object; no tests.

## 6. Assumptions and risks

- **Market (the one under test):** applicants act on flags. Dead if testers skim and submit anyway.
- **Behavioural:** prefill badges earn trust rather than complacency; a warning persuades without blocking.
- **Stakeholder (from `stakeholder-pushback.md`):** frontline director needs vetter–officer agreement >95% and a checklist owned by operations; head of product wants fix-through and funnel upside proven before scale; the sponsor's "patch, not cure" objection is still open.
- **Technical (untested):** a model can apply the checklist with the skill file's behaviour (cites rules, refuses outcome predictions, says "I am not sure" honestly) — `context-skill.md`'s five test questions are the first eval for this.
- **Data/governance (unaddressed):** real Singpass/MyInfo consent, retention and audit requirements. The prototype deliberately uses made-up personas only.

## 7. In and out of scope

**In scope (built and testable today):** child passport renewal, one planted case; simulated Singpass prefill with unlock-to-edit; guest path; forced failure, partial failure, retry and expiry; scripted vet result with inline fixes; submit-anyway; facilitator measurement of fix-through.

**Out of scope (deliberately):** adult and first-time applications (the type screen says "not built"); a real model call; real authentication, uploads, payment method, storage or submission; officer-side tooling; multiple simultaneous cases; accessibility and mobile polish (prompt chain stage 3, Refine, not yet run).

## 8. Engineering recommendation

Treat the prototype as a **decision record and test instrument, not a codebase**. Concretely:

1. Do not extend `index.html`. Rebuild on a real stack, keeping the behaviours as the spec: locked prefill, unlock-to-edit drops verification for good, failures fall back and never dead-end, retry asks before overwriting, the check never blocks, officers decide.
2. Build the vetter first as a prompt + context file (`PROMPTS.md` + `context-skill.md`), evaluated against the five test questions and their answer key before any UI work.
3. Wire the facilitator panel's numbers into real telemetry from day one — fix-through is the product's heartbeat metric, not an afterthought.
4. Run the fix-through test (5–10 users) on this prototype *before* committing engineering time; the kill switch in `build-log.md` says when to stop.
5. Resolve the open governance items (Singpass/MyInfo consent, checklist ownership and update lag) before touching real data — rule 1 of this repo stays: synthetic data only until then.
