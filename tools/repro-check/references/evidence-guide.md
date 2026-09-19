# Evidence guide: where proof lives in a reproduction package

<!--
THIS IS THE PART YOU WRITE (new this week: week 1 handed you this file
finished; the scaffolding fades). The skill uses this guide as its map:
for every kind of proof a rubric check names, this file says WHERE to
find it in a package and WHAT GOOD LOOKS LIKE when you do.

Under each family heading below, write:

- Where it lives: the exact places to look. In an eval bundle (which
  section of the package: the issue context, the repo-facts block, the
  claim comment, the repro report and its parts). In live mode (where
  on GitHub or in the draft: the issue thread, the repo's docs, the
  student's draft comment).
- What good looks like: one or two sentences someone else could apply.
  Prefer observable conditions ("the versions named match what the
  issue targets, or the difference is called out") over adjectives
  ("environment is thorough").

A rubric check whose evidence this guide cannot locate is a check
nobody else can execute; your operator swap showed you what that feels
like. Write the map you wish your executor had.
-->

## Environment

Where it lives: eval bundle — the `Environment:` line at the top of the
`repro_report_markdown` / repro report section, read against the issue's
`body_markdown` (any version/OS/config it names) and the `repo_facts`
block's latest-release line. Live mode — the environment line in the
draft repro comment, read against the issue thread and the repo's
`latest release` info.

What good looks like: a named tool/app version and OS/platform, not "my
machine" or "current version." If that version differs from what the
issue's reporter used, the report says so in the open (e.g., "issue
filed against 1.2.0, I tested 1.3.1") instead of letting the reader
assume they match.

## Steps

Where it lives: the numbered steps list in the repro report, read
against the exact commands/config/flags the issue's `body_markdown`
describes as the trigger.

What good looks like: following the steps as written, starting from the
stated starting state, reaches the same code path the issue names —
same flags, same config shape, same command form. A rewritten command
that "does something similar" but uses different syntax than the issue's
trigger does not count, even if the steps are numbered and detailed.

## Behavior shown

Where it lives: the artifact block inside the repro report (command
output, log excerpt, terminal capture), read side by side with the
"Expected"/"Actual" language the issue uses to describe the bug.

What good looks like: the captured output shows the same failure — same
error message, exit code, or observable symptom — that the issue
reports, not a different failure encountered along the way. A control
run showing the *non-buggy* case (as in a working comparison config) can
strengthen this, but only alongside the actual failing capture, not
instead of it.

## Honesty

Where it lives: the "Expected"/"Actual" conclusion at the end of the
repro report, and the claim/repro comment's own wording, read against
what the `behavior-shown` artifact actually contains.

What good looks like: the stated conclusion never outruns the artifact.
"I could not trigger this after N attempts on version X" backed by an
actual attempted repro is a full pass; "this confirms the reported bug"
when the artifact shows a different or partial symptom is a fail, even
if the rest of the write-up is thorough.

## Comms

Where it lives: eval bundle — the `repo_facts` block's contribution
policy / AI-use text, read against `claim_comment_markdown` and
`repro_report_markdown`'s comment wording. Live mode — the repo's
CONTRIBUTING.md / AI-policy doc and issue template, read against the
actual drafted comment text before posting.

What good looks like: match the check to what the stated policy actually
asks for, not to "an AI policy exists."
- Disclosure-required policy (states AI use must be named, with tool and
  extent of assistance): the comment text contains that explicit
  disclosure — silence is a fail no matter how good the repro is.
- Human-voice policy (requires comments be written by a human in their
  own words, without asking for an explicit AI disclosure statement):
  the comment reads as specific and in the student's own words rather
  than generic/templated — no disclosure sentence is needed to pass.
- No AI-specific policy stated: the comment matches the repo's requested
  template/tone rather than reading as generic boilerplate.
