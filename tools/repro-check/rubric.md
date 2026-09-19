# Rubric: is this reproduction package ready to post?

<!--
THIS IS THE PART YOU WRITE. The skill in SKILL.md executes whatever
checks you define here. It ships empty on purpose: the judgment is your
work.

A filled rubric must contain:

1. At least one row in the checks table. Each row needs all four
   columns:
   - Check: a short name (used in the output JSON).
   - Evidence: exactly what to look at, and where in the package. Name
     the part (the claim comment, the repro report's environment
     record, the artifacts read against the issue's description, the
     repo-facts block) or a location from your
     references/evidence-guide.md. "The report" is not a source; "the
     output excerpt read against the error the issue describes" is.
   - Pass condition: a decision rule about the OUTCOME that someone
     else could apply and get your answer. Judge the thing itself (does
     the artifact show the issue's behavior?), never the write-up's
     shape (how many steps it has, how long it is, whether it uses a
     template's headings). Structure-shaped checks are what make
     graders disagree with themselves.
   - Weight: `required` (a fail here holds the package) or `preferred`
     (never changes the verdict).

2. A verdict rule below the table: how the check grades combine into
   accept (ready) or reject (hold), including how `unclear` is
   treated. The verdict space is binary. If you write no rule for
   `unclear`, the skill treats it as fail.

Cover what actually gets bad packages posted. The lecture named the
proof families: the environment is recorded, the steps are complete
and followable, the behavior shown matches the issue (not an adjacent
one), the outcome is stated honestly (an evidenced cannot-reproduce is
a pass, a confident wrong-target is not), and the words respect the
repo's conventions. A rubric that ignores a family will fail eval
packages designed around that family.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| environment | The repro report's environment line (tool/app version, OS, relevant runtime flags), read against the versions the issue targets (from the issue body and the repo-facts block's latest-release line). See [environment](references/evidence-guide.md#environment). | Pass if the report names a concrete version and platform, and any divergence from the issue's target version is called out explicitly rather than silently substituted. Fail if no version/platform is named, or a divergence is present but unstated. | required |
| steps | The repro report's numbered steps and starting state, read against the exact trigger the issue describes (commands, config, flags). See [steps](references/evidence-guide.md#steps). | Pass if the steps as written reach the same code path / trigger condition the issue names, so a stranger following them lands on the same behavior. Fail if a step substitutes different syntax, flags, or config that never exercises the reported trigger, even if something breaks. | required |
| behavior-shown | The artifact (command output, log excerpt, screenshot) in the repro report, read against the issue's stated expected vs. actual behavior. See [behavior shown](references/evidence-guide.md#behavior-shown). | Pass if the captured artifact shows the same failure mode the issue reports (same error/exit code/symptom class). Fail if the artifact shows a different, adjacent failure, or no artifact is present at all. | required |
| honesty | The repro report's stated conclusion and the claim/repro comment's wording, read against what the artifact in `behavior-shown` actually demonstrates. See [honesty](references/evidence-guide.md#honesty). | Pass if the comment claims exactly what the evidence supports — including an evidenced "could not reproduce." Fail if the wording asserts more certainty or a stronger match than the artifact backs up. | required |
| conventions-disclosure | The repo-facts block's contribution/AI-use policy, read against the claim comment and repro comment text. See [comms](references/evidence-guide.md#comms). | Pass if the comments meet whatever the stated policy actually asks for: an explicit disclosure (naming the tool and extent of assistance) when the policy requires disclosure; a comment that reads as human-authored in its own words when the policy instead requires human-voiced comments with no explicit disclosure demanded; or simply the repo's stated template/conventions when no AI-specific policy is stated. Fail if the policy's actual requirement — disclosure, or human voice — is not met, regardless of how good the rest of the package is. | required |

## Verdict rule

Accept only if every required check passes. There are no preferred checks
in this rubric — all five gate the verdict, because each maps to a
category the eval set is designed to catch (a pass in the other four
never buys back a fail in `conventions-disclosure`, which is exactly what
makes the disclosure-wall package rejectable regardless of repro quality).
`unclear` on any required check counts as a fail on that check, not a
free pass — if the evidence guide's location doesn't let you tell either
way, the check has failed to demonstrate what it needs to.
