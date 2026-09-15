# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in
the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the
wrong label is not graded.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68

**Verdict output**

```
Ranked read-out (live mode, 3 candidates graded independently, then ranked by fit profile):

1. #68 — Keyword search raises `ZeroDivisionError` on empty index — accept
   Fit: touches the RAG retrieval pipeline directly — closer to the "AI-assisted
   development" surface I want to practice on than a pure backend fix, with a named
   test to un-xfail and a clear 2-4h estimate.

2. #60 — Faithfulness checker crashes on `text: None` — accept
   Fit: also inside the RAG/evaluation stack (faithfulness checking is part of the AI
   feedback pipeline), with explicit repro steps, though no effort estimate was given.

3. #72 — `verify_password` raises `UnknownHashError` — accept
   Fit: smallest estimated effort (1-2h) and cleanly scoped, but it's a general
   security/API fix rather than an AI-pipeline one, so it ranks third against my
   stated interest.

All three: no assignee, no linked PR, no comments (nothing claiming them), all carry
`good first issue`, none has a maintainer reply yet ("Maintainer responsive" —
preferred — fails identically on all three and doesn't break the tie).

{
  "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68",
  "checks": [
    {"name": "Maintainer alive", "grade": "pass", "evidence": "last commit 2026-08-24 by Andrew Burke (human), ~22 days before capture"},
    {"name": "Maintainer responsive", "grade": "fail", "evidence": "issue has 0 comments, no maintainer reply"},
    {"name": "Repo in use", "grade": "pass", "evidence": "repo not archived; last push to any branch 2026-09-10, ~5 days before capture"},
    {"name": "Scope fits a newcomer", "grade": "pass", "evidence": "single bounded bug: KeywordSearcher.search() should return [] on empty index instead of raising ZeroDivisionError; one file, one xfail test to un-mark, 2-4h estimate"},
    {"name": "Not already claimed", "grade": "pass", "evidence": "assignees: none; no linked PRs; no claim comments (0 comments total)"},
    {"name": "AI contribution allowed", "grade": "pass", "evidence": "no CONTRIBUTING.md or AI-policy file found in repo; silence passes"},
    {"name": "Good-first-issue label", "grade": "pass", "evidence": "labels include 'good first issue'"}
  ],
  "verdict": "accept"
}
```

---

## Eval iterations

Quote source text directly in each field below. Paraphrase does not satisfy them.

**Run history**

1. `--only issue-01,issue-02,issue-03` — 2/3
2. `--only issue-01` (after revising the "Scope fits a newcomer" check to stop treating
   multi-file docs edits as an umbrella issue) — 1/1
3. `--only issue-04,issue-05,issue-06,issue-07` — 4/4
4. `--only issue-08,issue-09,issue-10,issue-11` — 4/4
5. `--only issue-12,issue-13,issue-14,issue-15` — 3/4
6. `--only issue-15` (after folding "history of abandoned attempts / closed unmerged
   linked PRs" into the same check) — 1/1
7. `--only issue-16,issue-17,issue-18,issue-19,issue-20` — 3/5
8. `--only issue-19,issue-20` (after adding the "unendorsed feature request" clause to
   the same check) — 2/2
9. Full run (no `--save-run`) — 20/20
10. Full confirming run, `--save-run eval-run.txt` — **19/20** (matches `eval-run.txt`)

**Issue analysis**

issue-01, gold = accept, my rubric's final run = reject.

My rubric rejected issue-01 because the "Scope fits a newcomer" check fired: the issue
proposes a new docs page plus edits to four other files (`manage-pkgs.rst`,
`pip-interoperability.rst`, `new-features.md`, and optionally `troubleshooting.rst`) for
one GA feature rollout. My check's evidence is "issue body and comment thread," and on
this run it read "touches 5 files" as the issue being too broad for a newcomer — even
though every edit serves one clearly stated goal (give the new `conda install` PyPI
workflow a permanent docs home) and none of it is a maintainer-unsettled design
question. The gold label says accept, and I think that's right: it's a large but
single-purpose documentation task, not a bundle of independent deliverables. This also
isn't fully stable — my rubric's wording for this check has flipped between accept and
reject on this exact issue across different runs (see Run history), because "is this
really an umbrella?" is a judgment call for the LLM to make, not a number it can just
check.

**Check rationale**

Quoted as currently written in `rubric.md`, the "Scope fits a newcomer" pass condition:

> "issue is not an explicit umbrella/tracking issue (multiple independent user-facing
> deliverables explicitly meant to be split into separate tracked issues — a single
> bug's listed root causes or candidate fixes for one problem do NOT count), is not a
> pure usage/support question, the thread shows no unresolved design debate, no
> maintainer comment says the fix touches core internals, the thread does not show a
> history of 2+ closed/unmerged linked PRs or repeated claim-then-abandon cycles, and
> if the issue is a feature request (not a bug report) it carries some sign of
> maintainer buy-in (a maintainer/member comment, or an accepting label like "help
> wanted"/"good first issue"/"accepted") — an unendorsed feature idea with zero
> comments and no labels fails, since nobody has confirmed the work is wanted —
> multiple related file edits toward one clearly stated goal do not by themselves
> count as unscoped"

This check went through three revisions during eval iteration. The first pass just
said "single bounded task, not an umbrella issue," which wrongly rejected issue-01 for
touching multiple docs files — it was reading file count as scope, when what actually
matters is whether the work is one coherent goal or a bundle of separately-tracked
deliverables. The second revision (after issue-15) added the "history of abandoned
attempts" clause, because an issue can look perfectly bounded on its face —
unassigned, no open PR — while a trail of closed/unmerged linked PRs and repeated
claim-then-abandon cycles in the thread says it's actually much harder than it reads.
The third revision (after issue-19 and issue-20) added the "unendorsed feature
request" clause, to catch the opposite failure: a numbered list of causes/fixes for
one bug isn't an umbrella, but a feature idea with zero comments and no accepting
label is unscoped in a different way — nobody has confirmed the work is even wanted.
The final wording tries to separate "looks big" (file count, numbered lists) from "is
actually unscoped" (explicit tracking-issue structure, unsettled design, abandoned
history, or no maintainer buy-in on a feature ask) — the first is a shape a bounded
task can have; the second is what actually kills a first contribution.

**Trade-offs**

What this check gives up is determinism. Every other required check in my rubric
grades off a number or a fact you can point to — a commit date, an assignee field, a
release date — so the same issue grades the same way every run. "Scope fits a
newcomer" grades off a judgment call instead (is this an umbrella? is the design
still unsettled?), and issue-01 is direct proof that costs something: the exact same
rubric wording produced `accept` on one run and `reject` on another for the same
issue, with nothing about the issue itself changing. I accept that trade-off because
the alternative — a hard numeric rule like "reject if the issue touches more than N
files" — is worse in a different way: it would have rejected issue-01 permanently
(5 files) while still passing genuinely unscoped issues that happen to touch only one
file. A rule that can occasionally flip on a hard case beats a rule that is
consistently wrong on that same case. Nothing else in my rubric needed to change for
this: the other four required checks (maintainer alive, repo in use, not already
claimed, AI contribution allowed) stayed numeric/factual through every revision and
never flipped across reruns.

---

## Selection rationale

Graded on whether all three are answered, in your own words. Not on how good the
reasoning is, and not on length — a short honest answer to each earns the full marks.
This is also the basis for the claim comment you write in Unit 2.

**Selection rationale**

1. **Fit:** I've worked mainly in Python, and this issue is a contained bug in
   `rag/retriever/keyword_search.py` — one function (`KeywordSearcher.search()`) that
   should return an empty list on an empty index instead of raising
   `ZeroDivisionError` from rank-bm25's IDF computation. The estimated 2-4h effort fits
   what I can commit to for a first issue, and it's inside the RAG pipeline, which is
   the part of this codebase closest to what I actually want to get better at:
   AI-assisted workflows.

2. **What the verdict caught vs. what I weighed:** The verdict confirmed the
   mechanical stuff — repo alive and in use, nobody's claimed it, no policy blocking
   AI-assisted contributions, single bounded function. What it couldn't weigh is that
   I picked it over #60 and #72 specifically because it's the more central
   RAG-pipeline bug of the three, and because rank-bm25 is a library I haven't touched
   before — I wanted a first issue that would make me actually read someone else's
   library code, not just my own function. The rubric also flagged "Maintainer
   responsive" as a fail (no maintainer has commented on any of the three yet), which
   I'm weighing as low-risk here since the Path Review house rule means
   shared/unclaimed issues aren't a signal of anything going wrong — the repo's own
   recent commit activity already told me maintainers are active.

3. **Anticipated difficulty:** The fix itself sounds small and isolated, but I haven't
   reproduced it yet, so there's real risk the "2-4h" estimate is optimistic — I don't
   yet know if the empty-index case is truly isolated to one function, or if
   rank-bm25's IDF computation has other edge cases the fix needs to handle too. I also
   don't know rank-bm25's internals yet, so there may be a short learning curve before
   I can confirm the fix is complete rather than just silencing the crash. That's
   exactly what Unit 2's reproduction step is for.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
