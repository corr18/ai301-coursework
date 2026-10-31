# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-howard/pull/94

**Branch**

fix/75-env-example-openrouter-key

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Partial smoke run, `--only pkg-01,pkg-09,pkg-13` (the three packages I
   thought were closest calls: a template wall, adjacent extra scope, and a
   disclosed deferral): **3/3**. Not a scored run; it checked that the tool
   parsed and called the borderline cases before I spent a full run.
2. Full run, first rubric: **18/20** (clear-accept 5/7, not-tested 4/4,
   silent-drift 4/4, standards-wall 2/2, unreviewable 3/3). It passed the bar,
   but pkg-02 failed `clean-diff` and pkg-05 failed `decisive-evidence`, and
   both are gold accepts.
3. Partial re-grade after revising those two checks, `--only
   pkg-02,pkg-05,pkg-18,pkg-07,pkg-14,calib-04 --include-calibration`:
   **5/5 scored**, and calib-04 stayed reject. pkg-02 and pkg-05 flipped to
   accept; the canaries held.
4. Confirming full run, saved with `--save-run`: **20/20** (clear-accept
   7/7, not-tested 4/4, silent-drift 4/4, standards-wall 2/2,
   unreviewable 3/3). This is the committed `eval-run.txt`.

**Package analysis**

pkg-05 (nushell/nushell#18848, same-name keybindings collapsing on merge).
Gold label: **accept**. My rubric's decision: **reject** in run 2, failing
only `decisive-evidence`; **accept** in the final run.

The plan's test plan names two cases: "re-run the issue's script, expect
both `atuin` rows; re-run with two same-name same-key bindings, expect one
row plus a warning." The evidence pastes the issue script before (one
`atuin` row, `up`) and after (two rows, `char_r` and `up`), names
`cargo test -p nu-protocol` (312 tests), and says in prose "Same-key
redefine prints the one-time warning." My first `decisive-evidence` said
"every repro or failure case the test plan names is re-run," and the grader
read the same-key case as a test-plan case with no output, so it failed it:
"the plan's same-name same-key case is only asserted in prose ('prints the
one-time warning') with no output, row count or warning text shown."

That reading followed my wording, but the wording was wrong. The same-key
case isn't a way the bug shows up; it checks new behavior the fix adds (the
warning). The bug itself, rows disappearing, is shown before and after.
After I split "failure modes of the bug" (their after must be shown) from
"added behavior" (a stated result is enough), the final run passed it with
"Issue script before shows only 'up' row, after shows char_r and up rows;
same-key warning result stated."

**Check rationale**

`clean-diff`, pass condition as it now reads:

> Pass if every hunk is part of the fix or its tests, docs, or changelog, so a reviewer reads only the change. Adding the one import the fix uses is fine. Fail if the diff adds any of: a debug print or log (active or commented out), a commented-out block or earlier attempt, dead code (an unused function or variable, anything behind `allow(dead_code)` or an `_unused` name), a new TODO/FIXME, formatting-only or whitespace-only churn, a run of lines removed and re-added unchanged with no changed line among them (pure re-print churn), or an import reorganization the fix did not need. Lines removed and re-added unchanged around a real change in the same block (a multi-line call where one argument changes) are diff context, not churn.

The first version said "hunks whose removed and added lines are identical."
I wrote that for pkg-18, where a whole block of `println!` lines is removed
and re-added unchanged. But in run 2 it rejected pkg-02 (rich), a gold
accept. Its `save_html` and `save_svg` hunks remove and re-add
`html = self.export_html(` and `theme=theme,` unchanged, because the diff
rewrites the whole call to change one argument, `clear=clear` to
`clear=False`. The grader's evidence was "save_html/save_svg hunks remove
and re-add identical lines ... which the rubric fails as identical
remove/add churn." It's the same diff pattern with the opposite meaning: in
pkg-02 the unchanged lines are context around a real change, and in pkg-18
nothing changes. So the check now names the difference: churn is a run of
unchanged lines "with no changed line among them," and the last sentence
calls out the multi-line-call case. I didn't drop the rule, because
pkg-18's gold label depends on it.

**Trade-offs**

Both revisions loosened a check, so before the confirming full run I re-ran
canaries with `--only`: pkg-18 (unreviewable, the pure re-print churn the
old `clean-diff` wording was written for), pkg-07 and pkg-14 (not-tested,
evidence that runs only a control or an unchanged path), and calib-04 (the
trap where the plan names two failure modes and the evidence re-runs only
one). All four stayed reject while pkg-02 and pkg-05 flipped to accept, and
the confirming full run kept every category matched.

What the looser `clean-diff` gives up: someone could hide a re-indent of
untouched lines inside a block where one line really changes, and my check
would call it context. What the looser `decisive-evidence` gives up: a
"warning prints" sentence with no output now passes for added behavior, so
a PR could state a result it never actually saw. I accept both, because the
stricter versions rejected two PRs the gold set says are ready, and the
failure-mode evidence (the part that proves the bug is gone) still has to
be shown.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
