# Evidence guide: where evidence lives in a plan package

Each family below says where to look, first in an eval bundle and then in live
mode, and what good looks like when you get there.

## Diagnosis and grounding

**Where it lives.** Eval: the candidate plan's "Diagnosis" or opening line
(the cause it names), read against the repro-evidence block's steps, its
artifacts, its "Control" lines, and its "Expected" and "Actual" lines. The
issue body may propose a cause too; that is a claim, not evidence. Live: the
diagnosis section of `plan.md`, read against the student's own posted repro
comment on the issue (or the house repro pack quoted in the drafts).

**What good looks like.** The named cause is the thing the repro evidence
isolates, and every control run is consistent with it. A control that removes
the trigger and makes the failure go away supports the cause. A control that
exercises the component the plan blames and shows it working rules the cause
out. A step that shows the failure already present upstream of the blamed code
also rules it out. Good diagnosis can be one sentence that restates what the
evidence isolated.

## Scope

**Where it lives.** Eval: the candidate plan's "Scope", "In scope" and "Not in
scope" lines, its numbered "Approach" or "Proposed changes" list, and its
"Files" line, read against the issue's title and body. Live: the scope and
files sections of `plan.md`, read against the issue on GitHub.

**What good looks like.** Every numbered change is either the fix, a test for
the fix, or a doc line for the fix. Larger reworks appear only in the
not-in-scope line, as explicit deferrals. A drive-by rewrite looks like: a
migration to another library, a new setting or option, a restructure of a
module, a framework for retries, or fixes to a second behavior "while in the
area", listed as work the plan will do.

## Executability

**Where it lives.** Eval: the candidate plan's "Approach" steps and "Files"
line. Live: the approach and files sections of `plan.md`.

**What good looks like.** The plan names the file or code area it changes (a
function, a branch, a match site) and commits to one approach. A stranger
could open that file and start. A plan that says "somewhere", "whichever is
easier", "not sure which layer", "profile and optimize", or "investigate"
without a chosen change is not executable.

## Test plan

**Where it lives.** Eval: the candidate plan's "Test plan" line or section,
read against the repro-evidence block's numbered steps and its "Expected"
line. Live: the test plan section of `plan.md`, read against the student's
repro steps.

**What good looks like.** The test plan names something a reader can observe
that failed before and will pass after: the repro steps re-run with the
expected output stated, a named regression test with what it asserts, or a
measurement with a target. The control runs staying unchanged is a good
addition. "Run the full suite", "nothing else should break" or "should feel
fast" on their own name nothing observable about the fix.

## Honesty

**Where it lives.** Eval: the candidate plan's "Risk", "Risks", "Unknowns" or
"Open question" lines, and any claim in the plan that goes beyond the repro
evidence. Live: the risks and unknowns section of `plan.md`, and its
`## Deviations` section after the build, where any change from the posted plan
is recorded with the reason.

**What good looks like.** Things the evidence has not shown, such as an
unmeasured cost, an untested platform, or other code paths that might share
the bug, are stated as unknowns with what the author will do about them.
False confidence looks like "this will definitely fix it" or a cause asserted
past what any step shows. After a build, a deviation is recorded in the plan,
not only visible in the diff.

## Comms

**Where it lives.** Eval: the candidate plan comment, read against the thread
highlights (each line starts with a date, an author and their role in
parentheses) and against the repo-facts block's "contribution policy" line.
Live: the draft `comment.md`, read against the live issue thread (comment
authors' association badges: Owner, Member, Collaborator, Contributor) and the
repo's CONTRIBUTING file and any AI policy file (repo root, `.github/`, or
`docs/`).

**What good looks like.** When a maintainer has named a culprit, proposed or
rejected an approach, or posted a test build, the comment says how the plan
relates to that: following it, choosing one of the listed options, or saying
why it departs. An open PR on the same route is acknowledged, not raced. The
comment meets the repo's AI rule at the point that rule attaches: a
disclosure line when the policy requires disclosing AI use in comments or in
any form, nothing extra when the duty attaches only to the pull request, and
a person's own voice when the policy requires comments in the contributor's
own words. Boilerplate looks like a comment that could be posted on any issue
and never mentions what the thread already said.
