# Unit 4 — Test and Submit

Path: `beat-1-sandbox/unit-4/pull-request.md`

Record of the pull request you opened against the Path Review repo, and of the evaluation
runs that produced `eval-run.txt`.

---

## Your pull request

**Pull request**

https://github.com/codepath/pathreview-ai301-fa26-s3/pull/168

**Branch**

fix/72-verify-password-malformed-hash

## Eval iterations

**Run history**

1. Run 1 (full, 20 packages, saved with `--save-run`): `agreement: 15/20 scored items  (bar: 18/20: below the bar)`, with
   `categories: clear-accept 2/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3`.
   All five misses were clear-accepts my tool rejected: pkg-08, pkg-13, pkg-16 and pkg-19 on
   `repo-checks-run`, and pkg-05 on `decisive-evidence`.
2. Partial re-run after loosening those two checks, with the not-tested packages and a
   calibration trap as canaries (`--only pkg-05,pkg-08,pkg-13,pkg-16,pkg-19,pkg-04,pkg-07,pkg-10,pkg-14,calib-04 --include-calibration`):
   `agreement: 9/9 scored items`, and calib-04 still `reject`. Not scored.
3. Run 2 (full, confirming, saved with `--save-run eval-run.txt`):
   `agreement: 20/20 scored items  (bar: 18/20: PASS)`, with
   `categories: clear-accept 7/7  not-tested 4/4  silent-drift 4/4  standards-wall 2/2  unreviewable 3/3`.

Final score: 20/20, matching the agreement line in the committed `eval-run.txt`.

**Package analysis**

pkg-19 (mikefarah/yq, a YAML emitter fix). Gold label: `accept` ("terse but complete: one
emitter-state fix mirroring the double-quote writer, byte-identical round-trip shown with diff,
regression scenario added"). My tool in run 1: `reject`, on `repo-checks-run`. In run 2: `accept`.

The PR's test evidence re-ran the issue's input before and after; run 1's grader said
`decisive-evidence` passed: "The issue's input is re-run before (dedented `'`) and after
(`IDENTICAL` diff against source.yml)". The only failure was `repo-checks-run`: "Only the prose
claim "`go test ./...` passes (4108 tests)"; no result line or output is pasted." My first
version of that check required pasted output of the repo's suite, so a sentence naming the exact
command and its result did not count. That was stricter than the gold labels: four of the seven
clear-accepts (pkg-08, pkg-13, pkg-16, pkg-19) report the suite the same way, as a named command
plus a stated result. After I changed the check to accept a named check with a stated outcome, run 2's
evidence was "`go test ./...` passes (4108 tests)" and it passed, so pkg-19 was accepted, which
matches the gold label.

**Check rationale**

The `repo-checks-run` check, as it reads now in `tools/pr-precheck/rubric.md`:

> Evidence: The test-evidence section's record of the repo's own checks/suite (the commands the PR template or contributing guide lists)
>
> Pass condition: PASSES when the PR names at least one of the repo's own checks or its test suite by command or name AND states its outcome (pasted output, a result line, or a stated result such as "`go test ./...` passes (4108 tests)" or "`make functionaltest` passes, including the new case"), including an honestly reported failure or "no tests ran" with its reason. FAILS when no repo check or suite is named with an outcome at all, or the only claim is generic ("tests pass", "verified working") with no command or suite named.
>
> Weight: required

Why it reads this way: run 1's version said "PASSES when the PR shows the outcome of the repo's
own test suite or required checks (output or result lines) ... FAILS when no outcome of any repo
check is shown at all ("cargo test passes" with nothing shown counts as shown only if the result
line is pasted)". That cost me four clear-accepts. What this check is for is "did they run the
repo's own checks and say what happened", not "did they paste a log", so I changed it to require
the check to be named (by command or suite) with a stated outcome, and I kept honest failures and
"no tests ran" as passing. The fail case is now the vague claim with no check named ("tests
pass", "verified working"). I made the same change in `procedure.md`'s Check execution step 5. My
own PR uses this: `make test-unit` is reported with its real result (30 errors that also happen on
`main`, explained) instead of being hidden.

**Trade-offs**

This check now trusts a stated result without proof: "`cargo test` passes." counts. To make sure
that didn't let untested PRs through, I re-ran all four not-tested packages as canaries; pkg-04
is the clearest case. In run 2, its `repo-checks-run` now passes ("`cargo test` passes." names the repo's test command with a stated outcome),
but the package is still rejected, because `decisive-evidence` catches what matters: "no
before/after output for `bat Cargo.toml`". So the not-tested category stayed 4/4 and calib-04
stayed `reject`. The case I accept this check will miss on its own: a PR that names the suite and
claims it passes when it doesn't. My rubric relies on `decisive-evidence` and CI for that.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/pr-precheck/`.
