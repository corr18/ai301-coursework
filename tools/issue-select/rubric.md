# Rubric: is this a good first issue?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever checks
you define here. It ships empty on purpose: the judgment is your work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where. Name the source
     (repo-facts block, issue body, comment thread, or the locations in
     references/evidence-guide.md). "The repo" is not a source; "the last
     5 default-branch commit dates" is.
   - Pass condition: a condition someone else could apply and get your
     answer. Prefer thresholds with numbers ("a maintainer commented
     within 30 days") over adjectives ("maintainer is responsive").
   - Weight: `required` (a fail here rejects the issue) or `preferred`
     (never changes the verdict; a nice-to-have that helps rank the
     issues you accept).

2. A verdict rule below the table: how the check grades combine into
   accept or reject, including how `unclear` is treated. The verdict
   space is binary. If you write no rule for `unclear`, the skill treats
   it as fail.

Cover what actually kills first contributions. The lecture named four
families: the maintainer is alive, the repo is in use, the scope fits a
newcomer, and nobody else is already on it. A rubric that ignores a family
will fail eval issues designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| maintainer_liveness | In the repo-facts block, inspect the dates and authors of the last 5 default-branch commits and the maintainer first-response sample; in live mode use those same locations from `references/evidence-guide.md`. | At least one of the last 5 default-branch commits is within 180 days of the capture date (or today in live mode) and is authored by a human account, or is a bot merge of a human PR; alternatively, the first-response sample contains an Owner, Member, or Collaborator response within 90 days. | required |
| repository_in_use | In the repo-facts block, inspect `archived:`, `last push to any branch`, and `latest release`; in live mode use the repository banner, newest-push date, and Releases box named in `references/evidence-guide.md`. | The repository is not archived, and either its last push or its latest release is within 365 days of the capture date (or today in live mode). | required |
| newcomer_scope | Read the issue body and comment thread (the Scope section of `references/evidence-guide.md` identifies these sources). | The issue asks for one concrete, bounded outcome: it states a specific behavior, change, or set of closely related edits, and it is not a support question, an explicit umbrella/tracking issue (a numbered list of causes or suggested fixes for one named bug is a diagnosis, not an umbrella), an unresolved design/product decision, or an issue whose thread says it requires broad core-internals work. | required |
| unclaimed | Inspect `this issue: assignees` and `linked PRs` in the repo-facts block and read the Comments section for claim comments; in live mode use the issue sidebar and thread locations in `references/evidence-guide.md`. | There is no assignee, no open linked PR, and no comment in the 90 days before capture (or today in live mode) saying the author is working on or taking the issue, unless a maintainer later explicitly says the issue is available again. | required |
| ai_contribution_policy | Read the `contribution policy` line in the repo-facts block; in live mode read `CONTRIBUTING.md`, linked contributor documentation, dedicated AI-policy files, and templates as listed in `references/evidence-guide.md`. | The policy is silent about AI, permits AI-assisted contributions, or imposes conditions such as disclosure, testing, or personal understanding. Fail only when it explicitly says AI-generated code or documentation is not accepted. | required |

## Verdict rule

Accept only if every required check passes. Reject if any required check
fails or is unclear. Preferred checks, if added later, never change the
verdict and may only rank accepted issues.
