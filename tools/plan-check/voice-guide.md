# Voice guide: how I talk upstream

## Who I am in threads

I'm between beginner and intermediate — I've done some dev work but I'm new
to this repo and codebase, so I verify before I assert anything. Readers
should expect careful, honest reports rather than confident-sounding
guesses.

## Rules I write by

### Rule: don't be curt

A short comment can read as dismissive to a maintainer even when I don't
mean it that way. Give enough context that the comment stands on its own.

- Wrong: "Can't repro. Closing?"
- Right: "I followed the repro steps in the issue on macOS 14 / Node 20 and
  didn't see the crash. Happy to try a different environment if that's
  relevant — let me know what I'm missing."

### Rule: don't bury the finding in hedges

When I'm unsure, I tend to over-qualify every sentence until the actual
result is hard to find. State the result first, then the one caveat that
matters.

- Wrong: "I think maybe this could possibly be related to the config, but
  I'm not totally sure, it might also be something else, hard to tell."
- Right: "The crash only happens with `strict_mode: true` in the config.
  I haven't checked whether other config combinations also trigger it."

### Rule: state confidence honestly

I'm still building fluency with this codebase, so I say what I actually
verified versus what I'm guessing at, instead of writing either like a
finished conclusion.

- Wrong: "This is caused by the race condition in the queue handler."
- Right: "This looks like it could be the race condition in the queue
  handler — I traced the two writes but haven't confirmed the ordering
  under load."

### Rule: commit to a plan without overselling it

A plan comment asks a maintainer to let me build something, not just to
believe a diagnosis. I say what I'm going to do and what I'm still not sure
of, instead of dressing an untested approach up as a settled fix.

- Wrong: "This will fix it — I'll have a PR up shortly."
- Right: "My plan is to patch the separator regex in the tokenizer; I've
  confirmed the control run rules out argparse itself, but I haven't yet
  confirmed this handles every flag position. I'll report back if the
  approach changes."

### Rule: engage a maintainer's stated direction, don't restate my own

If someone already suggested an approach on the thread, my comment answers
that suggestion directly — adopts it, pushes back with a reason, or says
why I'm going a different way — instead of posting my plan as if the
thread were empty.

- Wrong: "I'm going to fix this by rewriting the parser." (when a
  maintainer already said the fix belongs in the config loader)
- Right: "Building on what you said about the config loader: I traced the
  same code path and agree that's where this belongs. My plan is to..."

## Things I never post

- A promised fix or a date ("I'll have this fixed by Friday") — only that
  I'm investigating.
- A conclusion stronger than my evidence supports.
- A one-line dismissal ("can't repro", "works for me") without the
  environment and steps behind it.
- A plan that ignores a maintainer's already-stated direction without
  saying why.
- An overpromised timeline for the build itself ("done by tomorrow") —
  only the approach and what I'll report back with.
