# Plan: issue #75, `.env.example` is missing the key the README tells you to set

Issue: https://github.com/codepath/pathreview-ai301-fa26-howard/issues/75
Built on: my repro comment on that issue (main at `99673c7`).

## Diagnosis

The README (line 24) and `docs/SETUP.md` (line 47) both tell a new contributor
to set `OPENROUTER_API_KEY` in `.env`, and `core/config.py` defines
`openrouter_api_key`, but `.env.example` has no `OPENROUTER` line at all. My
repro shows it: `grep -n OPENROUTER .env.example` exits 1. After posting the
repro I also checked it through the real code path: a `.env` made with
`cp .env.example .env` loads `openrouter_api_key=''` through
`core.config.Settings` (output in the Evidence block of
`plan-and-implement.md`). So someone following the
README ends up with a `.env` missing the variable the docs call required.

Tracing the open question from my repro: nothing in the code reads
`settings.llm_provider` or maps any `LLM_PROVIDER` value to OpenRouter.
`grep -rn "llm_provider" --include=*.py` only hits the field definition in
`core/config.py`. The only provider switch in the code is
`get_embedding_provider` in `ingestion/embeddings/provider.py`, which accepts
`"mock"` and `"openai"`. So the `LLM_PROVIDER` comment in `.env.example` is
accurate today, and the fix is to add the missing key, not to add an
`openrouter` option the code doesn't accept.

## Scope

One change, in one file: `.env.example`.

In scope:
- Add `OPENROUTER_API_KEY=` under the `# LLM provider` block, with a one-line
  comment saying the README's AI setup uses it.
- Keep the `LLM_PROVIDER` options comment as `"mock"` and `"openai"`, since
  those are the values the code accepts.

Not in scope:
- Wiring an `openrouter` value for `LLM_PROVIDER` into the code. That's a
  feature, not this docs mismatch.
- Adding `OPENROUTER_BASE_URL` or `OPENROUTER_MODEL`. `core/config.py` has
  working defaults for both, and the issue only asks about the key.
- Editing `README.md` or `docs/SETUP.md`. Once `.env.example` has the key,
  they're correct as written.
- Touching `core/config.py`.

## Approach

1. Branch `fix/75-env-example-openrouter-key` from `main` on my fork.
2. Edit `.env.example`: add the `OPENROUTER_API_KEY=` line and its comment in
   the `# LLM provider` block.
3. Re-run my repro steps and the `Settings()` check (test plan below).
4. Commit as `fix: add OPENROUTER_API_KEY to .env.example` with `Fixes #75`.

## Test plan

Re-run my repro steps from a clean checkout of the branch:

- `grep -n OPENROUTER .env.example` prints the new line and exits 0
  (before: no output, exit 1).
- `cp .env.example .env`, put a test value on the `OPENROUTER_API_KEY` line,
  and run
  `python3 -c "from core.config import Settings; print(Settings().openrouter_api_key)"`:
  it prints the test value (before: there was no line to fill, and it printed
  `''`).
- `LLM_PROVIDER` still loads as `mock`, so the default no-key setup is
  unchanged.

## Risks and unknowns

- I haven't confirmed whether the maintainers plan to wire OpenRouter into
  `LLM_PROVIDER` later. If they'd rather have the comment list `openrouter`
  now, that's a one-line follow-up, and I'll ask in the thread instead of
  guessing.
- Leaving the key empty instead of using a fake placeholder like
  `sk-or-your-key-here` is a judgment call. An empty value keeps
  `openrouter_api_key` at its `""` default for anyone not using AI features.

## Deviations

Nothing changed in the change itself; the plan held. The commit
(`227fb95` on `fix/75-env-example-openrouter-key`) touches only
`.env.example`, adds the `OPENROUTER_API_KEY=` line and its comment in the
`# LLM provider` block, and leaves the `LLM_PROVIDER` options as they were.
Every test-plan step came out as predicted: `grep` exits 0 on line 21, and a
`.env` copied from the example loads the test key through `Settings()` while
`llm_provider` stays `mock`.

What did change was the thread. My first plan comment on #75
(https://github.com/codepath/pathreview-ai301-fa26-howard/issues/75#issuecomment-6537943635)
was broader than this plan: it said I'd investigate the provider config, might
change the `LLM_PROVIDER` comment, and would check that the README and setup
docs explain the provider and key. By then the tracing was already done and
showed that only `.env.example` needed to change, so that comment promised work
I wasn't going to do. I posted a second comment
(https://github.com/codepath/pathreview-ai301-fa26-howard/issues/75#issuecomment-6537980056)
that matches this plan: one file, `LLM_PROVIDER` options unchanged, no README or
SETUP edits, verified through `Settings()`. That second comment is the plan I
built from.

Also fixed before posting, not during the build: my first draft of this plan
said my repro showed the `Settings()` result, but my posted repro never ran
`Settings()`. plan-check's live run flagged it, so I reworded the diagnosis to
say I checked it after the repro.
