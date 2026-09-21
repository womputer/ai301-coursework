# Rubric: is this a good first issue?

Seven required checks and three preferred ones, covering the five families
the eval set is built around: maintainer alive, repo in use, scope fits,
unclaimed, and policy allows AI.

Every recency threshold is measured against the capture date stamped on the
bundle in eval mode, and against today's date in live mode.

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| not-archived | The `archived:` field on the repo line. | The field reads `no`. | required |
| maintainer-active | Dates and author names in the "last 5 default-branch commits" list. | At least 2 of those 5 commits are dated within 90 days, and at least 1 of the 5 has a non-bot author. | required |
| repo-in-use | The "latest release" and "last push to any branch" fields. | A release published within 365 days, or a push to any branch within 90 days. | required |
| bounded-single-change | The issue title and body, the `linked PRs:` list, and any maintainer comment that sizes the work. | The issue asks for one change that lands in one pull request. Fails only on a disqualifier in note 1. | required |
| direction-settled | The comment thread with each comment's author_association, the labels, and the `linked PRs:` states. | What to build is already decided. Fails only on a disqualifier in note 2. | required |
| unclaimed | The `assignees:` and `linked PRs:` fields, and claim language in the thread. | No assignee, no linked pull request in the `open` state, and no claim comment dated within 90 days. | required |
| ai-contributions-allowed | The "contribution policy" line. | The policy does not ban AI-assisted contributions. See note 3. | required |
| release-recency | The "latest release" field. | A release published within 365 days. | preferred |
| maintainer-responsive | The "maintainer first-response sample" list. | At least one issue in the sample drew a first Owner, Member, or Collaborator reply in under 7 days. | preferred |
| newcomer-signposted | The issue's labels. | The issue carries `good first issue`, `help wanted`, `easy`, or an equivalent. | preferred |

## Verdict rule

Accept if and only if every required check grades `pass`; a `fail` or an
`unclear` on any required check rejects the issue, and preferred checks never
change the verdict.

Grade a required check `unclear` only when the evidence it names is absent
from the bundle, and treat that as a fail: a first issue you cannot verify is
not a first issue you should take. Use the preferred grades to rank the issues
that were accepted, not to decide them.

## Applying the checks

**Note 1, disqualifiers for `bounded-single-change`.** Fail the check when any
one of these is present, and pass it otherwise:

- the title or body frames the issue as a tracking issue, an umbrella, a
  megaissue, or a campaign worked through over time;
- the issue asks for several unrelated outcomes that different people would
  take as separate pull requests;
- three or more linked pull requests each implement a different part of the
  issue, which makes it a coordination point rather than a change;
- a maintainer states the fix requires changes to core internals;
- the issue is a usage question rather than a request for a change.

Count the outcomes the issue asks for, not the bullets it contains. A numbered
or bulleted list belongs to one outcome when the items are causes of a single
reported symptom, the section outline of one document, or follow-up
suggestions attached to one fix, and extras marked optional or `nice to have`
do not enlarge the scope. Several instances of the same small repeated fix are
one change, even when the list is open-ended or ends in `etc.`. A documentation
issue that specifies the outline of one new or rewritten page is one outcome,
however many sections that outline lists, and updating the existing pages that
point at it is part of the same change. A terse body, a
missing reproduction, or an unpolished writeup never fails this check: grade
the size of the work asked for, not the quality of the writing.

**Note 2, disqualifiers for `direction-settled`.** Fail the check when any one
of these is present, and pass it otherwise:

- the thread shows a design disagreement that no Owner, Member, or
  Collaborator has settled;
- two or more linked pull requests are closed unmerged, which says previous
  attempts did not land;
- the issue proposes a new user-facing feature and no maintainer has endorsed
  it, meaning no maintainer-applied `good first issue` or `help wanted` label
  and no maintainer comment inviting work.

Bug reports and documentation changes pass without endorsement, because the
behavior the project wants is already defined.

**Note 3, reading a contribution policy.** An outright ban ("we do not accept
AI-generated code") fails. Conditions pass: disclosing AI use, personally
understanding and testing the change, and human-reviewing the output are terms
to follow, not reasons to walk away. Silence passes, because most repositories
state nothing and that is not a restriction.
