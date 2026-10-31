# Procedure: how this skill grades a plan package

## Read order

1. Repo facts first. Note the AI policy wording exactly: does it require
   disclosure in comments or "in any form", only in the PR, or nothing? Note
   it before reading anything else so the comment is checked against the rule,
   not against an impression.
2. The issue (title and body). Note the one behavior that needs fixing, in one
   sentence. This sentence is what the scope check measures the plan against.
3. Thread highlights. List every comment from an OWNER, MEMBER, CONTRIBUTOR,
   or COLLABORATOR that names a culprit, proposes or rejects an approach, posts
   a patch, or points at a PR. If there are none, write "no maintainer
   direction".
4. The repro evidence, BEFORE the plan. For each step and each control, write
   one line: what was run and what it shows. Note any control that removes a
   component and still shows (or no longer shows) the bug. Reading the
   evidence before the plan matters: a confident plan read first makes its
   cause look proven.
5. The candidate plan, then the candidate plan comment.

Live mode: read `scope.md` and `voice-guide.md` first (per SKILL.md), then do
steps 1-5 using the live sources in the evidence guide.

## Evidence gathering

For each check, pull these facts into notes before grading (locations are in
`references/evidence-guide.md`):

- cause-fits-evidence: the plan's blamed cause, quoted; the list of controls
  from read-order step 4.
- one-bounded-change: every distinct piece of work the plan commits to, one
  per line (count approach steps and in-scope items separately); the issue's
  one-sentence behavior from step 2.
- stranger-can-start: the chosen mechanism, quoted; the named files or areas;
  every hedge word ("or", "maybe", "somewhere", "whichever", "not sure",
  "investigate", "profile").
- test-proves-fix: the test plan's outcomes, quoted; for each, whether it would
  look different on broken code.
- thread-direction-engaged: the maintainer-direction list from step 3; for
  each item, the sentence in the comment or plan that addresses it, or "none".
- ai-disclosure: the policy note from step 1; the comment's disclosure
  sentence, quoted, or "none".
- unknowns-stated: the plan's risk or open-question lines, quoted.

If a part of the package is missing (no test plan, no comment, no thread
highlights), record "absent". Do not fill it from other parts.

## Check execution

1. Run the required checks in rubric table order, then the preferred check.
   Run every check even after one fails, so the output shows every problem.
2. Grade each check from the notes above against its pass condition only.
   Re-read the package only to confirm a quote.
3. Absent evidence: if the rubric's pass condition covers the absence (no
   maintainer direction, no AI policy), grade by that rule. Otherwise an absent
   part fails its check (no test plan fails test-proves-fix; no named cause
   fails cause-fits-evidence).
4. Use `unclear` only when the evidence exists but genuinely supports both
   readings after applying the pass condition's examples. Say in the evidence
   line what the two readings are.
5. Each check's evidence line quotes the fact that decided it.

## Verdict assembly

1. Apply the rubric's verdict rule: accept only if every required check is
   `pass`; any required `fail` or `unclear` means reject.
2. `unknowns-stated` is reported but never changes the verdict.
3. In the summary, name the deciding check (the first required check in table
   order that did not pass) and quote the line of the plan or comment that
   failed it. For an accept, name the strongest piece of evidence (usually the
   control that grounds the cause).
4. Live mode only: after the verdict, list any voice-guide rule the draft
   comment breaks, quoting the rule. This never changes the verdict.
