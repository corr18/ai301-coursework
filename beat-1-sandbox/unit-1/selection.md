# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-howard/issues/75

**Verdict output**

```
I recommend taking **issue #75** as your first issue. It passed all five required checks in your rubric, so the verdict is **accept**.

| Check | Grade | Evidence |
|---|---|---|
| maintainer_liveness | pass | The newest of the last 5 commits on `main` is from Sep 16, 2026 (39 days ago). Its author is Aburke225, a human account, with `claude` as co-author. |
| repository_in_use | pass | There's no archived banner. The last push was Sep 16, 2026, well inside 365 days. The repo has no releases. |
| newcomer_scope | pass | It asks for one bounded fix: make `README.md` and `.env.example` agree on `OPENROUTER_API_KEY` and the `LLM_PROVIDER` options. It's labeled good first issue and tier-1, with an estimated 1–2 hours. |
| unclaimed | pass | Nobody is assigned, no pull requests are linked, and there are no comments at all. |
| ai_contribution_policy | pass | `docs/CONTRIBUTING.md` and the PR template say nothing about AI use. CONTRIBUTING.md does require tests for code changes. |

Things to know:
- **Fit profile is empty.** The "Your fit profile" section of `scope.md` still has its placeholder text. With only one candidate that didn't matter here, but if you grade several issues at once there's nothing to rank them by.
- **How I got the evidence.** The `gh` commands needed approval and didn't run, so everything above comes from the public GitHub pages. The commit list didn't show any pull request merges, and I didn't check the files in `ISSUE_TEMPLATE/`.
- **The fix isn't purely docs.** It's mostly doc edits, but the issue mentions that `core/config.py` defines both keys. Check that file so the docs match what the code actually reads.

{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-howard/issues/75",
  "checks": [
    {"name": "maintainer_liveness", "grade": "pass", "evidence": "Newest of last 5 main commits is 2026-09-16 (39 days ago), authored by human account Aburke225 (co-authored with claude)."},
    {"name": "repository_in_use", "grade": "pass", "evidence": "No archived banner; last push 2026-09-16, within 365 days; no releases."},
    {"name": "newcomer_scope", "grade": "pass", "evidence": "Single bounded fix: make README.md and .env.example agree on OPENROUTER_API_KEY / LLM_PROVIDER; labeled good first issue, tier-1, est. 1-2 hours."},
    {"name": "unclaimed", "grade": "pass", "evidence": "Assignees: none; Development: no branches or pull requests; zero comments."},
    {"name": "ai_contribution_policy", "grade": "pass", "evidence": "docs/CONTRIBUTING.md and PULL_REQUEST_TEMPLATE.md are silent on AI; only general testing requirements."}
  ],
  "verdict": "accept"
}
```

---

## Eval iterations

**Run history**

1. Full run, first draft rubric: **19/20** (missed issue-19, rejected on scope).
2. `--only issue-19` after adding a "diagnosis, not an umbrella" clause: 1/1.
3. Switched to my own rewritten rubric; `--only issue-19,issue-01`: **1/2** (issue-01 agreed, issue-19 rejected again on `newcomer_scope`).
4. Added the same clause to `newcomer_scope`; `--only issue-19`: 1/1.
5. Final full run, saved with `--save-run eval-run.txt`: **20/20** (claimed 4/4, clear-accept 8/8, dead-repo 3/3, policy 1/1, scope 4/4).

**Issue analysis**

issue-19 (zxcalc/zxlive#517, "Selecting large subgraphs in proof mode freezes the UI").
Gold label: **accept**. My rubric's decision before the fix: **reject**, failing
`newcomer_scope`. The body lists "two potential causes which should be fixed" and then
three "Additional suggestions" (multi-processing, lazy matching, threading). The grader read
that numbered list as an umbrella of separate tasks, which my scope check fails. But it is one
named bug, a UI freeze, filed and diagnosed by a COLLABORATOR, with no comments, assignee, or
linked PR. The list is a maintainer's diagnosis, not a tracking list. After I said that
explicitly in the check, the rubric accepted it, matching gold.

**Check rationale**

`newcomer_scope`, pass condition as currently written:

> The issue asks for one concrete, bounded outcome: it states a specific behavior, change, or set of closely related edits, and it is not a support question, an explicit umbrella/tracking issue (a numbered list of causes or suggested fixes for one named bug is a diagnosis, not an umbrella), an unresolved design/product decision, or an issue whose thread says it requires broad core-internals work.

Each exclusion maps to a scope-category eval issue: the umbrella clause catches issue-05
(codebase-wide type annotations) and issue-10 (a "megaissue"), the design-decision clause
catches issue-15 (years of debate, abandoned PRs) and issue-20 (a one-line feature wish). "Set
of closely related edits" keeps multi-file but single-purpose work such as issue-01's docs
checklist from failing. The parenthetical was added after issue-19 showed that a bare
"umbrella" exclusion also catches a maintainer's numbered diagnosis of one bug.

**Trade-offs**

The parenthetical makes the umbrella exclusion more lenient: a real tracking issue whose items
are all framed as "causes" of one symptom could now slip through. I accepted that risk, and I
checked that it changed nothing else: the final full run still rejected all four scope issues
(scope 4/4), including the umbrella cases issue-05 and issue-10, and the only verdict that
changed was issue-19 (reject → accept).

---

## Selection rationale

**Selection rationale**

1. #75 fits my interests because API keys and `.env` configuration are integral to all
   work in a repo. Keeping that setup secure and making sure it runs smoothly matters in
   every project, so getting it right here is worth learning. At an estimated 1–2 hours,
   it also fits the time I have.
2. The verdict was right about the time it would take and about the actual problem: the
   API key in `README.md` and `.env.example` don't agree.
3. Claiming it shouldn't be difficult, since it hasn't been taken: no assignee, no linked
   PR, and no comments.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
