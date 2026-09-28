---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

This tool answers exactly one question about exactly one PR package:
is this pull request ready to submit? A PR package is a candidate
pull request — its title, description, commit list, unified diff, and
test evidence — read against two fixed pieces of ground truth: the
plan it claims to implement (with any deviation notes) and the issue
that plan belongs to (its body, thread highlights, and the repo-facts
block naming that repo's stated rules). Never grade a different
question, never grade more than one package per run, and never grade
from impression — execute `rubric.md` via `procedure.md`, using
`references/evidence-guide.md` to find each fact.

## Inputs and modes

**Live mode** grades your own submission before you open it. Read:

- Your `plan.md`, including its Deviations section, from your working
  copy.
- Your branch's diff against the repo's default branch: run `git diff
  main...HEAD` (three dots) from your working copy. This diff, not
  your memory of what you changed, is the ground truth for every
  diff-reading check.
- Your commit list: `git log main..HEAD`.
- Your draft PR title and description, not yet posted.
- Your test evidence: captured output from re-running the plan's test
  plan, and from the repo's own stated checks.
- Your issue: its body, thread highlights (comments from the real
  repo), and the repo's actual PR template and CONTRIBUTING.md/AI
  policy, gathered from the real repo (the thread, the PR template,
  the stated policy) since live mode is not a frozen bundle.

A house-chain student (routed to a house plan and house repro pack
instead of their own week-3 work) reads the house plan and house
repro pack in place of `plan.md`; every check still grades the same
facts, just sourced from the house materials.

**Eval mode** grades a package bundle. The bundle is the whole world:
every fact — issue, thread highlights, repo-facts, plan-context (with
its own Deviations section if present), and the candidate PR's title,
description, commits, diff, and test evidence — comes from the bundle
text. Fetch nothing else; open no other file. Eval mode always grades
a complete package: run every check in `rubric.md` under its full
verdict rule, with no check skipped.

## The scope seam (live mode only)

In live mode, read `scope.md` before anything else, even before the
issue. If `scope.md`'s repo line still reads as a bracketed
placeholder, stop without grading and say to get the cohort's finalized
scope file from the instructor — never guess a repo. Otherwise, confirm
the PR package under review targets the repo `scope.md` names; refuse
to grade a PR package targeting any other repository, however close
the fix looks. Apply `scope.md`'s house rules (branch naming, one PR
per issue per student, template always used, credit attaches to
opening the PR) as part of what "ready to submit" means in live mode —
a package that violates a stated house rule is not ready, even if the
rubric's own checks all pass. Eval mode ignores `scope.md` entirely:
its bundle has no house to violate.

## The voice seam (live mode only)

In live mode, read `voice-guide.md` after gathering evidence and
before assembling the verdict. Hold the draft PR title and description
— the two pieces of outgoing text, nothing else in the package — against
every rule in `voice-guide.md`. Report any rule the draft breaks by
name in the reply's summary, quoting the line that breaks it. The
voice guide never changes the verdict on its own: a broken voice rule
is reported, not scored, unless a `rubric.md` check explicitly reads
voice (none currently do). Eval mode ignores `voice-guide.md` entirely
— voice is personal, carries no gold labels, and the rubric's own
Template/AI-use-disclosure/Maintainer-direction checks already cover
universal communication quality.

## Component reads

`rubric.md` defines the checks and the verdict rule — it decides WHAT
a ready PR looks like. `references/evidence-guide.md` maps WHERE each
evidence family lives, in both eval-bundle and live-mode locations.
`procedure.md` is executed exactly as written — it decides HOW the
grading happens, in what order, and how grades assemble into a
verdict. When `procedure.md` is silent on a step, report the gap in
the reply's summary instead of inventing a step to fill it. If
`rubric.md` has no checks in its table, or `procedure.md` has no
content under its stage headings, refuse to grade and say so plainly:
an empty rubric or procedure is not a tool that can be trusted to
invent judgment at runtime, by design.

## Verdict and output

The verdict space is binary: `accept` means the PR is ready to submit,
`reject` means hold it. There is no third verdict and no score.
End the reply with the fenced JSON block below, valid and last, with
nothing after it. A readable per-check summary may come before the
block, but the block is what the harness parses and it must be
present:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

Evidence first: no check gets a grade without the fact or quote from
`procedure.md`'s Evidence gathering stage that decided it — "looks
fine" is never evidence. Grade the thing, not the polish: judge the
diff against the plan, the evidence against the test plan, and the
description against the diff, never the write-up's length or
confidence. The rubric decides, not the run: apply `rubric.md`'s pass
conditions exactly as written, even when a result feels off — a
feeling is a reason to revise the rubric later, not to override a
check now. The procedure decides how, not the run: follow
`procedure.md`'s read order and check order as written, and report a
gap rather than improvising around it. Unclear defaults to fail unless
the rubric's verdict rule says otherwise: an unverifiable claim is a
claim that is not ready to submit.
