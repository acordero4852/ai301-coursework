# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`. This file is graded at the path above; a copy kept
anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/83

**Branch**

fix/68-empty-index-zerodivision

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

20/20

One run, on the final tool version at `tools/pr-precheck/`. The `eval-run.txt` I committed
records `agreement: 20/20 scored items (bar: 18/20: PASS)`, and that is the only run — the
last score in this list is that same 20/20.

**Package analysis**

`pkg-03` (category `silent-drift`, gold `reject`, verdict `reject` — agree).

The issue and accepted plan scope a one-line fix: replace the `?` on
`contract_repo_path(...)?` in `src/modules/directory.rs` with a fallback to the ordinary
`contract_path`-based rendering when it returns `None`. The candidate PR's diff does that,
but it also rewrites the unrelated `truncate()` helper (a working function, not touched by
the plan) and adds a brand-new `resolve_symlinks` config field plus its default in
`configs/directory.rs` — neither mentioned anywhere in the plan's Files/Scope or in the PR
description's "Changes"/"How Has This Been Tested" text. My rubric's "Diff matches plan
scope" and "Diff is reviewable" checks both read the diff against the plan and find hunks
that don't trace to the plan's fix and aren't disclosed as a deviation, so both required
checks fail and the package holds to reject, matching gold.

**Check rationale**

> Pass if every hunk traces to the plan's fix, with no debug leftovers, dead or
> commented-out code, unrelated formatting churn, or drive-by edits riding along outside
> the plan's scope. Fail if unrelated or unexplained material buries the actual change.

(the "Diff is reviewable" check, from `rubric.md`)

I kept this as a `required` check rather than downgrading it to `preferred` alongside
commit hygiene. A softer, preferred version would let a package like `pkg-03` still accept
on the strength of its other passing checks — the fix itself is correct and tested — even
though it smuggled an unrelated refactor and a new config surface into what the plan and
description both call a one-line guard. Rejecting that trade meant this check has to be
able to fail the verdict on its own, so I left it required.

**Trade-offs**

Keeping "Diff is reviewable" required and strict on any out-of-scope hunk means a PR that
bundles a genuinely harmless drive-by (a typo fix in an adjacent comment, say) fails the
same way `pkg-03` does, even when that extra hunk carries no risk. The check can't tell
"safe drive-by" from "smuggled feature" — it only checks whether every hunk traces to the
plan. That's the corner cut: reviewers using this tool have to split any incidental cleanup
into its own commit or PR rather than riding along, which is more overhead for the
genuinely trivial cases in exchange for catching packages like `pkg-03` deterministically.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
