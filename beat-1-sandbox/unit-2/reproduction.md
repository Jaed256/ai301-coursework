# Unit 2 — Claim and Reproduce

Path: `beat-1-sandbox/unit-2/reproduction.md`

Record of your claim and reproduction on the issue you chose in Unit 1, and of the
evaluation runs that produced `eval-run.txt`.

---

## Your identity upstream

**GitHub username**

Jaed256

---

## Posted upstream

**Claim comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72#issuecomment-5900121631

````markdown
I'd like to work on #72 as my Path Review contribution. I know several classmates have claimed it too; per the course house rules I'm posting my own claim and will do my own reproduction.

What I'll look at: `verify_password()` in `core/security.py` calls passlib's `pwd_context.verify()` directly, so from reading the code I expect a stored hash that isn't a recognizable bcrypt string (the test uses `"not_a_valid_bcrypt_hash"`) to raise `passlib.exc.UnknownHashError` instead of returning `False`.

Next step: set up my fork from `docs/SETUP.md`, run `tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format` with the strict xfail marker disabled, and call `verify_password()` directly against a malformed hash and a valid control hash. I'll post a reproduction report here with my environment, exact commands, and the output I get, whether or not it reproduces.
````

**Reproduction comment**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72#issuecomment-5900128645

````markdown
**Reproduction report for #72. Result: reproduced** on current `main`.

**Environment**
- OS: Ubuntu 24.04.4 LTS, x86_64 (Linux 6.18 cloud dev container)
- Python 3.11.15, fresh venv, `pip install -e ".[dev]"` (from `make setup`'s install step)
- passlib 1.7.4, bcrypt 4.3.0
- Code: my fork of `codepath/pathreview-ai301-fa26-s3` at `2f4e82f` (upstream `main` is also `2f4e82f` per `git ls-remote`)
- Postgres/Redis not started: this path is a pure unit test and needs neither

**Steps**
```bash
git clone https://github.com/Jaed256/pathreview-ai301-fa26-s3.git && cd pathreview-ai301-fa26-s3
git rev-parse --short HEAD        # 2f4e82f, same as `git ls-remote` of upstream main
python3 -m venv .venv && .venv/bin/pip install -e ".[dev]"
cp .env.example .env
T="tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format"
# 1) the covering test as shipped (strict xfail)
.venv/bin/python -m pytest "$T" -v
# 2) same test with the xfail disabled, to see the real error
.venv/bin/python -m pytest "$T" -q --runxfail --tb=short
# 3) direct calls: valid-hash control, then the malformed hash
.venv/bin/python -c "from core.security import verify_password, hash_password; h=hash_password('password'); print(verify_password('password', h), verify_password('nope', h))"
.venv/bin/python -c "from core.security import verify_password; verify_password('password', 'not_a_valid_bcrypt_hash')"
```

**Observed** (raw output; step 1 trimmed to the result line)

Step 1:
```
tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format XFAIL [100%]
```

Step 2:
```
tests/unit/test_security.py:227: in test_verify_with_wrong_hash_format
    result = verify_password("password", wrong_hash)
             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
core/security.py:37: in verify_password
    return bool(pwd_context.verify(plain_password, hashed_password))
                ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.venv/lib/python3.11/site-packages/passlib/context.py:2343: in verify
    record = self._get_or_identify_record(hash, scheme, category)
             ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.venv/lib/python3.11/site-packages/passlib/context.py:2031: in _get_or_identify_record
    return self._identify_record(hash, category)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
.venv/lib/python3.11/site-packages/passlib/context.py:1132: in identify_record
    raise exc.UnknownHashError("hash could not be identified")
E   passlib.exc.UnknownHashError: hash could not be identified
FAILED tests/unit/test_security.py::TestSecurity::test_verify_with_wrong_hash_format
1 failed in 0.29s
```

Step 3 (control, then malformed hash; last two lines of the traceback):
```
True False
    raise exc.UnknownHashError("hash could not be identified")
passlib.exc.UnknownHashError: hash could not be identified
```

**Expected:** `verify_password("password", "not_a_valid_bcrypt_hash")` returns `False` (fail closed), as the test asserts.
**Actual:** it raises `passlib.exc.UnknownHashError` from `pwd_context.verify()` at `core/security.py:37`. With a valid bcrypt hash it behaves correctly (`True` for the right password, `False` for a wrong one), so the problem is only the unrecognized-hash path.

Next I'll check whether other malformed inputs escape too: a bcrypt-looking but truncated hash like `$2b$12$tooshort` raises `ValueError: salt too small (bcrypt requires exactly 22 chars)` in the same environment, so I want to understand which exceptions a fail-closed `verify_password()` should catch.

_I used an AI assistant (Claude) to help run and write this up; I've read and checked the output._
````

## Eval iterations

**Run history**

1. Run 1 (full, 20 packages): `agreement: 18/20 scored items  (bar: 18/20: PASS)`, with
   `categories: clear-accept 6/8  disclosure 1/1  no-evidence 4/4  unfollowable-comms 3/3  wrong-target 4/4`.
   Misses: pkg-03 (failed `honest-outcome`) and pkg-05 (failed `steps-rerunnable`), both gold `accept`.
2. Partial re-run after loosening those two checks, with canaries
   (`--only pkg-03,pkg-05,pkg-18,pkg-15,pkg-14,pkg-20`): `agreement: 6/6 scored items`. Not scored.
3. Run 2 (full, confirming, saved with `--save-run eval-run.txt`):
   `agreement: 19/20 scored items  (bar: 18/20: PASS)`, with
   `categories: clear-accept 7/8  disclosure 1/1  no-evidence 4/4  unfollowable-comms 3/3  wrong-target 4/4`.

Final score: 19/20, matching the agreement line in the committed `eval-run.txt`.

**Package analysis**

pkg-09 (sharkdp/fd#2033, `--exec-batch` ordering). Gold label: `accept` (an honest
cannot-reproduce). My rubric in the committed run: `reject`, failed on `behavior-matches`.

The report says "I could NOT reproduce scenario 2" and shows a real attempt: 120000 long-named
files, two `--exec-batch` commands, and the log `ONE ONE ONE TWO TWO TWO`. My `behavior-matches`
check passes a cannot-reproduce only "when the artifact shows the issue's trigger actually run
and its real result". In run 1 the grader read the shown run as the trigger ("artifact shows two
--exec-batch commands run with the arg-size limit hit"). In run 2 it read the same text the other
way: "The shown run gives both commands the same file list, so it never makes one command hit
the limit first; the report admits 'my padding approach may not achieve that' and the padded run
is not shown." Both readings are defensible, because the issue's trigger is "the second command
hits the limit before the first", and the author themselves says they are not sure they created
that condition. So my rubric rejected it because the report's own honesty ("may not achieve
that") made it unclear that the trigger was exercised, and my check treats an unexercised
trigger as not matching. The gold label instead rewards the honest, well-evidenced attempt.
This is one of the arguable packages, and it shows my check is sensitive to how strictly the
grader reads "the issue's trigger".

**Check rationale**

The `honest-outcome` check, as it reads now in `tools/repro-check/rubric.md`:

> Evidence: Every claim in the claim comment and repro report ("reproduced", "confirmed", a root cause, "affects all versions", expected vs. actual) read against the artifacts actually shown
>
> Pass condition: PASSES when the central claims (reproduced / could not reproduce / a root cause / which versions or platforms are affected) are each backed by a shown artifact and the stated outcome matches what the artifact shows; a supporting side-observation stated in plain words without pasted output (for example a control run "without the flag it works") does not fail the check by itself, as long as the main reproduction is shown; an evidenced cannot-reproduce that names what differed from the reporter's setup PASSES. FAILS when the report claims more than it shows: "reproduced" over an artifact that does not show the bug, a root cause or "I verified" with no evidence shown, generalizing to versions or platforms not tested, or expected/actual that contradict the artifact.
>
> Weight: required

Why it reads this way: in run 1 its pass condition said "PASSES when every claim is backed by a
shown artifact". That failed pkg-03 (ripgrep), a good report, because one sentence about a
control run ("Dropping -r ... reports 1, 4, 7, 10 correctly") had no pasted output. That is a
side observation, not the claim the report rests on. I narrowed the check to the central claims
(reproduced / could not reproduce / root cause / which versions or platforms are affected) and
said explicitly that a supporting control stated in words does not fail it by itself, as long as
the main reproduction is shown. I kept the fail list (a root cause or "I verified" with nothing
shown, generalizing to untested versions) so pkg-15's unsupported race-condition diagnosis and
pkg-17's generalization to the Store release still fail.

**Trade-offs**

Loosening `honest-outcome` risks letting through reports that assert more than they show, so
before the confirming run I re-ran canaries with `--only pkg-03,pkg-05,pkg-18,pkg-15,pkg-14,pkg-20`:
pkg-15 and pkg-14 (no-evidence) still rejected, pkg-18 (private config) still rejected, and
pkg-20 (the one disclosure package) still rejected, while pkg-03 and pkg-05 flipped to accept
(6/6). The case I accept it will miss: a report whose main reproduction is shown but whose control
run, stated only in words, is actually false. My check now trusts that sentence. The confirming run
also showed a cost I did not plan for: pkg-09 agreed in run 1 but disagreed in run 2 under the same
`behavior-matches` wording, so on honest cannot-reproduce reports my rubric is not fully stable
between runs.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/repro-check/`.
