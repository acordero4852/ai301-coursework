# Plan: KeywordSearcher.index() raises ZeroDivisionError on an empty index

Issue: `codepath/pathreview-ai301-fa26-s3#68`

## Diagnosis

`KeywordSearcher.index()` (`rag/retriever/keyword_search.py`) always calls
`BM25Okapi(tokenized_corpus)`, even when `chunks` is empty. With an empty
corpus, `tokenized_corpus == []`, and `rank_bm25`'s `BM25._initialize`
computes `self.avgdl = num_doc / self.corpus_size` — dividing by
`corpus_size == 0`.

Reproduced locally against the repo's current `keyword_search.py` and an
installed `rank_bm25`:

```pycon
>>> s = KeywordSearcher()
>>> s.index([])
Traceback (most recent call last):
  ...
  File ".../rank_bm25.py", line 52, in _initialize
    self.avgdl = num_doc / self.corpus_size
ZeroDivisionError: division by zero
```

Control run — `search()` on the same class already guards this case and
does not crash:

```pycon
>>> s = KeywordSearcher()          # index() never called
>>> s.search("python", top_k=10)
[]                                  # logs "keyword_search_empty_index"
```

Control run — `index()` with a non-empty corpus works as expected:

```pycon
>>> s2 = KeywordSearcher()
>>> s2.index([{"id": 1, "text": "python programming"}])
>>> s2.search("python", top_k=10)
[{'id': 1, 'text': 'python programming', 'bm25_score': -0.27...}]
```

So the crash isn't in BM25 scoring or in `search()`'s empty-index handling
(both already work) — it's specifically that `index()` builds a
`BM25Okapi` unconditionally instead of guarding the empty-corpus case the
way `search()` already does.

## Scope

In scope: guard `index()` against an empty `chunks` list so it does not
build a `BM25Okapi` over zero documents, mirroring the existing guard
pattern in `search()`.

Not in scope: `BM25Okapi`/`rank_bm25` itself, `search()`'s ranking logic,
`_tokenize()`, and any other retriever.

## Files

- `rag/retriever/keyword_search.py` (the `index()` guard)
- `tests/unit/test_keyword_search.py` (un-xfail `test_empty_index`)

## Approach

1. In `KeywordSearcher.index()`, if `chunks` is empty: set `self.chunks =
   []` and `self.bm25 = None`, log `keyword_index_built` with
   `chunk_count=0`, and return before constructing `BM25Okapi`.
2. Otherwise, keep the existing tokenize-and-build path unchanged.
3. Remove the `@pytest.mark.xfail(strict=True, ...)` marker from
   `test_empty_index` in `tests/unit/test_keyword_search.py` (manifest
   H-01), since the test now passes.

## Test plan

- Re-run the reproduction above after the fix: `KeywordSearcher().index([])`
  must return `None` with no exception (currently raises
  `ZeroDivisionError`), and a subsequent `search("python", top_k=10)` must
  return `[]`.
- Run `tests/unit/test_keyword_search.py::TestKeywordSearcher::test_empty_index`
  with the `xfail` marker removed: it must pass outright (it was
  `strict=True`, so it would previously fail the suite if it started
  passing without the marker removed).
- Run the full `test_keyword_search.py` suite to confirm the non-empty-index
  behavior (already covered by the other tests) is unchanged.

## Deviations

Nothing changed; the plan held. The built diff on `fix/68-empty-index-zerodivision`
matches the Approach section one-for-one:

- Step 1 (guard `index()` on empty `chunks`, set `self.bm25 = None`, log
  `chunk_count=0`, return before `BM25Okapi`): implemented exactly as planned, at the
  same site (`rag/retriever/keyword_search.py`, top of `index()`).
- Step 2 (leave the non-empty path unchanged): the tokenize-and-build path below the new
  guard is untouched.
- Step 3 (remove the `@pytest.mark.xfail(strict=True, ...)` marker on `test_empty_index`,
  manifest H-01): removed, no other change to that test or the surrounding file.

No new files were touched, no additional guard or branch was added beyond the one the
plan named, and the Scope's not-in-scope line (`search()`, `_tokenize()`, `rank_bm25`
itself, other retrievers) held — the diff only touches `index()` and the one test
marker.
