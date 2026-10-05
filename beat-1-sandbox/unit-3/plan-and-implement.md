# Unit 3 — Plan and Build

Path: `beat-1-sandbox/unit-3/plan-and-implement.md`

Record of your plan, the branch you built it on, and the evaluation runs that produced
`eval-run.txt`.

---

## Posted upstream

**GitHub username**

Jaed256

**Plan comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72#issuecomment-5987717343

````markdown
Plan for #72, building on my reproduction above (fork at `2f4e82f`; Ubuntu 24.04, Python 3.11.15, passlib 1.7.4, bcrypt 4.3.0).

**Cause:** `verify_password()` in `core/security.py` returns `bool(pwd_context.verify(...))` with no error handling. My repro traceback shows the error raised during passlib's hash identification (`context.py:1132 ... UnknownHashError: hash could not be identified`), while the valid-hash control returns `True False` as expected, so only the unparseable-stored-hash path is broken. Two more things I checked in the same environment:

```
$ .venv/bin/python -c "from core.security import verify_password; verify_password('password', '\$2b\$12\$tooshort')"
...
ValueError: salt too small (bcrypt requires exactly 22 chars)
$ .venv/bin/python -c "import passlib.exc as e; print(e.UnknownHashError.__mro__)"
(<class 'passlib.exc.UnknownHashError'>, <class 'ValueError'>, <class 'Exception'>, <class 'BaseException'>, <class 'object'>)
```

So a truncated bcrypt-looking hash fails with a plain `ValueError`, and `UnknownHashError` is itself a `ValueError` subclass.

**Change (one function + tests):**
- `core/security.py`: wrap the `pwd_context.verify(...)` call in `verify_password()` with `except ValueError: return False`.
- `tests/unit/test_security.py`: remove the strict `xfail` marker on `test_verify_with_wrong_hash_format` (H-05) and add a regression test for the truncated-hash case.
- Not in scope: hashing schemes, `hash_password()`, logging, or other xfails. I'm deliberately not catching broad `Exception`, so a `TypeError` from bad input still raises.

**How I'll test it:** re-run my repro steps. The covering test should go from `XFAIL` to `1 passed`, the direct call with `"not_a_valid_bcrypt_hash"` should print `False` instead of raising, the valid-hash control should still print `True False`, and `tests/unit/test_security.py` should pass in full.

**Open question:** whether catching `ValueError` is too wide; I don't know of a `ValueError` passlib raises for a valid bcrypt hash, but I'd like a reviewer to confirm. I've seen #78 takes a similar approach; per the house rules I'm building my own from my own repro.

_I used an AI assistant (Claude) to help draft this plan; I've checked it against my repro output._
````

---

## Your branch

**Branch**

fix/72-verify-password-malformed-hash

(in https://github.com/Jaed256/pathreview-ai301-fa26-s3, two commits on top of `main` at `2f4e82f`)

**Evidence**

Environment: Ubuntu 24.04.4 (x86_64), Python 3.11.15, passlib 1.7.4, bcrypt 4.3.0, fork clone with `.venv` from `pip install -e ".[dev]"`. These are my Unit 2 repro steps re-run before and after the change.

```
--- BEFORE (main, 2f4e82f) ---
$ .venv/bin/python -m pytest "tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format" -q --tb=short -p no:warnings
x                                                                        [100%]
1 xfailed in 2.60s
$ .venv/bin/python -m pytest "tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format" -q --runxfail --tb=line -p no:warnings
.venv/lib/python3.11/site-packages/passlib/context.py:1132: passlib.exc.UnknownHashError: hash could not be identified
=========================== short test summary info ============================
FAILED tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format
1 failed in 0.26s
$ .venv/bin/python -c "from core.security import verify_password; print(verify_password('password', 'not_a_valid_bcrypt_hash'))"
passlib.exc.UnknownHashError: hash could not be identified
$ .venv/bin/python -c "from core.security import verify_password; print(verify_password('password', '\$2b\$12\$tooshort'))"
ValueError: salt too small (bcrypt requires exactly 22 chars)
$ .venv/bin/python -c "from core.security import verify_password, hash_password; h=hash_password('password'); print(verify_password('password', h), verify_password('nope', h))"
True False
```

```
--- AFTER (branch fix/72-verify-password-malformed-hash) ---
$ .venv/bin/python -m pytest "tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format" -q --tb=short -p no:warnings
.                                                                        [100%]
1 passed in 0.30s
$ .venv/bin/python -c "from core.security import verify_password; print(verify_password('password', 'not_a_valid_bcrypt_hash'))"
False
$ .venv/bin/python -c "from core.security import verify_password; print(verify_password('password', '\$2b\$12\$tooshort'))"
False
$ .venv/bin/python -c "from core.security import verify_password, hash_password; h=hash_password('password'); print(verify_password('password', h), verify_password('nope', h))"
True False
$ .venv/bin/python -m pytest tests/unit/test_security.py -q -p no:warnings
26 passed in 7.90s
```

## Eval iterations

**Run history**

1. Run 1 (full, 20 packages): `agreement: 19/20 scored items  (bar: 18/20: PASS)`, with
   `categories: clear-accept 6/7  scope-creep 4/4  thread-convention 2/2  unbuildable 3/3  wrong-cause 4/4`.
   The one miss: pkg-14 (gold `accept`), failed on `executable`.
2. Partial re-run after loosening `executable`, with the unbuildable packages as canaries
   (`--only pkg-14,pkg-10,pkg-17,pkg-18`): `agreement: 4/4 scored items`. Not scored.
3. Run 2 (full, confirming, saved with `--save-run eval-run.txt`):
   `agreement: 20/20 scored items  (bar: 18/20: PASS)`, with
   `categories: clear-accept 7/7  scope-creep 4/4  thread-convention 2/2  unbuildable 3/3  wrong-cause 4/4`.

Final score: 20/20, matching the agreement line in the committed `eval-run.txt`.

**Package analysis**

pkg-14 (zellij-org/zellij, OSC color responses leaking into the pane on SSH reattach). Gold
label: `accept` ("honestly scoped-down: reattach handshake fix with a regression-window repro;
defers the untestable Windows variant and says so"). My rubric in run 1: `reject`, on
`executable`. In run 2: `accept`.

In run 1 the grader's evidence was: "Files names only 'the client attach/reattach path' in
zellij-server and zellij-client; 'exact functions to be pinned in the PR after tracing' leaves
no specific file/function or committed fix site." My first `executable` check demanded "the
specific file(s) or function(s)", so a plan that named a code path in two named crates but left
the exact function for tracing failed. That reading was too literal: the plan had decided both
WHERE (the reattach path in `zellij-server` and the query issuance in `zellij-client`) and WHAT
(drain the OSC responses before wiring pane input). Someone could start on it today. After I
revised the check, run 2's evidence was: "Names the attach/reattach path in zellij-server and
query issuance in zellij-client; one approach (drain OSC responses before wiring pane input)",
and it passed. Its other checks passed both times; for example `bounded-scope`: "Windows variant
and theme-cache changes explicitly deferred with reasons."

**Check rationale**

The `executable` check, as it reads now in `tools/plan-check/rubric.md`:

> Evidence: The plan's Files list and Approach steps
>
> Pass condition: PASSES when a stranger could start the work without asking the author anything: the plan names where the change goes (specific file(s) or function(s), or a specific code path inside named modules/crates, e.g. "the client reattach path in zellij-client") and commits to one chosen approach. Pinning the exact function during the build is fine when the location and the approach are both decided. FAILS when files are missing or vague ("somewhere in the input stack"), when the approach is still undecided ("X or Y, whichever is easier", "not sure which layer"), or when the work is only "investigate / profile / poke around" with no change decided.
>
> Weight: required

Why it reads this way: its first version passed only when "the plan names the specific file(s)
or function(s) to change and commits to one chosen approach". That rejected pkg-14, where the
location was a named code path and the approach was fixed, but the exact function would be
pinned while tracing. The question this check really asks is "could a stranger start without
asking the author anything?", and the answer for pkg-14 is yes. So I widened the location part to
allow "a specific code path inside named modules/crates" and said explicitly that pinning the
exact function during the build is fine once location and approach are both decided. I kept the
two things that make a plan unbuildable as hard fails: no location at all ("somewhere in the input
stack") and an undecided approach ("X or Y, whichever is easier", "not sure which layer"). I made
the same change in `procedure.md`'s Check execution step 4 so the procedure and the rubric agree.

**Trade-offs**

Loosening `executable` risks passing vague plans, so I re-ran all three `unbuildable` packages as
canaries with `--only pkg-14,pkg-10,pkg-17,pkg-18`: pkg-10 ("profile-and-optimize with no
files"), pkg-17 ("gocui? tcell? not sure") and pkg-18 ("upstream or vendored, whichever is
easier") all stayed `reject`, and pkg-14 flipped to `accept` (4/4). The confirming full run then
showed nothing else moved (20/20, unbuildable 3/3). The case I accept it will miss: a plan that
names a real-sounding code path ("the request pipeline in core") that is still too broad to start
on. My check now trusts a named path plus a decided approach, so a plan that only sounds specific
could pass.

---

Related paths: `plan.md` and `eval-run.txt` in this directory; your skill's files in
`tools/plan-check/`.
