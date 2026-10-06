# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

---

## Posted upstream

**GitHub username**

womputer

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72#issuecomment-6009463571

Plan for `verify_password` failing open on malformed stored hashes, built on my reproduction at `f89c06f` with passlib 1.7.4 and bcrypt 4.3.0.

**Cause.** `verify_password` in `core/security.py:37` returns `bool(pwd_context.verify(...))` with nothing around it. From my repro script:

```
control  valid hash / right password -> True
control  valid hash / wrong password -> False
failing  malformed hash            -> raised passlib.exc.UnknownHashError: hash could not be identified
```

The controls show the verify call works on real hashes. Only the stored hash changes on the failing line.

While re-running before planning I tried more malformed shapes. A hash with a bcrypt prefix but a broken body raises a plain `ValueError` instead:

```
'not_a_valid_bcrypt_hash'    -> raised passlib.exc.UnknownHashError: hash could not be identified
'$2b$12$tooshort'            -> raised builtins.ValueError: salt too small (bcrypt requires exactly 22 chars)
```

`UnknownHashError` subclasses `ValueError`, so catching only `UnknownHashError` would leave the second case failing open.

**Change.** Wrap the existing call in `try` / `except ValueError: return False`, and update the docstring. In `tests/unit/test_security.py`, remove the `xfail` marker from `test_verify_with_wrong_hash_format` (H-05) and add one regression test for `$2b$12$tooshort`. Two files only. I'm not touching `hash_password`, the JWT helpers, the `CryptContext` config, or `api/routes/auth.py`, which already treats `False` as a failed login.

**Test.** Re-run my repro script: the malformed line should print `False` and both controls stay `True` / `False`. The H-05 test should go from `1 xfailed` to `1 passed`, and `make test-unit` should finish with no `XPASS(strict)`.

**Open points.** Catching `ValueError` is broader than the issue strictly needs. One side effect I found: a password with a NUL byte against a valid hash raises `PasswordValueError` today and will return `False` after this. That is still fail closed, but I'll flag it in the PR. I haven't checked bcrypt 5, which the project's `<5` pin excludes for now.

PR #75 takes the same `except ValueError` route. I'm building mine separately under the course rules, and mine adds the bcrypt-prefixed test. If the conventions here want something different, tell me.

---

## Your branch

**Branch**

fix/72-verify-password-fail-closed

**Evidence**

Environment: macOS 26.6.2 (arm64), Python 3.14.7, passlib 1.7.4, bcrypt 4.3.0,
pytest 9.1.1, clone of `codepath/pathreview-ai301-fa26-s1`. Before is `main` at
`f89c06f`. After is branch `fix/72-verify-password-fail-closed` at `bcc5084`.
`repro72.py` is the script from my unit 2 repro comment. `shapes72.py` runs
the same call against four malformed stored hashes.

Before (`main` at `f89c06f`):

```
$ python repro72.py
(trapped) error reading bcrypt version
Traceback (most recent call last):
  File ".../passlib/handlers/bcrypt.py", line 620, in _load_backend_mixin
    version = _bcrypt.__about__.__version__
              ^^^^^^^^^^^^^^^^^
AttributeError: module 'bcrypt' has no attribute '__about__'
control  valid hash / right password -> True
control  valid hash / wrong password -> False
failing  malformed hash            -> raised passlib.exc.UnknownHashError: hash could not be identified

$ python shapes72.py
'not_a_valid_bcrypt_hash'    -> raised passlib.exc.UnknownHashError: hash could not be identified
''                           -> raised passlib.exc.UnknownHashError: hash could not be identified
'$2b$notarealhash'           -> raised builtins.ValueError: not enough values to unpack (expected 2, got 1)
'$2b$12$tooshort'            -> raised builtins.ValueError: salt too small (bcrypt requires exactly 22 chars)

$ python -m pytest "tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format" -rxX
=========================== short test summary info ============================
XFAIL tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format - issue #72 (manifest H-05): password verify raises UnknownHashError instead of returning False
======================== 1 xfailed, 1 warning in 0.91s =========================
```

After (`fix/72-verify-password-fail-closed` at `bcc5084`; passlib's trapped
bcrypt-version warning trimmed from the script output):

```
$ python repro72.py
control  valid hash / right password -> True
control  valid hash / wrong password -> False
failing  malformed hash            -> False

$ python shapes72.py
'not_a_valid_bcrypt_hash'    -> False
''                           -> False
'$2b$notarealhash'           -> False
'$2b$12$tooshort'            -> False

$ python -m pytest "tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format" "tests/unit/test_security.py::TestSecurity::test_verify_with_malformed_bcrypt_hash" -rxX
========================= 2 passed, 1 warning in 0.35s =========================

$ make test-unit
================= 377 passed, 52 xfailed, 1 warning in 12.93s ==================
```

## Eval iterations

**Run history**

Two runs, in order. Only the full run is scored.

1. Full run: **20/20 scored items, bar 18/20: PASS.** Categories: clear-accept
   7/7, scope-creep 4/4, thread-convention 2/2, unbuildable 3/3, wrong-cause
   4/4. This is the run saved in `eval-run.txt`.
2. `--only pkg-04 --out` (partial): 1/1. Not a revision. I changed nothing in
   the skill files. I re-graded one package only to get its per-check grades
   for the package analysis below, because the full run's table shows
   verdicts but not which checks decided them.

The skill files I uploaded are the ones that run graded. Their fingerprints are
in the `eval-run.txt` header.

**Package analysis**

`pkg-04` (junegunn/fzf#4260, thread-convention). Gold label: `reject`. My
rubric's decision: `reject`.

This is the package I expected `thread-direction-engaged` to decide alone. The
plan is documentation only: a man page note, README examples and an FAQ entry
for the `> /dev/tty` workaround. In the thread, the owner named the culprit
("This seems to be the culprit", pointing at `src/tui/light_windows.go`) and
posted a patched test binary from commit 8916cbc. The plan comment never
mentions either. The re-grade failed that check with exactly that evidence:
"OWNER junegunn named src/tui/light_windows.go as culprit and posted patched
binary 8916cbc; comment proposes docs and never mentions it".

The per-check output showed two more required checks failing on this package:

- `scope-bounded` failed, because a docs-only plan "does not fix the reported
  behavior of keys not reaching less". My pass condition asks for "one bounded
  change that fixes the reported behavior". A docs plan is bounded, but it
  isn't a fix.
- `test-plan-decisive` failed, because the test plan "only confirms the
  documented '> /dev/tty' workaround works, already shown by the control run".
  Note 4 asks for a check that would have failed before and passes after. The
  workaround already worked before, so the test plan can't tell before from
  after.

`diagnosis-grounded` passed, and that's correct: the plan's cause matches
the repro and both controls. So pkg-04 is held three ways. I take that as a
sign the package was built well: a plan that ignores the person who already
found the bug usually also ends up planning the wrong change. It also means
pkg-04 doesn't show whether `thread-direction-engaged` works on its own.
pkg-20 is the package that tests a comms check alone: its plan is bounded and
follows the thread's direction, and only the missing AI disclosure that
ghostty's policy requires should hold it. My rubric rejected it too.

**Check rationale**

`thread-direction-engaged`, quoted as it currently reads in
`tools/plan-check/rubric.md`:

> | thread-direction-engaged | The thread highlights (or the live thread) for maintainer direction, read against the plan comment and the plan. | Where a maintainer or project collaborator has given direction on the fix in the thread, the comment and plan engage it: they follow it, build on it, or say why they depart from it. No maintainer direction in the thread also passes. See note 5. | required |

It reads this way because the thread-and-convention category has only two
packages, and missing either one breaks the category floor. The first version
I considered was "the plan comment references the thread". I rejected it before
running anything. Nearly every candidate comment says "report above" or names
a PR, so that version passes everything, pkg-04 included, whose comment says
"report above" and still ignores the owner.

The version I kept asks two questions. Did a maintainer actually give direction
(name the culprit, propose or reject an approach, post a test build, point at
a PR)? If so, does the comment take a position on it? Two parts of note 5 came
from reading the clear accepts before running. "Picking one of several options
a maintainer listed passes" is there for pkg-09: tmccombs listed three fix
options and the plan picks option 2. A stricter reading would fail it for
ignoring options 1 and 3. "Reporters' and other contributors' comments are not
maintainer direction" is there because pkg-15's thread has strong comments
from the reporter, and I didn't want a plan failed for disagreeing with a
non-maintainer.

The live run on my own plan showed the "no direction passes" clause working
too. On #72 every comment's author association is `NONE`, so the check passed
with "no maintainer direction", and it still noted that my comment
acknowledges PR #75.

**Trade-offs**

`thread-direction-engaged` only counts maintainer-role comments as direction.
A plan that ignores a strong, correct suggestion from a reporter or another
contributor still passes this check. I accept that miss, because counting every
commenter would fail plans that rightly disagree with a non-maintainer. On a
classroom repo like Path Review, where every commenter on #72 is a student
with association `NONE`, the check can't fail at all. There, the open PR rule
(acknowledge it rather than race it) only reaches the preferred
`comment-matches-plan` check.

It gave up nothing on the eval set, and here is how I know. The single full run
agreed on all 20 packages, including both thread-convention packages and all
seven clear accepts, so no accept was flipped by the check being too strict.
I made no revisions after that run, so there was nothing to re-check with
canaries. The pkg-04 re-grade agreed again with the same three required
failures, so the result was stable across two runs.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
