# AI user flow: Applicant clicks “Check my application” just before submitting, and gets a vet result telling them exactly what would get it returned.

**Scenario:** B · ICA passport applications

All data in this file is synthetic.

## The moment

Applicant clicks “Check my application” just before submitting, and gets a vet result telling them exactly what would get it returned.

## The iceberg

### Above the line: what the user sees

Instead of submit button, have a check and submit button that then shows a result card:  numbered list of blocking issues each with a plain-language fix, warnings, what was checked and fine.

### Below the line

- Data: Draft application fields and attached document metadata (type, date, image quality), plus the current published checklist per situation (adult renewal, child renewal, name change, etc.)
- Model: LLM that classifies the applicant’s situation, maps the draft against that situation’s checklist, and drafts the plain-language fixes.
- Rules: never predict or influence approval, never edit the applicant’s answers, never retain or echo personal data, one-pass flagging, must cite the checklist version, must say “an officer will confirm” when a rule is ambiguous.
- Handoffs: Edge cases - travel black lists, criminal records, should be vetted by a person

## The flow

1. **Trigger:** the applicant finishes the form and clicks “Check and submit my application” on the review page. also auto-offered if they hit Submit with
   - Trust gap: none. NA
2. **AI step:** vetter identifies the situation, runs the draft against that situation’s current checklist, and returns the vet result — blocking issues with fixes, warnings, checked-and-fine list
   - Trust gap: Hallucination. the biggest risk: the vetter invents a requirement that doesn’t exist, or misses one that does
3. **Human check:** First the applicant: they review each flag and decide whether to fix, which keeps them in control of their own application. Then the ICA officer: every submission still lands with an officer who makes the actual decision
   - Trust gap: Control. the applicant must stay free to disagree and submit
4. **Outcome:** he applicant fixes the blocking issues and submits a complete application in one pass, or submits anyway (allowed — the vetter cannot block submission), or sees “I am not sure — an officer will confirm” on ambiguous items and submits knowing which parts an officer will look at.
   - Trust gap: Black box. if the application is later returned by an officer despite a “Ready to submit: Yes”, the applicant can’t see why the vetter missed it. Mitigated by the checked-and-fine list (showing what was examined) and the explicit note that this is a completeness check, not a decision.

## Fallback when the AI is unsure (Advanced)

f the vetter can’t confidently classify the situation, or a checklist item is ambiguous or recently changed, it does not guess. It marks that item “I am not sure — an officer will confirm”, keeps it out of the blocking list, and still lets the applicant submit. If it can’t classify the situation at all, it says so and shows the plain checklist links for all situations — degrading to static guidance rather than a wrong-situation vet.

## How we measure trust (Expert)

fix-through rate (what share of flagged blocking issues applicants actually fix before submitting — low means the flags aren’t trusted or understood); false-flag dispute rate (applicants who submit anyway and then are not returned by an officer — each one is a flag that was wrong and erodes trust); and post-vet return rate (“Ready to submit: Yes” applications that an officer still returns — the vetter’s miss rate, the most trust-destroying failure).

## AI-UX readiness

**Scenario:** B · ICA passport applications
**Moment:** the applicant clicks “Check my application” on the review page just before submitting, and gets a vet result listing exactly what would get the application returned.
**Level reached:** 2 of 3
**What we ship:** Stay on a validator UI until the correction rate shows the AI removes more labor than it creates.

### Levels

- Level 01 · Trigger Accuracy: Pass. The trigger is user-initiated (a button click), so the “does it fire at the right moment” risk is nearly zero — the applicant decides the moment. The accuracy question moves one step inside: does the vetter classify the applicant’s situation correctly before vetting?

Vetter must be ≥90% confident in the situation classification (child renewal vs adult renewal vs name change, etc.) before running a checklist; below that it asks the applicant to confirm their situation first

How I’d check it: run 50 synthetic draft applications with known situations through the classifier and require ≥95% correct; log every “asked to confirm” in testing to see how often the threshold triggers
- Level 02 · System Transparency: Pass. Each issue is verifiable in under three seconds because it’s anchored to something the applicant can see: the flag names the field or document (“Parental consent — only one signature”), states the rule it comes from in plain language, and the result shows a “Rules current as of [date]” line. 

How the user checks the output: they compare the flag to their form. Each flag also links to the published guidance it came from
- Level 03 · Invisible Integration: Not yet. if applicants ignore the flags or find the vet result as skippable as the current guidance page, it adds a step without removing any labor.

### Trust gaps

- Black box: Vetter flags issues without specifying the rule that it violates Move: Link to the guidance pages, with each line
- Hallucination: Rules or applicant's situation is wrongly applied Move: Below a certain threshold, flag that the model is unsure, needs human input
- Control: If users are compelled to accept vetter's suggestions Move: Users can decide to submit or not, or continue editing
