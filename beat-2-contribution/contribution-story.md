# Contribution: README and `.env.example` disagree about which LLM API key to set

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** 1
**Student:** Caleb Orr
**GitHub Username:** corr18
**Issue:** https://github.com/codepath/pathreview-ai301-fa26-howard/issues/75
**Pull Request:** https://github.com/codepath/pathreview-ai301-fa26-howard/pull/94

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 9: Grew issue-select into issue-scout, ran my first scan, and patched two rubric misreads**

I installed the issue-scout skill over my unit 5 issue-select skill and wrote a search strategy from my fit profile: six `gh search issues` queries for config, env-var, API-key, and setup-docs work (plus C++ API and Python CLI work), and ranking filters that drop assigned, stale, frontend-labeled, and busy issues and put the fewest comments first. My first scan accepted 4 of its top 5, led by an env-var documentation issue in GenMotion, but it rejected toskar-model-data as unmaintained only because daily bot commits had pushed every human commit out of my "last 5 commits" check, so I changed `maintainer_liveness` to skip bot commits and count human-merged PRs. Grading my Issue Selection picks found a second gap: OpenImageIO#4781 is an umbrella issue the maintainer splits one header at a time, and my rubric rejected it for open PRs on *other* headers, so I added a carve-out that grades scope and claims for one named piece (`imagebufalgo.h` now passes), while Red-DiscordBot#6474 stays rejected because its PR is genuinely live. The scan also showed my fewest-comments ordering favors brand-new issues in small repos, which I logged as the next thing to tune.

**Unit 10: Nothing to update on**

Nothing to update on.

**Unit 11: Nothing to update on**

Nothing to update on.

**Unit 12: Nothing to update on**

Nothing to update on.

---

## Why I Chose This Issue

#75 fits my interests because API keys and `.env` configuration are integral to all work in a repo. Keeping that setup secure and making sure it runs smoothly matters in every project, so getting it right here is worth learning. At an estimated 1–2 hours, it also fits the time I have.

The verdict was right about the time it would take and about the actual problem: the API key in `README.md` and `.env.example` don't agree. What I weighed that the rubric could not was the syntax, the website, and the timing. Claiming it shouldn't be difficult, since it hasn't been taken: no assignee, no linked PR, and no comments.

I initially chose issue #75 based on its size and apparent scope, not because issue-scout had ranked it first. At that point, my fit profile was still empty, and issue-scout was developed afterward. However, the project became a useful opportunity to test the workflow I was building. I wanted to learn how to investigate an unfamiliar codebase, trace configuration settings from documentation into the actual implementation, and determine whether a seemingly documentation-focused issue required a deeper code-level understanding. It also gave me a real contribution to use when testing repro-check, plan-check, and pr-precheck.

Claim comment (2026-10-26): https://github.com/codepath/pathreview-ai301-fa26-howard/issues/75#issuecomment-6426755117

**Toolkit**

- Tool: issue-select (my rubric, later grown into issue-scout), run on 2026-10-25 first against three candidates (#75, #66, #60), then on #75 alone.
- What it produced: in the three-issue run, #75 and #66 accepted and #60 rejected on `unclaimed` (open PR #74), with #75 ranked first as the smallest job. On #75 alone: accept on all five checks (`maintainer_liveness`, `repository_in_use`, `newcomer_scope`, `unclaimed`, `ai_contribution_policy`), plus one warning: "The fix isn't purely docs … the issue mentions that `core/config.py` defines both keys. Check that file so the docs match what the code actually reads."
- Difficulties: the fit profile in `scope.md` was still a placeholder, so it ranked by size instead of fit; the `gh` commands needed approval and didn't run, so the evidence came from public GitHub pages.
- What changed: the warning shaped the next phases (I traced `core/config.py` before editing anything). In the eval, a numbered list of causes was read as an umbrella issue, so I added a "diagnosis, not an umbrella" clause to `newcomer_scope` (19/20 → 20/20).

---

## Understanding the Issue

**Problem Description**

The README (line 24) and `docs/SETUP.md` (line 47) both tell a new contributor to set `OPENROUTER_API_KEY` in `.env`, and `core/config.py` defines `openrouter_api_key`, but `.env.example` has no `OPENROUTER` line at all.

**Expected Behavior**

Someone who runs `cp .env.example .env` gets the variable the README tells them to set, and the `LLM_PROVIDER` comment lists every option the config supports.

**Current Behavior**

README.md and docs/SETUP.md both say to set `OPENROUTER_API_KEY`, but .env.example has no `OPENROUTER` line at all (grep exits 1). It only has `OPENAI_API_KEY`, and its comment offers only mock and OpenAI. So someone following the README ends up with a `.env` missing the variable the docs call required.

**Affected Components**

- `.env.example` (missing the key)
- `README.md` line 24 and `docs/SETUP.md` line 47 (the instructions that ask for it)
- `core/config.py` (`Settings`, which defines `llm_provider`, `openai_api_key` and `openrouter_api_key`)
- `ingestion/embeddings/provider.py` (`get_embedding_provider`, the only provider switch, which accepts `"mock"` and `"openai"`)

**Toolkit**

- Tool: issue-select's verdict and warning (above), then reading the files by hand.
- What it produced: the pointer to `core/config.py`, which turned "docs disagree" into "check what the code actually reads."
- Difficulties: none met.
- What changed: nothing.

---

## Reproduction Process

**Environment Setup**

macOS 26.6.2, git 2.39.5, fresh `git clone --depth 1` of `main` at `99673c7` (2026-09-16). This is a docs mismatch, so nothing needs to be installed or run for the reproduction itself. For the later `Settings()` check: Python 3.13.7, `pydantic-settings` 2.16.2. No setup challenges came up.

**Steps to Reproduce**

1. `git clone --depth 1 https://github.com/codepath/pathreview-ai301-fa26-howard.git && cd pathreview-ai301-fa26-howard` (HEAD `99673c7`)
2. `grep -n OPENROUTER README.md docs/SETUP.md` → README.md:24 and docs/SETUP.md:47 both ask for `OPENROUTER_API_KEY`
3. `grep -n OPENROUTER .env.example; echo "exit=$?"` → `exit=1`: no such line in the template

**Reproduction Evidence**

- **Commit showing reproduction:** upstream `main` at [`99673c7`](https://github.com/codepath/pathreview-ai301-fa26-howard/commit/99673c7), reproduced on a fresh clone; posted as the [reproduction comment](https://github.com/codepath/pathreview-ai301-fa26-howard/issues/75#issuecomment-6426965165) on 2026-10-26.
- **Screenshots/logs:**

```text
$ grep -n OPENROUTER README.md docs/SETUP.md
README.md:24:# Configure environment (add your OPENROUTER_API_KEY to .env)
docs/SETUP.md:47:# Edit .env and set your OPENROUTER_API_KEY (required for AI features)
$ grep -n OPENROUTER .env.example; echo "exit=$?"
exit=1
$ grep -n -A2 "LLM provider" .env.example
16:# LLM provider
17-# Options: "mock" (default, no API key needed), "openai"
18-LLM_PROVIDER=mock
$ grep -n -E "llm_provider|openai_api_key|openrouter_api_key" core/config.py
18:    llm_provider: str = Field(default="mock")
19:    openai_api_key: str = Field(default="")
20:    openrouter_api_key: str = Field(default="")
```

- **My findings:** core/config.py defines both keys, so the issue description matches what I see. One thing I noticed but hadn't confirmed yet: in core/config.py, I didn't find a line that maps an LLM_PROVIDER value to OpenRouter, so I didn't yet know what the valid value would be. Tracing that was my next step, before I changed any docs.

**Toolkit**

- Tool: repro-check, run live on my claim and repro drafts before posting.
- What it produced: accept, with one voice-guide note: "the claim gives line numbers (README line 24, SETUP.md line 47) but not the commit they come from. Adding 'on 99673c7' makes it match your rule." The posted claim says "On main at 99673c7", so I acted on it.
- Difficulties: in the eval, the first run scored 17/20, rejecting three honest reports (pkg-03 on `outcome-honest`, pkg-05 on `steps-rerunnable`, pkg-09 on `behavior-matches-issue`).
- What changed: loosened those three checks; a 7/7 partial re-run with canaries, then a 20/20 full run (commit `042a4a9`). Trade-off: a barely-tried cannot-reproduce could now pass the behavior check.

---

## Solution Approach

**Analysis**

Nothing in the code reads `settings.llm_provider` or maps any `LLM_PROVIDER` value to OpenRouter. `grep -rn "llm_provider" --include=*.py` only hits the field definition in `core/config.py`. The only provider switch in the code is `get_embedding_provider` in `ingestion/embeddings/provider.py`, which accepts `"mock"` and `"openai"`. So the `LLM_PROVIDER` comment in `.env.example` is accurate today, and the fix is to add the missing key, not to add an `openrouter` option the code doesn't accept.

**Proposed Solution**

One change, in one file: `.env.example`. Add `OPENROUTER_API_KEY=` under the `# LLM provider` block, with a one-line comment saying the README's AI setup uses it, and keep the `LLM_PROVIDER` options comment as `"mock"` and `"openai"`, since those are the values the code accepts.

Not changing: wiring an `openrouter` value for `LLM_PROVIDER` into the code (that's a feature, not this docs mismatch); adding `OPENROUTER_BASE_URL` or `OPENROUTER_MODEL` (`core/config.py` has working defaults for both); editing `README.md` or `docs/SETUP.md` (once `.env.example` has the key, they're correct as written); touching `core/config.py`.

**Implementation Plan**

1. Branch `fix/75-env-example-openrouter-key` from `main` on my fork.
2. Edit `.env.example`: add the `OPENROUTER_API_KEY=` line and its comment in the `# LLM provider` block.
3. Re-run my repro steps and the `Settings()` check.
4. Commit as `fix: add OPENROUTER_API_KEY to .env.example` with `Fixes #75`.

Deviations: nothing changed in the change itself; the plan held. What did change was the thread. My first plan comment (since deleted from the thread) was broader than this plan: it said I'd investigate the provider config, might change the `LLM_PROVIDER` comment, and would check that the README and setup docs explain the provider and key. By then the tracing was already done and showed that only `.env.example` needed to change, so that comment promised work I wasn't going to do. I posted a [second comment](https://github.com/codepath/pathreview-ai301-fa26-howard/issues/75#issuecomment-6537980056) that matches this plan. The order on the record: fix commit `227fb95` at 10:18 PM, then the first plan comment, then the second plan comment at 10:46 PM, 28 minutes after the commit.

**Toolkit**

- Tool: plan-check, run live on my plan draft before posting.
- What it produced: it flagged that my first draft said my repro showed the `Settings()` result, when my posted repro never ran `Settings()`.
- Difficulties: none in the eval (20/20 on the first rubric, commit `3017e32`); the live run's catch was the useful part.
- What changed: I reworded the diagnosis to say I checked `Settings()` after the repro. No rubric change.

---

## Testing Strategy

Re-run the repro steps on the branch, plus the closest check through the real code path: copy `.env.example` to `.env` and load it with `core.config.Settings`, the class the app reads its config from.

Before, on `main` at `99673c7`:

```text
$ grep -n OPENROUTER .env.example; echo exit=$?
exit=1
$ cp .env.example .env && python3 -c '...Settings()...'
llm_provider='mock' openrouter_api_key=''
```

After, on `fix/75-env-example-openrouter-key` at `227fb95`, with a test key filled into the copied `.env`:

```text
$ grep -n OPENROUTER .env.example; echo exit=$?
21:OPENROUTER_API_KEY=
exit=0
$ python3 -c '...Settings()...'
llm_provider='mock' openrouter_api_key='sk-or-test-value'
```

Before, `.env.example` has no `OPENROUTER` line (exit 1), so there is nothing to fill in. After, line 21 has it (exit 0), and once a value is filled in, `Settings()` loads it as `openrouter_api_key`. `llm_provider` stays `mock` in both. The repo's CI ran on the PR: all five jobs passed.

**Toolkit**

- Tool: none beyond plan-check's test-plan check (`test-proves-fix`); the test plan came from my plan.
- What it produced: the before/after above.
- Difficulties: none met.
- What changed: nothing.

---

## Implementation Notes

**2026-10-25**

Graded #75 with issue-select: accept on all five checks. Fixed the eval's umbrella misread (19/20 → 20/20).

**2026-10-26**

Claimed #75 at 4:37 PM and posted the reproduction at 4:48 PM, both after repro-check said accept.

**2026-10-30**

Traced `llm_provider` through the code: only the field definition reads it, and the only provider switch accepts mock and openai, so the fix is the missing key only. Committed the fix (`227fb95`, `.env.example` only), posted a first plan comment that was broader than the work, then replaced it at 10:46 PM with a narrower one matching the plan. Opened PR #94 at 11:04 PM ET.

**2026-11-20**

PR #94 still has 0 comments and 0 reviews.

**2026-11-25**

Ticked the five CI-backed boxes in PR #94's checklist (all five jobs green) and filled in the PR template's Summary, Changes, Screenshots / Demo, and Notes for Reviewers sections, with `Closes #75`. Left "New/updated tests" and the seeded-bug xfail box unticked: the PR adds no tests, and #75 (manifest entry H-06) has no xfail marker.

[Optional: add any other entries you remember, with dates, including wrong turns. Delete this bracket if there are none.]

**Toolkit**

- Tool: none for the build itself; repro-check, plan-check and pr-precheck gated each post.
- What it produced: n/a
- Difficulties: the plan comment was finalized after the fix was committed, so plan-check checked a plan that described finished work.
- What changed: I committed the implementation before posting my final plan, which meant plan-check was evaluating a plan after the work had already been completed. Looking back, I should have written and reviewed the plan before making code changes. That would have allowed plan-check to serve its intended purpose: identifying unsupported assumptions, narrowing the implementation scope, and catching problems before they became part of the code. I learned that the order in which I use the tools matters just as much as the checks they perform.

---

## Pull Request

**PR Link:** https://github.com/codepath/pathreview-ai301-fa26-howard/pull/94

**PR Description:** "fix: add OPENROUTER_API_KEY to .env.example". The body says the README and `docs/SETUP.md` tell contributors to set `OPENROUTER_API_KEY` in `.env` and that `core/config.py` reads it as `openrouter_api_key`, but a `.env` copied from `.env.example` lacked it; the change is commit `227fb95`, linked to #75. When I opened it on 2026-10-30, the template's Summary, Changes, Screenshots and Notes for Reviewers sections were left empty and the "Closes #" line had no number. On 2026-11-25, still before any review, I filled them in: the Summary names the one-line `.env.example` change; Changes records that the `LLM_PROVIDER` options were deliberately left as `mock` / `openai` and that README, SETUP and `core/config.py` are untouched; Screenshots / Demo has the before/after `grep` and `Settings()` output; Notes for Reviewers explains the empty value and offers the `LLM_PROVIDER` comment as a follow-up. The issue line now reads `Closes #75`, and the five CI-backed checklist boxes are ticked.

**Maintainer Feedback:**

- 2026-11-20: none yet (0 comments, 0 reviews on PR #94)

**Status:** Awaiting review (opened 2026-10-30, 11:04 PM ET, which GitHub shows as Oct 31; all five CI jobs green; no maintainer review yet)

**Toolkit**

- Tool: pr-precheck.
- What it produced: in the eval, 18/20 → 20/20 after revising `clean-diff` (unchanged lines around a real change are context, not churn) and `decisive-evidence` (bug failure modes must show after-output; added behavior can be stated). Commit `6d49437`.
- Difficulties: I did not save the verdict or the run output on PR #94, so I cannot reliably state what pr-precheck concluded or claim that it approved or rejected the PR. The PR went up with its template sections empty, and nothing on record shows the tool flagging that.
- What changed: PR #94 was opened with an empty pull request template, and I did not preserve a pr-precheck verdict from that run. This showed me that creating a tool and evaluating it successfully against test cases does not automatically mean I am using it effectively in a real contribution. Next time, I would complete the PR template first, run pr-precheck against the actual pull request description and diff, and save its verdict before opening the PR. This would make the tool part of my submission process instead of something I only use during development.

---

## Learnings and Reflections

**Technical Skills Gained**

Through this contribution, I improved my ability to trace configuration settings through an unfamiliar codebase, particularly understanding how values defined in `.env` files connect to the application's `Settings()` configuration. I also learned how to use command-line tools like `grep` and interpret exit codes as evidence when verifying whether a configuration key existed. Beyond the implementation itself, I gained experience creating before-and-after reproductions and tuning AI evaluation rubrics against test sets. I learned that a tool's evaluation score only matters if its decisions make sense when applied to real work.

**Challenges Overcome**

One of my biggest challenges was getting the evaluation criteria to distinguish between situations that looked similar but required different judgments. For example, issue-select initially rejected a legitimate issue because it interpreted a maintainer's numbered diagnosis as an umbrella issue. I also struggled with repro-check rejecting honest reproduction reports because its original requirements were too strict. I addressed these problems by examining the false rejections, adjusting the rubric, and rerunning evaluations to make sure the changes fixed the issue without allowing previously rejected cases to pass incorrectly.

**What I'd Do Differently Next Time**

If I were starting this contribution again, I would follow the tools in their intended order instead of allowing implementation to get ahead of planning. I would write and validate my plan before touching the code, complete the pull request template before submission, and run pr-precheck against the actual PR description and code changes. I would also save every tool verdict and evaluation result so I could refer back to the evidence later. Finally, I would be more proactive about following up on a pull request that has not received maintainer feedback.

---

## Resources Used

- [Issue #75](https://github.com/codepath/pathreview-ai301-fa26-howard/issues/75): the issue thread itself, which named `core/config.py`.
- The repo's own `README.md` (line 24), `docs/SETUP.md` (line 47), `core/config.py` and `ingestion/embeddings/provider.py`, which defined what the fix had to match.
- `docs/CONTRIBUTING.md` and the PR template, for the repo's contribution rules.
