# Plan: issue #72, `verify_password` fails open on malformed stored hashes

Issue: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72
My repro comment: https://github.com/codepath/pathreview-ai301-fa26-s1/issues/72#issuecomment-5864448640

## Diagnosis

`verify_password` in `core/security.py:37` returns
`bool(pwd_context.verify(plain_password, hashed_password))` with no exception
handling. When the stored hash is not something passlib can identify, passlib
raises instead of returning a boolean, and the exception reaches the caller.

The evidence I rely on, from my repro at `f89c06f` (passlib 1.7.4, bcrypt 4.3.0):

```
control  valid hash / right password -> True
control  valid hash / wrong password -> False
failing  malformed hash            -> raised passlib.exc.UnknownHashError: hash could not be identified
```

```
  File ".../core/security.py", line 37, in verify_password
    return bool(pwd_context.verify(plain_password, hashed_password))
  File ".../passlib/context.py", line 1132, in identify_record
    raise exc.UnknownHashError("hash could not be identified")
```

The two controls show `pwd_context.verify` itself works for real bcrypt
hashes. The only thing that changes between the controls and the failing line
is the stored hash, so the cause is the missing handling of passlib's
malformed-hash errors in `verify_password`, not bcrypt or the context config.

One more observation from re-running before planning: a stored hash that
starts with a bcrypt prefix but is malformed raises a plain `ValueError`,
not `UnknownHashError`:

```
'not_a_valid_bcrypt_hash'    -> raised passlib.exc.UnknownHashError: hash could not be identified
''                           -> raised passlib.exc.UnknownHashError: hash could not be identified
'$2b$notarealhash'           -> raised builtins.ValueError: not enough values to unpack (expected 2, got 1)
'$2b$12$tooshort'            -> raised builtins.ValueError: salt too small (bcrypt requires exactly 22 chars)
```

`UnknownHashError` is a subclass of `ValueError`, so catching `ValueError`
covers all four shapes.

## Scope

In scope:

- `verify_password` in `core/security.py`: catch `ValueError` around the
  existing `pwd_context.verify` call and return `False`. Update its docstring
  to say a malformed stored hash returns `False`.
- `tests/unit/test_security.py`: remove the `xfail(strict=True)` marker from
  `test_verify_with_wrong_hash_format` (manifest H-05), leaving its assertion
  unchanged, and add one regression test for a bcrypt-prefixed malformed hash
  (`$2b$12$tooshort`) so the `ValueError` shape is covered too.

Not in scope:

- `hash_password`, `create_access_token`, `decode_access_token`, and the
  `CryptContext` configuration.
- `api/routes/auth.py`, the only caller. It already treats `False` as a failed
  login, so it needs no change.
- The `(trapped) error reading bcrypt version` warning from passlib 1.7.4 with
  bcrypt 4.x. It is unrelated and passlib already traps it.
- Logging or alerting on malformed hashes in the database.

## Files

- `core/security.py`
- `tests/unit/test_security.py`

## Approach

1. Create branch `fix/72-verify-password-fail-closed` from `main` at `f89c06f`.
2. In `verify_password`, wrap the `return bool(pwd_context.verify(...))` in
   `try` / `except ValueError: return False`, with a one-line comment saying
   `UnknownHashError` is a `ValueError` subclass.
3. Update the docstring's `Returns:` line.
4. Delete the `@pytest.mark.xfail(...)` decorator on
   `test_verify_with_wrong_hash_format`.
5. Add `test_verify_with_malformed_bcrypt_hash` asserting
   `verify_password("password", "$2b$12$tooshort") is False`.
6. Run `make lint`, `make typecheck`, and `make test-unit`.
7. Commit as `fix: fail closed in verify_password on malformed stored hashes`,
   with `Fixes #72` in the body.

## Test plan

Re-run my unit 2 repro steps against the branch:

1. `python repro72.py`. Expected after the fix:

   ```
   control  valid hash / right password -> True
   control  valid hash / wrong password -> False
   failing  malformed hash            -> False
   ```

   The two controls must be unchanged.
2. `python -m pytest "tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format" -rxX`.
   Expected: `1 passed`, where it was `1 xfailed` before.
3. The four-shape script above. Expected: every line prints `False`.
4. `make test-unit`. Expected: no failures and no `XPASS(strict)`, and the new
   regression test passes.

## Risks and unknowns

- Catching `ValueError` is broader than catching only `UnknownHashError`. I
  checked one side effect: a password containing a NUL byte checked against a
  valid hash currently raises `passlib.exc.PasswordValueError` (also a
  `ValueError`), and after this change it returns `False`. That is still fail
  closed for a login, but it is a behavior change, and I will call it out for
  review.
- I have not checked other passlib or bcrypt versions. The project pins
  `bcrypt<5`, and the exception types could differ under bcrypt 5.
- PR #75 on this issue takes the same `except ValueError` route. I am building
  mine independently under the course rules. Mine adds the bcrypt-prefixed
  regression test.
- I ran on Python 3.14, newer than the 3.11 the project targets. The failure
  is in passlib's hash identification, which does not depend on the Python
  version.

## Deviations

The code change went as planned: the same two files, the same
`except ValueError`, the xfail marker removed, and the one new regression
test. Three things differed from the plan as written.

1. **Commit message scope.** The plan's message had no scope. CONTRIBUTING's
   scope list (`ingestion`, `rag`, `agent`, `safety`, `api`, `frontend`) has
   nothing for `core/`, so I kept `fix:` with no scope rather than pick a
   scope that doesn't fit. PR #75 uses `fix(security)`. I'll match whatever
   review asks for.
2. **`make typecheck` does not run in my environment.** mypy stops on
   `numpy/__init__.pyi:737: Type statement is only supported in Python 3.12
   and greater`. That happens because my venv is Python 3.14 while mypy is
   configured for 3.11. I checked that the same error happens on untouched
   `main` (stashed my change and re-ran), so it comes from my environment, not
   this change. `mypy --follow-imports=silent core/security.py` reports no
   issues. CI's `typecheck` job is the real check here.
3. **Python patch version.** The build ran on Python 3.14.7. My unit 2 repro
   was on 3.14.6. The before output is the same on both.

Results against the test plan: `repro72.py` prints `False` for the malformed
hash and the controls are unchanged. All four shapes print `False`. The H-05
test went from `1 xfailed` to `1 passed`. `make test-unit` gives
`377 passed, 52 xfailed` with no `XPASS(strict)`. `make lint` and
`black --check` pass.

Nothing here changes the approach in my posted plan comment, so the
comment does not need an update on the issue.
