# Procedure: how this skill grades a plan package

## Read order

1. Read the issue first (title, body, labels). Write down, in one line,
   the behavior the issue reports and what the reporter expected.
2. Read the thread highlights next (live: the issue's comments). List
   every maintainer comment (OWNER, MEMBER, COLLABORATOR) that names a
   culprit, picks a direction, mentions an open PR, or asks someone to
   test something. Ignore bots. In live mode on Path Review, classmates'
   claim and repro comments are not maintainer direction.
3. Read the repo-facts block (live: CONTRIBUTING.md, AI_POLICY.md, issue
   and PR templates). Copy the contribution-policy wording about AI use
   word for word.
4. Read the repro evidence before the plan. For each numbered step and
   each control run, write one line: what was run, and what it showed.
   Mark which runs FAILED and which CONTROL runs behaved correctly, and
   note any step that says where the failure is raised (a stack trace,
   `--debug` output, a timing matrix). This list is what the diagnosis
   gets checked against, so it must exist before the plan is read.
   (Live mode: the repro evidence is the student's posted repro comment
   on the issue, or what the drafts quote from it.)
5. Read the candidate plan: Diagnosis, Scope, Files, Approach, Test plan,
   Risks/Unknowns, and any Deviations section.
6. Read the candidate plan comment last, the way a maintainer on the
   thread would read it.

## Evidence gathering

1. grounded-diagnosis: copy the plan's one-sentence cause. Next to it,
   put the step list from Read order step 4 and any maintainer-stated
   culprit from step 2.
2. bounded-scope: list every file and every Approach step. Tag each one
   "fix" (changes the reproduced behavior), "test" (tests that fix), or
   "extra" (anything else: rewrite, refactor, migration, upgrade, new
   option, new framework, CI change, docs not needed by the fix). Copy
   the not-in-scope line, if there is one.
3. executable: copy the file paths/function names the plan commits to,
   and the chosen approach. Copy any hedge words ("or", "maybe", "not
   sure", "somewhere", "whichever").
4. decisive-test: copy the test plan's expected result. Copy the repro's
   failing output it is supposed to replace.
5. honest-unknowns: copy every certainty phrase in the plan and comment
   ("root cause is", "definitely", "guaranteed", "PR up by", "this week")
   and the plan's stated risks/unknowns.
6. thread-and-conventions: put the maintainer list from Read order step
   2 next to the plan comment, and the AI-policy wording from step 3
   next to the plan comment.

## Check execution

1. Run the checks in rubric order: grounded-diagnosis, bounded-scope,
   executable, decisive-test, honest-unknowns, thread-and-conventions,
   then names-the-plan.
2. grounded-diagnosis: go through the step list one line at a time. If
   any control run behaves correctly while the blamed component is still
   in play, or any step shows the failure happening before/without the
   blamed component, grade fail and quote that step. If every step fits
   the cause, grade pass.
3. bounded-scope: if any item is tagged "extra" and is part of the work
   to be done (not listed as deferred or out of scope), grade fail and
   name it. Deferring a harder variant with a reason is not "extra".
4. executable: fail if there is no location at all (no file, function,
   or named code path in a named module), or if a hedge word leaves the
   approach undecided. A named code path plus a decided approach passes
   even if the exact function will be pinned while building.
5. decisive-test: pass only if the expected result is something that
   would look different on the unfixed code (specific output, value,
   exit code, or a named test that fails before and passes after).
6. honest-unknowns: fail if a certainty phrase claims more than the step
   list shows or promises a delivery date; otherwise pass.
7. thread-and-conventions: fail if a listed maintainer direction is not
   engaged by the comment, or if the AI-policy wording requires
   disclosure in issues/comments (or "all AI usage") and the comment has
   no disclosure. A PR-only disclosure rule and conditional rules
   (understand/test/human-written) do not require a disclosure line.
8. When the evidence a check needs is genuinely absent from the package
   (not just hard to find), grade it `unclear` and say what is missing.
   Do not guess.
9. A check may be graded from the notes gathered above without
   re-reading the whole package; re-read only the part a note points to
   if the note is ambiguous.
10. Every grade gets a one-line evidence quote or fact.

## Verdict assembly

1. Apply the rubric's verdict rule: accept only if all six required
   checks are `pass`.
2. Count any required `unclear` as `fail`.
3. Ignore the preferred check's grade for the verdict, but still report
   it.
4. In the readable summary, name the first required check that failed
   (in rubric order) as the deciding check and quote its evidence line.
5. Emit the JSON block exactly as SKILL.md specifies, with one entry per
   rubric check, and nothing after it.
