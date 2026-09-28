# Evidence guide: where the proof lives

The rubric names evidence; this file says where to find it. Five headings, one
per proof family. Each has the same two parts: where it lives, and what good
looks like there.

In eval mode every location below is a part of the package bundle. In live mode
the issue side comes from GitHub (the issue page, its thread, `CONTRIBUTING.md`
and any `AI_POLICY.md` it links, and the repo's `.github/` templates) and the
candidate side is the student's draft files.

## Environment

**Where it lives.** The environment lines of the repro report, usually the
first block or a line near the top. Read it against two things: the
`- bug reports:` line of the repo-facts block, which says what the project's
own template asks reporters for, and any version or platform the issue itself
names.

**What good looks like.** Three things are present: the operating system, the
version of the software under test as a release, tag, or commit, and the
version of every dependency the behavior runs through. Which dependencies count
is decided by the issue: a passlib exception needs the passlib and bcrypt
versions, a Windows-only driver bug needs the driver. A bare "latest" or
"on my machine" is not a version. A one-line environment record is complete if
it names all three.

## Steps

**Where it lives.** The numbered steps, command block, or script in the repro
report, plus every file, fixture, config, or service those steps reference.

**What good looks like.** A stranger holding only this comment can run it.
Commands appear as commands, not as prose about commands. Inputs are public,
pasted inline, or constructible from what is written. The steps include
whatever the outcome depends on, including the flag, driver, or backend when
the issue is specific to one. Brevity is fine and is not a defect: the question
is whether the reader can re-run it, never how many steps there are.

## Behavior shown

**Where it lives.** The artifact: the transcript, log excerpt, error text, exit
code, or measurement pasted into the repro report. Read it against the failure
the issue describes in its body and in the thread highlights.

**What good looks like.** The artifact is output the run produced, and it shows
the issue's own failure on the specifics the issue gives. Match the error type,
the exit code, and the symptom, not the general area. An exit 1 argument error
is not an exit 101 panic. A compile error is not an invalid-path error. Output
showing only that the program starts, or that a session exists, shows nothing
about the reported bug. Where the report says it could not reproduce, the
artifact should still show the attempt at the issue's own scenario, and the
report should name what about the environment differed.

## Honesty

**Where it lives.** The report's own conclusion sentences, its expected and
actual lines, and its claim comment, each read against the artifact directly
above or below them.

**What good looks like.** Every assertion is one the artifact supports. A root
cause is named only when the output demonstrates it. Words like verified,
confirmed, and guaranteed appear only over a run that shows the thing. Expected
and actual are stated the way round the artifact shows them. A deviation from
the issue's version, platform, or procedure is stated rather than passed over
in silence. An honest "could not reproduce, here is what I ran and what
differed" is a good outcome, not a failed one.

## Comms

**Where it lives.** The claim comment, and the contribution-policy line of the
repo-facts block, which summarizes `CONTRIBUTING.md` and any dedicated AI
policy file. Live mode reads those files on the repo directly, plus the
`.github/` issue and pull-request templates.

**What good looks like.** The claim comment names the specific behavior being
taken on, in terms that could not be pasted onto another issue, and says what
the author will produce next. It promises an artifact rather than a date: a
repro report on the way, not a fix by tomorrow. Being new is fine to say
plainly. On the policy side, read what the duty attaches to and not merely
whether a policy exists. A duty to disclose AI use "in any form", or
specifically in issues and comments, applies now. A duty that attaches to the
pull request or to the code does not apply to a comment yet. A requirement that
comments be written by a human in their own words is about voice, not
disclosure. Silence is not a rule.
