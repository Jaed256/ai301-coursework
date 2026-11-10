# Contribution: GNOme composition parser silently ignores any text that is not `Symbol(count)` (tacular #54)

Path: `beat-2-contribution/contribution-story.md`

This file is Assignment 5, and it is graded at the path above; a copy kept anywhere else in
the repository is not read. **Keep every section header below under its exact name** — the
format criteria grade exactly that. Your course repo link is the whole submission: no tool
files and no evidence uploads are part of Assignment 5.

**Contribution Number:** [1]
**Student:** Jaed Pizarro
**GitHub Username:** Jaed256
**Issue:** https://github.com/tacular-omics/tacular/issues/54
**Pull Request:** [GitHub PR link, once opened]

One repository, built across units 5 to 8, telling the whole story of your real
contribution. Fill it as you work, not at the end — the sections below ask for evidence that
is far easier to write down as it happens than to reconstruct in the last weekend.

Contribution Number stays `[1]` unless you make more than one contribution this term; it
just numbers them.

---

## Toolkit

Every story section below, from Why I Chose This Issue through Pull
Request, ends with a Toolkit block: which tool did that phase's work
and how you used it, what it produced, what difficulties or
shortcomings it presented, and what you changed or would change
because of them. Together the blocks are the thread the rubric's
Toolkit points read, and they are the raw material for your unit-9
portfolio, so keep them current as you work rather than
reconstructing them later.

A Toolkit block looks like this (copy it into each story section and
fill it there):

```text
**Toolkit**

- Tool: [which tool did this phase's work, and how you used it]
- What it produced: [described in a sentence or a short excerpt]
- Difficulties: [what fought you or fell short, or "none met"]
- What changed: [the patch, revision, or workaround, what you would
  update, or "nothing"]
```

This section also keeps one short dated entry per week of tool work,
starting below: what you built or changed that week, what broke, and
what you changed because of it. The blocks tell the story where it
happened; these entries are the toolkit's own timeline.

All four entries are graded. Keep every heading below, and fill each
one in the week it belongs to.

**Unit 5: built issue-scout (3 Python search queries + ranking filters) and patched its rubric after the first wild scan**

I turned my Unit 1 issue-select skill into issue-scout. It runs three Python `good first issue`/`help wanted` searches from my fit profile, drops results that are assigned, stale (over 60 days), over 10 comments, or tagged frontend/question, ranks by fewest comments then newest, and grades the top 5 with my rubric. The first scan (2026-11-10) accepted all 5, but two came from a workshop exercise repo created that day with 0 stars, and Pycord passed even though its policy says AI use by new contributors "will likely result in the direct closure" of the PR. I added a required `established-repo` check, tightened `ai-policy` to fail "discouraged for newcomers" policies, and dropped `workshop`/`exercise` labels in the strategy. The Unit 1 eval still scored 20/20 after the changes. Ranking still favors brand-new tiny repos, because search results carry no star count; I logged that in scan-notes.md rather than pretend it's fixed.

**Unit 6: installed the voice guard with upstream rules and wrote a CLAUDE.local.md for tacular**

I added an "Upstream: how I talk to maintainers" section to my voice guide, with rules for claims, questions and pings, and wired check_voice.py in as a PreToolUse hook. The wiring test stopped `gh issue comment --help` as expected. On the real claim the guard caught me: my first draft said "Then I'll add the full-string check...", which is a fix promise before I'd reproduced anything, so I cut it down to the first step and a two-day report-back. `gh` wasn't authenticated in my environment, so I posted through the browser and ran `check_voice.py --draft` by hand first. The guard log has both the stop and the pass. tacular already ships a 300-line CLAUDE.md and an AGENTS.md that points to it, so I dropped the @AGENTS.md import and kept my CLAUDE.local.md to the conventions and the one subsystem I'm touching. I kept it out of my diff with .git/info/exclude.

**Unit 7: [one line on the tool work]**

[2-4 sentences: the ship-gate and your gate manifest. What it produced
on your real branch, what fought you, what changed because of it.]

**Unit 8: [one line on the tool work]**

[2-4 sentences: the PR-monitor and your playbook. What it read, what it
drafted, what you did with the draft.]

---

## Why I Chose This Issue

My first choice was pantsbuild/pants #15376, one of the picks the scout accepted. When I read the whole thread, though, someone had already asked to work on it in July, and when I tried it on Pants 2.30.0 the warning didn't show up at all, because options are parsed natively now. So I re-ran the scout's Python good-first-issue + bug search with tighter limits: no assignee, 0-2 comments, no linked PR, opened since mid-September. That gave 470 hits across 93 repos. I screened the threads by hand (qim3d #301 already had an "I'd like to work on this" comment), reproduced the two survivors locally, and graded three. The scout accepted tacular #54 (straightforward) and tractusx-sdk #281, and rejected pycord #3437 on ai-policy. I picked tacular #54. It's a data/scientific Python library, which is my fit profile. The maintainer wrote out the cause, the function to change, the acceptance tests and the scope, and the repo says AI-assisted contributions are welcome. I want to learn how a small library keeps generated data byte-identical while it changes the parser that builds it, and how to write a regression test in a project's own style.

On the tool: the scout's best call was rejecting pycord #3437. On paper it looked better than either pick (filed by a maintainer, on a milestone, with a clear repro), but its policy says AI use by new contributors "will likely result in the direct closure" of the PR. My Unit 5 patch to `ai-policy` is what caught that. Where it fell short: `established-repo` passed tacular, which has 1 star, only because an outside contributor had a PR merged the day before. That's a thin pass, and I noted it in scan-notes.md. It also couldn't see that pants #15376 no longer reproduces. Only running the code showed that, so I now reproduce before I claim.

Claim comment: https://github.com/tacular-omics/tacular/issues/54#issuecomment-6782339353 (posted 2026-11-10)

**Toolkit**

- Tool: issue-scout. I ran a replacement search with its Q1 qualifiers plus no-assignee, comment and linked-PR limits, then `issue-scout: grade` on three URLs with an evidence file I gathered from the issue pages and fresh clones.
- What it produced: accept for tacular #54 (all seven required checks pass, difficulty straightforward, responsive-maintainers unclear because the thread had no replies yet). It also gave accept for tractusx-sdk #281 (ECA needed, `refine-me!` label) and reject for pycord #3437 on ai-policy.
- Difficulties: the GitHub search API hit its secondary rate limit twice, and `gh` was unauthenticated, so I gathered evidence by hand. `established-repo` passed a 1-star repo on the committer rule. The scout can't tell that an old issue no longer reproduces.
- What changed: I logged the committer-rule weakness in scan-notes.md. I also changed my process to reproduce candidates before I claim, after pants #15376 turned out to be fixed already.

---

## Understanding the Issue

**Problem Description**

[In your own words, what is broken or missing?]

**Expected Behavior**

[What should happen?]

**Current Behavior**

[What actually happens?]

**Affected Components**

[Which parts of the codebase are involved?]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Reproduction Process

**Environment Setup**

Linux 6.18, Python 3.13.18, uv. tacular `main` at commit `4fa80f9` (2026-11-09), package version 2.1.0. Setup is the one from CONTRIBUTING.md: clone, then `uv sync` (the same as `just install`). The reproducer feeds a synthetic OBO term through `gno._entries`, so I didn't need the 129 MB GNOme download. On unchanged main, `uv run pytest tests/test_datagen_gno.py -q` gives `7 passed`. No setup problems came up, apart from uv printing a deprecation warning about `UV_NATIVE_TLS` from my environment, which doesn't matter here.

**Steps to Reproduce**

1. `git clone https://github.com/tacular-omics/tacular.git && cd tacular && uv sync`
2. Save the issue's reproducer as `repro54.py`, looped over every row of the issue's table: `"Hex(6)NeuAc(1)"`, `"Hex(6)NeuAc"`, `"Hex(6)Xenon"`, `"Hex"`, `"Xenon(1)"`. Each one goes in as the `GNO:00000202` property of one `[Term]` in a `StringIO` OBO file, then through `get_obo_metadata`, `read_obo` and `gno._entries`, printing `(formula, monoisotopic_mass)`.
3. `uv run python repro54.py`. "Hex(6)NeuAc" and "Hex(6)Xenon" are both kept as `C36H60O30, 972.3169405209001`, and "Hex" is kept as `('', 0.0)`. Only "Xenon(1)" is skipped, with a warning.

**Reproduction Evidence**

- **Commit showing reproduction:** none
- **Screenshots/logs:**

```
'Hex(6)NeuAc(1)'   -> [('C47H77NO38', 1263.4123570297998)]
'Hex(6)NeuAc'      -> [('C36H60O30', 972.3169405209001)]
'Hex(6)Xenon'      -> [('C36H60O30', 972.3169405209001)]
'Hex'              -> [('', 0.0)]
Unknown glycan symbol 'Xenon' in composition 'Xenon(1)' (known symbols: [...])
'Xenon(1)'         -> []
```

- **My findings:** It reproduces on current main, and all five rows match the issue's table. The dropped `NeuAc` and `Xenon` produce no warning at all. A counted unknown symbol warns and is skipped, but an uncounted one disappears without a trace. The bare "Hex" case also gets past the guard at `src/tacular/_datagen/gno.py:140`: it checks `formula is None and mono_mass is None and avg_mass is None`, and `""` and `0.0` aren't `None`. Before this I'd also tried pants #15376, which did not reproduce on Pants 2.30.0. That's part of why I re-picked.

**Toolkit**

- Tool: repro-check, with the wild scope from Unit 6. I graded claim-draft.md alone first, then claim-draft.md and repro-draft.md together.
- What it produced: accept both times. On the pair, the six required checks passed (env-recorded, steps-rerunnable, behavior-matches, honest-outcome, claim-specific, conventions), and template-asks was unclear because I hadn't given it the bug template.
- Difficulties: it passed honest-outcome but flagged two weak spots. I'd said the bare "Hex" "gets past the guard" without showing the guard, and I wrote "Before my change, 7 passed" while also saying I hadn't changed any code.
- What changed: I quoted the guard line (`gno.py:140`), pasted the real pytest output, and reworded the line to "On unchanged main". The voice guard passed the revised report.

---

## Solution Approach

**Analysis**

[Your analysis of the root cause: what is causing the issue, traced
to the files and functions involved.]

**Proposed Solution**

[High-level description of your fix approach, and what you are
deliberately NOT changing.]

**Implementation Plan**

1. [Modify file X to do Y]
2. [Add or change Z]
3. [Update tests]

[Record deviations here as they happen: what changed mid-build and
why. A recorded deviation is honest work; silent drift is not.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Testing Strategy

[What observable checks prove the fix: your reproduction steps re-run
against the built change (before and after), and the repo's own test
suite or checks with the outcome shown. "Tests pass" with nothing
observable named is not a strategy.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**[Date]**

[Entry]

**[Date]**

[Entry]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Pull Request

**PR Link:** [GitHub PR URL when opened]

**PR Description:** [what the PR says; much of the content above
adapts into it]

**Maintainer Feedback:**

- [Date]: [feedback received, or "none yet"; silence is a normal
  state and costs you nothing]
- [Date]: [how you addressed it]

**Status:** [Awaiting review / Iterating / Merged / Closed. The
status is a fact you record, never a grade: the PR is graded as
opened.]

**Toolkit**

- Tool:
- What it produced:
- Difficulties:
- What changed:

---

## Learnings and Reflections

**Technical Skills Gained**

[What you learned technically, named concretely.]

**Challenges Overcome**

[What was hard and how you solved it; point at the real events, the
Toolkit blocks above are full of them.]

**What I'd Do Differently Next Time**

[Reflection on your process.]

---

## Resources Used

[What actually helped, not a reading list. List things this story
already shows you using: the doc you followed to set the environment
up, the thread that explained the bug, the person who answered you.
One real entry beats ten plausible ones.]

- [Link to documentation that helped]
- [Discussion, issue, or post that helped]
