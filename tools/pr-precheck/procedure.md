# Procedure: how this tool grades a PR package

## Read order

1. Read the issue (title, body) and write one line: the behavior the
   issue reports.
2. Read the thread highlights (live: the issue's comments). List every
   maintainer (OWNER, MEMBER, COLLABORATOR) direction: a chosen approach, a
   question to answer, a requested change. In live mode on Path Review,
   classmates' comments and PRs are not maintainer direction.
3. Read the repo-facts block (live: `.github/PULL_REQUEST_TEMPLATE.md`,
   `docs/CONTRIBUTING.md`, any AI policy). Copy, word for word, every
   stated PR ask: required sections, checklist items, "closes #" rules,
   files every PR must include (changelog, whatsnew, docs, i18n), the
   checks to run, and the AI-use disclosure rule.
4. Read the plan BEFORE the PR: its scope (in / not in), files, approach,
   test plan, and deviation notes. Write two lists: "planned changes" and
   "planned test cases" (each failure mode the test plan names). Also note
   anything the plan explicitly defers. The plan comes first because every
   side-by-side check compares the PR to it.
5. Read the repro evidence inside the plan context and note the exact
   trigger (the failing input or command) and its failing output.
6. Read the diff hunk by hunk and the commit list (live: `git diff
   main...HEAD` and `git log main..HEAD --oneline`). Do NOT read the
   description yet, so the description cannot color the read of the diff.
7. Read the test evidence.
8. Read the PR title and description last.

## Evidence gathering

1. Diff vs plan: make a table with one row per hunk: file, what it
   changes, and which planned change or deviation note it belongs to
   ("none" if it belongs to neither). Then go through "planned changes"
   and mark each one present in the diff, deferred in a note, or missing.
2. Description vs diff: list every claim the description makes about what
   changed (including "exactly as planned", "no other changes", each
   Changes bullet). Next to each, write whether the diff shows it. Then
   list each functional hunk and whether the description mentions it.
3. Evidence vs test plan: for each item in "planned test cases", record
   whether the evidence re-runs that trigger, and copy the before and after
   output lines. Note if the evidence instead runs a control, an unchanged
   path, or only states a result in words.
4. Repo checks: copy the result lines of every repo check or suite run the
   evidence shows (pass, fail, "no tests ran").
5. Debris scan: list every hunk line that is a debug print/log, commented-
   out code, a dead function or unused helper, a stray TODO/FIXME, or pure
   formatting/import/whitespace churn on lines the fix does not need.
6. Standards: next to each stated ask from Read order step 3, write where
   the PR honors it (section filled, "Closes #N" present, required file in
   the diff, disclosure line present) or "missing". Next to each
   maintainer direction from step 2, write whether the PR engages it.
7. Shortfalls: copy every deviation note and every description sentence
   about something left out or changed.

## Check execution

1. Run the checks in rubric order: diff-matches-plan,
   description-faithful, decisive-evidence, repo-checks-run, clean-diff,
   standards-met, then honest-shortfalls.
2. diff-matches-plan: fail if any hunk row says "none" for a functional
   change, or any planned change is "missing" with no note. Quote the
   hunk or the missing item.
3. description-faithful: fail if any description claim has "diff does not
   show it", or any functional hunk is unmentioned while the description
   claims completeness. Quote the claim.
4. decisive-evidence: fail if the main trigger from the repro lacks a
   re-run with before and after output shown, or if any other planned test
   case is not mentioned at all and not deferred. An additional case
   reported with a specific observed result in words passes. A re-run of
   only a control or unchanged path is not a re-run.
5. repo-checks-run: pass if at least one of the repo's own checks or its
   suite is named (command or suite name) with a stated outcome, pasted or
   in words, even a failure honestly explained. Fail on generic "tests
   pass" with nothing named.
6. clean-diff: fail if the debris list is not empty; quote the first
   item.
7. standards-met: fail if any stated ask is "missing" or a maintainer
   direction is not engaged; quote the ask.
8. If the evidence a check needs is genuinely absent from the package (not
   hard to find, but not there), grade it `unclear` and say what is
   missing.
9. Grade from the gathered tables; re-read the package only where a table
   row is ambiguous.
10. Every grade gets one evidence line quoting the deciding fact.

## Verdict assembly

1. Apply the rubric's verdict rule: accept only if all six required checks
   are `pass`.
2. Count each required `unclear` as `fail`.
3. Report the preferred check, but never let it change the verdict.
4. Name the deciding check in the summary: the first failing required
   check in rubric order, with its evidence line. If none failed, say all
   required checks passed.
5. Emit the JSON block exactly as SKILL.md specifies, one entry per rubric
   check in rubric order, and nothing after it.
