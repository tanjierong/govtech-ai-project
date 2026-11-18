# One Pager

Use case: Scenario B · ICA passport applications. All data in this file is synthetic and labelled as made up.

## Problem and user

The user is a passport applicant in the online application and renewal channel — someone who does this perhaps once every 10 years. The most confused group is parents applying for a child.

The problem: 17% of submissions are returned for errors or missing documents, adding ~6 days each. Applicants don't know what a complete application looks like for their situation. The status quo is a generic guidance page, and only 38% read it before applying — so generic guidance isn't working. 520,000 applications and renewals a year go through this channel.

Constraints: final decisions stay with ICA officers. The vendor withdraws in 10 weeks and the date won't move.

## Opportunity and evidence

The moment: pre-submission — the stage just before an applicant submits. An automated vetter checks the draft application against the checklist for the applicant's situation and tells them exactly what would get it returned, before they submit.

Hypothesis: by providing an automated vetter to passport applicants before they submit, I can reduce the return rate of applications, reducing the amount of time overall. Riskiest assumption (kill switch): applicants will follow instructions from an automated vetter — if they make no change after seeing the suggestions, stop.

Evidence (synthetic user quotes):

- "I renewed mine fine, but for my daughter I had no idea I needed both parents to sign — it came back after a week."
- "I only found out the photo was too old after they returned it. Why couldn't it tell me before I submitted?"
- "The guidance page is long and none of it felt like it was about my case."

Success metrics: returns rate by officers below 5% (from 17% today; success signal for the first experiment is 17% to 10%). Supporting metrics: agreement rate between vetter and officer manual checks, changes by applicants post-vetting, median end-to-end application time.

## Experience and trust
## Risks (market, technical, team)

Stakeholder pushback, from a role-played review (all roles and figures synthetic):

- **Frontline operations director (technical/trust risk):** a "Ready to submit: Yes" that an officer still returns lands as a complaint on operations, and "Submit anyway" creates a record of knowingly accepted defects. Checklist ownership and rule-change lag are unresolved until named. Mitigation agreed: the checklist comes from the operations team and stays theirs to version; vetter-officer agreement rate measured against a threshold the director sets (95%+ discussed, backed by eval data); below-confidence items surface as "I am not sure — an officer will confirm" rather than passing silently; miss rate (post-vet returns) published with a stop line; a defined rule-change update process with a named maximum lag.
- **Head of product (market/value risk):** the demo is one scripted case; no behavioural evidence yet that applicants read, trust or act on flags — the kill-switch signal (applicants make no change) is untested, and "Submit anyway" could reveal the vetter is a speed bump. Mitigation agreed: rapid user test measuring fix-through rate with the pass bar named before the test; paper funnel on sample returned applications to measure what share of real return reasons the vetter would catch (the upside ceiling) before any build.
- **Chief executive sponsor (strategy/reputation risk):** the vetter keeps applicants in the loop, while the strategy kernel says to take them out of it — why fund the patch instead of the cure (form simplification, data pulling)? Plus headline risk if a government AI appears to "approve" an application that is then rejected. Mitigation: position the vetter as step one of the strategy — it ships inside the 10-week vendor window, and its flag frequency generates the prioritised backlog for the data-pulling integrations; the vetter never predicts outcomes and is labelled a completeness check with officers deciding; the kill switch makes this a bounded experiment, not a programme commitment.
## Build plan and handoff
## Agent role and guardrails
## Roles and operating model
## Strategy and roadmap

### Strategy kernel

**Diagnosis:** Applicants cannot understand the complexity of the application form. They submit this perhaps once every 10 years or so, and they submit it as best they can, but they have no idea where the pitfalls are.

**Guiding policy:** Take the applicants themselves out of the loop where possible.

**Coherent actions:**

1. Simplify the form steps
2. Change data/information needed so it can be pulled
3. Pulling information on behalf of the applicants where possible

## Metrics plan and stop line
## Evals and gates
## Decision: continue, pivot or stop
## Open issues
