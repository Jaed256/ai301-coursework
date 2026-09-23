# Rubric: is this a good first issue?

<!--
Filled rubric for Unit 1. Every recency threshold is measured against the
bundle's capture date in eval mode, and against today in live mode.
"Maintainer" means a commenter or opener tagged OWNER, MEMBER, or
COLLABORATOR. Bots (usernames ending in [bot], or zulipbot-style
automation accounts) are never maintainers and never claimants.
-->

## Checks

| Check | Evidence | Pass condition | Weight |
|---|---|---|---|
| not-archived | The `archived:` value on the repo line of Repo facts (live: the "This repository has been archived" banner on the repo front page) | `archived: no` (no archived banner). Any archived repo fails, however good the issue. | required |
| maintainer-alive | The "last 5 default-branch commits" list under Repo facts: each commit's date and author (live: the commit history on the default branch) | At least one of the listed commits is dated within 180 days of the capture date AND is human work: authored by a non-bot account, or a bot (e.g. `kubernetes-prow[bot]`) merging a human's pull request. Commits that are only automated dependency bumps or bot-generated chores do not count. Missing releases do not matter here. | required |
| unclaimed | "this issue: assignees:" and "linked PRs:" under Repo facts, plus every comment in the Comments section (PR links, "I'll take this", "working on this", "can I work on this") | ALL of: (a) assignees is `none`; (b) no linked or thread-mentioned PR is in state `open`; (c) no non-maintainer, non-bot comment claiming the work is dated within 90 days before the capture date. Closed or merged PRs, and claim comments older than 90 days with no open PR, are stale and do not block. Live mode in Path Review: other students' claim comments are ignored per the house rule in scope.md; assignees and open PRs still count. | required |
| bounded-scope | The issue title, body, and full comment thread | FAILS if any of: (1) the issue is an umbrella: it calls itself a tracking/umbrella/meta/"megaissue", or it asks for the same kind of change repeated across many independent modules/files codebase-wide (e.g. "add type hints across the codebase") so that it is meant to be split into many separate PRs by different people. Counting bullet points is NOT the test: one bug with several listed causes, one feature's docs touching a few named pages, or a body with optional "additional suggestions" is still ONE bounded piece of work (optional suggestions are not required scope); (2) the thread shows the design or approach has been debated across 3+ maintainer/contributor exchanges or 12+ months without a maintainer stating the final spec; (3) 2 or more earlier PRs for it were closed unmerged AND the issue has been open more than 12 months; (4) it is a usage/support question; (5) a maintainer says the fix requires changes to core internals. Otherwise PASSES: one bug, one docs page/section, or one small feature. A terse body or missing repro steps is NOT a fail. | required |
| spec-settled | The issue type/labels, the opener's author_association, maintainer comments, and the body's stated solution | Bug reports and documentation tasks PASS if the body states the wrong behavior or the missing content. Feature/enhancement requests PASS only if (a) the opener is a maintainer, OR a maintainer commented approving it, inviting a contributor, or giving implementation direction; AND (b) the body contains no open product decision (e.g. an asset, name, or behavior marked "TBD" or left for the maintainers to choose). A feature request opened by a non-maintainer or a bot with zero maintainer comments FAILS. | required |
| ai-policy | The "contribution policy" line under Repo facts (live: CONTRIBUTING.md, linked contributor docs, AI_POLICY.md) | FAILS only on an outright ban on AI-generated or AI-assisted code or documentation (e.g. "We do not accept AI-generated code"). Conditions (disclose AI use, personally understand/test/explain the change, human review before submitting, low-effort AI PRs get closed) PASS. No CONTRIBUTING file or no statement on AI PASSES. | required |
| friendly-label | The issue's labels | Carries `good first issue`, `easy`, `help wanted`, or an equivalent newcomer label | preferred |
| maintainer-filed | The opener's author_association on the issue | Opened by an OWNER, MEMBER, or COLLABORATOR (a maintainer defined the task) | preferred |
| responsive-maintainers | The "maintainer first-response sample" under Repo facts, and maintainer comments in this thread | At least one sampled issue got a maintainer reply within 7 days, or a maintainer commented on this issue within the last 90 days | preferred |

## Verdict rule

Accept if and only if all six required checks (not-archived,
maintainer-alive, unclaimed, bounded-scope, spec-settled, ai-policy) grade
`pass`. Any required `fail` rejects. `unclear` on a required check counts as
`fail`, except for ai-policy, where missing policy information counts as
`pass` (silence is not a restriction). Preferred checks never change the
verdict; they only rank issues that are already accepted.
