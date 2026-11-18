# Prompt chain

**Scenario:** B · ICA passport applications
**Prototype today:** A made-up child passport renewal with planted errors where clicking “Check and submit” shows a scripted vet result whose flags can be fixed on the spot or ignored via “Submit anyway” — with a facilitator panel that reports each tester’s fix-through rate.

All data is synthetic and labelled as made up.

## 1. Expand

**Prompt:** Add a simulated Singpass login step before the application form. After “login”, prefill the form with the applicant’s synthetic MyInfo data (name, NRIC, date of birth, residential address, and for child renewals, the children linked to the parent). Prefilled fields are visually marked “From Singpass” and the applicant confirms rather than retypes them. The vetter then treats prefilled fields as verified, so its checks focus on what the applicant added by hand: photo, supporting documents, consent signatures, and the chosen situation.

**Business rules:**
- Prefilled Singpass fields are read-only by default; the applicant unlocks one to edit it, and any edited field loses its “From Singpass” badge and goes back into the vetter’s full-check list.
- The vetter never flags an unedited prefilled field as a blocking issue — verified data is not re-questioned.
- For a child renewal, the applicant picks the child from the Singpass-linked list; free-typing a child not on the list triggers a warning, not a block.
- The prototype uses a fake Singpass screen with made-up personas only; no real Singpass, MyInfo or NRIC data, ever.
- Logging out or timing out clears all prefilled data from the draft.

**Edge cases:**
- The applicant’s MyInfo address is outdated and they need to edit a “verified” field.
- A parent has a child not linked in Singpass (e.g. recently adopted) and must attach proof instead.
- Two children with very similar names in the picker — wrong-child selection.
- The applicant declines Singpass and continues as a guest: everything falls back to manual entry and the vetter checks all fields.

**Result:** _fill in after you run it: what worked, what broke._

## 2. Behavior

**Prompt:** When the Singpass prefill fails (service down or returns nothing), show a plain-language message — “We could not fetch your details from Singpass. You can fill them in yourself.” — switch all fields to manual entry, keep anything already typed, and tell the vetter to run its full checklist as if the applicant were a guest. The flow never dead-ends: Singpass failing costs convenience, not the ability to apply.

**Forced failure:** The simulated MyInfo fetch returns an empty response after a successful login (logged in, but no profile data comes back). The screen must show the fallback message, not a blank form or a spinner forever.

**Business rules:**
- A prefill failure must never block starting or submitting an application.
- Never show raw error text or codes to the applicant; always the plain-language fallback message.
- After a failed prefill, no field may still carry a “From Singpass” badge — nothing unverified gets presented as verified.
- Draft data already entered by the applicant survives the failure; nothing is wiped.
- The vetter’s result must note “details entered manually” so expectations match a guest application.

**Edge cases:**
- Prefill succeeds for the parent but fails for the children list (partial failure) — prefill what arrived, manual-entry the rest.
- Singpass session expires mid-application — offer re-login, keep the draft.
- The applicant retries Singpass after a failure and it works — prefill must not silently overwrite fields they typed in the meantime; ask before replacing.

**Result:** _fill in after you run it: what worked, what broke._

## 3. Refine

**Prompt:** Make the verified-versus-manual distinction consistent and quiet everywhere: one “From Singpass” badge style, one lock icon, identical spacing and label casing across all form sections; the fallback and error messages all in the same voice as the vet result (short sentences, no jargon); the vet result card and the prefill badges use the same visual language so the whole flow feels like one system, not a form with an AI bolted on.

**Business rules:**
- One badge style and one message tone across every screen; no synonyms like “verified” on one screen and “confirmed by Singpass” on another.
- Every screen keeps the officer-decides note visible in the same position.
- All synthetic personas are labeled “made up” in the demo data file.

**Edge cases:**
- Long names and long addresses from MyInfo must not break the field layout or truncate silently.
- A screen-width (mobile) view where badges wrap — the lock and badge must stay attached to their field.
- Mixed state on review: some fields verified, some edited, some manual — the review page must make the three states distinguishable at a glance.

**Result:** _fill in after you run it: what worked, what broke._
