# Voice guide: how I talk upstream

<!--
THIS IS THE PART YOU WRITE (new this week). Live mode reads this file
before any comment of yours goes out the door; eval mode ignores it
entirely, because your voice is yours and carries no gold labels.

This is not etiquette. "Be polite and concise" is advice for everyone
and therefore rules for no one. Write rules YOU need, in your own
words, each one concrete enough that the skill can hold a draft
against it and say which rule it breaks.

Three sections. Fill all three.
-->

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

## Things I never post

- A promised fix or a date ("I'll have this fixed by Friday") — only that
  I'm investigating.
- A conclusion stronger than my evidence supports.
- A one-line dismissal ("can't repro", "works for me") without the
  environment and steps behind it.
