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
**Pull Request:** https://github.com/tacular-omics/tacular/pull/55

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

**Unit 7: installed the ship-gate, wrote a 5-row manifest from tacular's CI, and got the branch through it**

My manifest runs `uv sync --locked --all-extras --all-groups`, then ruff check, ruff format --check, ty and pytest, all taken from CONTRIBUTING.md and `.github/workflows/ci.yml`. I ran each one on unchanged main first, and all passed, so the excluded table is empty. The hook test gave the two expected stops (voice guard and `ship-gate: STOP.`). On my branch, the first thing that stopped me was my own manual format check: "1 file would be reformatted", a blank-line slip in my new tests. The first full ship-gate run then rejected the branch with `standards-met [unclear]`, because `gh` isn't authenticated here, so it couldn't fetch the issue thread to confirm I hadn't ignored any maintainer direction. I didn't loosen the rubric, since the check is right to want the thread. Instead I captured the live thread from the browser and served it to the gate through a small `gh issue view` stand-in, and the next run passed. My container can't push or open a PR with `gh`, so I committed the same tree to my fork through GitHub's web upload, checked that the tree hash matched my local commit, re-ran the gate on that exact head (PASS), and opened the PR in the browser after a manual voice-guard check.

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

tacular builds its GNOme glycan lookups by reading composition strings like "Hex(2)HexNAc(1)". The parser only picks out the `Symbol(count)` pieces, so anything else in the string is thrown away without a word, and the entry is kept with a wrong formula and mass.

**Expected Behavior**

A composition that isn't made entirely of `Symbol(count)` tokens (apart from surrounding whitespace) is reported with a warning that names the full string and the leftover text, and the entry is skipped. That's how an unknown counted symbol like "Xenon(1)" is already handled.

**Current Behavior**

"Hex(6)NeuAc" is kept as C36H60O30, 972.317 Da. That's 291.095 Da light, because the uncounted NeuAc is dropped. "Hex(6)Xenon" gives the same values. A bare "Hex" is kept with formula `""` and mass 0.0. None of them logs a warning.

**Affected Components**

`_parse_glycan_composition` in `src/tacular/_datagen/gno.py` (the `re.findall` at line 45) is the cause. `_entries` in the same file (the all-`None` guard at line 140) is why the bare "Hex" survives. The tests live in `tests/test_datagen_gno.py`. The same `_datagen` code feeds both the `data_gen` regeneration pipeline and `tacular update gno`.

**Toolkit**

- Tool: my CLAUDE.local.md and one scoped read of `src/tacular/_datagen/gno.py` and its test file, plus the repo's own CLAUDE.md, which Claude Code loads automatically.
- What it produced: the four answers above, with line numbers checked against main `4fa80f9`. The issue's line numbers were still correct.
- Difficulties: none met. The maintainer had already pinned the cause and the lines, so the work was confirming them rather than finding them.
- What changed: nothing.

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

`_parse_glycan_composition` in `src/tacular/_datagen/gno.py` (line 42) reads a GNOme composition string with `re.findall(r"([A-Za-z0-9]+)\((\d+)\)", composition_str)` (line 45). `findall` returns only the matches, so any text the pattern skips, like the uncounted `NeuAc` in "Hex(6)NeuAc", is dropped with no warning. The function then returns a composition built from what did match. The only warning path is the unknown-symbol branch (line 49), and it runs only on matched tokens, so it never sees uncounted text. `_entries` (line 90) drops an entry only when formula, mono mass and average mass are all `None` (line 140). For the bare "Hex", `findall` returns nothing and the function returns an empty `Counter`, which gives formula `""` and mass `0.0`. Neither is `None`, so the entry is kept.

**Proposed Solution**

In `_parse_glycan_composition`, after matching, remove every `Symbol(count)` match from the string. If anything but whitespace is left, log a warning that names the full composition string and the leftover text, then return `None`, following the shape of the existing unknown-symbol branch. `_entries` already skips an entry whose composition is `None`.

Not changing (the issue's out-of-scope list):

- `_entries` and its guard
- `data_gen/generator/gen_gno.py`
- the other builders
- `_GLYCAN_COMPOSITIONS`
- how a counted unknown symbol behaves
- keeping partly parsed entries
- `src/tacular/gno/data.py`
- the version

**Implementation Plan**

1. Branch `fix/54-gno-unconsumed-composition` from `main`.
2. In `gno.py`, keep the token pattern inside the function, use it for both `findall` and `sub`, and add the consumed-whole-string check before the unknown-symbol loop.
3. In `tests/test_datagen_gno.py`, add tests on the `_terms` helper and `caplog` for "Hex(6)NeuAc", "Hex" and "Hex(6)Xenon" (no entry, and the warning names the string and the leftover text). Add one more showing a whitespace-padded valid string still parses.
4. Add a CHANGELOG entry under `## [Unreleased]`.
5. Re-run the reproducer and the manifest checks.
6. Skip the optional 129 MB `gen-gno` check and say so in the PR.

Deviations:

- After plan-check, I dropped "compile the pattern once", so the change stays inside the function and in scope.
- `strip()` on the leftover text also accepts whitespace between tokens, not just at the ends. I didn't plan that, so I flagged it in the PR for the maintainer.
- The plan said one commit. The PR has three (fix, tests, changelog), because I had to commit through GitHub's web upload one file at a time.

**Toolkit**

- Tool: plan-check, with the wild scope from Unit 6, on plan.md and plan-comment.md for issue #54.
- What it produced: accept, with all six required checks passing (grounded-diagnosis, bounded-scope, executable, decisive-test, honest-unknowns, thread-and-conventions) and names-the-plan also passing. It flagged "compile the token pattern once" as borderline scope if hoisted out of the function. It also flagged two voice notes on the comment: show the output instead of only claiming "all five rows match", and say where I ran it.
- Difficulties: plan-check had no GitHub access, so it graded the thread from the evidence file I gave it, and it said so. Separately, the voice guard stopped plan-comment.md for promising the fix ("I'll add tests..."), even though plan-check had passed it.
- What changed: I kept the pattern inside the function, pasted the reproducer output and environment into the comment, and rewrote the plan as a question to the maintainer ("Does that approach match what you had in mind?"). The voice guard then passed it. I didn't post it, as the assignment says.

---

## Testing Strategy

1. **Reproducer, before and after.** The issue's reproducer, looped over all five rows of its table.
   - On main `4fa80f9`, "Hex(6)NeuAc" and "Hex(6)Xenon" print `[('C36H60O30', 972.3169405209001)]` and "Hex" prints `[('', 0.0)]`.
   - On the branch, all three print `[]`, each after a warning like `Unparsed text 'NeuAc' in glycan composition 'Hex(6)NeuAc' (expected only Symbol(count) tokens)`.
   - "Hex(6)NeuAc(1)" still gives C47H77NO38 / 1263.412, and "Xenon(1)" still gives the old unknown-symbol warning.
2. **New tests fail without the fix.** With `gno.py` put back to main, the new tests give `3 failed, 8 passed`. The whitespace test passes either way, because it guards the "surrounding whitespace" part of the issue.
3. **Existing tests unchanged.** `tests/test_datagen_gno.py` gives 11 passed, including `test_build_parses_valid_composition` and the six other original tests.
4. **The repo's own checks, via my ship manifest.** `uv run ruff check src tests` passes. `ruff format --check` reports 97 files already formatted. `uv run ty check src` passes. `uv run pytest tests` gives 1291 passed, 8 skipped (1287 on main, plus my 4). `release_version.py check` passes.
5. **Not run:** `just -f data_gen/justfile gen-gno` (data.py byte-identical). My network proxy refuses purl.obolibrary.org, so I couldn't download GNOme.obo. I said so in the PR.

**Toolkit**

- Tool: the ship-gate, through my manifest's five checks, plus my own runs of the reproducer and pytest (with and without the fix), all saved in test_evidence.md.
- What it produced: every check passed on the final head `defcbfb`, and the before/after output above.
- Difficulties: the first `ruff format --check` on my branch failed ("1 file would be reformatted") because of an extra blank line in the new tests. The GNOme download was blocked.
- What changed: I ran `uv run ruff format` on the test file, amended the commit, and re-ran all five checks. I recorded the skipped gen-gno run in the evidence and the PR instead of claiming data.py is unchanged.

---

## Implementation Notes

[A running log, added to as you work. Date each entry. What you
built, decisions you made when you made them, wrong turns included.]

**2026-11-10 (morning)**

Reproduced on main `4fa80f9` with `uv sync` and the issue's reproducer. All five rows of the table matched. The bare "Hex" case gets past the guard at line 140, because `""` and `0.0` aren't `None`.

**2026-11-10 (evening)**

- Installed the ship-gate and filled the manifest from CONTRIBUTING.md and ci.yml. All five checks passed on main.
- Wrote plan.md and plan-comment.md. plan-check accepted the plan. The voice guard stopped the comment for promising the fix, so I turned it into a question.
- Built the fix on `fix/54-gno-unconsumed-composition`:
  - In `_parse_glycan_composition`, `re.sub` takes out the `Symbol(count)` tokens, then I `.strip()` what's left. If anything remains, it logs a warning and returns `None`.
  - Added four tests and a CHANGELOG entry under "### Fixed".
- Format check failed on one blank line. I fixed it and amended.
- Changed the CHANGELOG line from "The bundled GNOme data is unchanged" to "This change does not touch the bundled GNOme data", because I couldn't run gen-gno to prove byte-identity.
- Tried to download GNOme.obo for the byte-identical check. purl.obolibrary.org was refused by my proxy, and the raw GitHub URL in `gno.py` returned 404.
- `git push` to my fork was refused by my environment's git proxy. I recreated the branch on my fork through GitHub's web upload (three commits). The browser checked each file's git blob hash before upload, and the final tree hash matched my local commit (`dde75fd`).
- Re-ran the ship-gate on that head: PASS. Opened PR #55.

**Toolkit**

- Tool: Claude Code in my clone, with CLAUDE.local.md and the repo's CLAUDE.md loaded, for the edit, the tests and the check runs.
- What it produced: a 66-line diff in three files (gno.py +15/-1, the tests +42, CHANGELOG +9).
- Difficulties: no push access from my environment, the blocked GNOme download, and one formatting slip.
- What changed: I used web-upload commits checked by tree hash instead of a push, disclosed the skipped gen-gno in the PR, and fixed the format with ruff.

---

## Pull Request

**PR Link:** https://github.com/tacular-omics/tacular/pull/55

**PR Description:** It follows the repo's PR template:

- **What this changes:** compositions the `Symbol(count)` tokens don't fully account for are skipped with a warning naming the string and the leftover text. "Fixes #54".
- **Why:** `re.findall` drops unmatched text, so "Hex(6)NeuAc" was 291.095 Da light and "Hex" was kept at 0.0. The change stays inside `_parse_glycan_composition`.
- **How it was tested:** the reproducer before and after, the four new tests (three fail on main), and the five CONTRIBUTING.md checks with their output. The checklist is ticked, with a note that I ran the parts of `just check` separately.
- **Notes for the maintainer:**
  - I didn't run gen-gno, because the download was blocked.
  - Whitespace between tokens is also accepted. I asked whether they'd rather allow it only at the ends.
  - AI disclosure: Claude Code read the code, wrote the fix, tests and description, and ran the checks. Each commit has a Co-Authored-By line, and I'm responsible for the change.

**Maintainer Feedback:**

- 2026-11-10: none yet (PR opened today)

**Status:** Awaiting review

**Toolkit**

- Tool: the ship-gate (manifest checks plus my pr-precheck in live mode), run by hand with `check_gate.py --branch --title ... --body-file pr_draft.md`. Then the voice guard's `--draft` check, because I opened the PR in the browser and the hook couldn't see it.
- What it produced:
  - Run 1: STOP. pr-precheck rejected the branch on `standards-met [unclear]`, because the issue thread couldn't be fetched (`gh` returned 403).
  - Run 2, with the thread supplied: PASS. pr-precheck noted that "The bundled GNOme data is unchanged" sat awkwardly next to my "can't confirm byte-identity" note, and that my AI line should say who ran the checks.
  - Runs 3 to 5: PASS each time after I reworded those, rebased onto the fork's commits, and added the attribution footer.
- Difficulties: the hook path (opening the PR through `claude` with `gh`) wasn't usable, because my environment can't authenticate `gh` or push. The ship-gate also stopped one of my own shell commands, because it contained the PR command inside a heredoc ("couldn't read this command").
- What changed:
  - I gave the gate the real thread through a `gh issue view` stand-in serving the text I captured from the issue page.
  - I made sure the gate's last PASS was on the exact head and description the PR used.
  - I kept the rubric as is, because the unclear grade was about missing evidence, not a wrong check.

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
