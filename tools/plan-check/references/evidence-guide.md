# Evidence guide: where evidence lives in a plan package

Each family below says where to look (eval bundle first, then live mode) and
what good looks like. In eval mode the bundle text is the whole world; in live
mode the drafts are graded the way a maintainer on the thread would read the
posted comment.

## Diagnosis and grounding

Where it lives:
- Eval: the candidate plan's "Diagnosis" line or opening sentences (the cause
  it blames), and the repro-evidence block: its numbered steps, every "Control"
  run, any intermediate output (debug traces, dumped tables, timing matrices),
  and the Expected/Actual lines.
- Live: the cause in the student's `plan.md`; the steps and output in the
  student's posted repro comment on the issue (or, on the house issue, the
  repro evidence the drafts quote).

What good looks like: the cause the plan blames is the thing the controls point
at. Line every control up against the cause: if a control removes or bypasses
the blamed component and the bug still happens, or shows the blamed component
working in the same build, or shows the damage already present before the
blamed stage runs, the cause is ruled out. A cause the thread or issue author
states confidently is not evidence by itself; only the repro steps and controls
are.

## Scope

Where it lives:
- Eval: the plan's "Scope"/"Change" section, its "In scope" and "Not in scope"
  lines, and the numbered approach steps (count what each step commits to).
  The issue title and body say what behavior needs fixing.
- Live: the same parts of `plan.md`, read against the issue body.

What good looks like: one change that removes the reported behavior, plus the
tests that prove it. Stated deferrals ("not in scope: X, because Y") are good.
A bundle of extra work (dependency bumps, migrations, rewrites, new options,
UI, retry frameworks, CI changes) is scope creep even when one item in it is
the right fix.

## Executability

Where it lives:
- Eval: the plan's "Approach" steps and "Files" line; any hedges such as
  "or", "maybe", "somewhere", "whichever", "not sure", "investigate".
- Live: the same parts of `plan.md`.

What good looks like: one chosen mechanism and a named place to put it (a file
path, a module, a named component). A stranger could open that file and start.
Leaving exact function names to the build is fine if the area is named and the
plan says how it will pin them.

## Test plan

Where it lives:
- Eval: the plan's "Test plan" section, read against the repro-evidence
  block's steps and Expected line.
- Live: the test plan in `plan.md`, read against the steps in the posted repro
  comment.

What good looks like: re-run the repro steps (or a named regression test of the
issue's case) with a stated expected result that would be different on the
broken code: an exit code, a printed value, a file contents, a behavior. "The
full suite passes" only proves nothing else broke; it does not prove the fix.

## Honesty

Where it lives:
- Eval: the plan's "Risk" or open-question lines, and any certainty claims in
  the diagnosis or comment ("this is definitely", "cannot affect").
- Live: the same parts of `plan.md`, plus its Deviations section after the
  build (what changed between the posted plan and the built change, and why).

What good looks like: the plan says what it has not checked yet (an unmeasured
cost, an untested platform, a sibling site it will look at) and what it will
do about it. False confidence is a claim the evidence does not back.

## Comms

Where it lives:
- Eval: the "Candidate plan comment" section, read against (1) the "Thread
  highlights" list, looking at comments by OWNER, MEMBER, CONTRIBUTOR, or
  COLLABORATOR accounts, and (2) the "Repo facts" block's contribution policy
  line, which states any AI policy.
- Live: the student's draft plan comment, read against the live issue thread
  (maintainer comments, linked PRs) and the repo's CONTRIBUTING.md or AI
  policy file. The student's `voice-guide.md` is checked separately.

What good looks like: if a maintainer named the culprit, proposed or rejected
an approach, posted a patch to test, or there is an open PR for the same fix,
the comment says so and says how the plan relates to it. If the repo requires
AI disclosure in comments (or "in any form"), the comment has a plain sentence
naming the tool and how much it helped. A policy that only asks for disclosure
in the PR, or only asks that comments be in your own words, does not require a
disclosure sentence in the comment.
