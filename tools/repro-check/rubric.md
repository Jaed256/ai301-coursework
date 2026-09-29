# Rubric: is this reproduction package ready to post?

<!--
Filled rubric for Unit 2. Every check reads the thing itself against the
issue (the artifact, the version, the words), never the write-up's shape.
"The issue's target" means the version/branch/platform the issue says is
affected (for example "latest", "main", a named release, "Windows only").
Course packages are AI-assisted work: every package is treated as if an AI
assistant helped write its comments.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| env-recorded | The repro report's environment record (OS/platform, the project's version or commit, and any component the issue says matters: driver, backend, build profile, shell, dependency version), read against the issue's target in the issue body and thread | PASSES when the report names the OS/platform AND the project's version or commit AND every component the issue itself says changes the behavior, AND the tested version is the issue's target (or newer), or the report explicitly says it tested a different version and why. FAILS when there is no environment record, when an issue-relevant component is missing (e.g. no driver on a driver-specific issue), or when an older/different version than the issue's target was tested without saying so. | required |
| steps-rerunnable | The repro report's steps and inputs (commands, config files, input data), read as a stranger with only public resources | PASSES when someone else could re-run the attempt from what the report shows: the exact commands or UI actions are shown, and every input/config they need is either included, publicly available, or described precisely enough to recreate (what it contains and the specific element that triggers the bug, e.g. "a valid `dependencies:` list plus an unrecognized `category:` section"), and any condition the issue requires (platform, flag, setting) is part of the steps. FAILS when a step depends on something private or unshared (a private repo, an unshown config, "our internal setup"), or when the steps are missing or vague ("set it up and run it") so nobody could repeat them. Terse is fine if it is complete. | required |
| behavior-matches | The artifacts in the repro report (pasted output, logs, error text, described screenshots) read against the behavior the issue describes, and the report's inputs read against the issue's trigger | PASSES when at least one shown artifact displays the same symptom the issue reports (same error type/message, same wrong output, same crash vs. no-crash) produced by the issue's own trigger (same syntax, input, or configuration, or a minimal version of it). For a cannot-reproduce, PASSES when the artifact shows the issue's trigger actually run and its real result. FAILS when there is no artifact at all, when the artifact shows a different behavior (e.g. a graceful validation or syntax error instead of the reported crash, a compile error instead of the reported runtime error, a terminal that stays alive when the issue reports a crash), or when the input was changed so it no longer exercises the issue's trigger. | required |
| honest-outcome | Every claim in the claim comment and repro report ("reproduced", "confirmed", a root cause, "affects all versions", expected vs. actual) read against the artifacts actually shown | PASSES when the central claims (reproduced / could not reproduce / a root cause / which versions or platforms are affected) are each backed by a shown artifact and the stated outcome matches what the artifact shows; a supporting side-observation stated in plain words without pasted output (for example a control run "without the flag it works") does not fail the check by itself, as long as the main reproduction is shown; an evidenced cannot-reproduce that names what differed from the reporter's setup PASSES. FAILS when the report claims more than it shows: "reproduced" over an artifact that does not show the bug, a root cause or "I verified" with no evidence shown, generalizing to versions or platforms not tested, or expected/actual that contradict the artifact. | required |
| claim-specific | The candidate claim comment, read against the issue title and body | PASSES when the claim names something specific to THIS issue (the symptom, error, component, file, or version) and states a concrete next step that is investigation or reproduction work the author can actually do. FAILS on a +1 / me-too / "can I work on this" with no intent, on boilerplate that would fit any issue, on requests to assign or reserve the issue with nothing specific, or when it promises a fix, a guaranteed result, or a delivery date. | required |
| conventions | The repo-facts block's contribution policy line (live: CONTRIBUTING.md, AI_POLICY.md, issue templates), read against the claim comment and repro report, treating the package as AI-assisted | FAILS when the policy requires AI use to be disclosed in issues or comments (or "all AI usage in any form") and neither comment contains a disclosure that names the AI assistance; FAILS when the policy bans AI-assisted contributions outright. PASSES when there is no AI policy, when the policy only sets conditions (understand/test/take responsibility, human in the loop, comments in your own words), or when disclosure is required only in pull requests (a comment is not a PR). PASSES when a required disclosure is present. | required |
| template-asks | The repo-facts "bug reports" line (the issue template's asks), read against the repro report | The report covers what the repo's bug template asks for (e.g. latest version checked, minimal steps, logs) | preferred |

## Verdict rule

Accept (ready to post) if and only if all six required checks (env-recorded,
steps-rerunnable, behavior-matches, honest-outcome, claim-specific,
conventions) grade `pass`. Any required `fail` holds the package. `unclear`
counts as `fail`, except in a live claim-only draft, where checks that need
the repro report are graded `unclear` with "not yet applicable: claim-only
draft" and are left out of the verdict (only claim-specific and conventions
decide it). The preferred check never changes the verdict.
