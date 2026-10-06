# Procedure: how this skill grades a plan package

Four stages, run in order. Do not grade any check until the Read order and
Evidence gathering stages are finished and their notes are written down.

## Read order

1. Read the repo-facts block first (live: the repo's CONTRIBUTING file and any
   AI policy file, per `references/evidence-guide.md`). Note, in one line
   each: the contribution-policy line as written, and whether it states any AI
   rule. If it states one, note exactly what the rule attaches to: comments,
   issues, the pull request, code, or "any form".
2. Read the issue next. Note the reported behavior in one sentence: what
   happens, and what should happen instead. Note any cause the issue itself
   proposes, marked as "issue's claim, not yet evidence".
3. Read the thread highlights (live: the full issue thread). For each comment,
   note the author's role (OWNER, MEMBER, COLLABORATOR, CONTRIBUTOR, or a
   reporter or other user). For every maintainer-role comment, write down any
   direction it gives: a culprit named, an approach proposed or rejected, a
   patch or test build posted, a PR pointed at. If there are none, write "no
   maintainer direction".
4. Read the repro-evidence block before the plan. Note: the steps, the
   artifact each step produced, the expected and actual lines, and every
   control run with what it changed and what it showed. Write down what the
   evidence isolates as the trigger or cause, and what it rules out.
5. Only now read the candidate plan. Note its stated cause, its in-scope and
   not-in-scope lines, every numbered change it commits to, its file list, its
   test plan, and its risks or unknowns.
6. Read the candidate plan comment last. Note the approach it announces, any
   maintainer direction or PR it mentions, any AI disclosure line, and any
   date or guarantee it promises.

The order matters: the evidence and the thread must be fixed in your notes
before you see the plan, so the plan's own confident framing cannot set what
counts as the cause or the direction.

## Evidence gathering

For each check, pull these facts from the notes above. If a fact is missing
from the notes, go back to the named package part and read it again before
grading.

1. diagnosis-grounded: the plan's stated cause (step 5) next to the list of
   what the repro evidence isolates and rules out (step 4). Record every
   control run whose result touches the component the plan blames.
2. scope-bounded: the issue's reported behavior (step 2) next to the plan's
   numbered changes and file list (step 5). Record each change the plan
   commits to building, and mark it "needed for the fix", "test or docs for
   the fix", "explicitly deferred", or "extra".
3. executable: the plan's file list and approach (step 5). Record the files or
   code areas named, and quote any wording that leaves a decision open
   ("somewhere", "whichever", "not sure", "investigate", "maybe").
4. test-plan-decisive: the plan's test plan (step 5) next to the repro's steps
   and expected line (step 4). Record each test named and the observable
   outcome it claims.
5. thread-direction-engaged: the maintainer-direction list (step 3) next to the
   plan comment's announced approach and mentions (step 6) and the plan's
   approach (step 5).
6. ai-policy-satisfied: the AI rule and its attachment point (step 1) next to
   the plan comment's disclosure line or its absence (step 6).
7. unknowns-stated: the plan's risks and unknowns (step 5) next to anything
   the plan asserts that no repro step shows (step 4).
8. comment-matches-plan: the comment's announced approach and any date or
   guarantee (step 6) next to the plan's approach and scope (step 5).

In eval mode, all of this comes from the bundle only. Never fetch anything.

## Check execution

1. Run the checks in the rubric table's order, top to bottom: the six required
   checks, then the two preferred ones.
2. For each check, apply its pass condition and its note in `rubric.md` to the
   facts recorded for it in Evidence gathering. Grade `pass` or `fail` and
   write one line of evidence: the quote or fact that decided it.
3. Grade `unclear` only when the evidence the check names is absent from the
   package, for example no test plan, no plan comment, or no repro evidence.
   Say which part was absent. Do not grade `unclear` because a judgment is
   hard; make the call from the rubric's note.
4. Grade each check on its own facts. A failure on one check does not change
   the grade of another. Grade every check even after a required check fails,
   so the output shows the author everything that needs to change.
5. A check may be graded from the notes without re-reading the package only
   when the notes already hold the quote that decides it. Otherwise re-read
   the named part.
6. When a check passes by the rubric's stated condition but feels wrong, it
   still passes. Note the tension in the summary.

## Verdict assembly

1. List the six required checks with their grades.
2. If every required check is `pass`, the verdict is `accept`.
3. If any required check is `fail` or `unclear`, the verdict is `reject`.
   `unclear` counts as `fail`.
4. Preferred checks never change the verdict. Report them after the required
   checks.
5. In the summary before the JSON block, name the deciding check or checks for
   a `reject`, with the quote that decided each. For an `accept`, say that all
   required checks passed and list any preferred check that failed.
6. In live mode only, after the checks, compare the draft comment against each
   rule in `voice-guide.md` and list any broken rule by name, quoting it. This
   never changes the verdict.
7. End with the JSON block the skill specifies, with every check in rubric
   order, and nothing after it.
