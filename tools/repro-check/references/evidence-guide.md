# Evidence guide: where proof lives in a reproduction package

This is the rubric's map. For every family, it says where to look in an
eval bundle and in live mode, and what good looks like there.

## Environment

- Where it lives: eval: the "Environment" line or block at the top of the
  "Candidate repro report"; the issue's target is in the "Issue" section
  (body, labels) and "Thread highlights" (e.g. "confirmed on main", "only
  on Windows", "release build only"). Live: the first lines of the student's
  repro draft; the target is in the live issue body and thread.
- What good looks like: OS/platform plus the project's version or commit,
  plus every component the issue or thread says changes the behavior
  (driver, backend, shell, build profile, dependency version). The tested
  version is the issue's target or newer; if it is not, the report says so
  in words ("I tested 1.5.3 because..."). A version that differs silently
  from the one the issue targets is not a sufficient record.

## Steps

- Where it lives: eval: the numbered steps and code blocks in the
  "Candidate repro report", plus any config or input file content it shows.
  Live: the same parts of the student's draft; public repo docs (README,
  SETUP.md) count as public resources a stranger can use.
- What good looks like: a stranger could start from a stated state (a
  clean install, a named commit, a config shown in full) and reach the
  trigger using only what is shown or public. Every input the issue's
  trigger needs is present. Nothing depends on a private repo, an
  unshared config, or "my usual setup". Length does not matter: three
  exact commands are enough.

## Behavior shown

- Where it lives: eval: pasted output, logs, error text, and described
  screenshots inside the "Candidate repro report"; compare them with the
  error/output quoted in the "Issue" section. Live: the pasted output in
  the draft, compared with the live issue body.
- What good looks like: the artifact shows the SAME symptom the issue
  reports (same error type or message, same wrong value, same crash or
  exit code), produced by the issue's own trigger. Watch for adjacent
  behavior: a changed input that yields a different error (syntax error,
  arg-validation error, unbound variable, compile error), an older
  version's different behavior, or output that shows the program working
  normally. A control run (same steps without the trigger) that behaves
  correctly strengthens the artifact but is not required.

## Honesty

- Where it lives: the sentences in the claim comment and repro report
  that say what happened ("reproduced", "confirmed", "root cause is",
  "Expected / Actual", "affects all versions"), next to the artifacts
  above.
- What good looks like: each claim points at an artifact that shows it,
  and "Actual" describes what the artifact displays. A cannot-reproduce is
  honest when it shows the real attempt and names what differed from the
  reporter's setup. Red flags: certainty words ("guaranteed", "100%",
  "verified") with no artifact, a diagnosis with no evidence, a claim about
  versions or platforms that were not tested, "expected" and "actual"
  swapped relative to what was shown.

## Comms

- Where it lives: eval: the "Candidate claim comment" read against the
  issue title/body; the "contribution policy" and "bug reports" lines of
  the repo-facts block read against both comments. Live: the student's
  claim draft against the live issue; CONTRIBUTING.md, AI_POLICY.md or
  similar, and the issue/PR templates in the repo.
- What good looks like: the claim names this issue's specifics and a
  concrete next step that is investigation, not a promised fix or date,
  and does not ask to have the issue reserved. For AI policy, treat the
  work as AI-assisted: if the repo requires disclosing AI use in issues
  or comments (or "all AI usage"), one of the comments says an AI
  assistant was used and how; a PR-only disclosure rule does not apply to
  comments; conditional policies (understand, test, human in the loop,
  own words) need no disclosure line; no policy means nothing to disclose.
