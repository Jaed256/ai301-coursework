# Evidence guide: where evidence lives in a PR package

## Plan fidelity (harness category: silent-drift)

- Where it lives: eval: the plan-context block's scope (in / not in
  scope), files, approach, and any deviation notes; the candidate PR's
  unified diff and commit list; the description's claims about what
  changed. Live: `plan.md` (scope, files, approach, `## Deviations`), the
  output of `git diff main...HEAD`, and the description in `pr_draft.md`.
- What good looks like: every changed file and hunk falls inside the
  plan's approach or a deviation note, and every planned change is in the
  diff or deferred in a note. The description's claims ("implements the
  plan", "no other changes", each Changes bullet) are each true of the
  diff. Drift in either direction fails: extra unannounced work (a new
  option, a rename pass, a rewrite of a file the plan never named), or a
  description claiming a change the diff does not contain.

## Test evidence (harness category: not-tested)

- Where it lives: eval: the candidate PR's test-evidence section, read
  next to the plan's test plan and the repro evidence's trigger. Live:
  `test_evidence.md` (and the Testing section of `pr_draft.md`), next to
  `plan.md`'s test plan and the posted repro comment.
- What good looks like: the issue's own failing trigger re-run with the
  output shown before (failing) and after (fixed), matching the plan's
  expected result, for every failure mode the test plan names; plus the
  repo's own checks with their result lines visible (an honestly reported
  failure or "no tests ran" still counts). Not good: "tested locally",
  "works on my machine", "tests pass" with nothing shown, or a re-run of a
  control or an unchanged path.

## Diff quality (harness category: unreviewable)

- Where it lives: eval: the candidate PR's unified diff and commit list.
  Live: `git diff main...HEAD` and `git log main..HEAD --oneline`.
- What good looks like: only hunks that make the fix or test it. Debris
  tells: debug prints or logs, commented-out code or earlier attempts,
  dead or unused functions, stray TODO/FIXME, import reordering, re-indent
  or whitespace churn on lines the fix does not need, unrelated edits.

## Standards and comms (harness category: standards-wall)

- Where it lives: eval: the repo-facts block's PR-template asks,
  contributing instructions, and AI-use policy; the thread highlights'
  maintainer direction; the description and diff where each ask should be
  honored. Live: `.github/PULL_REQUEST_TEMPLATE.md`, `docs/CONTRIBUTING.md`,
  the issue thread, and `pr_draft.md`.
- What good looks like: every section the template requires is filled
  with real content, "Closes #N" is present when asked, any file every PR
  must include (changelog, whatsnew, docs, i18n) is in the diff, and when
  the policy requires AI-use disclosure the description says how AI was
  used. Explicit maintainer direction is engaged. Not good: a required
  checklist or link missing, a required file absent, no disclosure under a
  disclosure policy.
