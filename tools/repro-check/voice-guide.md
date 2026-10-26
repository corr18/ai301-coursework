# Voice guide: how I talk upstream

## Who I am in threads

I'm a student making my first contributions through AI301. I know my
way around Python projects and `.env` setup, but I'm new to this repo
and its maintainers. When I comment, I'm reporting what I ran and what
I saw, and saying what I'll look at next. I'm not promising fixes yet.

## Rules I write by

### Rule: Name the actual thing

Every comment has to mention something only this issue has: the file,
the variable, the command, or the exact mismatch. If the comment would
make sense pasted under a different issue, rewrite it.

- Wrong: "Hello, I'd like to work on this issue, it looks like a good first one for me."
- Right: "I'd like to take this then list things such as: `README.md` line 24 says to add `OPENROUTER_API_KEY`, but `.env.example` only has `OPENAI_API_KEY`, and I want to check which one `core/config.py` actually uses before I touch the docs."

### Rule: Promise the next step, not the finish

I only commit to the next thing I'll actually do. No dates, no "I'll fix it," no asking anyone to reserve the issue for me, if I can't finish and someone else does, then that's on me.

- Wrong: "Please assign this to me, I'll have a PR up by Friday."
- Right: "Next I'm going to trace where `llm_provider` is read so the docs list the options the code really accepts." This allows for clear and concise movement and tracing on all of my steps taken.

### Rule: Show it, then say it

Anything I claim to have seen has to come with the command and its
output pasted in. If I didn't capture it, I don't claim it.

- Wrong: "Confirmed, the env file is definitely wrong."
- Right: "On commit `99673c7`, `grep -n OPENROUTER .env.example` prints nothing, while `grep -n OPENROUTER README.md` prints line 24 (output below)." Again, this allows for organization and clear documentation.

### Rule: Say where I ran it

I list my environment (OS, the commit or version I tested, and the tools I used) any time I report a result, and I point out when it's different from what the issue describes.

- Wrong: "I reproduced it on my machine."
- Right: "Environment: macOS 26.6.2, git 2.39.5, a fresh clone of `main` at `99673c7`." This all helps with organization and upfrontness.

### Rule: Be upfront about AI help

If an AI tool helped me write or check a comment, I say so in one plain sentence and say that I ran every step myself. This helps with proper disclosure.

- Wrong: (an AI-polished comment posted with no mention of it)
- Right: "I used Claude Code to help draft and check this comment; I ran every command myself."

## Things I never post

- A fix date or a "guaranteed" anything due to things unforeseen that could occur.
- "+1", "same here", or "any update?" with nothing new attached.
- A claim of "reproduced" without the output that shows it.
- "Same as above, can confirm" on a classmate's repro. My proof comes from my own environment and experiences.
- Flattery or begging ("amazing project, kindly assign me").
