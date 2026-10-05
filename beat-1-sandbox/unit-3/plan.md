# Plan: #72 `verify_password` fails closed on malformed stored hashes

Issue: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72
My reproduction: https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72#issuecomment-5900128645

## Diagnosis

`verify_password()` in `core/security.py` returns `bool(pwd_context.verify(plain_password, hashed_password))` with no error handling, so any exception passlib raises while identifying or parsing the stored hash escapes to the caller.

The repro evidence I rely on (from my Unit 2 repro comment, fork at `2f4e82f`):

- With the strict xfail disabled, the covering test fails inside `verify_password`, and the traceback shows the error raised by passlib's hash identification, not by bcrypt checking a password:
  ```
  core/security.py:37: in verify_password
      return bool(pwd_context.verify(plain_password, hashed_password))
  .venv/lib/python3.11/site-packages/passlib/context.py:1132: in identify_record
      raise exc.UnknownHashError("hash could not be identified")
  E   passlib.exc.UnknownHashError: hash could not be identified
  ```
- Control: with a valid bcrypt hash the same function behaves correctly, `True` for the right password and `False` for a wrong one (`True False`). So hashing and normal verification are fine; only the "stored hash can't be parsed" path is broken.
- Related case from the same environment: a bcrypt-looking but truncated hash (`$2b$12$tooshort`) raises `ValueError: salt too small (bcrypt requires exactly 22 chars)` from the same call.

In passlib 1.7.4, `passlib.exc.UnknownHashError` is a subclass of `ValueError` (`UnknownHashError.__mro__` = `(UnknownHashError, ValueError, Exception, ...)`), so both malformed-hash cases surface as `ValueError`.

## Scope

In scope:
- Make `verify_password()` return `False` when passlib raises `ValueError` (which includes `UnknownHashError`) for a stored hash it can't identify or parse.
- Remove the strict `xfail` marker from `test_verify_with_wrong_hash_format` (manifest H-05), as the issue asks.
- Add one regression test for the truncated-bcrypt `ValueError` case.

Not in scope:
- Changing the `CryptContext` schemes, adding hash migration/rehash, or touching `hash_password()`.
- Logging or metrics for malformed hashes (could be a follow-up issue).
- Any other seeded bugs or xfail markers in the test suite.
- Catching broad `Exception`: a `TypeError` from passing a non-string, for example, is a programming error and should still raise.

## Files

- `core/security.py`: `verify_password()` only.
- `tests/unit/test_security.py`: remove the xfail marker on `test_verify_with_wrong_hash_format`; add `test_verify_with_truncated_bcrypt_hash`.

## Approach

1. Branch `fix/72-verify-password-malformed-hash` from `main` (`2f4e82f`) in my fork.
2. In `verify_password()`, wrap the `pwd_context.verify(...)` call in `try` / `except ValueError: return False`, with a one-line comment that `UnknownHashError` is a `ValueError` subclass.
3. Remove the `@pytest.mark.xfail(strict=True, reason="issue #72 ...")` decorator. Check `pyproject.toml` for a matching suppression (I found none with `grep -n "H-05\|#72" pyproject.toml`).
4. Add the truncated-hash test asserting `verify_password("password", "$2b$12$tooshort") is False`.
5. Run the test plan below, then `ruff` and `mypy` on the two files.

## Test plan

Re-run my Unit 2 repro steps against the change:

1. `.venv/bin/python -m pytest "tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format" -q --tb=short`
   - Before: `XFAIL` (and with `--runxfail`, `FAILED ... passlib.exc.UnknownHashError: hash could not be identified`).
   - Expected after: `1 passed` (no xfail marker any more).
2. Direct call: `.venv/bin/python -c "from core.security import verify_password; print(verify_password('password', 'not_a_valid_bcrypt_hash'))"`
   - Before: traceback ending in `passlib.exc.UnknownHashError: hash could not be identified`.
   - Expected after: prints `False`.
3. Control, unchanged behavior: `... h=hash_password('password'); print(verify_password('password', h), verify_password('nope', h))`
   - Before and expected after: `True False`.
4. Truncated hash: `print(verify_password('password', '$2b$12$tooshort'))`
   - Before: `ValueError: salt too small (bcrypt requires exactly 22 chars)`. Expected after: `False`.
5. `.venv/bin/python -m pytest tests/unit/test_security.py -q`: all tests pass, none xfailed for #72.

## Risks and unknowns

- Catching `ValueError` could hide a different `ValueError` that passlib raises for a valid hash. I don't know of one for bcrypt verification, and a failed verify should be `False` anyway, but this is the main thing I want reviewers to look at.
- I haven't run the full CI (five jobs) yet; I'll check that on the PR in Unit 4.
- Another student has an open PR (#78) for this issue. Per the house rules that doesn't block mine, but my PR may end up redundant.

## Deviations

Code: nothing changed; the plan held. The branch has exactly the two planned edits: `verify_password()` now catches `ValueError` and returns `False`, the H-05 `xfail` marker is gone, and `test_verify_with_truncated_bcrypt_hash` is added. Every test-plan step gave the expected result (covering test `1 passed`, direct call `False`, truncated hash `False`, control `True False`, `tests/unit/test_security.py` 26 passed). `pyproject.toml` had no H-05 suppression to remove.

Process: I didn't `git push` from my local clone. I put the two changed files on the branch with GitHub's web upload, which made two commits (one per file) instead of one. GitHub also auto-named the branch `Jaed256-patch-1`, so I renamed it to `fix/72-verify-password-malformed-hash`. Neither changes the code; `plan.md` and `comment.md` are not on the branch.

Also noted: the whole `tests/unit` suite has 30 errors that exist on `main` too (same count before and after my change: 345 passed / 53 xfailed before, 347 passed / 52 xfailed after). They aren't from this change, so they stay out of scope.
