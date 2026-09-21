# Unit 1 - Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72

**Verdict output**

Live-mode run of the installed skill on three candidates. All three are accepted.
The selected issue, #72, carries `"verdict": "accept"` in the JSON block below.
The skill ranked #69 first on fit; why I took #72 anyway is in the selection
rationale.

```
## Evidence gathered

**Repo:** `codepath/pathreview-ai301-fa26-s1` — archived: `false`, pushed_at `2026-09-16` (today: `2026-09-21`), 0 releases, no CONTRIBUTING/AI-policy ban (root `CONTRIBUTING.md` 404s; `docs/CONTRIBUTING.md` exists, describes AI-assisted, Conventional-Commits workflow; repo's own commits carry `Co-Authored-By: Claude Opus 5`).

**Last 5 default-branch commits:** all by human `Aburke225` (Andrew Burke), 2026-08-24 → 2026-09-16, all within 90 days.

**Maintainer-responsiveness sample:** issues #52 and #43 each got their first (and only) reply from `Aburke225` (COLLABORATOR) ~6 days after opening — under 7 days.

## Per-check grades

Repo-level (shared by all three candidates): `not-archived` PASS, `maintainer-active` PASS, `repo-in-use` PASS (no release, but push 5 days ago), `ai-contributions-allowed` PASS. Preferred: `release-recency` FAIL, `maintainer-responsive` PASS.

**#73** — README/`.env.example` key-name mismatch: `bounded-single-change` PASS (one sync of two files), `direction-settled` PASS (bug report, collaborator-filed), `unclaimed` PASS (no assignee, 0 comments), `newcomer-signposted` PASS (`good first issue`). → **accept**

**#72** — `verify_password` should fail closed, not raise: `bounded-single-change` PASS (one fix + drop one xfail marker), `direction-settled` PASS, `unclaimed` PASS (no assignee, 0 comments; a merged PR in an unrelated student's fork name-drops this issue number, but that's not a claim or linked PR on this repo's issue), `newcomer-signposted` PASS. → **accept**

**#69** — output parser crashes on a top-level JSON array: `bounded-single-change` PASS (one fix + drop one xfail marker), `direction-settled` PASS, `unclaimed` PASS **via the Path Review house rule** — `jacho15` (author_association `NONE`, a classmate) posted "I'd like to attempt this!" 2 days ago, which would normally fail the check, but scope.md says classmates' claim comments don't block an issue here, `newcomer-signposted` PASS. → **accept**

## Ranked read-out

1. **#69** — best fit: it's a literal "small parsing fix" (output-parser fallback), ships with an existing xfail test to flip, and is a compact, unfamiliar module to read fast.
2. **#72** — equally small and test-backed, a fail-closed correctness fix in `core/security.py`; ranks just behind #69 only because parsing work is named more directly in the fit profile than auth edge cases.
3. **#73** — a real documentation fix (matches the profile), but it's a plain two-file text sync with no test attached, so it teaches less of what the profile says is wanted ("changes that come with a test").

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/69",
    "checks": [
      {"name": "not-archived", "grade": "pass", "evidence": "repo field archived: false"},
      {"name": "maintainer-active", "grade": "pass", "evidence": "5/5 last commits within 90 days, all authored by human Aburke225"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "last push 2026-09-16, 5 days before today 2026-09-21"},
      {"name": "bounded-single-change", "grade": "pass", "evidence": "one fix: 'fallback path should handle array responses' plus dropping one xfail marker"},
      {"name": "direction-settled", "grade": "pass", "evidence": "bug report opened by COLLABORATOR Aburke225; bug reports pass without endorsement"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: []; jacho15's 'I'd like to attempt this!' comment (2026-09-19) is a classmate claim, which scope.md's house rule says does not block the issue"},
      {"name": "ai-contributions-allowed", "grade": "pass", "evidence": "docs/CONTRIBUTING.md states no AI ban; repo commits themselves are Claude-co-authored"},
      {"name": "release-recency", "grade": "fail", "evidence": "GET /releases returns []"},
      {"name": "maintainer-responsive", "grade": "pass", "evidence": "issues #52 and #43 each drew Aburke225 (COLLABORATOR) first reply within ~6 days"},
      {"name": "newcomer-signposted", "grade": "pass", "evidence": "labels include good first issue"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72",
    "checks": [
      {"name": "not-archived", "grade": "pass", "evidence": "repo field archived: false"},
      {"name": "maintainer-active", "grade": "pass", "evidence": "5/5 last commits within 90 days, all authored by human Aburke225"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "last push 2026-09-16, 5 days before today 2026-09-21"},
      {"name": "bounded-single-change", "grade": "pass", "evidence": "one fix: fail-closed behavior in verify_password plus dropping one xfail marker"},
      {"name": "direction-settled", "grade": "pass", "evidence": "bug report opened by COLLABORATOR Aburke225; bug reports pass without endorsement"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: []; comments: 0; the only cross-reference is a merged PR in an unrelated student fork (foojanbabaeeian/ai301-coursework-Fozhan) that names this issue number, not a claim or linked PR on this repo"},
      {"name": "ai-contributions-allowed", "grade": "pass", "evidence": "docs/CONTRIBUTING.md states no AI ban; repo commits themselves are Claude-co-authored"},
      {"name": "release-recency", "grade": "fail", "evidence": "GET /releases returns []"},
      {"name": "maintainer-responsive", "grade": "pass", "evidence": "issues #52 and #43 each drew Aburke225 (COLLABORATOR) first reply within ~6 days"},
      {"name": "newcomer-signposted", "grade": "pass", "evidence": "labels include good first issue"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s1/issues/73",
    "checks": [
      {"name": "not-archived", "grade": "pass", "evidence": "repo field archived: false"},
      {"name": "maintainer-active", "grade": "pass", "evidence": "5/5 last commits within 90 days, all authored by human Aburke225"},
      {"name": "repo-in-use", "grade": "pass", "evidence": "last push 2026-09-16, 5 days before today 2026-09-21"},
      {"name": "bounded-single-change", "grade": "pass", "evidence": "one outcome: make README.md and .env.example agree on the LLM API key name"},
      {"name": "direction-settled", "grade": "pass", "evidence": "docs/bug report opened by COLLABORATOR Aburke225; no design debate in thread (0 comments)"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: []; comments: 0; no linked PRs"},
      {"name": "ai-contributions-allowed", "grade": "pass", "evidence": "docs/CONTRIBUTING.md states no AI ban; repo commits themselves are Claude-co-authored"},
      {"name": "release-recency", "grade": "fail", "evidence": "GET /releases returns []"},
      {"name": "maintainer-responsive", "grade": "pass", "evidence": "issues #52 and #43 each drew Aburke225 (COLLABORATOR) first reply within ~6 days"},
      {"name": "newcomer-signposted", "grade": "pass", "evidence": "labels include good first issue"}
    ],
    "verdict": "accept"
  }
]
```
```

---

## Eval iterations

**Run history**

Eight runs, in order. Only the three full runs are scored; the five `--only` runs
were cheap canaries used to test a wording change before paying for a full run.

1. `--only issue-04,issue-09,issue-12,issue-20` (partial): 3/4. issue-04 wrongly rejected.
2. `--only issue-04,issue-05,issue-06,issue-10` (partial): 4/4, after the first fix.
3. Full run: **18/20 scored items, bar 18/20: PASS.** clear-accept 6/8. Misses: issue-01 and issue-19, both on `bounded-single-change`.
4. `--only issue-01,issue-19,issue-05,issue-10,issue-04` (partial): 4/5. issue-19 flipped, issue-05 and issue-10 held.
5. Full run: **20/20 scored items, bar 18/20: PASS.**
6. `--only issue-01,issue-02,issue-05,issue-06,issue-07,issue-10,issue-19` (partial): 6/7, after restructuring the rubric and adding `repo-in-use`. issue-01 rejected.
7. `--only issue-01,issue-10,issue-14` (partial): 3/3, after adding the documentation clause to note 1.
8. Full run: **20/20 scored items, bar 18/20: PASS.** Categories: claimed 4/4, clear-accept 8/8, dead-repo 3/3, policy 1/1, scope 4/4. This is the run saved in `eval-run.txt`.

**Issue analysis**

`issue-19` (zxcalc/zxlive, "Selecting large subgraphs in proof mode freezes the UI").
Gold label: `accept`. My rubric's final decision: `accept`. It rejected the issue on
run 3 and has accepted it since run 4, and why it first read the issue wrong is the
useful part.

The body opens "There are two potential causes which should be fixed:" followed by a
numbered pair, then "Additional suggestions:" followed by three more numbered items.
My `bounded-single-change` check failed it, because the check was written to fail "a
checklist of sub-items that are independent pieces of work". The check was counting
bullets. Five numbered items looked like five pieces of work.

They are not five pieces of work. They are one reported symptom, the UI freezing, with
two diagnosed causes and three optional optimisations attached to it. The fix is one
pull request against one symptom. So I rewrote the check to count outcomes rather than
list items. issue-01 was failing the same way, a documentation page whose body is a
seven-bullet outline of that one page's sections, and it needed a second pass: outcome
counting alone left it flipping between runs, so note 1 now says in as many words that
a page outline is one outcome however many sections it lists.

**Check rationale**

`repo-in-use`, quoted as it currently stands in `tools/issue-select/rubric.md`:

> | repo-in-use | The "latest release" and "last push to any branch" fields. | A release published within 365 days, or a push to any branch within 90 days. | required |

I added this check because the lecture names "repo in use" as its own family, defined
as "Releases ship; people depend on it", and my rubric did not have a required check
that tested it. Family 2 was resting on `not-archived`, which only catches issue-17.
issue-02 and issue-07 were actually being rejected by `maintainer-active`, a family 1
check. The verdicts were right and the reasoning was coming from the wrong place.

The wording is an `or` because a release threshold alone gets two cases backwards.
issue-06 is a gold accept with `latest release: none published`, so any rubric that
requires a release rejects a good issue. And sympy and tldr-pages ship rarely, 465 and
516 days since their last release at capture, while both push daily; requiring a recent
release would mark two of the most active repositories in the set as unused. Pairing
the two fields fixes both: a project either ships releases or visibly moves, and a dead
repository does neither. issue-02 (last push 2024-06-19, release 2023-12-09) and
issue-07 (last push 2025-02-27, no releases) fail both halves.

**Trade-offs**

`repo-in-use` gives up precision on the push half. "A push to any branch within 90
days" is a weak signal: a dependabot commit on a side branch satisfies it, and so does
a fork's stale branch getting a single update. It measures whether the wire is live,
not whether anyone ships or depends on the software, which is what the family is
actually about. I accept that because the two checks divide the work: `maintainer-active`
already requires 2 human commits on the default branch in 90 days, so the human-activity
question is answered there, and `repo-in-use` only has to catch the case where a repo
has gone quiet in every sense. A repo that pushes constantly, never releases and has no
users would pass my rubric, and nothing in the eval set tests that.

Nothing else moved, and here is how I know. Run 6 re-graded the seven issues this change
could plausibly break: issue-06 (the accept with no releases), issue-05 and issue-10 (the
two active repos with stale releases), issue-02 and issue-07 (the two dead repos that
must still fail), and issue-01 and issue-19 (the scope cases from the previous fix). Six
of the seven held. The one that did not, issue-01, was failing on `bounded-single-change`,
not on `repo-in-use`, and run 7 confirmed the documentation clause fixed it without
touching issue-10 or issue-14.

---

## Selection rationale

**Selection rationale**

*1. Fit to my interests and to the time available.*

#72 is a Python fix in `core/security.py`: `verify_password` lets passlib's
`UnknownHashError` escape when the stored hash is malformed, and it should fail closed
by returning `False`. Python is my strongest language and this is the kind of bug I
already work on, a function handling bad input the wrong way. The issue names both
files, `core/security.py` and `tests/unit/test_security.py`, and estimates 1 to 2
hours. There is an existing test marked `@pytest.mark.xfail` against manifest id H-05,
so the fix comes with its own proof: remove the marker and the test has to pass. That
is the habit I want to build, and it is the reason I took #72 over #73, which is a
README and `.env.example` mismatch with no test to write.

*2. What the verdict identified correctly, and what I weighed that the rubric could not.*

The rubric got the mechanical facts right on all three candidates. The repo is alive
(last commit 2026-09-16 by a human), #72 has no assignee and no linked pull requests,
it carries a maintainer-applied `good first issue` label, and `docs/CONTRIBUTING.md`
says nothing about AI use, so `ai-contributions-allowed` passes on silence. The skill
also correctly refused to treat the classmate's claim comment on #69 as a blocker,
which is the Path Review house rule in `scope.md` doing its job rather than the rubric.

All three passed every required check identically, so the checks could not separate
them. The ranking came from the fit profile, and it put #69 first, ahead of #72, on the
grounds that "parsing work is named more directly in the fit profile than auth edge
cases". I took #72 instead, for a reason the fit profile does not encode: #69 already
has a classmate saying "I'd like to attempt this!" and #72 has nobody on it. The house
rule is right that a shared issue costs neither of us course credit, but for a first
contribution I would rather not duplicate someone's work in week one. That is a
preference about the classroom, not about the code, and it is not something a rubric
check should be deciding.

One more thing the rubric saw and then set aside. `maintainer-responsive` graded
`unclear` on every candidate: no maintainer first-response is observable anywhere in
this repository. Because it is a preferred check it cannot reject anything, which is
the right call here. In a real project that signal would worry me a great deal. In a
course sandbox where the instructor is the only maintainer, it means nothing.

*3. Anticipated difficulty in claiming it.*

Low. #72 had no comments and no assignee when I graded it, so I am not racing anyone,
unlike #69 where a classmate has already said they want it. The Path Review house rule
says a shared issue costs nobody anything, but an unclaimed one is still simpler. The
real work is in Unit 2: writing a claim comment with the voice guide, then reproducing
the failure, which here means running the xfail test and watching `UnknownHashError`
come out instead of `False`. The risk I see is not the claim, it is passlib. I have not
used it, so I need to read how it decides a hash is unrecognisable before I can be sure
returning `False` is the whole fix and not just a caught exception.

---

Related paths: `eval-run.txt` in this directory; the skill's files in
`tools/issue-select/`.
