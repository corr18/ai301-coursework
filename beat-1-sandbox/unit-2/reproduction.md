# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

**GitHub username**

corr18

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-howard/issues/75#issuecomment-6426755117

````markdown
Hi! I'd like to take this as my first contribution. On main at 99673c7, the README.md on (line 24) tells says to add OPENROUTER_API_KEY to .env, but .env.example only has OPENAI_API_KEY and lists mock/openai as the LLM_PROVIDER options, while core/config.py defines both openai_api_key and openrouter_api_key. docs/SETUP.md (line 47) says OpenRouter too. Next, I'm going to trace which provider values the code actually reads, so the docs end up matching the code.
````

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-howard/issues/75#issuecomment-6426965165

````markdown
Repro-

Reproduced on current `main`: the setup docs and `.env.example` disagree about which API key to set.

**Environment:** macOS 26.6.2, git 2.39.5, fresh `git clone --depth 1` of `main` at `99673c7` (2026-09-16). This is a docs mismatch, so nothing needs to be installed or run.

**Steps** (from a clean clone):

$ git clone --depth 1 https://github.com/codepath/pathreview-ai301-fa26-howard.git && cd pathreview-ai301-fa26-howard
$ git rev-parse --short HEAD
99673c7
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


Expected: someone who runs `cp .env.example .env` gets the variable the README tells them to set, and the `LLM_PROVIDER` comment lists every option the config supports.

Actual: README.md and docs/SETUP.md both say to set `OPENROUTER_API_KEY`, but .env.example has no `OPENROUTER` line at all (grep exits 1). It only has `OPENAI_API_KEY`, and its comment offers only mock and OpenAI. core/config.py defines both keys, so the issue description matches what I see.

One thing I noticed but haven't confirmed yet: in core/config.py, I didn't find a line that maps an LLM_PROVIDER value to OpenRouter, so I don't yet know what the valid value would be. Tracing that is my next step, before I change any docs.
````

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Full run, first rubric (six required checks, one preferred): **17/20**
   (clear-accept 5/8, disclosure 1/1, no-evidence 4/4, unfollowable-comms 3/3,
   wrong-target 4/4). All three misses were gold accepts that I rejected:
   pkg-03 on `outcome-honest`, pkg-05 on `steps-rerunnable`, pkg-09 on
   `behavior-matches-issue`.
2. Loosened those three checks: a plain side observation in prose isn't an
   overclaim; an input described precisely enough to recreate counts as
   shown; a cannot-reproduce whose attempt says it may not have reached the
   trigger can still pass the behavior check. Ran
   `--only pkg-03,pkg-05,pkg-09,pkg-20,pkg-02,pkg-15,pkg-18` (the three misses
   plus canaries: pkg-20 for the disclosure category, and one each from
   wrong-target, no-evidence, and unfollowable-comms, one for each loosened
   check): **7/7**.
3. Confirming full run, saved with `--save-run eval-run.txt`: **20/20**
   (clear-accept 8/8, disclosure 1/1, no-evidence 4/4, unfollowable-comms 3/3,
   wrong-target 4/4).

**Package analysis**

pkg-09 (sharkdp/fd#2033, `--exec-batch` commands running out of order when the
argument-size limit is hit). Gold label: **accept**. My rubric's decision on
the first run: **reject**, failing `behavior-matches-issue`. The report is a
cannot-reproduce: it built 120000 long file names, ran two `--exec-batch`
commands, and showed `order.log` with every ONE before every TWO. It then
admits its padding "may not achieve" the condition the issue needs, since fd
seemed to flush both buffers at the same boundary. The grader read that
admission as "the trigger was never produced," so the artifact didn't show the
issue's behavior. But that's what an honest cannot-reproduce looks like: a
real attempt at the issue's trigger, with output shown, that says plainly
where it might have fallen short and what a triggering setup probably needs.
Judging that admission is the honesty check's job, not the behavior check's.
After I said so in the check, the rubric accepted pkg-09, matching gold.

**Check rationale**

`behavior-matches-issue`, pass condition as it now reads:

> The shown artifact is the issue's behavior, produced by the issue's trigger: same error/symptom class, same kind of failure (a crash is a crash, a panic is a panic, wrong output is wrong output), same input shape. A cannot-reproduce passes here when it is a genuine attempt at the issue's trigger with its output shown, even if the report itself admits its setup may not have reached the triggering condition; whether that admission is honest is `outcome-honest`'s job, not this check's. This allowance covers only reports that say they did NOT reproduce; a report that claims to reproduce must show the issue's own behavior. Fail if there is no artifact, if the artifact shows only that the program runs, or if it shows an adjacent behavior (a different error, a graceful validation message instead of the reported crash, a compile error from a modified input, garbled output presented as a crash).

The failure list maps to the wrong-target and no-evidence packages: "graceful
validation message instead of the reported crash" is pkg-02 (exit 1 vs. the
issue's exit 101), "compile error from a modified input" is pkg-08 (`$b`
unbound in jq), "garbled output presented as a crash" is pkg-17, and "only
that the program runs" is pkg-14 (a version banner and session list). The
cannot-reproduce allowance was added after pkg-09, and it's fenced off to
reports that say they did NOT reproduce, so a confident wrong-target report
can't use it.

**Trade-offs**

All three revisions loosened checks, so each one could let a bad package
through. The biggest risk is the pkg-09 allowance: a lazy cannot-reproduce that
barely tried could now pass the behavior check and lean on `outcome-honest` to
catch it. The steps change has a similar risk: "described precisely enough to
recreate" is a judgment call, and a vague description could slip through. I
accepted that because the alternative was rejecting honest reports, which is
the opposite of what phase 2 rewards. I checked the cost with canaries before
spending the full run: pkg-02, pkg-15, pkg-18, and pkg-20 all still rejected,
and the confirming full run kept every reject category at 100%.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
