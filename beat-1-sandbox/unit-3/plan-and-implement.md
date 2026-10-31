# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

**GitHub username**

corr18

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-howard/issues/75#issuecomment-6537980056

````markdown
Plan: Fix missing OPENROUTER_API_KEY in .env.example

My plan is to fix the environment configuration mismatch identified in Issue #75. The README and `docs/SETUP.md` instruct contributors to configure `OPENROUTER_API_KEY`, but `.env.example` does not currently include that variable. This causes contributors who copy the example environment file to have a missing configuration value.

Planned Steps:

1. Review `core/config.py` to confirm that `openrouter_api_key` is an existing configuration setting and understand how it is loaded.
2. Inspect `.env.example` to identify where the missing environment variable belongs within the existing configuration structure.
3. Add `OPENROUTER_API_KEY=` under the `LLM provider` section, with a brief comment explaining its purpose.
4. Leave the existing `LLM_PROVIDER` options unchanged (`mock` and `openai`), since those are the provider values currently supported by the code.
5. Verify the change by copying `.env.example` into `.env`, supplying a test API key, and confirming that `core.config.Settings()` successfully loads the value.
6. Re-run the original reproduction checks to ensure the missing variable issue is resolved.

Scope:

The change will be limited to `.env.example`. I will not modify `core/config.py`, introduce a new OpenRouter provider option, or make unnecessary changes to the README or setup documentation.

Expected Outcome:

Contributors will be able to follow the existing setup instructions and find all required environment variables in `.env.example`. The existing default configuration will remain unchanged, and users who do not need AI features will still be able to use the mock provider without an API key.

I will keep the implementation minimal and verify that the updated environment template works through the application's existing configuration-loading process.
````

---

## Your branch

**Branch**

fix/75-env-example-openrouter-key

**Evidence**

My Unit 2 repro steps (the `grep` checks), plus the closest check through the real code path: copy `.env.example` to `.env` and load it with `core.config.Settings`, the class the app reads its config from. Environment: macOS 26.6.2, Python 3.13.7, `pydantic-settings` 2.16.2.

Before, on `main` at `99673c7`:

```
+ git rev-parse --abbrev-ref HEAD
main
+ git rev-parse --short HEAD
99673c7
+ grep -n OPENROUTER README.md docs/SETUP.md
README.md:24:# Configure environment (add your OPENROUTER_API_KEY to .env)
docs/SETUP.md:47:# Edit .env and set your OPENROUTER_API_KEY (required for AI features)
+ grep -n OPENROUTER .env.example
+ echo exit=1
exit=1
+ grep -n -A3 'LLM provider' .env.example
16:# LLM provider
17-# Options: "mock" (default, no API key needed), "openai"
18-LLM_PROVIDER=mock
19-OPENAI_API_KEY=sk-your-key-here
+ cp .env.example .env
+ python3 -c 'from core.config import Settings; s=Settings(); print('\''llm_provider=%r openrouter_api_key=%r'\'' % (s.llm_provider, s.openrouter_api_key))'
llm_provider='mock' openrouter_api_key=''
+ rm .env
```

After, on `fix/75-env-example-openrouter-key` at `227fb95`. The extra step fills a test key into the copied `.env`:

```
+ git rev-parse --abbrev-ref HEAD
fix/75-env-example-openrouter-key
+ git rev-parse --short HEAD
227fb95
+ grep -n OPENROUTER README.md docs/SETUP.md
README.md:24:# Configure environment (add your OPENROUTER_API_KEY to .env)
docs/SETUP.md:47:# Edit .env and set your OPENROUTER_API_KEY (required for AI features)
+ grep -n OPENROUTER .env.example
21:OPENROUTER_API_KEY=
+ echo exit=0
exit=0
+ grep -n -A5 'LLM provider' .env.example
16:# LLM provider
17-# Options: "mock" (default, no API key needed), "openai"
18-LLM_PROVIDER=mock
19-OPENAI_API_KEY=sk-your-key-here
20-# OpenRouter key used for AI features (see README Quick Start)
21-OPENROUTER_API_KEY=
+ cp .env.example .env
+ python3 -c 'from core.config import Settings; s=Settings(); print('\''llm_provider=%r openrouter_api_key=%r'\'' % (s.llm_provider, s.openrouter_api_key))'
llm_provider='mock' openrouter_api_key=''
+ sed -i '' 's/^OPENROUTER_API_KEY=$/OPENROUTER_API_KEY=sk-or-test-value/' .env
+ python3 -c 'from core.config import Settings; s=Settings(); print('\''llm_provider=%r openrouter_api_key=%r'\'' % (s.llm_provider, s.openrouter_api_key))'
llm_provider='mock' openrouter_api_key='sk-or-test-value'
+ rm .env
```

Before, `.env.example` has no `OPENROUTER` line (exit 1), so there is nothing to fill in. After, line 21 has it (exit 0), and once a value is filled in, `Settings()` loads it as `openrouter_api_key`. `llm_provider` stays `mock` in both.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run, first rubric (six required checks, one preferred): **20/20**
   (clear-accept 7/7, scope-creep 4/4, thread-convention 2/2, unbuildable 3/3,
   wrong-cause 4/4). Saved with `--save-run`; this is the committed
   `eval-run.txt`. No rubric change came after it.

Not a scored run: after the full run I re-graded `--only pkg-20,pkg-09` with
`--out` (2/2, same files, nothing edited) only to read their per-check grades
for the package analysis below. A partial run can't replace the saved file,
so the final score on record is still 20/20.

**Package analysis**

pkg-20 (ghostty-org/ghostty#11261, a stale `prev` pointer in
`Terminal.print` after hyperlink storage grows the page). Gold label:
**reject**. My rubric's decision: **reject**, failing only `ai-disclosure`.

It's a good plan, and my rubric passed it on everything else. The cause is
grounded (the control with the hyperlink start removed passes, which isolates
growth-during-print), it's one bounded change with stated not-in-scope lines,
it follows mitchellh's direction in the thread to recompute `prev` only when
capacity changes, and both fuzz cases become regression tests. The only miss
is the comment. Ghostty's repo facts say "All AI usage in any form must be
disclosed, stating the tool used and the extent of the assistance," and the
package set treats every plan as AI-assisted, but the comment never mentions
AI. The grader's evidence line was: "Policy: 'All AI usage in any form must be
disclosed...'; the comment contains no sentence disclosing AI use."

I read it the same way because the rule is the repo's, not mine. Posting that
comment on ghostty breaks a written policy, and a perfect plan doesn't fix
that. The contrast is pkg-09 (fd), which also has no disclosure in its comment
but passed: fd's policy asks for the tool and extent "in the pull request" and
"states no disclosure ask for issue comments."

**Check rationale**

`ai-disclosure`, pass condition as it now reads:

> Treat every package as AI-assisted work. Pass if the repo states no AI policy, or its policy asks for disclosure only in the pull request (not in issue comments), or the policy asks only that comments be in the contributor's own words. Fail if the policy requires disclosing AI use in comments or "in any form", and the comment has no sentence stating that AI was used, which tool, and how much.

I wrote it this way after reading every package's contribution policy line
next to its comment before the first run. The first version I considered was
"fail if the repo has an AI policy and the comment doesn't disclose." That
would have rejected pkg-05 (conda, which only asks you to review AI content
before a PR) and pkg-09 (fd, disclosure "in the pull request"), both gold
accepts. So the check reads the policy for where it wants disclosure, and only
fails when that's the comment or "in any form." I also rejected the opposite
reading, "pass unless the comment admits it used AI." pkg-20's gold note says
every package is treated as AI-assisted, so a missing disclosure can't be
read as "no AI was used." That's why the check opens with "Treat every package
as AI-assisted work."

**Trade-offs**

`ai-disclosure` gives up the case where a policy asks for comments "in the
contributor's own words" (ripgrep, pkg-03 and calib-04) but doesn't say
"disclose." My check passes that with no disclosure sentence. A stricter
maintainer might want one anyway. Both of those comments happen to disclose,
so the eval doesn't test this, and I accept that a ripgrep-style comment with
no disclosure would get through my rubric.

It also leans on how the policy is worded in the repo-facts block. A vague
policy ("be transparent about tools") would probably grade `unclear`, which my
verdict rule counts as a fail, so the check rejects when it's unsure.

I know nothing else moved because I never revised the rubric. The first full
run was 20/20 with every category matched, and the `--only pkg-20,pkg-09`
re-grade on the same files gave the same verdicts.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
