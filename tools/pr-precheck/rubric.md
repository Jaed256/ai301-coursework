# Rubric: is this pull request ready to submit?

<!--
Filled rubric for Unit 4. Every check reads the PR's artifacts against
the plan (including its deviation notes), the issue, and the repo's stated
standards, never the formatting. Course packages are AI-assisted work:
every PR is treated as if an AI assistant helped write it. An honestly
disclosed shortfall (stated in the plan's notes AND in the description)
is not drift.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| diff-matches-plan | Every hunk of the diff (files and what each hunk changes) read against the plan's in-scope / not-in-scope statements, its files list, its approach, and its deviation notes | PASSES when every change in the diff is part of the plan's approach or is explained in a deviation note, AND every change the plan commits to is in the diff or is explicitly deferred in a deviation note. FAILS when the diff adds something the plan did not name (a new option/flag/setting, a rewrite or rename pass, an unrelated file, a change to something the plan scoped out) without a deviation note, or when the diff is missing a planned change with no note saying so. | required |
| description-faithful | Each claim in the PR description ("implements the plan exactly", "no other changes", "the docs now document X", the Changes bullets) read against the diff | PASSES when every claim the description makes about what changed is true of the diff, and every functional change in the diff is mentioned or covered by the description. FAILS when the description claims a change the diff does not contain, claims fidelity the diff contradicts ("exactly as planned", "no functional changes outside X") while the diff does more, or omits a functional change. | required |
| decisive-evidence | The PR's test-evidence section read against the plan's test plan and the reproduction's trigger (the failing case from the repro evidence) | PASSES when the evidence re-runs the issue's own trigger (the main failing case from the repro, not a control or an unchanged path) and shows its observed output before AND after the fix, with the after matching the plan's expected result. When the plan's test plan names more than one case, the main trigger needs shown output; each additional case must be re-run with its observed result stated (a specific observed result in words, e.g. "same-key redefine prints the one-time warning", is enough for an additional case) or explicitly deferred. FAILS when the main trigger's before/after output is not shown (only "tested locally", "works on my machine", "tests pass"), when only a control or an unchanged path is re-run, or when a planned case is silently skipped (not mentioned at all). | required |
| repo-checks-run | The test-evidence section's record of the repo's own checks/suite (the commands the PR template or contributing guide lists) | PASSES when the PR names at least one of the repo's own checks or its test suite by command or name AND states its outcome (pasted output, a result line, or a stated result such as "`go test ./...` passes (4108 tests)" or "`make functionaltest` passes, including the new case"), including an honestly reported failure or "no tests ran" with its reason. FAILS when no repo check or suite is named with an outcome at all, or the only claim is generic ("tests pass", "verified working") with no command or suite named. | required |
| clean-diff | The unified diff's hunks and the commit list | PASSES when every hunk serves the fix or its tests. FAILS when the diff contains debris: debug prints/logging left in, commented-out code or earlier attempts, dead functions or unused helpers, TODO/FIXME left on unrelated lines, or formatting/import/whitespace churn in lines the fix does not touch. Commit message style alone does not fail this check. | required |
| standards-met | The repo-facts block's PR-template asks, contributing instructions, and AI-use policy (live: `.github/PULL_REQUEST_TEMPLATE.md`, `docs/CONTRIBUTING.md`) read against the PR description and diff, plus explicit maintainer direction in the thread | FAILS when a stated required ask is visibly ignored: a required section or checklist missing, a required "closes #N" link missing, a required file (changelog/whatsnew entry, docs, i18n) the template says every PR must include absent from the diff; OR the policy requires disclosing AI use and the description has no disclosure naming the AI assistance; OR an explicit maintainer direction in the thread is ignored without explanation. PASSES when every stated ask is honored or the repo states none. | required |
| honest-shortfalls | The plan's deviation notes and the description's statements about anything left out or changed | Anything deferred or changed from the plan is stated in both the plan's notes and the description, with a reason | preferred |

## Verdict rule

Accept (ready to submit) if and only if all six required checks
(diff-matches-plan, description-faithful, decisive-evidence,
repo-checks-run, clean-diff, standards-met) grade `pass`. Any required
`fail` holds the package. `unclear` on a required check counts as `fail`.
The preferred check never changes the verdict. A disclosed shortfall that
the plan's deviation notes and the description both state is not a fail
of any check.
