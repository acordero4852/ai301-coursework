# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

- Where it lives: the candidate plan's "Diagnosis" section states the
  cause. The repro-evidence block's numbered steps (and any control or
  contrast run inside it — a rerun with a flag removed, on a different
  version, with logging enabled) pin down what actually happens.
- What good looks like: the stated cause explains every step in the
  repro evidence, including the control runs, with nothing left
  unexplained or contradicted. A diagnosis that a control run rules out
  ("if X were the cause, the control run would also fail — it
  doesn't") is wrong even if it is stated with total confidence. A
  diagnosis that never mentions or engages the repro evidence at all
  has nothing to be graded against, and fails the same way.

## Scope

- Where it lives: the plan's "Scope" section (in-scope / not-in-scope
  lines) and its "Files" or "Approach" section, read against the issue
  body and the repro evidence for what the fix actually needs to
  touch.
- What good looks like: one change a maintainer could review as a
  single fix for this issue — named files or areas, an explicit
  not-in-scope line that closes off adjacent temptations (a related
  redesign, a dependency migration, a new option). A plan that grows
  past the issue's own ask — "while I'm in here" work, a rewrite
  wrapped around the fix, a multi-front campaign — is scope creep even
  when the core fix inside it is correct.

## Executability

- Where it lives: the plan's "Files" and "Approach" sections: which
  files or areas are named, what will change in them, and in what
  order.
- What good looks like: a stranger could start work from the plan
  alone, with no question left for the author. Named files (not
  "somewhere in the parser"), a chosen approach (not "option A or B,
  whichever's easier"), and an order of steps. Any real decision
  pushed to build time — which layer, which strategy, which file —
  means the plan isn't executable yet, no matter how confident its
  prose reads.

## Test plan

- Where it lives: the plan's "Test plan" section, read against the
  repro-evidence block's steps and artifacts (the exact commands,
  outputs, and exit codes it recorded).
- What good looks like: a named, observable outcome tied back to the
  repro evidence — rerunning the repro's failing step and citing the
  expected output or exit code, or a specific new test case that would
  fail today and pass after the fix. "Run the full test suite" and
  "should feel faster/right" name no observable outcome for the fix
  itself and fail even when the rest of the plan is strong (see
  calib-04: solid plan, rejected on this check alone).

## Honesty

- Where it lives: risk/unknown/deferral language anywhere in the plan
  (often in Diagnosis, Scope, or a dedicated "Risks"/"Unknowns" line),
  read against how the same plan asserts its diagnosis and scope
  elsewhere.
- What good looks like: telling stated unknowns from false confidence.
  A plan that names what it's deferring and why ("Windows variant
  untestable here, deferring") is honest, even if the deferral is
  itself arguable — that is a pass, not a scope violation, because the
  boundary was disclosed rather than hidden. A plan that asserts an
  unverified claim as settled fact, or quietly drops scope without
  saying so, fails this check. A mid-build deviation gets recorded in
  `plan.md` itself (live mode only); a deviation that only shows up in
  the diff, never in the plan, is not honest work.

## Comms

- Where it lives: the candidate plan comment, read against two package
  sections: the "Thread highlights" (any explicit maintainer/owner
  direction already on the issue) and the "Repo facts" block (stated
  contribution policy, templates, and any AI-use disclosure
  requirement).
- What good looks like: thread-aware, not boilerplate. If the thread
  highlights contain explicit maintainer direction (a settled approach,
  a named prior-art PR, an owner's diagnosis), the comment engages it
  instead of ignoring or re-litigating it. If the repo-facts block
  states an AI-use disclosure requirement, the comment discloses —
  every package here is treated as AI-assisted work, so a repo that
  requires disclosure and gets none fails this check regardless of how
  good the plan underneath is (see pkg-20: excellent plan, rejected
  here alone).
