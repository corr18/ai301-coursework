# Toolkit Portfolio

**Student:** Caleb Nathaniel Orr

## Featured Tools

I chose these three because they show the clearest progression from finding a contribution, to proving the issue, to deciding whether a proposed pull request is ready. issue-scout also shows how an early Unit 1 rubric grew into a broader discovery workflow, while repro-check and pr-precheck show how I used evaluation misses to revise the standards before applying them to the issue #75 contribution and PR #94.

**issue-scout**

**What it is, and the judgment hole I wrote:** issue-scout searches GitHub for possible newcomer contributions, ranks the results, and then applies a rubric to decide whether an issue is a realistic first contribution. The judgment I added is the five-check rubric: `maintainer_liveness`, `repository_in_use`, `newcomer_scope`, `unclaimed`, and `ai_contribution_policy`. I also defined the fit profile around configuration, `.env`, and secrets work, with small, focused tier-1 issues. The six-query search strategy and ranking filters turn that profile into a repeatable search instead of relying on issue labels or a single broad search.

**How it evolved, dated:**

- 2026-10-25 (`a7bcd98`): issue-select eval 19/20 → 20/20 after adding a "diagnosis, not an umbrella" clause to `newcomer_scope` (issue-19, zxlive#517). The original rule treated a maintainer's numbered list of causes or suggested fixes as an umbrella issue, even when the list described one specific bug.
- 2026-11-03 (scan notes; recorded in `f969786` on 2026-11-07): rebuilt as issue-scout with four `gh search issues` queries and ranking filters, growing to six queries on 2026-11-06. The earlier workflow depended too much on manually supplied candidates; the expanded search made discovery part of the tool and let the rubric evaluate a wider set of issues.
- 2026-11-06 (scan notes; recorded in `f969786` on 2026-11-07): umbrella carve-out that grades one named piece, after OpenImageIO#4781 was rejected for PRs on other headers. The rubric was too broad in how it interpreted related work: activity on other headers should not automatically disqualify a separately scoped, named fix.
- 2026-11-07 (scan notes; recorded in `f969786`): `maintainer_liveness` changed to skip bot commits and count human-merged PRs, after toskar-model-data was wrongly called unmaintained. Bot activity had been distorting the signal, so I changed the check to focus on evidence of actual human maintenance.

**Evidence:**

Before/after, `newcomer_scope` pass condition (2026-10-25, `a7bcd98`):

```text
Before: ...it is not a support question, an explicit umbrella/tracking issue, ...
After:  ...it is not a support question, an explicit umbrella/tracking issue
        (a numbered list of causes or suggested fixes for one named bug is a
        diagnosis, not an umbrella), ...
```

From the scout's real verdict on issue #75 (2026-10-25):

```text
The fix isn't purely docs. It's mostly doc edits, but the issue mentions that
`core/config.py` defines both keys. Check that file so the docs match what the
code actually reads.
```

Unit 9 scan (2026-11-07, `f969786`): accepted 4 of its top 5. After the carve-out (regrade on 2026-11-06), `imagebufalgo.h` (OpenImageIO#4781) passes; Red-DiscordBot#6474 is still rejected because its PR is genuinely live.

This evidence shows that the rubric is not just looking for beginner labels: it checks scope and maintenance signals, and the changes made it less likely to reject a focused contribution for the wrong reason.

**repro-check**

**What it is, and the judgment hole I wrote:** repro-check evaluates a reproduction package—the issue claim, reproduction steps, and reported outcome—and decides whether the evidence is credible and ready to share. My judgment is expressed in checks such as `behavior-matches-issue`, `outcome-honest`, and `steps-rerunnable`. The important distinction is between a report that honestly says an attempted reproduction did not happen and a report that claims to have reproduced the issue but does not show the issue's actual behavior.

**How it evolved, dated:**

- 2026-10-26 (`042a4a9`): first full eval 17/20, missing three gold accepts (pkg-03 `outcome-honest`, pkg-05 `steps-rerunnable`, pkg-09 `behavior-matches-issue`). The first rubric was too strict about valid reports: it rejected honest outcomes, reproduction steps that were still useful to another person, and a genuine attempt that did not reproduce the issue.
- 2026-10-26 (`042a4a9`): loosened those three checks; canary run 7/7, confirming run 20/20. I adjusted the pass conditions to accept those legitimate cases while keeping the cannot-reproduce allowance limited to reports that show the attempted trigger and clearly state that the issue did not reproduce.

**Evidence:**

Rule added to `behavior-matches-issue` (2026-10-26, `042a4a9`):

```text
A cannot-reproduce passes here when it is a genuine attempt at the issue's
trigger with its output shown, ... This allowance covers only reports that say
they did NOT reproduce; a report that claims to reproduce must show the issue's
own behavior.
```

Real use: from the reproduction comment posted on #75 ([issuecomment-6426965165](https://github.com/codepath/pathreview-ai301-fa26-howard/issues/75#issuecomment-6426965165)):

```text
$ grep -n OPENROUTER README.md docs/SETUP.md
README.md:24:# Configure environment (add your OPENROUTER_API_KEY to .env)
docs/SETUP.md:47:# Edit .env and set your OPENROUTER_API_KEY (required for AI features)
$ grep -n OPENROUTER .env.example; echo "exit=$?"
exit=1
```

The output made the gap concrete: the documentation told users to configure `OPENROUTER_API_KEY`, but the example environment file did not include that key.

A catch from repro-check's live run on my claim draft (2026-10-26), before I posted it:

```text
Verdict: accept … Voice-guide notes:
- "Say where I ran it": the claim gives line numbers (README line 24, SETUP.md line 47)
  but not the commit they come from. Adding "on 99673c7" makes it match your rule.
```

The posted claim ([issuecomment-6426755117](https://github.com/codepath/pathreview-ai301-fa26-howard/issues/75#issuecomment-6426755117)) opens with "On main at 99673c7".

**pr-precheck**

**What it is, and the judgment hole I wrote:** pr-precheck evaluates a proposed pull request against the plan and issue it claims to implement, then decides whether the PR is ready to submit. My judgment is in checks such as `clean-diff` and `decisive-evidence`: the first distinguishes harmless surrounding context from actual unexplained churn, while the second asks whether the evidence is strong enough for the particular kind of change being claimed.

**How it evolved, dated:**

- 2026-10-30 (`6d49437`): first full eval 18/20; `clean-diff` rejected pkg-02 (rich) and `decisive-evidence` rejected pkg-05 (nushell), both gold accepts. The original checks confused unchanged context around a legitimate edit with meaningless churn, and demanded output for a case where a clearly stated result was sufficient.
- 2026-10-30 (`6d49437`): redefined churn as unchanged lines "with no changed line among them", and split bug failure modes (after must be shown) from added behavior (a stated result is enough). Canaries pkg-18, pkg-07, pkg-14 and calib-04 still rejected; final run 20/20. This preserved the useful rejections while correcting the two false rejections from the first run.
- 2026-10-31: used on PR #94. I did not save the verdict or the run output, so I cannot reliably state what pr-precheck concluded or claim that it approved or rejected the PR.

**Evidence:**

Before/after on PR #94's fix (branch `fix/75-env-example-openrouter-key`):

```text
Before (main @ 99673c7):
$ grep -n OPENROUTER .env.example
exit=1

After (227fb95):
$ grep -n OPENROUTER .env.example
21:OPENROUTER_API_KEY=
exit=0
$ python3 -c 'from core.config import Settings; ...'
llm_provider='mock' openrouter_api_key='sk-or-test-value'
```

The case that drove the `clean-diff` change: in pkg-02 the unchanged lines `html = self.export_html(` and `theme=theme,` were context around a real one-argument change (`clear=clear` → `clear=False`). In pkg-18 the same pattern was pure `println!` churn.

The before-and-after output demonstrates the actual scope of PR #94: the missing example key was added, and the configuration check then showed the key could be read.

## Inventory

- plan-check: Installed 2026-10-30 (`3017e32`); its first evaluation scored 20/20 and I did not revise it. In live use, it caught my plan draft claiming a `Settings()` result that the reproduction evidence had not shown.
- issue-select: The Unit 1 issue-grading tool from 2026-10-25; its rubric and fit profile became the foundation for issue-scout in Unit 9 on 2026-11-03.

## The Arc

**Beat 1**

Date: 2026-10-25

Friction: issue-select rejected issue-19 even though it was a gold accept, because it read a maintainer's numbered diagnosis of one bug as an umbrella issue.

Change: I added the "diagnosis, not an umbrella" clause to `newcomer_scope`, distinguishing a list of causes or fixes for one named bug from a tracking issue.

Result: The evaluation improved from 19/20 to 20/20; the four scope issues remained rejected, and issue #75 was accepted.

**Beat 2**

Date: 2026-10-26

Friction: repro-check rejected three honest gold-accept reports, leaving the first full evaluation at 17/20.

Change: I loosened `outcome-honest`, `steps-rerunnable`, and `behavior-matches-issue`, while limiting the cannot-reproduce exception to genuine attempts with their output shown.

Result: The canary run held at 7/7, the confirming run reached 20/20, and I posted the reproduction for issue #75.

**Beat 3**

Date: 2026-10-30

Friction: My plan draft claimed a `Settings()` result that the posted reproduction did not show.

Change: plan-check's live run flagged the unsupported claim, so I corrected the plan comment and replaced my first comment with a more carefully scoped explanation.

Result: The plan aligned with the evidence, and commit `227fb95` changed only `.env.example`.

**Beat 4**

Date: 2026-10-30

Friction: pr-precheck rejected two ready PR packages in its first full evaluation, scoring 18/20.

Change: I refined the `clean-diff` churn definition and separated evidence requirements for bug failure modes from those for added behavior.

Result: The confirming evaluation reached 20/20; PR #94 was opened on 2026-10-31. It had not received a review at the time recorded in this portfolio.

**Beat 5**

Date: 2026-11-06 to 2026-11-07

Friction: issue-scout treated a repository as inactive because bot commits distorted its activity signal, and it rejected a focused OpenImageIO issue because related PRs touched other headers.

Change: I added the one-named-piece carve-out for scope, then changed `maintainer_liveness` to skip bot commits and count human-merged PRs.

Result: `imagebufalgo.h` passed after the carve-out, while Red-DiscordBot#6474 remained rejected because its PR was genuinely live.

**Beat 6**

Date: 2026-11-07

Friction: The fewest-comments ranking can push brand-new issues in small repositories above stronger candidates, even when low comment count is not enough evidence of contribution quality.

Change: I logged the ranking skew as a known issue, but did not change the ranking behavior.

Result: none

**Beat 7**

Date: 2026-11-25

Friction: PR #94 had received no review since it was opened on 2026-10-31, so the contribution had not yet received maintainer feedback.

Change: none

Result: none

## Limitations and What's Next

The toolkit helps me find issues and evaluate reproduction and pull-request evidence, but it does not make the final decision for a maintainer or guarantee that a contribution will be reviewed or merged. Its judgments depend on the evidence GitHub exposes and the quality of the package being evaluated; a rubric can still miss context, and a ranking can overvalue a weak signal such as low comment count. PR #94 is also a reminder that a technically focused contribution can remain unanswered after it is opened. My next step would be to improve issue-scout's ranking so comment count is balanced against issue quality and repository context, then add a lightweight follow-up or review-status check that makes stalled contributions visible without treating silence as a rejection. The Unit 6–8 tools are not recorded in this repository, so I am not presenting them as completed tools.

**Tool limits.**

issue-scout: Its fewest-comments ranking can favor brand-new issues in small repositories, even when those issues are not the strongest contribution opportunities.

repro-check: A low-effort cannot-reproduce attempt can pass `behavior-matches-issue` if it shows an attempted trigger and states the outcome honestly; the other checks still need to establish that the steps and report are useful.

pr-precheck: A re-indent inside a changed block can be treated as context rather than churn, and a claim that a warning prints can pass without the actual output being shown.
