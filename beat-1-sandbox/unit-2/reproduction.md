# Unit 2 - Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

---

## Your identity upstream

**GitHub username**

womputer

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72#issuecomment-5864437082

I'd like to pick this one up. On a fresh clone at `f89c06f`, `verify_password` in `core/security.py:37` calls `pwd_context.verify(...)` with no exception handling, so when the stored hash is not a recognisable format passlib raises `UnknownHashError` and it escapes to the caller instead of the function returning `False`. The covering test `test_verify_with_wrong_hash_format` is still marked `xfail(strict=True)` against manifest id H-05.

I see several classmates working this issue already. I'm reproducing and reporting independently rather than adding to their threads.

Next from me is a repro report with my environment, the exact commands, and the output they produced. If it reproduces I'll attempt the fix after that, which I expect means catching the passlib error in `verify_password` and dropping the xfail marker. This is my first contribution here, so tell me if I have the conventions wrong.

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72#issuecomment-5864448640

**Reproduced.** `verify_password` raises `passlib.exc.UnknownHashError` instead of returning `False` when the stored hash is not a recognisable format.

**Environment**

- OS: macOS 26.6.2 (arm64)
- Python: 3.14.6, fresh venv
- passlib 1.7.4, bcrypt 4.3.0, pytest 9.1.1
- Repo: fresh clone of this repository at commit `f89c06f`, clean working tree
- No `.env` or Docker needed: every field on `Settings()` in `core/config.py` has a default, so `core/security.py` imports standalone.

**Steps**

```
git clone https://github.com/codepath/pathreview-ai301-fa26-s1.git
cd pathreview-ai301-fa26-s1
python3 -m venv .venv && . .venv/bin/activate
pip install "passlib[bcrypt]" "bcrypt<5" "python-jose[cryptography]" \
            "pydantic[email]" pydantic-settings pytest
python -m pytest "tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format" -rxX
```

Then, to see the raised exception directly next to a control, save this as `repro72.py` in the repo root and run `python repro72.py`:

```python
from core.security import hash_password, verify_password

good = hash_password("correct-horse")
print("control  valid hash / right password ->", verify_password("correct-horse", good))
print("control  valid hash / wrong password ->", verify_password("wrong", good))
try:
    r = verify_password("password", "not_a_valid_bcrypt_hash")
    print("failing  malformed hash            ->", r)
except Exception as e:
    print(f"failing  malformed hash            -> raised {type(e).__module__}.{type(e).__name__}: {e}")
```

**Expected:** all three lines print a boolean. A hash that cannot be identified should fail closed and return `False`.

**Actual:** the two controls return booleans; the malformed hash raises.

```
control  valid hash / right password -> True
control  valid hash / wrong password -> False
failing  malformed hash            -> raised passlib.exc.UnknownHashError: hash could not be identified
```

The pytest run agrees, via the project's own marker:

```
tests/unit/test_security.py x                                            [100%]
XFAIL tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format
  - issue #72 (manifest H-05): password verify raises UnknownHashError instead of returning False
1 xfailed, 1 warning in 0.25s
```

Traceback, trimmed to the relevant frames:

```
  File ".../core/security.py", line 37, in verify_password
    return bool(pwd_context.verify(plain_password, hashed_password))
  File ".../passlib/context.py", line 1132, in identify_record
    raise exc.UnknownHashError("hash could not be identified")
passlib.exc.UnknownHashError: hash could not be identified
```

**Two things I should flag rather than leave for someone else to find.**

I ran on Python 3.14.6, while the other reports on this issue are on 3.11 through 3.13. The failure is in passlib's hash identification step, which does not depend on the Python version, and the controls behave identically, so I do not think it matters here. Saying it anyway.

The run also prints `(trapped) error reading bcrypt version` with an `AttributeError: module 'bcrypt' has no attribute '__about__'` above my output. That is the known passlib 1.7.4 / bcrypt 4.x version-probe mismatch, passlib traps it and continues, and it is unrelated to this issue. I left it in the paste rather than trimming it so the output matches what you would actually see.

**Next step:** attempt the fix, catching passlib's error inside `verify_password` so it returns `False`, and removing the `xfail` marker from `test_verify_with_wrong_hash_format`.

## Eval iterations

**Run history**

Three runs, in order. Only the two full runs are scored.

1. `--only pkg-09,pkg-20,pkg-10,pkg-02,pkg-17,pkg-11` (partial): 6/6. A smoke run on
   the six packages I judged hardest before paying for a full run: the two packages
   that split on AI disclosure, both honest cannot-reproduces, and two wrong-targets.
2. Full run: **20/20 scored items, bar 18/20: PASS.** Categories: clear-accept 8/8,
   disclosure 1/1, no-evidence 4/4, unfollowable-comms 3/3, wrong-target 4/4.
3. Full run: **20/20 scored items, bar 18/20: PASS.** Same categories. I reworded one
   sentence of note 3 in `rubric.md` for clarity after run 2, which changed the file's
   fingerprint, so I re-ran to keep `eval-run.txt` describing the rubric I actually
   uploaded. The verdicts were identical. This is the run saved in `eval-run.txt`.

**Package analysis**

`pkg-09` (an honest cannot-reproduce on an argument-size reordering issue). Gold label:
`accept`. My rubric's decision: `accept`. It is worth explaining because two separate
checks could have rejected it, and both had to be written carefully to avoid that.

The first is `behavior-matches-issue`. pkg-09's author tried the issue's scenario and
the bug did not appear. A check phrased as "the artifact shows the issue's reported
behavior" rejects that package, and it also rejects pkg-10, which is the same shape.
Both are gold accepts. The lecture's line is that an evidenced cannot-reproduce is a
real result, so note 2 of my rubric says the check passes when the artifact shows a
real attempt at the issue's own scenario and the report names what differed. pkg-09
names uniform name lengths and a 2 MiB ARG_MAX, which is exactly that.

The second is `ai-disclosure-where-required`, and this is the one that took the most
care. pkg-09's repo policy says AI-assisted contributions "must state the tool and the
extent of its use **in the pull request**", and pkg-09's comments do not disclose
anything. pkg-20's repo requires that "**all AI usage in any form** must be disclosed"
and names issues and comments specifically, and pkg-20's comments also do not disclose.
Both packages are AI-assisted work with no disclosure line, yet gold accepts one and
rejects the other. The difference is what the policy's duty attaches to. A duty owed on the pull request is not yet due on an issue
comment. So my check reads the attachment point rather than the presence of a policy.
pkg-20 is the single package in the `disclosure` category, so getting this wrong costs
the category floor no matter what the total is.

**Check rationale**

`ai-disclosure-where-required`, quoted as it currently reads in
`tools/repro-check/rubric.md`:

> | ai-disclosure-where-required | The repo-facts block's contribution-policy line, and both comments. | The package satisfies the repo's stated AI rules for comments posted on an issue. See note 5. | required |

with note 5:

> **Note 5, reading the repo's AI rules.** Treat the package as AI-assisted work.
> Then read what the policy's duty attaches to:
>
> - If the policy requires disclosing AI use in comments, issues, or contributions of any form, and neither comment discloses it, fail.
> - If the policy's disclosure duty attaches only to the pull request or the code, comments that do not disclose still pass; that duty is not due yet.
> - If the policy requires that comments to maintainers be written by a human in their own words, pass when the comments read as a person's own writing and fail when they read as unedited generated text.
> - Policies that require understanding, testing, and taking responsibility for the work, with no disclosure duty stated, pass without a disclosure line.
> - Silence passes. Most repositories state nothing, and that is not a rule.

The first version I considered was one line: fail when the repo has an AI policy and
the comments do not disclose. I rejected it before running anything, because reading
the twenty policy lines side by side showed it would fail five of the eight gold
accepts. pkg-03, pkg-05, pkg-07, pkg-09 and pkg-12 all sit under some stated AI policy.
Only one of them, pkg-07, actually discloses. The other four pass because their
policies ask for something else: responsibility and understanding (pkg-05, pkg-12), a
human voice in comments (pkg-03), or disclosure owed later in the pull request
(pkg-09). A check that just counts policies would have scored at most 15 of 20.

The four bullets after the first exist because each one is a real package. The "in
their own words" bullet is pkg-03. The "understanding and responsibility" bullet is
pkg-05 and pkg-12. The pull-request bullet is pkg-09. The "in any form" bullet is
pkg-20. Silence covers the other twelve.

**Trade-offs**

This check hands the verdict to a summary of a policy rather than the policy itself. In
eval mode I read one `- contribution policy:` line in the repo-facts block, which is
somebody's precis of `CONTRIBUTING.md` plus any linked AI policy file. If that summary
flattens a distinction, my check inherits the error with no way to notice. The
pull-request-versus-comments distinction that decides pkg-09 against pkg-20 survives
only because whoever wrote those two lines happened to preserve the attachment point.
A blunter summary of the same two repos would have made the packages look identical and
the category unwinnable.

It gave up nothing on the eval set, and here is how I know. I ran the six hardest
packages before the full run, and `pkg-09` and `pkg-20` were two of the six precisely
because they are the pair this check has to split. They split correctly at about $1.20,
which is what told me the full run was worth buying. The full run then agreed on all
twenty, so no other package moved.

Live mode found the limit that the eval set cannot show. Running the check against my
own issue, the skill went looking for `CONTRIBUTING.md` in the repository root and in
`.github/`, and the file is actually at `docs/CONTRIBUTING.md`. It found it, and it
confirmed there is no AI policy anywhere in that repo, so the check passes on silence.
But a policy filed somewhere my evidence guide does not name is a policy my rubric
would miss and score as silence, which fails open. That is the case I accept it will
miss.

---

Related paths: `eval-run.txt` in this directory; the skill's files in
`tools/repro-check/`.
