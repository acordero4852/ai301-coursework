# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`. This file is graded at the path above; a copy kept anywhere else in the
repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Posted upstream

andycordero4852

[Your GitHub username, exactly as it appears on your profile - no @, no
profile URL. Your comment upstream is identified by this name, and it is
the only thing that ties it to you. Several students may plan the same
house issue, so this is what keeps their comments off your score and
yours off theirs.]

[https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68#issuecomment-5805081660](https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68#issuecomment-5805081660)

[Link to the comment where you posted your plan on the issue. Use the comment's own
permalink. **Then paste the text of that comment underneath the link** — the pasted text is
what this field is graded on, so copy across what you actually posted.]

Reproduced this. Environment: Python 3.14.7, `rank-bm25` 0.2.2,`structlog` 26.1.0 (matches the repo's `rank-bm25>=0.2.2` pin), macOS.

```sh
>>> from rag.retriever.keyword_search import KeywordSearcher
>>> s = KeywordSearcher()
>>> s.index([])
Traceback (most recent call last):
  ...
  File ".../rank_bm25.py", line 52, in _initialize
    self.avgdl = num_doc / self.corpus_size
ZeroDivisionError: division by zero
index() always builds a BM25Okapi from the tokenized corpus, even when
chunks is empty. With chunks == [], rank_bm25's BM25._initialize
divides num_doc by corpus_size, and corpus_size is 0.
```

Two control runs rule out the rest of the class:

```sh
>>> KeywordSearcher().search("python", top_k=10)   # index() never called
[]                                                   # search()'s own guard handles this fine
>>> s2 = KeywordSearcher()
>>> s2.index([{"id": 1, "text": "python programming"}])
>>> s2.search("python", top_k=10)
[{'id': 1, 'text': 'python programming', 'bm25_score': -0.27...}]  # non-empty index works as expected
```

So it's isolated to `index()` not guarding the empty-corpus case the way
`search()` already does — matches the existing test_empty_index xfail
(manifest H-01). I'll post my plan next.

---

## Your branch

**Branch**

`fix/68-empty-index-zerodivision`

**Evidence**

### Before (Unit 2 reproduction, unpatched `keyword_search.py`)

```pycon
>>> from rag.retriever.keyword_search import KeywordSearcher
>>> s = KeywordSearcher()
>>> s.index([])
Traceback (most recent call last):
  ...
  File ".../rank_bm25.py", line 52, in _initialize
    self.avgdl = num_doc / self.corpus_size
ZeroDivisionError: division by zero
```

Control runs from the same reproduction, showing the rest of the class already worked:

```pycon
>>> KeywordSearcher().search("python", top_k=10)   # index() never called
[]                                                   # search()'s own guard handles this fine
>>> s2 = KeywordSearcher()
>>> s2.index([{"id": 1, "text": "python programming"}])
>>> s2.search("python", top_k=10)
[{'id': 1, 'text': 'python programming', 'bm25_score': -0.27...}]  # non-empty index works as expected
```

### After (same steps, re-run on `fix/68-empty-index-zerodivision`)

```pycon
>>> from rag.retriever.keyword_search import KeywordSearcher
>>> s = KeywordSearcher()
>>> s.index([])
>>> s.search("python", top_k=10)
[]
```

`index([])` now returns normally instead of raising, and `search()` afterward still returns
`[]` — the same observable behavior the Before block's control run already showed for
`search()` alone.

### Verification

```text
$ python -m pytest tests/unit/test_keyword_search.py -v
tests/unit/test_keyword_search.py::TestKeywordSearcher::test_results_sorted_by_score_descending PASSED
tests/unit/test_keyword_search.py::TestKeywordSearcher::test_query_matching_no_documents PASSED
tests/unit/test_keyword_search.py::TestKeywordSearcher::test_query_matching_multiple_documents PASSED
tests/unit/test_keyword_search.py::TestKeywordSearcher::test_case_insensitive_matching PASSED
tests/unit/test_keyword_search.py::TestKeywordSearcher::test_top_k_limit PASSED
tests/unit/test_keyword_search.py::TestKeywordSearcher::test_top_k_larger_than_results PASSED
tests/unit/test_keyword_search.py::TestKeywordSearcher::test_results_have_bm25_score PASSED
tests/unit/test_keyword_search.py::TestKeywordSearcher::test_results_preserve_chunk_fields PASSED
tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index PASSED
tests/unit/test_keyword_search.py::TestKeywordSearcher::test_index_not_called_returns_empty PASSED
tests/unit/test_keyword_search.py::TestKeywordSearcher::test_multi_word_query PASSED
tests/unit/test_keyword_search.py::TestKeywordSearcher::test_tokenization PASSED
tests/unit/test_keyword_search.py::TestKeywordSearcher::test_tokenization_case_handling PASSED
tests/unit/test_keyword_search.py::TestKeywordSearcher::test_large_corpus PASSED
tests/unit/test_keyword_search.py::TestKeywordSearcher::test_special_characters_in_query PASSED
tests/unit/test_keyword_search.py::TestKeywordSearcher::test_exact_phrase_matching PASSED
tests/unit/test_keyword_search.py::TestKeywordSearcher::test_single_word_chunks PASSED

============================== 17 passed in 0.08s ===============================
```

`test_empty_index` (manifest H-01) passes with its `@pytest.mark.xfail(strict=True, ...)`
marker removed — it was `strict=True`, so the suite would have failed outright if the test
passed while still marked xfail. All 16 other tests, covering non-empty-index behavior,
remain unchanged and passing.

### Result

The reproduced crash (`ZeroDivisionError` from `rank_bm25`'s `avgdl = num_doc /
corpus_size` on a zero-length corpus) is gone: `index([])` on the built change returns
normally, and the previously-xfailed regression test for this exact issue now passes
unmarked. Every other test in the file — covering `index()` and `search()` with non-empty
input — still passes, so the fix does not change behavior outside the empty-index case the
plan scoped it to.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Smoke run, `--limit 3`: 3/3 agree (`pkg-01` wrong-cause, `pkg-02` and `pkg-03`
   clear-accept, all matched gold). No revision needed.
2. Full run (no `--limit`/`--only`, no `--save-run`): 19/20 agree, category floor met
   (`clear-accept 6/7`, `scope-creep 4/4`, `thread-convention 2/2`, `unbuildable 3/3`,
   `wrong-cause 4/4`). One disagreement: `pkg-14` (gold accept, rubric reject, failing
   `Executability`).
3. Confirming full run, `--save-run eval-run.txt` (2026-09-24T00:52:10Z): 18/20 agree,
   category floor met (`clear-accept 5/7`, others unchanged). Two disagreements this
   time: `pkg-13` and `pkg-14` (both gold accept, both failing `Executability`). No
   rubric, evidence-guide, or procedure text changed between runs 2 and 3 — the extra
   disagreement on `pkg-13` is grading variance between runs on a borderline case, not a
   revision; the bar (18/20) still passes exactly, so this run stands as the confirming
   one rather than re-running to chase a better number.

**Package analysis**

Package ID: `pkg-14` (zellij-org/zellij#5174)

Rubric verdict: reject (failed `Executability`)

Gold verdict: accept (category: clear-accept)

Reasoning: the candidate plan names the general area ("the client attach/reattach path in
`zellij-server`... and `zellij-client`'s terminal query issuance") and a concrete method
already working to pin the exact site ("exact functions to be pinned in the PR after
tracing the query issuance with debug logs, which I have working"), but does not name the
exact function(s) up front. My `Executability` check's pass condition treats "no undecided
branch points... left for build time" as the bar, and my grading procedure read "exact
functions to be pinned... after tracing" as a real where-decision deferred to build time,
so it failed the check. The gold label reads it as accept because a stranger already knows
which two subsystems to open and has a working, named method (a specific debug-log trace)
to find the precise site — that's different from a plan that names no file or area at all
(e.g. `pkg-17`/`pkg-18`, both correctly rejected as unbuildable). My check doesn't yet
distinguish "the exact line is unpinned but the method to find it is concrete and named"
from "I don't know where this is."

**Check rationale**

> Executability | The plan's files/areas, approach, and order of work. | Pass if a
> stranger could start working from the plan alone: named files or areas, a concrete
> approach, no undecided branch points ("layer A or B, not sure") left for build time.
> Fail if any real decision (what to change, where, or how) is deferred to whoever builds
> it. | required

This check exists because the eval set's `unbuildable` category (`pkg-10`, `pkg-17`,
`pkg-18`) is built entirely around plans that defer every real decision to build time
("recover() somewhere", "upstream or vendored, whichever is easier", "investigate the
input stack, not sure which layer") — my rubric needs a check that reads the plan's
Files/Approach and fails exactly that shape, and this check does: all three `unbuildable`
packages correctly reject on it. I did not revise this check after the confirming run,
since `pkg-13`/`pkg-14` are a narrower, arguable edge (a named subsystem plus a concrete
pinning method) rather than the "no idea where" shape the check was built to catch, and
the bar was already met without loosening it.

**Trade-offs**

This check's strictness is what makes the `unbuildable` category go 3/3 (a looser
"names a general area" bar would risk passing `pkg-17`'s "gocui? tcell? not sure" or
`pkg-18`'s "upstream or vendored, whichever is easier" if either had named even one
subsystem). The cost, visible in this run, is `pkg-13` and `pkg-14`: both name a
subsystem and a concrete way to pin the exact site rather than the exact site itself, and
my check reads that as a still-open real decision. I accept that trade-off for now rather
than loosen the pass condition to admit "method to find the site is named, even if the
site itself isn't" — the bar (18/20) is already met at 18/20 and 19/20 across two full
runs, and loosening this wording risks blurring the exact line that keeps the
`unbuildable` category at 3/3, which I'd want to re-check with `pkg-10,pkg-17,pkg-18` as
canaries before ever changing it.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
