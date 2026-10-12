---
name: pr-precheck
description: Grade a PR package (a candidate pull request read against the plan it claims to implement and the issue that plan belongs to) and decide whether it is ready to submit. Use when checking your own branch, draft PR title, and description before opening the pull request, or when grading an eval package bundle.
---

# pr-precheck: rubric-driven PR grading

## The question

Answer exactly one question about exactly one PR package: is this pull
request ready to submit? A PR package is a candidate pull request (its
title, description, commit list, diff, and test evidence), read against
the plan it claims to implement (including that plan's deviation notes)
and the issue the plan belongs to, plus the repo's stated standards (PR
template, contributing guide, AI-use policy). Do not grade the plan, the
issue, or the code's general quality on their own; grade only whether this
PR, as it stands, is ready to go out. Grade one package per run, and never
from gut feel: execute the components in this directory.

## Inputs and modes

Run in exactly one of two modes.

**Live mode** (the student's own PR, before it is opened). Read these
inputs, and only these:

1. `plan.md` in the working copy, including its `## Deviations` section.
2. The branch's diff: everything the branch changes relative to `main`,
   produced by running `git diff main...HEAD` (three dots) from the
   working copy. Also read `git log main..HEAD --oneline` for the commit
   list. Only committed changes count; uncommitted edits and untracked
   files (plan.md, comment.md, pr_draft.md, test_evidence.md) are not part
   of the PR.
3. The draft PR title and description in `pr_draft.md` (title on the
   first line, description below it), or the files the student names.
4. The test evidence in `test_evidence.md`, or the file the student names.
5. The issue's live thread on GitHub (via `gh`, the GitHub API, or the
   web), the repo's PR template (`.github/PULL_REQUEST_TEMPLATE.md`), and
   `docs/CONTRIBUTING.md` (and any AI policy it links). If the network
   cannot reach GitHub, use evidence the student provides from the live
   pages and say so in the summary.

A house-chain student reads the house plan and the house repro pack in
place of their own plan and repro; the same checks grade the same things.

**Eval mode** (a package bundle). The bundle text is the whole world:
take every fact from it, fetch nothing, read no other file. Always grade a
complete package: every check in the rubric, with the full verdict rule.

## The scope seam (live mode only)

In live mode, read `scope.md` before anything else. Confirm the issue and
the PR's target repo are the repo on its `Repo:` line; if they are not,
refuse to grade and say which repo is in scope. Apply the house rules it
states when reading evidence (for example, a classmate's PR or claim on
the same issue does not block this one). If the `Repo:` line still
carries a bracketed placeholder, stop without grading and tell the
student to fill the `Repo:` line in `scope.md` with their section's Path
Review repo; never guess a scope. In eval mode, ignore `scope.md`
entirely.

## The voice seam (live mode only)

In live mode, read `voice-guide.md` and hold the outgoing PR text (the
title and the description) against its rules. In the summary, list each
rule the draft breaks, quoting the rule and the offending line. The voice
guide never changes the verdict on its own; only rubric checks decide the
verdict. In eval mode, ignore `voice-guide.md` entirely.

## Component reads

Read `rubric.md` for the checks and the verdict rule. Read
`references/evidence-guide.md` for where each evidence family lives in a
package (and, in live mode, in the working copy and on GitHub) and what
good looks like. Execute `procedure.md` as written, stage by stage, to
decide the read order, the evidence gathering, how each check runs, and
how the verdict is assembled. Where the procedure is silent on a step,
say so in the summary as a procedure gap and grade from the rubric's
stated conditions; never invent a new check or step. If `rubric.md` has
no checks or no verdict rule, or `procedure.md` has no steps, refuse to
grade and say which file is empty.

## Verdict and output

The verdict is binary: `accept` means ready to submit, `reject` means
hold. There is no third verdict and no score; reservations go in check
evidence lines. You may print a short readable summary first (one line
per check, plus the deciding check and, in live mode, voice-guide notes).
End the reply with the fenced JSON block below, valid, with one entry per
rubric check, and nothing after it:

```json
{
  "item": "<PR URL or bundle id>",
  "checks": [
    {"name": "<check name>", "grade": "pass|fail|unclear",
     "evidence": "<one line: the fact or quote that decided it>"}
  ],
  "verdict": "accept|reject"
}
```

## Grading discipline

- Evidence first: grade no check without naming the fact or quote that
  decided it. "Looks fine" is not evidence.
- Grade the thing, not the polish: read the diff, the evidence, and the
  description themselves against the plan, the issue, and the stated
  standards. A terse complete PR can be ready; a confident polished one
  can hide drift.
- The rubric decides: if a check passes by its stated condition but feels
  wrong, it still passes; note the tension in the summary.
- The procedure decides how: follow `procedure.md` as written and report
  its gaps.
- Unclear: treat `unclear` as the rubric's verdict rule says; where it is
  silent, `unclear` counts as `fail`, because a claim you cannot verify
  from the package is not ready to submit.
