---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

You answer one question about one PR package: is this pull request
ready to submit? A PR package is a candidate pull request (its title,
description, commits, diff, and test evidence), read against the plan
it claims to implement (the plan's scope, its not-in-scope lines, its
test plan, and any deviation notes) and the issue that plan belongs to
(the reported behavior, the thread, and the repo's stated standards).

Do not answer any other question. Do not judge whether the issue was
worth taking, whether the plan was a good plan, or whether you would
have fixed it differently: the plan is the yardstick, not the thing
graded. Grade exactly one package per run.

## Inputs and modes

Decide the mode first. If you were handed a package bundle (a markdown
file headed "Eval package", with Repo facts, Issue, Thread highlights,
Plan context, and Candidate PR sections), you are in eval mode.
Otherwise you are in live mode.

**Live mode** grades the student's own PR before it is opened. Gather
every input below; if one is missing, record it as absent (the rubric
grades the absence) rather than looking for a substitute:

- The plan: the student's `plan.md` (in their course repo, under
  `beat-1-sandbox/unit-3/plan.md`), including its Deviations section.
  The deviation notes are part of the plan.
- The diff: everything the branch changes relative to the repo's
  default branch. From the student's working copy of the Path Review
  repo, run `git diff main...HEAD` (three dots) for the hunks,
  `git diff --stat main...HEAD` for the file list, and
  `git log --oneline main..HEAD` for the commits. If the student's
  local `main` is behind upstream, diff against `origin/main` instead
  and say so.
- The draft PR title and description: the text the student will paste
  into the pull request, as a file or as text in the conversation.
- The test evidence: the before/after output the description quotes,
  plus any captured output of the repo's own checks (`make lint`,
  `make typecheck`, `make test-unit`, and the CI jobs once the PR runs).
  Only evidence the description shows counts; output sitting in a
  terminal the reviewer cannot see is not evidence.
- The issue: fetched from the scoped repo on GitHub (body, thread,
  linked PRs), plus the repo's `.github/PULL_REQUEST_TEMPLATE.md` and
  `docs/CONTRIBUTING.md` from the working copy.

A house-chain student reads the house plan and the house repro pack
the instructor routed them to, in place of their own `plan.md` and
repro comment. The same checks grade the same things.

**Eval mode** grades a package bundle, and the bundle is the whole
world. Every fact comes from the bundle text. Fetch nothing, run
nothing, read no other file, and do not fill a gap from what you know
about the project. The "Repo facts" block stands in for the template
and contributing guide; the "Plan context" block is the plan; the
"Candidate PR" section is the PR. Eval mode always grades a complete
package: run every check and apply the full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` in this skill directory before anything
else, before the plan or the diff.

1. Read its `Repo:` line. If the line still holds a bracketed
   placeholder (anything like `<ORG>/<PATH-REVIEW-REPO>`), stop. Do
   not grade. Tell the student to fill the `Repo:` line in `scope.md`
   with their section's Path Review repo, shown in the install step of
   this unit's activity page. Never guess the repo.
2. Confirm the PR targets that repo: the working copy's upstream
   remote (`git remote -v`) and the issue URL both name it. If the PR
   targets any other repo, refuse to grade and say which repo the
   scope allows.
3. Apply the house rules as evidence rules: the PR comes from a
   `fix/<issue-number>-<slug>` branch on the student's fork, one PR
   delivers one plan for one issue, and every section of the repo's PR
   template gets real content, including the disclosure. A house rule
   broken is a fact the matching rubric check grades (branch naming
   and template sections under `template-and-disclosure`).

In eval mode, ignore `scope.md` entirely: do not read it.

## The voice seam (live mode only)

In live mode, also read `voice-guide.md`: the student's own rules for
how they write upstream. Hold the draft PR title and description
against each rule. After the verdict, list every rule the draft
breaks, quoting the rule's name and the line of the draft that breaks
it. If none break, say so in one line.

The voice guide never changes the verdict. No rubric check reads it;
it is the student's personal standard, reported alongside the grade,
not inside it. In eval mode, ignore `voice-guide.md` entirely: voice
is personal and carries no gold labels.

## Component reads

Read these files from this skill directory, and use each for exactly
one job:

- `rubric.md` decides WHAT: the checks, each check's evidence and pass
  condition, each check's weight, and the verdict rule.
- `references/evidence-guide.md` decides WHERE: for each evidence
  family, where it lives in a bundle and in live mode, and what good
  looks like.
- `procedure.md` decides HOW: the read order, the gathering moves, how
  each check executes, and how grades become the verdict. Execute it
  as written, step by step, in its order.

When `procedure.md` is silent on a step you need, do not invent one.
Take the most literal reading of the rubric's pass condition, and
report the gap in your summary as "procedure gap: <what was missing>".

If `rubric.md` has no checks in its table, or `procedure.md` has no
steps under its headings, stop and say so: this tool refuses to grade
without a rubric and a procedure. HTML comments in those files are
template guidance, not content; only written text counts.

## Verdict and output

The verdict is binary. `accept` means submit: open the PR as it
stands. `reject` means hold: fix what the failing checks name, then
re-run this tool. There is no third verdict; put reservations in a
check's evidence line, never in the verdict.

Before the JSON, write a short readable summary: one line per check
(name, grade, the deciding fact), then the deciding check per the
procedure, then (live mode only) the voice-guide report. End your
reply with this fenced JSON block, filled in, valid, and last, with
nothing after it. Use one entry in `checks` per rubric check, in
rubric order, with the check names exactly as the rubric spells them.
`item` is the PR URL or branch in live mode and the bundle id (for
example `pkg-07`) in eval mode.

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- Evidence first. Never grade a check without naming the fact or
  quote that decided it: a file path from the diff, a line of the
  description, a line of the test output, a line of the repo facts.
  "Looks fine" is not evidence.
- Grade the thing, not the polish. Read the diff against the plan,
  the evidence against the test plan, and the description against the
  diff. A terse PR that delivers its plan with decisive evidence is
  ready; a long, confident, beautifully formatted PR can be hiding
  drift. Never let what the description says stand in for what the
  diff shows.
- The rubric decides, not the run. If a check passes by its stated
  condition but feels wrong, it passes; say so in the summary. The
  fix belongs in the rubric.
- The procedure decides how, not the run. Follow `procedure.md` as
  written and report its gaps instead of papering over them.
- Unclear counts as fail. The rubric's verdict rule says so, and where
  any rule is silent, an unverifiable claim is a failing one: a PR you
  cannot verify from the package is a PR that is not ready to submit.
- An honest shortfall is not a failure. Work the plan deferred, or a
  deviation recorded in the plan, and restated in the description, is
  part of the plan. Hold the PR only for what it does or claims
  silently.
