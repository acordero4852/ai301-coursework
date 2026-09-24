# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`. This file is graded at the path above; a copy
kept anywhere else in the repository is not read.

Complete every labelled field below. Each is graded on its own; content placed under the wrong
label is not graded.

---

## Your identity upstream

acordero4852

[Your GitHub username, exactly as it appears on your profile — no `@`, no profile URL. Your
comments upstream are identified by this name.]

---

## Posted upstream

**Claim comment**

[https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68](https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68)

[Link to the comment where you claimed the issue. Use the comment's own permalink, not the
issue page on its own. **Then paste the text of that comment underneath the link** — the
pasted text is what this field is graded on, so copy across what you actually posted.]

**Reproduction comment**

[https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68#issuecomment-5805081660](https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68#issuecomment-5805081660)

Reproduced this. Environment: Python 3.14.7, `rank-bm25` 0.2.2, `structlog` 26.1.0 (matches the repo's `rank-bm25>=0.2.2` pin), macOS.

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

`index()` always builds a `BM25Okapi` from the tokenized corpus, even when
`chunks` is empty. With `chunks == []`, `rank_bm25`'s `BM25._initialize`
divides `num_doc` by `corpus_size`, and `corpus_size` is 0.

Two control runs rule out the rest of the class:

```pycon
>>> KeywordSearcher().search("python", top_k=10)   # index() never called
[]                                                   # search()'s own guard handles this fine
>>> s2 = KeywordSearcher()
>>> s2.index([{"id": 1, "text": "python programming"}])
>>> s2.search("python", top_k=10)
[{'id': 1, 'text': 'python programming', 'bm25_score': -0.27...}]  # non-empty index works as expected
```

So it's isolated to `index()` not guarding the empty-corpus case the way
`search()` already does — matches the existing `test_empty_index` xfail
(manifest H-01). I'll post my plan next.

## Eval iterations

Answer all four sections. Quote source text directly; paraphrase does not satisfy these
fields.

**Run history**

1. Smoke run, `--limit 3`: 2/3 agree. `pkg-03` disagreed (gold accept, rubric reject),
   failing `conventions-disclosure`.
2. Reread `pkg-03` against its issue: ripgrep's stated policy requires comments be
   written by a human in their own words, it does not require an explicit AI-disclosure
   statement. The check as written treated any stated AI policy as a disclosure
   requirement. Revised the `conventions-disclosure` pass condition and the evidence
   guide's Comms section to distinguish "disclosure-required" policies from "human-voice"
   policies. Re-ran the disagreeing package plus the one-package `disclosure` canary,
   `--only pkg-03,pkg-20`: 2/2 agree.
3. Full run (no `--limit`/`--only`, no `--save-run`): 18/20 agree, category floor met
   (`disclosure 1/1`). Two new disagreements surfaced: `pkg-09` and `pkg-10`, both gold
   accept, both failing `behavior-shown`.

4. Confirming full run, `--save-run eval-run.txt` (2026-09-19T01:44:17Z): 18/20 agree,
   category floor met (`disclosure 1/1`). `pkg-09` and `pkg-10` remained the only
   disagreements, both failing `behavior-shown`; the bar was already met so these were
   left as a known gap rather than revised further.

**Package analysis**

`pkg-03` (BurntSushi/ripgrep#2779) — gold: accept. My rubric's first draft said reject,
failing `conventions-disclosure`, because the check only recognized one shape of AI
policy: "must explicitly disclose use of AI, naming the tool and extent." ripgrep's
CONTRIBUTING.md instead asks that comments be "written by humans in their own words" —
a human-voice requirement, not a disclosure requirement — and the candidate comment met
that bar without needing a disclosure sentence. After revising the check to treat these
as two distinct policy shapes, my rubric now reads it correctly as accept, matching gold.

**Check rationale**

> | conventions-disclosure | The repo-facts block's contribution/AI-use policy, read
> against the claim comment and repro comment text. See
> [comms](references/evidence-guide.md#comms). | Pass if the comments meet whatever the
> stated policy actually asks for: an explicit disclosure (naming the tool and extent of
> assistance) when the policy requires disclosure; a comment that reads as
> human-authored in its own words when the policy instead requires human-voiced comments
> with no explicit disclosure demanded; or simply the repo's stated template/conventions
> when no AI-specific policy is stated. Fail if the policy's actual requirement —
> disclosure, or human voice — is not met, regardless of how good the rest of the
> package is. | required |

It reads this way because the first draft collapsed "an AI policy exists" into "AI use
must be disclosed," which is what failed `pkg-03`. I rejected leaving it as a single
disclosure test once `pkg-03` showed a repo whose policy is satisfied by voice alone;
the check now branches on which requirement the repo-facts block actually states.

**Trade-offs**

Loosening `conventions-disclosure` to accept a human-voiced comment with no explicit
disclosure statement risks missing a repo that requires *both* human voice and
disclosure, or a human-voice-only repo where the comment is voiced well but the work
behind it wasn't actually reviewed by a human — the check can only read the text, not
verify what produced it. To guard the one case this course set is built to catch, I
re-ran the `disclosure` category's single package, `pkg-20`, as a canary after the
change (`--only pkg-03,pkg-20`): it still correctly rejects, so the loosening didn't
erase the disclosure-required path.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
