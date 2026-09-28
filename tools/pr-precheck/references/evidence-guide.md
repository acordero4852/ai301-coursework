# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

- Where it lives, eval bundle: the plan-context block's Scope (in-scope
  / not-in-scope) and Files lines, and its Deviations section if one is
  present, read against the candidate PR's unified diff (which files
  and hunks actually changed) and its description (what it claims to
  have changed).
- Where it lives, live mode: your own `plan.md`'s Scope/Files sections
  and its Deviations section (the same file, not a separate log), read
  against your branch's diff — produced with `git diff main...HEAD`
  from your working copy — and your draft PR description.
- What good looks like: every file or behavior the diff touches falls
  inside the plan's stated scope, or the plan's Deviations section
  names that exact file or behavior and explains the mismatch (see
  `ai301-coursework/beat-1-sandbox/unit-3/plan.md`'s Deviations
  section, which states plainly that the diff matched the plan
  one-for-one, file by file and step by step — that same standard of
  naming applies when it did *not* hold). A deviation is honest when it
  is named before or at submission, not discovered by the reader.
- What silent drift looks like: the diff touches a file or adds a
  behavior the plan's Scope never named and no deviation note explains
  (drift by addition); the diff skips something the plan's Approach
  committed to with no deviation note saying why (drift by omission);
  or the description claims the diff delivers more, or less, fidelity
  to the plan than the diff itself shows.
- Rubric checks this maps to: **Diff matches plan scope**, **Description
  matches diff**.

## Test evidence (harness category: not-tested)

- Where it lives, eval bundle: the candidate PR's test-evidence
  section (its before/after claims), read against the plan-context
  block's Test plan section and the repro evidence's own steps; and
  the repo-facts block's stated CI command or check suite, read
  against whatever the test-evidence section says was run.
- Where it lives, live mode: your captured test output (terminal
  output or a saved log) from re-running the plan's Test plan steps,
  plus the repo's stated CI command (from its CONTRIBUTING.md or CI
  config) and whether you ran it and what it reported.
- What good looks like: a named, observable outcome — a specific
  command paired with its output or exit code, or a specific test
  name paired with its pass/fail result — that ties back to what the
  plan's Test plan or the issue's repro steps asked to verify. If
  repo-facts names a standard check or suite, the PR states plainly
  that it was run and what it reported (pass, or fail with an
  explanation), not just that "CI is green."
- What decisive is not: "tests pass," "works now," or "should be
  fine" name no observable outcome and prove nothing on their own,
  regardless of how confidently they are stated.
- Rubric checks this maps to: **Evidence is observable**, **Repo's
  own checks were run**.

## Diff quality (harness category: unreviewable)

- Where it lives, eval bundle: the candidate PR's unified diff and its
  commit list, read against the plan-context block's Scope/Files
  lines (what the fix was supposed to touch).
- Where it lives, live mode: your branch's diff (`git diff
  main...HEAD`) and `git log main..HEAD` for the commit list, read
  against your own `plan.md`.
- What good looks like: every hunk traces to the plan's fix — a
  reviewer could point at each changed line and say which part of the
  Approach it implements. Nothing else rides along.
- The debris tells: debug prints or leftover logging added only for
  local diagnosis, dead or commented-out code, whitespace/formatting
  churn across files the fix never touches, and drive-by edits (a
  rename, a lint fix, a dependency bump) that the plan's Scope never
  called for. Any one of these, present without explanation, makes the
  diff harder to review than the fix alone would be — that is what
  "unreviewable" means here, independent of whether the fix itself is
  correct.
- Rubric checks this maps to: **Diff is reviewable**, **Commit
  hygiene**.

## Standards and comms (harness category: standards-wall)

- Where it lives, eval bundle: the repo-facts block's stated PR
  template sections and its stated contribution/AI-use policy, read
  against the candidate PR's description; and the issue's thread
  highlights (explicit maintainer/owner direction already posted), read
  against the same description.
- Where it lives, live mode: the target repo's actual PR template and
  CONTRIBUTING.md (or AI_POLICY.md) on GitHub, read against your draft
  description; and the issue thread itself for any maintainer comment
  giving explicit direction.
- What good looks like: every section the template asks for carries
  real content specific to this PR — not a restated section header or
  a copy-pasted placeholder. If repo-facts states an AI-use disclosure
  requirement, the description discloses accurately; if repo-facts
  states no such requirement, there is nothing to disclose and the
  check passes by default. If the thread highlights contain explicit
  maintainer direction, the description is consistent with it or names
  a deliberate, explained departure; if the thread highlights are
  empty, there is no direction to engage and the check passes by
  default.
- What a visibly ignored ask looks like: a template section left
  blank or answered with boilerplate ("N/A" with no elaboration where
  detail was clearly asked for), a stated disclosure requirement met
  with silence, or a description that proceeds as if a maintainer's
  posted direction were never written.
- Note: a description's claim about *what changed* that contradicts
  the diff is plan fidelity, not standards-wall — this family only
  covers whether the PR honored the repo's and thread's stated asks,
  not whether its content matches the diff.
- Rubric checks this maps to: **Template sections completed**,
  **AI-use disclosure**, **Maintainer direction engaged**.

## In eval mode

Eval mode reads only the bundle text above. Nothing is fetched from
GitHub, no live file is opened, and no fact is assumed beyond what a
section of the bundle states. If a bundle section is silent on
something this guide names (no thread highlights, no stated AI
policy, no Deviations section), that silence is itself the fact to
record — grade the relevant check's pass condition against "nothing
stated," never invent detail to fill the gap.
