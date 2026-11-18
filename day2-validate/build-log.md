# Build log

**Scenario:** B · ICA passport applications and renewals

All data referenced here is synthetic and labelled as made up.

## Riskiest assumption

**Assumption:** applicants will actually change their application details when prompted with a list of suggestions to change.

- **Risk type:** Market. The question is not whether the model can find issues; it is whether people act on the flags. Only 38% (made-up figure) read the current guidance, so attention and follow-through cannot be assumed.
- **Kill switch:** applicants will follow instructions from an automated vetter. If testers see the vet result and make no change — skim past the flags, click "Submit anyway" without fixing — the assumption is dead and we stop before building further.
- **Success signal:** the fix-through rate — of the blocking issues shown, the share fixed before submission. Pass bar, named before the test runs: **at least 60% of blocking issues fixed across testers** (bar is mine to adjust before testing starts, but it is set in advance so the test can fail).

## How the prototype tests it

The prototype (`prototype/index.html`) is the test instrument:

- One made-up child renewal case with 2 planted blocking issues and 1 warning.
- Every flag is fixable on the spot, and "Submit anyway, without fixing" is always available, so the tester has a genuine choice — the behaviour we are measuring.
- On submission the prototype shows a facilitator panel with that run's numbers: blocking issues fixed (0, 1 or 2 of 2), whether the warning photo was replaced, whether submission happened with or without fixes, and the run's fix-through rate.
- The facilitator records each run's numbers by hand; the prototype stores nothing.

## Log

- Run the test with 5–10 people. Each run is one tester going from the review page to submission.
- Fix-through rate across all runs ≥60%: assumption survives, continue to the prompt chain and Living PRD.
- Below 60%, or testers mostly "Submit anyway": kill switch fires — rework the result card (fewer flags, stronger wording, different moment) or stop.
