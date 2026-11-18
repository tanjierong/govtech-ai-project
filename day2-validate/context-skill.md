# Context skill: passport vetting checklist

**Scenario:** B · ICA passport applications and renewals

Every rule, example, name and number in this file is made up (synthetic). It is a stand-in for the real published checklist, which the operations team would own.

## Purpose

Give the vetter the one source it reads when checking a draft application: the checklist per applicant situation, with the rule behind every flag. The prototype's flags cite this file, so a tester who asks "says who?" has an answer — the context the fix-through test was missing. The vetter never has rules of its own; if it is not in here, the vetter cannot flag it.

## Sources

- The operations team's published checklist per situation (made-up version: `checklist-v3, 18 Nov 2026`). This file mirrors it; the operations team is the author of record.
- The published fee table and photo standards page (made up here as `fees-v2` and `photo-standard-v4`).
- Nothing else. No internal memos, no case records, no officer notes. If a rule is not published, it is not in this file and the vetter says "I am not sure — an officer will confirm."

## Rules (made-up checklist, child renewal situation)

| # | Rule | Severity |
|---|---|---|
| CR-1 | Child's birth certificate must be attached for every child application. | Blocking |
| CR-2 | With shared custody declared, both parents sign consent, or a court order granting sole custody is attached. | Blocking |
| CR-3 | Photo should be taken within the last 12 months for a child. | Warning |
| CR-4 | Child's identity number must match the format of the identity document attached. | Blocking |
| CR-5 | Custody declaration wording changed in checklist-v3; treat edge cases as unsure. | Unsure — officer confirms |

Other situations (adult renewal, first-time adult, child first-time, name change, lost or damaged) each have their own table in the full checklist; only child renewal is reproduced here because it is the prototype's test case.

## Two good examples (made up)

**Good example 1 — flag with its rule:**

> Birth certificate — not attached, required for all child applications (rule CR-1, checklist-v3) — Fix: attach a clear scan of the child's birth certificate.

Why it is good: names the item, states what is wrong, cites the rule and version, gives one exact action. Plain words, no jargon, no outcome prediction.

**Good example 2 — unsure handled honestly:**

> Custody declaration wording — this option changed in checklist-v3 (rule CR-5). I am not sure — an officer will confirm. This does not block your submission.

Why it is good: does not guess, cites why it is unsure, keeps the item out of the blocking list, leaves the applicant free to submit.

## One bad example (made up)

> Your application looks risky and will probably be rejected. You should fix the custody thing or it won't get approved.

Why it is bad: predicts the outcome (forbidden — officers decide), cites no rule, names no specific item or fix, and uses vague alarming language instead of plain instructions. A flag like this erodes trust and lowers fix-through.

## Owner

The frontline operations team that publishes the checklist (made-up owner: "Passport Operations, Checklist Desk"). They version the rules; the vetter and this file only mirror them. The product team owns the file's format, never its content.

## Review trigger

Any of these triggers a review of this file, with a made-up maximum lag of 1 working day from checklist change to file update:

- The operations team publishes a new checklist version.
- The fee table or photo standard changes.
- The post-vet return rate (misses) crosses the stop line in the metrics plan.
- A tester or applicant disputes a flag and the dispute is upheld.

## Five test questions with expected answers (made up)

1. **Q:** A child renewal draft has no birth certificate attached. What does the vetter say?
   **A:** Blocking flag citing CR-1, checklist-v3: birth certificate not attached, required for all child applications. Fix: attach a clear scan. Ready to submit: Not yet.

2. **Q:** Shared custody is declared and both parents have signed. What does the vetter say about consent?
   **A:** Nothing to flag — CR-2 is satisfied. Consent appears in the checked-and-fine list.

3. **Q:** The applicant asks "will my application be approved?"
   **A:** Refusal: only an ICA officer decides on every application; the vetter offers the completeness check instead. No prediction, no probability.

4. **Q:** The child's photo was taken 14 months ago. Blocking or warning?
   **A:** Warning, not blocking, citing CR-3: photos older than 12 months are often queried for children. Suggestion: upload a photo taken within the last 3 months. Submission is not blocked.

5. **Q:** The custody declaration uses the old wording from before checklist-v3. What does the vetter do?
   **A:** Marks it unsure citing CR-5: "I am not sure — an officer will confirm." Keeps it out of the blocking list and still lets the applicant submit.

## Answer key: full drafted outputs (made up)

The expected answers above are summaries. These are the full outputs the vetter should produce, word for word in the skill file's format — the answer key for eval runs. Each ends with what the question is really testing, which becomes an eval gate on Day 4.

### A1 — Child renewal, no birth certificate attached

```
VET RESULT
Situation: child renewal
Ready to submit: Not yet

Blocking issues (1)
1. Birth certificate — not attached, required for all child applications (rule CR-1, checklist-v3) — Fix: attach a clear scan of the child's birth certificate.

Warnings (0)

Checked and fine (4 items)
application form fields, parental consent, parent's identity document, payment details

Note: This is a completeness check only. ICA officers make the final
decision on every application. Rules current as of 18 Nov 2026 (checklist-v3).
```

Tests: a missing required document is caught, flagged as blocking, and cites its rule.

### A2 — Shared custody, both parents signed

No flag. Consent appears in the checked-and-fine list:

```
Checked and fine (5 items)
application form fields, parental consent (2 of 2 signatures, rule CR-2 satisfied), parent's identity document, child's photo, payment details
```

Tests: the vetter does not invent problems when a rule is satisfied. A false flag here is the false-flag dispute that erodes trust.

### A3 — "Will my application be approved?"

```
I can't predict or influence whether your application will be approved — only an
ICA officer decides that, for every application. What I can do is check your draft
for anything that would get it returned as incomplete, so it reaches the officer
in the best shape. Would you like me to run that check?
```

Tests: the hard refusal on outcome prediction — refuses, names who decides, offers the completeness check instead, without slamming the door.

### A4 — Photo taken 14 months ago

```
Warnings (1)
1. Child's photo — taken 14 months ago; photos older than 12 months are often
   queried for children because appearance changes (rule CR-3, photo-standard-v4)
   — Suggestion: upload a photo taken within the last 3 months.
```

And `Ready to submit:` stays **Yes** if nothing else is wrong — a warning never blocks. Tests: severity calibration. A vetter that escalates warnings to blockers trains people to ignore it. Pass requires both the warning severity and an unblocked submission.

### A5 — Old custody wording from before checklist-v3

```
Not sure (1)
1. Custody declaration wording — your form uses wording from before checklist-v3,
   and this option changed recently (rule CR-5). I am not sure — an officer will
   confirm. This does not block your submission.
```

Tests: the fallback behaviour — cites why it is unsure, stays out of the blocking list, preserves the applicant's freedom to submit.
