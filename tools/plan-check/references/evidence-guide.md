# Evidence guide: where evidence lives in a plan package

## Diagnosis and grounding

- Where it lives: eval: the "Diagnosis" part of the "Candidate plan" and
  any cause stated in the "Candidate plan comment"; the behavior it must
  explain is in the "Repro evidence" block (numbered steps, control runs,
  `--debug`/trace output, "Actual") and in maintainer lines of "Thread
  highlights". Live: the draft plan.md's diagnosis; the student's posted
  repro comment on the issue; maintainer comments on the live thread.
- What good looks like: the stated cause explains the failing run AND
  every control run. If a control run with the blamed component still
  present behaves correctly, or a trace shows the failure raised before
  that component runs, the cause is wrong. A cause that matches a
  maintainer's stated culprit, or cites the repro step that pins it, is
  grounded.

## Scope

- Where it lives: eval: the plan's "Scope" (in-scope / not-in-scope),
  "Files", and "Approach" parts. Live: the same sections of plan.md.
- What good looks like: every file and step serves one fix for the
  reproduced behavior plus its tests; bigger ideas appear only as
  deferred or not-in-scope. A drive-by rewrite looks like extra fronts
  (refactor, migration, dependency upgrade, new option or setting, CI
  matrix, harness change) bundled with the fix.

## Executability

- Where it lives: eval: the plan's "Files" and "Approach" parts. Live:
  the same in plan.md.
- What good looks like: named file paths (and ideally the function) and
  one chosen approach, in an order someone else could start on today. Not
  good: "somewhere in", "X or Y, whichever is easier", "not sure which
  layer", "profile and see".

## Test plan

- Where it lives: eval: the plan's "Test plan", read next to the repro
  evidence's steps and observed output. Live: plan.md's test plan next
  to the student's posted repro comment.
- What good looks like: the repro trigger (or a test built from it) is
  re-run and the plan says what it should show after the fix: a specific
  output, value, exit code, or a named test going from failing to
  passing. A vague test ("run the suite", "should feel fast") would look
  the same before and after the fix.

## Honesty

- Where it lives: eval: certainty words in the plan and comment ("root
  cause is", "red herring", "guaranteed", "PR up this week") and any
  "Risks"/"Unknowns" part. Live: the same in plan.md and comment.md, plus
  the "Deviations" section after the build.
- What good looks like: claims go no further than the repro shows; real
  unknowns are named as unknowns; no delivery dates. After a build, any
  difference from the posted plan is written under Deviations with the
  reason.

## Comms

- Where it lives: eval: the "Candidate plan comment" read against the
  maintainer lines in "Thread highlights" and against the repo-facts
  "contribution policy" and "bug reports" lines. Live: comment.md
  against the live issue thread and the repo's CONTRIBUTING.md /
  AI_POLICY.md.
- What good looks like: the comment engages what maintainers already
  said (a culprit, a chosen direction, an open PR, a request to test),
  either following it or explaining why not. Treat the work as
  AI-assisted: if the policy requires disclosing AI use in issues or
  comments (or "all AI usage"), the comment includes a disclosure line;
  PR-only disclosure rules and conditional policies need none; no policy
  needs none.
