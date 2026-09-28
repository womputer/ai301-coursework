# Rubric: is this reproduction package ready to post?

Eight required checks and two preferred ones, covering the proof families the
eval set is built around: the environment is recorded, the steps are
followable, the behavior shown is the issue's own, the outcome is stated
honestly, the claim comment earns a maintainer's attention, and the repo's
stated rules about AI are respected.

Every check reads the thing itself against the issue. None of them reads the
write-up's shape: length, heading count, template sections and tone are not
evidence, in either direction. A terse complete report is ready; a polished
empty one is not.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| environment-recorded | The environment lines of the repro report, read against the repo-facts block's bug-template asks. | The report names the operating system, the version of the software under test (a release, tag, or commit), and the versions of any dependency the issue's behavior depends on. | required |
| steps-rerunnable | The steps or commands in the repro report, plus any inputs, files, or config they reference. | A stranger holding only this comment could run the same thing: the commands are given concretely, and every input they need is public, included inline, or reproducible from what is written. See note 1. | required |
| artifact-present | The repro report's body, looking for output produced by the run. | The report shows at least one artifact the run produced: a transcript, log excerpt, error text, exit code, or measurement. Described or asserted results are not artifacts. | required |
| behavior-matches-issue | The artifact read line by line against the failure the issue describes. | The artifact shows the issue's own reported behavior, matching it on the specifics the issue gives (the error, the exit code, the symptom). See note 2 for the cannot-reproduce case. | required |
| deviations-acknowledged | The report's environment and steps compared with the version, platform, and procedure the issue names. | Where the run differs from the issue on anything that could change the outcome, the report says so in words. No difference to acknowledge also passes. | required |
| outcome-matches-artifact | The report's stated conclusion compared with what its own artifact shows. | The conclusion the report states is the one its artifact supports. See note 3. | required |
| claim-is-specific | The claim comment. | The claim names the specific behavior being taken on in terms that fit this issue and no other, and says what the author will produce next. See note 4. | required |
| ai-disclosure-where-required | The repo-facts block's contribution-policy line, and both comments. | The package satisfies the repo's stated AI rules for comments posted on an issue. See note 5. | required |
| control-run | The repro report, looking for a second run under changed conditions. | The report includes a contrasting run showing the behavior is absent when the triggering condition is removed. | preferred |
| next-step-named | The closing lines of the repro report or claim comment. | The author names a concrete next action, such as the fix they will attempt or the question they need answered. | preferred |

## Verdict rule

Accept if and only if every required check grades `pass`; a `fail` or an
`unclear` on any required check holds the package, and preferred checks never
change the verdict.

Grade a required check `unclear` only when the evidence it names is absent, and
treat that as a fail: proof you cannot verify is proof that is not ready to
post. Use the preferred grades to rank packages that were accepted, never to
decide them.

## Applying the checks

**Note 1, what makes steps rerunnable.** Fail when the run depends on something
the reader cannot obtain: a private repository, an unshared config file, an
internal service, or a local path with no stated contents. Fail also when a
step the outcome depends on is left out, such as the driver or backend on an
issue whose behavior is specific to one. Do not fail a report for being brief:
three commands that work are rerunnable, and a long narrative that never gives
a command is not.

**Note 2, the cannot-reproduce case.** A report that says it could not
reproduce the issue passes `behavior-matches-issue` when its artifact shows a
real attempt at the issue's own scenario and the report names what differed
about its environment or setup. An evidenced cannot-reproduce is a useful
result and is ready to post. What fails is an attempt at a different scenario,
whether or not it reproduced something.

**Note 3, honesty is a comparison, not a tone.** Fail when the report asserts
more than the artifact shows: naming a root cause nothing in the output
demonstrates, calling a result guaranteed or verified with no run behind it, or
describing the artifact as confirming the issue when it shows a different
failure or only shows that the software starts. Fail also when the stated
expected and actual are the wrong way round relative to the artifact.
The gap between the claim and the artifact is the failure, not the confidence
itself.

**Note 4, what makes a claim specific.** Fail a claim comment that would read
identically on any other issue: an assignment request, a plus-one, or a
statement of interest with no behavior named. Fail a promised delivery date or
a guaranteed outcome, because the author cannot hold either. A claim comment
may be short; it has to be about this issue.

**Note 5, reading the repo's AI rules.** Treat the package as AI-assisted work.
Then read what the policy's duty attaches to:

- If the policy requires disclosing AI use in comments, issues, or
  contributions of any form, and neither comment discloses it, fail.
- If the policy's disclosure duty attaches only to the pull request or the
  code, comments that do not disclose still pass; that duty is not due yet.
- If the policy requires that comments to maintainers be written by a human in
  their own words, pass when the comments read as a person's own writing and
  fail when they read as unedited generated text.
- Policies that require understanding, testing, and taking responsibility for
  the work, with no disclosure duty stated, pass without a disclosure line.
- Silence passes. Most repositories state nothing, and that is not a rule.
