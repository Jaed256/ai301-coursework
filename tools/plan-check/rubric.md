# Rubric: is this plan ready to post and build from?

<!--
Filled rubric for Unit 3. Every check reads the plan against the issue,
the thread, and the repro evidence, never against its formatting.
"Maintainer" means a commenter tagged OWNER, MEMBER, or COLLABORATOR.
Course packages are AI-assisted work: every plan and plan comment is
treated as if an AI assistant helped write it.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| grounded-diagnosis | The plan's stated cause (Diagnosis section and the comment's cause claim) read against every step and control run in the repro evidence block, and against any cause a maintainer stated in the thread | PASSES when the stated cause is consistent with ALL of the repro evidence: it explains the failing run AND the control runs (a control that behaves correctly with the blamed component still in play rules that component out), and it does not contradict a step that shows where the failure happens (e.g. "--debug shows the error is raised before X runs" rules out X). FAILS when any repro step or control run contradicts the blamed component, when the plan dismisses part of the evidence as a "red herring" without showing why, or when the cause is asserted with no link to the evidence. | required |
| bounded-scope | The plan's scope statement (in-scope / not-in-scope), its Files list, and its Approach steps, read against what the issue and repro evidence actually ask for | PASSES when the plan makes one bounded change that addresses the reproduced behavior: the files and steps all serve that fix (plus its tests), and anything larger is explicitly deferred or split out. A plan that deliberately scopes DOWN (fixes the confirmed case, defers a harder variant with a stated reason) PASSES. FAILS when the plan adds work the issue never asked for alongside the fix: rewrites, refactors, migrations, dependency upgrades, new options/settings, new frameworks, CI matrix changes, or "while I'm in there" cleanups, even if the core fix inside is correct. | required |
| executable | The plan's Files list and Approach steps | PASSES when a stranger could start the work without asking the author anything: the plan names where the change goes (specific file(s) or function(s), or a specific code path inside named modules/crates, e.g. "the client reattach path in zellij-client") and commits to one chosen approach. Pinning the exact function during the build is fine when the location and the approach are both decided. FAILS when files are missing or vague ("somewhere in the input stack"), when the approach is still undecided ("X or Y, whichever is easier", "not sure which layer"), or when the work is only "investigate / profile / poke around" with no change decided. | required |
| decisive-test | The plan's Test plan read against the repro evidence's steps and observed output | PASSES when the test plan names an observable result that distinguishes fixed from broken for THIS bug: re-running the repro trigger (or a test built from it) and stating the specific output, value, or exit code expected after the fix. FAILS when the test is only "run the full test suite", "should feel fast", "nothing else should break", or any outcome that would look the same before and after the fix. | required |
| honest-unknowns | The plan's risks/unknowns statements and the plan comment's certainty words ("root cause is", "guaranteed", "will have a PR up by"), read against what the repro evidence shows | PASSES when claims stay within what the evidence shows and real unknowns are named as unknowns (or the plan is small enough to have none). FAILS when the plan or comment states a cause, outcome, or delivery as certain that the evidence does not establish, or promises a PR by a date. | required |
| thread-and-conventions | The plan comment read against the thread highlights (especially maintainer comments: a stated culprit, a chosen direction, an open PR, a request to test) and against the repo-facts contribution policy, treating the package as AI-assisted | FAILS when (a) a maintainer in the thread has stated a culprit, direction, or request and the plan comment ignores it or goes another way without saying why; OR (b) the repo's policy requires AI use to be disclosed in issues/comments (or "all AI usage in any form") and the comment has no disclosure naming the AI assistance; OR (c) the policy bans AI-assisted contributions outright. PASSES otherwise: no maintainer direction to engage, or the comment engages it (follows it, builds on an open PR, or explains a different path); and the policy is absent, conditional (understand/test/take responsibility, human-written comments), PR-only for disclosure, or satisfied by a disclosure line. | required |
| names-the-plan | The plan comment read on its own | The comment states the cause, the file(s) it will change, and how the fix will be tested, so a reader of the thread alone knows the plan | preferred |

## Verdict rule

Accept (ready to post and build) if and only if all six required checks
(grounded-diagnosis, bounded-scope, executable, decisive-test,
honest-unknowns, thread-and-conventions) grade `pass`. Any required `fail`
holds the package. `unclear` on a required check counts as `fail`: a plan
that cannot be verified from the package is not ready. The preferred check
never changes the verdict.
