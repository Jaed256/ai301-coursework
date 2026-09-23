# Unit 1 — Issue Selection

Path: `beat-1-sandbox/unit-1/selection.md`

Record of the issue carried into Unit 2, and of the evaluation runs that produced
`eval-run.txt`.

---

## Selected issue

**Issue link**

https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72

**Verdict output**

Live-mode run of my installed `issue-select` skill on three candidates (#72, #68, #69),
run on 2026-09-22. #72 is ranked first and its verdict is `accept`. Pasted verbatim:

````
## Summary

All three issues come from the scoped repo (`codepath/pathreview-ai301-fa26-s3`), so all are valid candidates. Repo-level facts apply to all three identically: **not-archived** (archived: false), **maintainer-alive** (commits by Aburke225, a human, dated 2026-09-16 — 6 days old), and **ai-policy** (CONTRIBUTING.md has no AI statement; silence passes) all pass. Per the Path Review house rule, classmates' claim comments are ignored for **unclaimed** — only assignees/open PRs count, and none exist on any of the three.

**Ranked (all three accept):**

1. **#72** — `verify_password` raises `UnknownHashError` instead of returning `False`. Smallest, most self-contained fix (1–2h, one function in `core/security.py`), matches the Python-backend fit profile cleanly, and has only one classmate claim.
2. **#68** — Keyword search `ZeroDivisionError` on empty index. Bounded fix in `rag/retriever/keyword_search.py` (2–4h), one classmate claim.
3. **#69** — Output parser crashes on top-level JSON array. Equally bounded (2–4h) in `rag/generator/output_parser.py`, but has two classmates already claiming it — more contention, though the house rule means it doesn't block taking it.

All three are bug reports opened by a maintainer (COLLABORATOR Aburke225) stating the wrong behavior, none are umbrella issues, and none show a maintainer requiring core-internals changes.

```json
[
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/72",
    "checks": [
      {"name": "not-archived", "grade": "pass", "evidence": "Repo facts: archived: false"},
      {"name": "maintainer-alive", "grade": "pass", "evidence": "Last commit 2026-09-16 by Aburke225 (human, COLLABORATOR), 6 days before capture date 2026-09-22"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: none; linked PRs: none; only comment is a classmate's claim (sseid4, NONE), ignored per Path Review house rule"},
      {"name": "bounded-scope", "grade": "pass", "evidence": "Single bug in core/security.py: catch UnknownHashError and return False, one covering test file"},
      {"name": "spec-settled", "grade": "pass", "evidence": "Bug report opened by maintainer (Aburke225, COLLABORATOR) stating exact wrong behavior vs. expected"},
      {"name": "ai-policy", "grade": "pass", "evidence": "docs/CONTRIBUTING.md (189 lines) contains no statement on AI; silence passes"},
      {"name": "friendly-label", "grade": "pass", "evidence": "Labels: bug, good first issue, api, tier-1"},
      {"name": "maintainer-filed", "grade": "pass", "evidence": "Opened by Aburke225, COLLABORATOR"},
      {"name": "responsive-maintainers", "grade": "unclear", "evidence": "No maintainer first-response sample given and no maintainer comment on this thread"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/68",
    "checks": [
      {"name": "not-archived", "grade": "pass", "evidence": "Repo facts: archived: false"},
      {"name": "maintainer-alive", "grade": "pass", "evidence": "Last commit 2026-09-16 by Aburke225 (human, COLLABORATOR), 6 days before capture date 2026-09-22"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: none; linked PRs: none; only comment is a classmate's claim (acordero4852, NONE), ignored per Path Review house rule"},
      {"name": "bounded-scope", "grade": "pass", "evidence": "Single bug in rag/retriever/keyword_search.py: index([]) should not raise ZeroDivisionError, one covering test file"},
      {"name": "spec-settled", "grade": "pass", "evidence": "Bug report opened by maintainer (Aburke225, COLLABORATOR) stating exact wrong behavior vs. expected (search() already handles it, index() should too)"},
      {"name": "ai-policy", "grade": "pass", "evidence": "docs/CONTRIBUTING.md contains no statement on AI; silence passes"},
      {"name": "friendly-label", "grade": "pass", "evidence": "Labels: bug, good first issue, rag, tier-1"},
      {"name": "maintainer-filed", "grade": "pass", "evidence": "Opened by Aburke225, COLLABORATOR"},
      {"name": "responsive-maintainers", "grade": "unclear", "evidence": "No maintainer first-response sample given and no maintainer comment on this thread"}
    ],
    "verdict": "accept"
  },
  {
    "item": "https://github.com/codepath/pathreview-ai301-fa26-s3/issues/69",
    "checks": [
      {"name": "not-archived", "grade": "pass", "evidence": "Repo facts: archived: false"},
      {"name": "maintainer-alive", "grade": "pass", "evidence": "Last commit 2026-09-16 by Aburke225 (human, COLLABORATOR), 6 days before capture date 2026-09-22"},
      {"name": "unclaimed", "grade": "pass", "evidence": "assignees: none; linked PRs: none; both comments are classmates' claims (Yina-Mu, tonybuii2003, both NONE), ignored per Path Review house rule"},
      {"name": "bounded-scope", "grade": "pass", "evidence": "Single bug in rag/generator/output_parser.py: fallback path calls .items() on a list, one covering test file"},
      {"name": "spec-settled", "grade": "pass", "evidence": "Bug report opened by maintainer (Aburke225, COLLABORATOR) stating the exact AttributeError and required fallback behavior"},
      {"name": "ai-policy", "grade": "pass", "evidence": "docs/CONTRIBUTING.md contains no statement on AI; silence passes"},
      {"name": "friendly-label", "grade": "pass", "evidence": "Labels: bug, good first issue, rag, tier-1"},
      {"name": "maintainer-filed", "grade": "pass", "evidence": "Opened by Aburke225, COLLABORATOR"},
      {"name": "responsive-maintainers", "grade": "unclear", "evidence": "No maintainer first-response sample given and no maintainer comment on this thread"}
    ],
    "verdict": "accept"
  }
]
```
````

---

## Eval iterations

**Run history**

1. Run 1 (full, 20 issues): `agreement: 18/20 scored items  (bar: 18/20: PASS)`, with
   `categories: claimed 4/4  clear-accept 6/8  dead-repo 3/3  policy 1/1  scope 4/4`.
   The two misses were both clear-accepts my rubric rejected on `bounded-scope`:
   issue-01 and issue-19.
2. Partial re-run after revising `bounded-scope` (`--only issue-01,issue-19,issue-05,issue-10`,
   the two misses plus two scope canaries): `agreement: 4/4 scored items`. Partial run, not scored.
3. Run 2 (full, confirming, saved with `--save-run eval-run.txt`):
   `agreement: 20/20 scored items  (bar: 18/20: PASS)`, with
   `categories: claimed 4/4  clear-accept 8/8  dead-repo 3/3  policy 1/1  scope 4/4`.

Final score: 20/20, matching the agreement line in the committed `eval-run.txt`.

**Issue analysis**

issue-09 (conda/conda#7617, "conda config clear option"). My rubric's decision: `accept`.
Gold label: `accept`. They agree.

This issue is the one most likely to trick a rubric, because it looks old and taken: it was
opened in 2018, it has a closed PR (`linked PRs: conda/conda#11627 (closed)`), and there is a
claim comment in the thread ("I'd like to take a swing at this as my first open-source
contribution", MesaJonathan, 2022-01-20). My rubric still accepted it because each of those
signals is covered by a threshold rather than by its presence:

- `unclaimed` only blocks on an assignee, an **open** PR, or a claim comment "dated within 90
  days before the capture date". The grader's evidence line was: "assignees: none; linked PR
  #11627 closed; only claim comment (2022-01-20) is stale, well outside 90 days before
  2026-08-05 capture".
- `bounded-scope` clause (3) needs "2 or more earlier PRs ... closed unmerged AND the issue has
  been open more than 12 months". One closed PR is not a pattern of failed attempts, so it
  passed ("Single small feature (`conda config --clear`) with only 1 prior closed PR").
- `spec-settled` passed because the opener, jakirkham, is a MEMBER and the body gives the exact
  desired behavior (`conda config --clear channels` producing `channels: []`).
- `maintainer-alive` passed on the 2026-08-04 human commits.

The only thing that failed was a preferred check (`responsive-maintainers`), which never changes
the verdict. The contrast case is issue-15 (zulip), which has two closed PRs and years of
design discussion, and my rubric rejects it on the same clause (3) that lets issue-09 through.

**Check rationale**

The `bounded-scope` check, as currently written in `tools/issue-select/rubric.md`:

> Evidence: The issue title, body, and full comment thread
>
> Pass condition: FAILS if any of: (1) the issue is an umbrella: it calls itself a tracking/umbrella/meta/"megaissue", or it asks for the same kind of change repeated across many independent modules/files codebase-wide (e.g. "add type hints across the codebase") so that it is meant to be split into many separate PRs by different people. Counting bullet points is NOT the test: one bug with several listed causes, one feature's docs touching a few named pages, or a body with optional "additional suggestions" is still ONE bounded piece of work (optional suggestions are not required scope); (2) the thread shows the design or approach has been debated across 3+ maintainer/contributor exchanges or 12+ months without a maintainer stating the final spec; (3) 2 or more earlier PRs for it were closed unmerged AND the issue has been open more than 12 months; (4) it is a usage/support question; (5) a maintainer says the fix requires changes to core internals. Otherwise PASSES: one bug, one docs page/section, or one small feature. A terse body or missing repro steps is NOT a fail.
>
> Weight: required

Why it is written this way: in run 1 this check's first clause only said an issue fails if it
is "an umbrella, tracking, or \"megaissue\" list of many sub-items meant to be split across
separate PRs". The grader read that as "count the bullet points" and rejected two good issues:
issue-01 because its body "lists 5 separate sub-items (new page + updates to 3 other docs
pages...)", and issue-19 because it listed two causes plus "Additional suggestions". Both are one
coherent piece of work. So I rewrote clause (1) so that an umbrella is something that calls
itself tracking/umbrella/megaissue, or asks for the same change across many independent modules
codebase-wide, and I added the explicit line "Counting bullet points is NOT the test" with the
exact shapes that fooled it (several causes, one feature's docs on a few pages, optional
suggestions). Clauses (2) and (3) use numbers (3+ exchanges or 12+ months of debate; 2+ closed
PRs and open 12+ months) so an old issue with one failed attempt, like issue-09, is not
punished.

**Trade-offs**

Loosening clause (1) risks letting a real umbrella through, so I re-ran the two scope canaries
together with the fixes: `--only issue-01,issue-19,issue-05,issue-10`. issue-05 (sympy
type-annotation sweep) and issue-10 (tldr "megaissue") both still rejected, and issue-01 and
issue-19 flipped to accept (4/4). The confirming full run then showed nothing else moved: 20/20,
scope 4/4. The case I accept this check will miss: a large issue whose body is a single
well-written feature but that secretly needs a big design (no self-description as an umbrella,
no thread debate yet, no failed PRs). My rubric would call that bounded; only `spec-settled`
or a human read would catch it.

---

## Selection rationale

<!-- DRAFT written with Claude. Reword these three answers in your own words before submitting. -->

**Selection rationale**

1. Fit and time: #72 is a Python backend bug in one function (`verify_password` in
   `core/security.py`) with one covering test, and the issue estimates 1–2 hours. Python is
   the language I am most comfortable in, and a small, security-related fix is something I
   can reproduce, test, and finish alongside my other classes before Unit 2's deadlines.
2. What the verdict got right, and what I weighed that it could not: the skill correctly saw
   a maintainer-filed bug in an active repo with no assignee, no open PR, and no AI ban, and it
   correctly ignored the classmate's claim comment under the Path Review house rule. What the
   rubric cannot judge is whether the fix is really as small as it looks: catching
   `UnknownHashError` is easy, but I also want to check whether passlib can raise other errors
   (for example `ValueError`) on bad hashes, and whether "fail closed" should also log
   something. I also looked at the code myself: `verify_password` is currently a one-line
   `pwd_context.verify(...)` call, which confirms the scope.
3. Anticipated difficulty in claiming it: another student (sseid4) already posted a claim on
   2026-09-22, and two students reference it from their coursework repos. The house rule says
   that does not block me, but my claim comment will need to acknowledge the other claim
   politely and say what I will do (reproduce first, then a PR with the xfail marker removed and
   green CI). Setting up the project locally (Docker/Makefile, all five CI jobs) may be the
   harder part.

---

Related paths: `eval-run.txt` in this directory; your skill's files in
`tools/issue-select/`.
