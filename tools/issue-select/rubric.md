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
| Maintainer alive | "last 5 default-branch commits" under Repo facts | at least 1 of the last 5 commits is dated within 90 days of the capture/today date, and is authored by a human (not a `[bot]` account merging nothing) | required |
| Maintainer responsive | "maintainer first-response sample" under Repo facts, or Owner/Member/Collaborator-badged comments in the thread | at least one comment from an Owner/Member/Collaborator-badged account appears within 30 days of the issue being opened | preferred |
| Repo in use | "latest release" and "last push to any branch" under Repo facts; "archived:" flag on the repo line | repo is not archived, AND (latest release is within 180 days OR last push to any branch is within 90 days) | required |
| Scope fits a newcomer | issue body and comment thread | issue is not an explicit umbrella/tracking issue (multiple independent user-facing deliverables explicitly meant to be split into separate tracked issues — a single bug's listed root causes or candidate fixes for one problem do NOT count), is not a pure usage/support question, the thread shows no unresolved design debate, no maintainer comment says the fix touches core internals, the thread does not show a history of 2+ closed/unmerged linked PRs or repeated claim-then-abandon cycles, and if the issue is a feature request (not a bug report) it carries some sign of maintainer buy-in (a maintainer/member comment, or an accepting label like "help wanted"/"good first issue"/"accepted") — an unendorsed feature idea with zero comments and no labels fails, since nobody has confirmed the work is wanted — multiple related file edits toward one clearly stated goal do not by themselves count as unscoped | required |
| Not already claimed | "this issue: assignees:" under Repo facts; "linked PRs:" with state; Comments section for claim language | no assignee is set, no linked PR is open, and no claim comment ("I'll take this" / "working on this") appears within the last 14 days without having gone stale | required |
| AI contribution allowed | "contribution policy" line under Repo facts (CONTRIBUTING.md / AI_POLICY.md / templates) | no outright ban on AI-assisted contributions ("we do not accept AI-generated code"); disclosure/testing/review conditions do not fail this check | required |
| Good-first-issue label | issue's labels | issue carries a `good first issue` (or equivalent) label | preferred |

## Verdict rule

Accept if and only if every `required` check grades `pass`. Any `required`
check that grades `fail` or `unclear` rejects the issue. `preferred` checks
never change the verdict — they only rank accepted issues (more preferred
passes = ranked higher). `unclear` on a required check counts as `fail`:
a first issue you cannot verify is not a first issue to take.
