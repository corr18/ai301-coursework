# Evidence guide: where proof lives in a reproduction package

The map for every check in `rubric.md`. In eval mode the bundle is the
whole world: quote it, never fetch. In live mode, issue-side evidence
comes from GitHub and the candidate side is the student's draft files.

## Environment

- Where it lives:
  - Eval bundle: the first lines of `## Candidate repro report`
    (usually "Environment: ..."), compared against the issue body's own
    environment line, version mentions in `## Thread highlights`, and
    the bug-report template asks in `## Repo facts`.
  - Live: the top of the student's repro draft; the issue body and
    comments on GitHub; the repo's `.github/ISSUE_TEMPLATE/` files.
- What good looks like: the tool version, OS/platform, and install
  method are named, plus every setting the issue or an owner comment
  says changes the outcome (driver, shell, backend, build profile,
  language order). When the version or platform differs from the
  issue's, a sentence says so ("filed on 0.63.1; tested on 0.64.1").
  A version that differs silently, especially an older release against
  a bug confirmed on latest/main, is a deviation, not a record.

## Steps

- Where it lives:
  - Eval bundle: the numbered steps or the shell transcript in
    `## Candidate repro report`, plus any inline config/input files.
  - Live: the same parts of the student's repro draft.
- What good looks like: start from a clean state (fresh dir, empty
  config, a stated input file) and reach the issue's own trigger with
  commands or UI actions a stranger could type. Every input is shown or
  public. A step like "set up the project", "in our monorepo", or
  "with my usual config" breaks the chain. The trigger must be the
  issue's trigger: same syntax, same flag, same operator, same input
  shape. Five lines can be complete.

## Behavior shown

- Where it lives:
  - Eval bundle: code blocks and quoted output in
    `## Candidate repro report` (stdout/stderr, exit codes, logs,
    file contents, described screenshots), read against the error text
    and symptom in `## Issue` and owner trigger notes in
    `## Thread highlights`.
  - Live: the output blocks in the repro draft, read against the issue
    body on GitHub.
- What good looks like: put the artifact next to the issue's symptom
  and ask "is this the same failure?" Match on the kind of failure
  (panic vs. graceful error, crash vs. garbled output, exit 101 vs.
  exit 1, runtime error vs. compile error) and on the message. An
  artifact that only proves the program runs (a version banner, a
  session list) shows nothing. A changed input that produces a
  different error is an adjacent behavior. A control run (trigger
  removed, behavior gone) strengthens the case but is not required.

## Honesty

- Where it lives: the outcome sentences ("Reproduced", "Cannot
  reproduce", "Expected / Actual", "root cause is") in the repro report
  and any repro claims in the claim comment, each traced back to an
  artifact in the report.
- What good looks like: every claim points at something shown.
  "Reproduced" sits on an artifact of the issue's behavior; "cannot
  reproduce" sits on a real attempt with its output and names what
  differed and what a triggering setup likely needs (that passes).
  Red flags: certainty words ("guaranteed", "definitely", "verified",
  "on two machines") with no artifact behind them; a root cause
  asserted without shown evidence; scope widened to a platform or
  release that was not tested; expected/actual that contradict the
  shown output.

## Comms

- Where it lives:
  - Eval bundle: `## Candidate claim comment` read against `## Issue`;
    both comments read against the `contribution policy` and
    `bug reports` lines in `## Repo facts`.
  - Live: the claim draft against the GitHub issue; the repo's
    CONTRIBUTING.md, AI_POLICY.md / AI_USAGE_POLICY.md, and issue/PR
    templates; `scope.md` house rules.
- What good looks like:
  - The claim names this issue's specifics (its symptom, component, or
    trigger) and a concrete next step, and promises only investigation.
    "Kindly assign me, fixed in 2 days guaranteed" and "+1 same here"
    are boilerplate that could sit under any issue.
  - AI policy, three cases. (1) The policy requires disclosure ("all AI
    usage must be disclosed", "state the tool used"): treat the
    package as AI-assisted, and look for an explicit sentence in either
    comment saying AI was used and how; none means fail. (2) The policy
    requires human-written comments: first-person, specific wording
    satisfies it. (3) The policy is permissive ("welcome; you are
    responsible") or silent: nothing to disclose.
