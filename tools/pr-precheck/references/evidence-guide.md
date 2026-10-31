# Evidence guide: where evidence lives in a PR package

Each family below says where to look (eval bundle first, then live mode) and
what good looks like. In eval mode the bundle text is the whole world. In live
mode the PR is graded the way a reviewer will read it once opened: only what
the title, description, commits, and diff show counts.

## Plan fidelity (harness category: silent-drift)

Where it lives:
- Eval: the "Plan context" block: the plan's scope ("one bounded change"),
  its "Not in scope" line, its "Files" line, and any deviation or deferral
  note. Read against the "Diff" section (each `+++ b/<path>` header and each
  `@@` hunk) and the "Description" section's claims.
- Live: the student's `plan.md` (Scope, Not in scope, Approach, and the
  Deviations section). Read against `git diff --stat main...HEAD` and
  `git diff main...HEAD` from the working copy, and the draft description.

What good looks like: every changed file is on the plan's files line (or is a
test, doc, or changelog file for the change), and every hunk does a piece of
the planned work. Anything the plan scoped out is absent. Anything planned but
missing is named as deferred in both the plan and the description. Silent
drift runs both ways: more than the plan (a new option, a rename pass, a
rewrite, reworded unrelated text) or less than the plan with no note. A
description that claims "exactly as planned", "no changes beyond the fix", or
a deliverable the diff does not contain is drift too, however good the rest
reads.

## Test evidence (harness category: not-tested)

Where it lives:
- Eval: the "Test evidence" section and any before/after blocks in the
  "Description", read against the plan's "Test plan" line and the "Repro
  evidence" line in the plan context.
- Live: the before/after output the draft description quotes (re-run of the
  student's repro steps and test-plan steps on `main` and on the branch), and
  the outcome of the repo's own checks: `make lint`, `make typecheck`,
  `make test-unit`, and once the PR is open, the five CI jobs.

What good looks like: the issue's failing case, as the test plan names it, is
re-run with an observable before and after that would differ on broken code
(an exit code, a printed value, a behavior). Every case the test plan names
is re-run, not one of two. The run goes through the path the fix changed, not
a control that never broke. The repo's suite or checks are named with their
result. "Tested locally", "works now", or "tests pass" with nothing named is
not evidence.

## Diff quality (harness category: unreviewable)

Where it lives:
- Eval: the "Diff" section (every `+` and `-` line) and the "Commits" list.
- Live: `git diff main...HEAD` and `git log --oneline main..HEAD`.

What good looks like: a reviewer reads only the change. Every hunk is part of
the fix, its tests, or its docs. The debris tells are lines added for the
author's own debugging (`println!`, `eprintln!`, `print(`, `console.log`,
`DEBUG` logs, active or commented out), commented-out code or first
attempts, dead functions or variables (`_unused`, `allow(dead_code)`), new
TODO or FIXME comments, and churn: re-indents, re-wrapped lines, re-ordered
or re-grouped imports, and hunks that remove and re-add identical lines.
Commit messages like "wip", "fmt", or "misc cleanups while debugging" point
at where the debris is.

## Standards and comms (harness category: standards-wall)

Where it lives:
- Eval: the "Repo facts" block: the "pull requests" line (template sections,
  required checklist items, issue-link line, changelog or whatsnew asks,
  title conventions) and the "contribution policy" line (any AI policy).
  Read against the "Title", "Description", "Commits", and "Diff".
- Live: the repo's `.github/PULL_REQUEST_TEMPLATE.md` (Summary, Issue with
  `Closes #`, Changes, Testing checklist, Screenshots / Demo, Notes for
  Reviewers) and `docs/CONTRIBUTING.md` (branch naming
  `<type>/<issue-number>-<description>`, conventional commits, green CI), plus
  the house rules in `scope.md`. The course requires an AI-use disclosure in
  every PR description, so live mode reads the policy as "disclose in the PR".

What good looks like: each section the template asks for is present with real
content, each required checklist item is met or honestly marked, the
issue-link line carries the real issue number, and a changelog or whatsnew
entry is in the diff when the repo asks for one. If the policy requires AI
disclosure in the PR or "in any form", the description says plainly that AI
was used, which tool, and how much. A policy that only asks for comments in
the contributor's own words needs no disclosure sentence. Limits and
deferrals the description states belong here too: naming what was not
verified reads as honesty, not weakness. Whether the description's claims
match the diff is plan fidelity, above.
