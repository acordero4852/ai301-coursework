# Procedure: how this tool grades a PR package

## Read order

1. Read the issue (title, body, thread highlights) and the repo-facts
   block first. Note: what the issue actually asks for, the repo's
   stated PR template sections, its stated contribution/AI-use policy,
   and any explicit maintainer/owner direction already posted on the
   thread.
2. Read the plan-context block next — the accepted plan's Scope,
   Files, and Test plan sections, and its Deviations section if one is
   present (live mode: your own `plan.md`, same sections). Note: every
   file or area the plan names in scope, every line naming something
   explicitly not in scope, the test plan's stated observable outcome,
   and the exact text of any deviation note. These are the fixed
   ground truth the diff and description are checked against — read
   them before either, never after.
3. Read the candidate PR's diff and commit list next, before the
   description. Note: every file the diff touches and, for each, a
   one-line summary of what changed in it; anything in the diff that
   is not an obvious direct fix (leftover debug code, formatting-only
   hunks, unrelated files).
4. Read the candidate PR's description and test-evidence section
   last, since both are judged against everything gathered in steps
   1-3: the description against the diff you already summarized, the
   test-evidence against the plan's test plan and the repo's stated
   checks.

This order matters because the plan, the diff, and the repo/thread
facts are the ground truth; the description and the test-evidence
section are the PR's own claims about that ground truth, and every
side-by-side check below reads a claim against a fact gathered
earlier, never the other way around.

## Evidence gathering

For each family, pull the fact and record it verbatim (a quote, file
name, or line), not a paraphrase:

- **Plan fidelity**: list every file/behavior the diff touches (step
  3) next to the plan's Scope/Files lines (step 2). For each diff
  item, record one of: "in scope" (named in Files/Scope), "covered by
  deviation" (quote the Deviations-section line that names it), or
  "unaccounted" (neither). Separately, list every change the
  description claims and record whether the diff item list from above
  actually contains it.
- **Test evidence**: record the test-evidence section's claims
  verbatim (command, output, exit code, or named test result) and pair
  each with the plan's Test plan line it's supposed to satisfy; record
  "no observable outcome" for any claim that names none. Record
  separately whether the repo-facts block names a standard CI command
  or suite, and if so, whether the test-evidence section states it was
  run and what it reported.
- **Diff quality**: from the step-3 file/hunk summary, record any hunk
  that is not a direct fix for the issue (debug leftovers, dead or
  commented-out code, formatting-only churn, drive-by edits) and which
  file it's in. A diff with no such hunks gets "none found" recorded,
  not skipped.
- **Standards and comms**: record, section by section, whether the
  repo-facts block's stated PR template asks are answered with real
  content in the description ("filled" or "blank/boilerplate", quoting
  the section if blank/boilerplate). Record whether repo-facts states
  an AI-use disclosure requirement and, if so, whether the description
  discloses (quote the disclosure line, or record "no disclosure
  found"). Record whether the thread highlights contain explicit
  maintainer direction and, if so, whether the description engages it
  (quote the direction and the description's response, or record
  "ignored").
- **Commit hygiene**: record whether each commit message describes
  the change and references the issue number, or record which
  commits don't.

A family with nothing in the package to find (no Deviations section,
no thread highlights, no stated AI policy) is not a gap to invent
evidence for — record "none present" and let the check's pass
condition in `rubric.md` decide what that means.

## Check execution

Run the checks in the rubric's table order: Diff matches plan scope,
Description matches diff, Evidence is observable, Repo's own checks
were run, Diff is reviewable, Template sections completed, AI-use
disclosure, Maintainer direction engaged, Commit hygiene. Each check
is graded only from the evidence recorded for its own family in the
previous stage — do not re-read the whole package per check.

For each check, apply its pass condition from `rubric.md` to the
recorded evidence and grade `pass`, `fail`, or `unclear`:

- Grade `fail` when the recorded evidence directly meets the fail
  condition (an "unaccounted" diff item; a test claim with no
  observable outcome; a named CI check silently skipped; a debris hunk
  found; a blank template section; a stated disclosure requirement met
  with none found; ignored maintainer direction).
- Grade `unclear` only when the package gives genuinely no evidence to
  decide the check either way (for example: the plan-context block
  itself never states a Test plan, so there is nothing for "Evidence
  is observable" to check the claim against). Do not use `unclear` as
  a stand-in for `fail` when evidence is present but weak — present
  evidence that fails the stated pass condition is `fail`.
- Grade `pass` otherwise, including every check whose evidence-guide
  "default" applies (no stated AI policy, no thread direction, no
  named CI check) — a default pass still cites the recorded "none
  present" fact as its evidence.

Every check's grade must cite the fact recorded for it in Evidence
gathering; a grade with no cited fact is not finished.

## Verdict assembly

1. Apply the rubric's verdict rule: accept only if every `required`
   check (all checks except Commit hygiene) grades `pass`. Any single
   required `fail` or `unclear` holds the package to `reject`.
   `preferred` checks (Commit hygiene) never change this outcome.
2. If more than one required check failed or graded unclear, the
   deciding check quoted in the output's summary and in the JSON's
   evidence field for that check is the first one to fail in rubric
   order (Diff matches plan scope first, Commit hygiene last, per the
   table above) — pick one deciding check, but still report every
   failing check's own grade and evidence in the `checks` array; the
   "first failing required check" rule only decides which one gets
   named as the reason in prose, not which ones appear in the output.
3. If every required check passes, the verdict is `accept`, and each
   check's `"evidence"` field cites the fact that earned the pass
   (including a cited "none present" fact for a default pass).
4. Re-running the same recorded evidence through this rule must always
   produce the same verdict: assemble the verdict from the grades
   only, never by re-weighing the PR itself at this stage.

## Dry run

A package where the diff adds one out-of-scope helper function with
no deviation note, and whose description says "all tests pass" with
no command or output: Read order pulls the plan's Scope/Files and the
repo's stated test command first; Evidence gathering records the
helper function as "unaccounted" against the plan's Files list, and
records "all tests pass" as "no observable outcome" against the
repo's stated command. Check execution grades "Diff matches plan
scope" `fail` (unaccounted helper, no deviation note) and "Evidence is
observable" `fail` (no observable outcome), while the remaining
checks may still `pass` on their own evidence. Verdict assembly holds
the package to `reject` on the first required fail in rubric order —
"Diff matches plan scope" — and reports that check's evidence
("`helper_slugify()` added in `utils.py`, not named in plan Files/Scope,
no Deviations section present") as the deciding fact, while still
listing "Evidence is observable" as a second failing check in the
`checks` array.
