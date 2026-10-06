# Rubric: is this plan ready to post and build from?

Six required checks and two preferred ones. Each covers one way a bad plan gets
posted: the diagnosis contradicts the reproduced evidence, the change is
unbounded, a stranger could not start building it, the test plan proves
nothing observable, the comment ignores what the thread already settled, or
the comment breaks the repo's stated AI rules.

Every check reads the thing itself against the issue and its repro evidence.
None of them reads the write-up's shape. Length, headings, confidence and
polish are not evidence in either direction. A terse plan that names its cause,
its file, its bounds and its observable test is ready. A long confident one
can still be wrong or unbuildable.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diagnosis-grounded | The plan's stated cause, read against every step and every control run in the repro-evidence block. | The cause the plan names is consistent with everything the repro evidence shows, and no control run or step in that evidence rules it out. See note 1. | required |
| scope-bounded | The plan's in-scope and not-in-scope statements, its numbered changes, and its file list, read against what the issue asks for. | The plan is one bounded change that fixes the reported behavior. Every item it commits to building is needed for that fix, its tests, or its docs. See note 2. | required |
| executable | The plan's approach and file list. | A stranger could start building without asking the author anything: the plan names the files or code areas it changes and commits to one approach. See note 3. | required |
| test-plan-decisive | The plan's test plan, read against the repro evidence's steps and its stated expected result. | The test plan names at least one observable outcome that shows the reported behavior is fixed, such as a re-run of the repro steps with the expected output, a named regression test with its assertion, or a measured value. See note 4. | required |
| thread-direction-engaged | The thread highlights (or the live thread) for maintainer direction, read against the plan comment and the plan. | Where a maintainer or project collaborator has given direction on the fix in the thread, the comment and plan engage it: they follow it, build on it, or say why they depart from it. No maintainer direction in the thread also passes. See note 5. | required |
| ai-policy-satisfied | The repo-facts block's contribution-policy line (live: the repo's CONTRIBUTING and any AI policy file), and the plan comment. | The plan comment satisfies the repo's stated AI rules for comments posted on an issue. See note 6. | required |
| unknowns-stated | The plan's risks or unknowns, read against what the repro evidence has not yet shown. | Where the plan relies on something the evidence has not demonstrated, it says so as an unknown or a risk instead of asserting it as fact. | preferred |
| comment-matches-plan | The plan comment read against the plan. | The comment describes the same approach and bounds as the plan, and promises no delivery date or guaranteed outcome. | preferred |

## Verdict rule

Accept if and only if every required check grades `pass`. A `fail` or an
`unclear` on any required check holds the package (`reject`). Preferred checks
never change the verdict; they are reported so the author can improve an
accepted plan.

Grade a required check `unclear` only when the evidence the check names is
absent from the package, for example a plan with no test plan at all. Treat
`unclear` as `fail`: a plan whose readiness cannot be verified from the package
is not ready to post and build from.

## Applying the checks

**Note 1, grounding is a comparison with the controls.** Read each control run
and ask what it held constant and what it changed. If the plan blames a
component that a control run exercised and showed working, the diagnosis is
ruled out and the check fails, however detailed the plan is. Fail also when a
repro step shows the failure already present before the point the plan blames,
for example data already lost before the code the plan wants to change runs.
Fail when the plan adopts a cause from the issue or thread that the package's
own measurements contradict. A plan that restates the cause the repro evidence
isolates passes, even in one sentence.

**Note 2, what bounded means.** Fail when the plan bundles the fix with work
the issue did not ask for: a library or framework migration, a rewrite or
redesign of the surrounding component, a new user-facing option or setting, a
module restructure, a retry framework, or "while I am in the area" fixes to
other behavior. It fails even when the core fix inside it is correct, because
the bundle is what gets built. Adding regression tests for the fix is in scope.
Deferring a larger rework explicitly, with a reason, passes: an honest
scoped-down plan is bounded.

**Note 3, what a stranger needs.** Fail when the plan defers its real decisions
to build time: no files or code areas named, a change placed "somewhere", a
choice left as "X or Y, whichever is easier", layers listed with "not sure",
or an approach of "investigate", "profile", or "poke around" with nothing
chosen. Naming one file and one change is enough to pass.

**Note 4, decisive means observable.** Fail a test plan that is only "run the
full test suite", "make sure nothing else breaks", "should feel fast", or any
outcome a reader could not observe and check. The suite may be part of a
decisive test plan; it cannot be all of it. The test plan must include at least
one check that would have failed before the fix and passes after it, stated as
something observable.

**Note 5, reading maintainer direction.** Direction means a maintainer, owner,
member, or collaborator in the thread doing one of these: naming the culprit
code, proposing or preferring an approach, rejecting an approach, posting a
patch or test build and asking for testing, or pointing at an existing PR.
Fail when the comment proposes something else, or a different kind of change
(for example docs instead of the code fix the maintainer isolated), without
mentioning that direction. Picking one of several options a maintainer listed
passes. Reporters' and other contributors' comments are not maintainer
direction, but an open PR taking the same route should be acknowledged rather
than raced; a comment that ignores one is a preferred-level note under
`comment-matches-plan`, not a fail here.

**Note 6, reading the repo's AI rules.** Treat the package as AI-assisted work.
Then read what the policy's duty attaches to:

- If the policy requires disclosing AI use in comments, issues, or
  contributions of any form, and the plan comment does not disclose it, fail.
- If the policy's disclosure duty attaches only to the pull request or the
  code, a comment that does not disclose still passes; that duty is not due
  yet.
- If the policy requires that comments to maintainers be written by a human in
  their own words, pass when the comment reads as a person's own writing and
  fail only when it reads as unedited generated text.
- Policies that require understanding, testing, and taking responsibility for
  the work, with no disclosure duty stated, pass without a disclosure line.
- Silence passes. Most repositories state nothing, and that is not a rule.
