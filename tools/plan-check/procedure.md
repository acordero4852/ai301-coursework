# Procedure: how this skill grades a plan package

## Read order

1. Read the issue (title, body, labels) and the repo-facts block
   first, so the fix's real ask and the repo's stated rules are in
   mind before anything else is judged. Note: what the issue asks for,
   any stated AI-use disclosure requirement, and the contribution
   policy.
2. Read the repro-evidence block next, before the plan. Note: every
   step's command/output/exit code, and which step is a control or
   contrast run (a rerun with something changed — a flag removed, a
   version bumped, logging turned on). These steps are the fixed
   ground truth every later check reads the plan against.
3. Read the thread highlights. Note: any explicit maintainer/owner
   direction already stated on the issue (a settled approach, a named
   prior-art PR, an owner's own diagnosis).
4. Read the candidate plan in full (Diagnosis, Scope, Files, Approach,
   Test plan, any Risks/Unknowns line), then the candidate plan
   comment last, since the comment is judged against everything
   gathered in steps 1-3.

This order matters because every check below reads the plan and
comment against facts gathered in steps 1-3, never the other way
around: the repro evidence and thread are fixed; the plan is what's
on trial.

## Evidence gathering

For each family, pull the fact and record it verbatim (a quote or
step number), not a paraphrase:

- **Diagnosis**: the plan's stated cause (Diagnosis section), plus the
  specific repro-evidence step(s) it must explain, including any
  control/contrast step. Record whether each repro step is consistent
  with, silent on, or contradicts the stated cause.
- **Scope**: the plan's in-scope/not-in-scope lines and its Files
  list. Record every file or area named, and every task in the
  Approach that is not a direct fix for the issue's own behavior.
- **Executability**: the Files and Approach sections. Record any
  branch point where the plan names more than one option without
  choosing ("A or B", "wherever's easier", "somewhere in X").
- **Test plan**: the Test plan section, plus the repro-evidence steps
  it claims to reuse. Record the named outcome (an output, exit code,
  or specific test case) or its absence.
- **Honesty**: any risk/unknown/deferral language, wherever it appears
  in the plan. Record each deferral and whether it is named as a
  deferral versus silently dropped from scope.
- **Comms**: the candidate plan comment. Record whether it references
  the thread highlights' maintainer direction (if any exists) and
  whether it includes an AI-use disclosure (if the repo-facts block
  requires one).

A family with nothing in the package to find (e.g., no thread
highlights exist, or the plan states no risks at all) is not a gap to
invent evidence for — record "none present" and let the check's pass
condition decide what that means.

## Check execution

Run the checks in the rubric's table order: Diagnosis, Scope,
Executability, Test plan, Honesty, Comms. Each check is graded only
from the evidence recorded for its own family in the previous stage —
do not re-read the whole package per check.

For each check, apply its pass condition from `rubric.md` to the
recorded evidence and grade `pass`, `fail`, or `unclear`:

- Grade `fail` when the recorded evidence directly meets the fail
  condition (e.g., a control run rules out the stated cause; a branch
  point is left undecided; the comment omits required disclosure).
- Grade `unclear` only when the package gives genuinely no evidence to
  decide the check either way (e.g., the repo-facts block states no
  AI policy and the thread highlights are empty, so there is nothing
  for Comms to fail or pass against) — do not use `unclear` as a
  substitute for `fail` when the evidence is simply weak; weak but
  present evidence that fails the stated pass condition is `fail`, not
  `unclear`.
- Grade `pass` otherwise.

Every check's grade must cite the fact recorded for it in Evidence
gathering; a grade with no cited fact is not finished.

## Verdict assembly

1. Apply the rubric's verdict rule: accept only if all six checks
   (Diagnosis, Scope, Executability, Test plan, Honesty, Comms) grade
   `pass`. Any single `fail` or `unclear` holds the package to reject
   — there is no averaging and no preferred/required split in this
   rubric, since every check is required.
2. The deciding check(s) for the output are every check that graded
   `fail` or `unclear`; quote that check's recorded evidence fact in
   the JSON's `"evidence"` field for that check, and in the summary
   line for it.
3. If every check passes, the verdict is `accept` and each check's
   `"evidence"` field cites the fact that earned the pass (e.g., which
   repro step the diagnosis explains, which file the test plan's
   observable outcome maps to).
4. Re-running the same recorded evidence through this rule must always
   produce the same verdict: the rule is mechanical once the six
   grades are fixed, so assemble the verdict from the grades only,
   never by re-weighing the plan itself at this stage.
