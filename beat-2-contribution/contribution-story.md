# Contribution: Keyword search raises `ZeroDivisionError` when the index is empty

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Andy Cordero
**GitHub Username:** acordero4852
**Issue:** https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68
**Pull Request:** https://github.com/codepath/pathreview-ai301-fa26-s3/pull/83

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: issue-scout is my renamed Unit 1 skill, and no scan has run yet**

The installed issue-scout (2026-10-06) is my Unit 1 issue-select skill under a new name. My evidence guide is unchanged, and the rubric has one added clause saying tests, benchmarks and docs for existing behavior are not feature requests. The Unit 5 scout package with search was not available to me, so the tool still only grades issue URLs I hand it, and as of 2026-10-07 my search strategy has no queries and I have run no scan. I picked issue #68 from my Unit 1 live run instead. The only changes so far are a backup of both skills and a draft strategy file that records the gap.

**Unit 6: voice-guard not installed, repo-memory file written (2026-10-12)**

Facts (verified 2026-10-12): The voice-guard package and its setup instructions were not available to me, so I did not install it and no guard check has run. My claim on #68 was posted on 2026-09-19 without a guard, and no guarded draft exists. I wrote a 28-line `CLAUDE.local.md` in my Path Review clone from `docs/CONTRIBUTING.md`, the Makefile and the PR template, kept out of commits through `.git/info/exclude`; I have not yet confirmed a fresh session loads it. [YOU: what helped, what fell short, and what you changed or would change for each tool.]

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

**Facts (verified 2026-10-12):** I claimed issue #68 on 2026-09-19, picked from my Unit 1 issue-select live run (#68, #60, #72), not from a Unit 5 scan. No search ran: my search strategy has no queries and the scout only grades issue URLs. On 2026-10-12 I ran the scout in direct mode on #68, #60 and #72. It graded #68 accept: every required check passed, "maintainer responsive" (preferred) failed because no maintainer has commented in 32 days, and it carries the good first issue label. The scout passed "not already claimed" only through the Path Review house rule: six open PRs (including my #83) link to #68. I found no AI-use policy in CONTRIBUTING.md, the PR and issue templates, the README or SETUP.md; that is not proof of permission. Saved scan: `ai301-lab-evidence/scans/2026-10-12-scan-direct.md` (local only).

[YOU: why #68 fits you and what you hope to learn.]

[YOU: your read of the scout, including that it could not search.]

**Claim comment:** https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68#issuecomment-5738409951 (posted 2026-09-19)

**Toolkit**

- Tool: issue-scout, used in live mode on explicit issue URLs (no search; my strategy has no queries).
- What it produced: a direct-mode scan of #68, #60 and #72 on 2026-10-12; #68 graded accept, with "maintainer responsive" failing and "not already claimed" passing only through the Path Review house rule.
- Difficulties: [YOU]
- What changed: [YOU]

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

Facts (checked 2026-10-12, read-only; I installed nothing and changed no files):

- OS: macOS 26.6.2, arm64. Base commit: `2f4e82f` (equals `upstream/main` and `origin/main`). Branch: `fix/68-empty-index-zerodivision` (fix commit `eb628fb`, tracks my fork). Remotes: `origin` = my fork, `upstream` = codepath/pathreview-ai301-fa26-s3.
- Required by the repo (docs/SETUP.md, pyproject.toml, CI): Python >= 3.11 (CI uses 3.11), Node >= 18, npm >= 9, Docker >= 24 with Compose for Postgres 16, Redis 7 and Chroma. Python has no lockfile (deps in `pyproject.toml`, `rank-bm25>=0.2.2`, `structlog>=24.1.0`); the frontend has `package-lock.json`.
- Documented setup (docs/SETUP.md, Makefile): `cp .env.example .env`, `docker compose up -d`, `make setup` (venv, `pip install -e ".[dev]"`, pre-commit, `alembic upgrade head`, seed script, `npm install`). Tests for this issue need only the Python venv: `make test-unit`.
- Actual versions in my `.venv` (created 2026-09-28): Python 3.11.9, rank-bm25 0.2.2, structlog 26.1.0, pytest 9.1.1, ruff 0.16.9, mypy 2.3.1, black 26.5.1, pathreview 0.1.0 (editable). System tools: git 2.56.0, node v22.23.2, npm 10.9.8, Docker 29.8.2 (daemon not running today), GNU Make 3.81. `tests/unit/test_keyword_search.py`: 17 passed on my branch today.
- Check this: my 2026-09-24 repro comment on the issue says Python 3.14.7, but the venv is 3.11.9 (and system `python3` is 3.14.7). Say which interpreter produced the repro.
- Not recorded here: `.env` (holds secrets, kept out of the story).

[YOU: which setup commands you actually ran, and the problems you hit and how you solved them, or "none came up". I do not know this from the repo.]

**Steps to Reproduce**

Facts (baseline re-run 2026-10-12 by my coding assistant against commit `2f4e82f`, unpacked with `git archive` so my working tree stayed untouched; project `.venv`, Python 3.11.9, rank-bm25 0.2.2, structlog 26.1.0, macOS 26.6.2 arm64):

1. Start at baseline commit `2f4e82f52efbcfcc57d65b3fa5348672163ca088` (equals `upstream/main`) with the Python dependencies from `pip install -e ".[dev]"`; no services needed.
2. `python -c "from rag.retriever.keyword_search import KeywordSearcher; KeywordSearcher().index([])"`
3. Observed: traceback ending `rank_bm25.py", line 52, in _initialize ... ZeroDivisionError: division by zero`; exit code 1.
4. Controls: `KeywordSearcher().search("python")` without calling `index()` returned `[]` (exit 0); `index([{"text": "python code"}])` then `search("python")` returned 1 result (exit 0).
5. Test form: `pytest tests/unit/test_keyword_search.py --runxfail -k test_empty_index` failed with the same ZeroDivisionError (exit 1).

[YOU: confirm these steps match what you ran on 2026-09-24, and which interpreter that was.]

**Reproduction Evidence**

- **Commit showing reproduction:** none exists. My branch has one commit past the baseline, `eb628fb`, which is the fix, not a reproduction (my reproduction was posted as a comment: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68#issuecomment-5805081660).
- **Screenshots/logs:** full command output with exit codes kept locally (not in the repo).
- **My findings:** The behavior in the issue reproduced at baseline in every run above. The issue's claim that `search()` already returns `[]` held. I have not checked whether `rag/retriever/hybrid.py` hits this crash. [YOU: your own findings in your words.]

**Toolkit**

- Tool: repro-check, applied on 2026-10-12 to my already-posted claim and repro comments for #68.
- What it produced: verdict reject (hold) on the evidence as posted. Environment graded unclear (my comment says Python 3.14.7; the project venv that reproduced is 3.11.9; no OS version or base commit stated); steps, behavior-shown, honesty and conventions-disclosure passed. It noted that "rule out the rest of the class" claims more than two controls show.
- Difficulties: [YOU]
- What changed: [YOU]

---

## Solution Approach

**Analysis**

Confirmed (traced 2026-10-12 at baseline `2f4e82f`): `KeywordSearcher.index()` in `rag/retriever/keyword_search.py` set `self.chunks = chunks` and then always built `BM25Okapi(tokenized_corpus)`. With `chunks == []` the corpus is empty, and rank-bm25's `_initialize` (`rank_bm25.py` line 52, version 0.2.2) computes `num_doc / self.corpus_size` with `corpus_size == 0`, which raises `ZeroDivisionError`. `search()` already returns `[]` when `self.bm25` is unset or `self.chunks` is empty, so only `index()` was unguarded. `test_empty_index` (manifest H-01) was `xfail(strict=True)` for this reason and failed with the same error under `--runxfail`.

Not established: whether any runtime path calls `index([])`. A search of `rag/`, `api/`, `agent/`, `ingestion/`, `core/` and `scripts/` found no caller of `KeywordSearcher.index()` outside the tests; `rag/retriever/hybrid.py` only takes a `KeywordSearcher` and calls `search()`. So the crash is confirmed on the method and the test, not on a user-facing flow. I have not checked behavior when a chunk lacks a `text` key; that is a different input and outside this issue.

[YOU: your analysis of the cause in your own words.]

**Proposed Solution**

Verified plan (from my 2026-09-24 plan comment, https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68#issuecomment-5805282718): guard the empty case in `index()` so no `BM25Okapi` is built over zero documents; leave the non-empty path unchanged; remove the `@pytest.mark.xfail` marker from `test_empty_index`. Not changing: `search()`, `_tokenize()`, rank-bm25, `hybrid.py`, or any other retriever. Scope boundary: `rag/retriever/keyword_search.py` and `tests/unit/test_keyword_search.py` only.

[YOU: why this approach, in your words.]

**Implementation Plan**

1. `rag/retriever/keyword_search.py`: in `index()`, if `chunks` is empty, set `self.bm25 = None`, log `keyword_index_built` with `chunk_count=0`, and return before building the index.
2. `tests/unit/test_keyword_search.py`: delete the `xfail` marker on `test_empty_index` (manifest H-01).
3. Checks: `index([])` raises nothing and `search()` after it returns `[]`; `test_empty_index` passes unmarked; the rest of `test_keyword_search.py` still passes (17 passed on the branch, 2026-10-12); `make check && make test-unit` and CI green per docs/CONTRIBUTING.md.

Recorded deviation (verified from the diff, `2f4e82f..eb628fb`, +4 -4): my plan comment said the guard would set `self.chunks = []`. The commit does not add that line, because `self.chunks = chunks` already runs just above the guard, so `chunks` is already `[]` there. [YOU: whether you count this as a deviation, and anything else that changed.]

**Toolkit**

- Tool: plan-check was not run. It is not installed in `~/.claude/skills`; a copy exists in `ai301-coursework/tools/plan-check/`. The plan was already posted (2026-09-24) and built (commit `eb628fb`, 2026-09-28) before this check.
- What it produced: nothing. I traced the code by hand and with the baseline run instead.
- Difficulties: [YOU]
- What changed: [YOU]

---

## Testing Strategy

Facts (commands run 2026-10-12 from the repo root with the project `.venv`; branch `fix/68-empty-index-zerodivision` at `eb628fb`; baseline `2f4e82f`; commands from docs/CONTRIBUTING.md, Makefile and .github/workflows/ci.yml; Python checks need only the venv, no Docker):

Smallest regression check: `test_empty_index` (`index([])` then `search("python", top_k=10) == []`).
- Before (baseline, `pytest tests/unit/test_keyword_search.py --runxfail -k test_empty_index`): 1 failed, `ZeroDivisionError: division by zero` at `rank_bm25.py:52`, exit 1. Direct call `index([])` also exit 1.
- After (branch, `pytest tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index -v`): 1 passed, no xfail marker, exit 0.

Other checks on the branch:
- `pytest tests/unit/test_keyword_search.py -q`: 17 passed (non-empty behavior unchanged), exit 0.
- `ruff check .`: All checks passed. `black --check .`: 110 files unchanged (CI uses `--check`; `make format` writes files, so I did not run it). `mypy api/ core/ ingestion/ rag/ agent/ safety/ --ignore-missing-imports`: no issues in 76 files. All exit 0.
- `pytest tests/unit -q --tb=short`: 376 passed, 52 xfailed, 3 warnings, exit 0. The 52 xfails are other seeded bugs, not this fix.
- `pytest tests/integration`: no tests ran, exit 5; CI treats exit 5 as success, and the real job needs Postgres and Redis through Docker, whose daemon is not running here. Not a pass from me.
- `cd frontend && npm test -- --run`: did not finish locally (no output in about 3 minutes; I stopped it). Unverified here; my PR body and CI report it green. This change touches no frontend code.
- CI on PR #83 (checked 2026-10-12): lint, typecheck, test-unit, test-integration, frontend and eval all SUCCESS.

My rationale: I used `test_empty_index` as the regression check because it is the issue's own covering test, so it fails before the fix and passes after with no new test needed. I ran the full unit suite to confirm other behavior stayed the same, and ruff, black and mypy because CONTRIBUTING.md and CI require them. I left integration and frontend to PR #83's CI, because I did not complete them locally.

**Toolkit**

- Tool: no extension tool; pytest, ruff, black, mypy and the repo's CI, run through my coding assistant on 2026-10-12.
- What it produced: the before/after results above, with exit codes, kept in a local log.
- Difficulties: the frontend test run hung locally and was not completed; integration tests need Docker, which is not running. [YOU: anything else.]
- What changed: [YOU]

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**2026-09-19** (fact) Posted my claim comment on #68 (issuecomment-5738409951). [YOU: what you did and decided that day.]

**2026-09-24** (fact) Posted a repro comment (issuecomment-5805081660) and a plan comment (issuecomment-5805282718). [YOU: what you did and decided that day.]

**2026-09-28** (fact) Committed `eb628fb` "fix(rag): guard KeywordSearcher.index() against empty corpus" at 13:14 -0400 (+4 -4: an early-return guard in `index()` in `rag/retriever/keyword_search.py`, and the `xfail` marker removed from `test_empty_index`) and opened PR #83 at 17:28 UTC with that single commit. [YOU: the decisions you made while building it, and any wrong turns.]

**2026-10-12** (fact) I did not change code today. I re-checked the existing commit against the plan: the diff touches only the two planned files, `search()`, `_tokenize()` and `hybrid.py` are unchanged, and no dependency changed. One difference from the plan comment: the guard does not set `self.chunks = []`, because `self.chunks = chunks` already runs just above it (recorded under Solution Approach). Evidence did not undermine the fix: baseline fails, branch passes `test_empty_index`, and the unit suite is 376 passed, 52 xfailed. [YOU: anything you want to add.]

**Toolkit**

- Tool: none of the course extensions did this phase's work. I built the fix on 2026-09-28; today's step re-checked it with git and pytest through my coding assistant.
- What it produced: no new code. A verified diff and the dated entries above.
- Difficulties: [YOU]
- What changed: [YOU]

---

## Pull Request

**PR Link:** https://github.com/codepath/pathreview-ai301-fa26-s3/pull/83

**PR Description:** PR #83, "fix(rag): guard KeywordSearcher.index() against empty corpus", opened 2026-09-28 from `acordero4852:fix/68-empty-index-zerodivision` into `main` (closes #68). It guards the empty-corpus case in `KeywordSearcher.index()` the way `search()` already does (sets `self.bm25 = None` and returns before building `BM25Okapi`) and removes the `xfail` marker from `test_empty_index` (manifest H-01). Two files changed: `rag/retriever/keyword_search.py` (+4) and `tests/unit/test_keyword_search.py` (-4). The PR body includes an AI-use disclosure.

**Maintainer Feedback:**

- 2026-10-07: none yet (PR page shows no comments or reviews).

**Status:** Awaiting review (open, not draft, as of 2026-10-07). All CI checks on the PR passed: lint, typecheck, test-unit, test-integration, frontend, and RAG Evaluation.

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
