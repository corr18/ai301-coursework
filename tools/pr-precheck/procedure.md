# Procedure: how this tool grades a PR package

## Read order

Live mode: read `scope.md` and `voice-guide.md` first (per SKILL.md), then do
steps 1-6 using the live sources in the evidence guide. Eval mode: start at
step 1.

1. Repo facts. Write down every ask the repo states as required for a PR (each
   template section or checklist item, an issue-link line, a changelog or
   whatsnew entry, tests with changes, a title convention) as a numbered list.
   Copy the AI policy wording exactly: does it require disclosure in the PR or
   "in any form", ask only for own words, or say nothing?
2. The issue and thread. Write the one behavior the PR must fix, in one
   sentence.
3. The plan context, BEFORE any part of the candidate PR. Write three lists:
   (a) every piece of work the plan commits to, one per line; (b) everything
   it scopes out ("not in scope", "deferred", "follow-up"); (c) every case its
   test plan names, with its expected result. Note any deviation note. Reading
   the plan first matters: once you have read a confident description, its
   claims start to look like the plan.
4. The diff. For each file, write its path and one line per hunk saying what
   the hunk does. Do this before reading the description, so the hunk list is
   what the diff shows, not what the description says it shows.
5. The commits. Note each message.
6. The description and title, then the test evidence.

## Evidence gathering

Locations for each family are in `references/evidence-guide.md`. Before
grading, pull these into notes:

- diff-matches-plan: pair every hunk from read-order step 4 with the
  plan-commitment line from step 3(a) it delivers, or write "unplanned". Flag
  any hunk that touches something in step 3(b). Then, for each line in 3(a),
  name the hunk that delivers it, or the plan note and description sentence
  that defer it, or write "missing".
- description-matches-diff: list every sentence of the description that says
  what changed or how faithful the change is. For each, name the hunk that
  makes it true, or write "not in diff".
- decisive-evidence: for each test-plan case from step 3(c), mark it
  "failure mode" (a repro or a way the reported bug shows up) or "added
  behavior" (a warning, message, or control the fix adds or keeps). Quote the
  evidence lines that re-run it, and write the before and the after values,
  or "not re-run". A failure mode needs its after shown; an added-behavior
  case needs its observed result stated. Note which path the run exercises (the failing case or a
  control). Quote the suite or check command and its result, or write "no
  suite outcome".
- clean-diff: from the hunk list, copy every added or removed line that is a
  print or log statement, a comment containing code, a function or variable
  nothing calls, a TODO or FIXME, or a line whose only change is whitespace,
  ordering, or formatting. For any block of lines removed and re-added
  unchanged, check whether a changed line sits inside the same block: if one
  does, the unchanged lines are context; if none does, the block is churn. Note commit messages such as "wip", "fmt", or
  "cleanup" as pointers to look at those hunks again.
- template-and-disclosure: for each numbered ask from step 1, quote where the
  PR meets it (description line, diff file, commit) or write "absent". Quote
  the description's AI-disclosure sentence, or write "none".
- unknowns-stated: quote the description's limit, deferral, or open-question
  lines, and any certainty claim.

If a part of the package is missing (no test evidence, no description, no plan
test plan), write "absent". Do not fill it from another part.

## Check execution

1. Run the required checks in rubric table order, then the preferred check.
   Run every check even after one fails, so the output shows every problem.
2. Grade each check only from the notes above against its pass condition.
   Re-open the package only to confirm a quote.
3. Absent evidence: if the pass condition covers the absence (no AI policy, no
   template), grade by that rule. Otherwise an absent part fails its check: no
   test evidence fails decisive-evidence; a missing required template section
   fails template-and-disclosure.
4. A hunk marked "unplanned" fails diff-matches-plan unless a deviation note
   in the plan covers it. Its size and how sensible it looks do not matter.
5. A deferral passes only when both the plan and the description state it.
   Missing work with no note fails.
6. Use `unclear` only when the evidence exists and genuinely supports both
   readings after applying the pass condition's examples. Name the two
   readings in the evidence line.
7. Each check's evidence line quotes or names the fact that decided it: the
   file and hunk, the description sentence, the test output line, or the
   repo-facts ask.

## Verdict assembly

1. Apply the rubric's verdict rule: accept only if all five required checks
   are `pass`. Any required `fail` or `unclear` means reject.
2. `unknowns-stated` is reported but never changes the verdict.
3. In the summary, name the deciding check: the first required check, in
   rubric table order, that did not pass. Quote the hunk, sentence, or missing
   ask that failed it. If more than one check failed, list the others after
   it in table order. For an accept, name the strongest piece of evidence
   (usually the before/after of the failing case).
4. Live mode only: after the verdict, list every voice-guide rule the draft
   title or description breaks, quoting the rule and the line. This never
   changes the verdict.
5. End with the JSON block from SKILL.md, checks in rubric order.
