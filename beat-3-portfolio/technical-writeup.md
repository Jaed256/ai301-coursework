# [Your title: say what you did]

<!--
SKELETON. Claude pasted in the proof blocks and the dated facts. Every sentence of prose is yours to write (Unit 9 rule).
Write where you see "✍️". Keep these three ## headings exactly as spelled. Delete every HTML comment and every ✍️ marker when you're done.
Aim for 500-1,000 words in total.

Rubric checklist:
[ ] Overview: what the work was, your part in it, and how it ended (PR #55 merged 2026-11-11). Link the PR, and name the parts of the tools you wrote yourself.
[ ] What happened: real moments in order, with at least two pieces of proof on the page, each right next to its claim.
[ ] Proof comes from this term's real work (all the blocks below do).
[ ] A change you made: show a second version and what caused it. Moments 1, 3 and 4 each have one.
[ ] A result that landed: moment 6, the merge and the maintainer's comment.
[ ] Limitations and what's next: a real limit, and a next step that follows from it.
-->

## Overview

✍️ <!-- One short paragraph with the whole story: the project (tacular, a Python library of proteomics lookups), the problem (#54), what you did (fix + tests), and how it ended (PR #55 merged 2026-11-11).
Links: issue https://github.com/tacular-omics/tacular/issues/54 · PR https://github.com/tacular-omics/tacular/pull/55
Tool parts you wrote yourself (say which): the issue-scout rubric and search strategy, the voice guide's Upstream rules, the ship manifest, and the PR-monitor playbook. -->

## What happened

### 1. Picking the issue, and the scout rule I changed

✍️ <!-- Date 2026-11-10. The first wild scan accepted all 5 issues, but 2 came from a workshop repo created that day, and pycord #3437 passed even though its policy warns new contributors off AI. You patched ai-policy (and added established-repo). Later, the scout rejected pycord #3437 when it re-graded it. Your first pick, pants #15376, turned out to be claimed in July and didn't reproduce on Pants 2.30, so you re-picked tacular #54. -->

The `ai-policy` row in my scout rubric, before and after:

```text
Before: FAILS only on an outright ban on AI-generated or AI-assisted code or documentation ...
After:  FAILS on an outright ban ..., AND on a policy that tells new contributors not to use AI
        or warns their AI-assisted work will likely be closed (e.g. "use of AI tools by new
        contributors is highly discouraged and will likely result in closure") ...
```

The scout's verdict on the replacement scan:

```text
3. pycord #3437, AppInfo.integration_types_config round-trip: reject. It fails ai-policy.
   CONTRIBUTING.md says AI use by new contributors "is highly discouraged, and will likely
   result in the direct closure of your issue or pull request."
1. tacular #54, GNOme composition parser silently ignores text that isn't Symbol(count): accept.
   Difficulty: straightforward.
```

### 2. Reproducing the bug

✍️ <!-- What the bug is in plain terms, and what you ran (main 4fa80f9, Python 3.13.18, uv sync, the issue's reproducer looped over its table). -->

```text
'Hex(6)NeuAc(1)'   -> [('C47H77NO38', 1263.4123570297998)]
'Hex(6)NeuAc'      -> [('C36H60O30', 972.3169405209001)]   <- NeuAc silently dropped
'Hex(6)Xenon'      -> [('C36H60O30', 972.3169405209001)]
'Hex'              -> [('', 0.0)]                          <- kept with an empty formula
Unknown glycan symbol 'Xenon' in composition 'Xenon(1)' (known symbols: [...])
'Xenon(1)'         -> []
```

### 3. The claim, and my voice guard stopping me

✍️ <!-- The guard stopped your first claim draft for promising a fix before you'd reproduced anything. Say what you changed. The same thing happened again on the plan comment. -->

```text
Draft 1 (stopped): "Then I'll add the full-string check to `_parse_glycan_composition`,
                    following the shape of the existing unknown-symbol branch."
Guard log: 2026-11-10 18:34 [manual] stop: rule broken: "Before I've fixed something, I only
           commit to the next thing I'll check and to reporting what I find. No fix promises,
           no dates." | target: claim-draft.md
Posted:    "I'd like to work on #54. My first step is to run your reproducer on current main
            and check both cases you listed ... I'll post what I find here within two days."
```

### 4. The fix, the tests, and the ship-gate

✍️ <!-- The cause (re.findall returns only the matches) and your fix. The 4 tests, 3 of which fail without the fix. Then the first ship-gate run stopped the PR because it couldn't read the issue thread, and you supplied the real thread rather than loosening your rubric. The ruff format slip. -->

```diff
-        tokens = re.findall(r"([A-Za-z0-9]+)\((\d+)\)", composition_str)
+        token_pattern = r"([A-Za-z0-9]+)\((\d+)\)"
+        tokens = re.findall(token_pattern, composition_str)
+        unconsumed = re.sub(token_pattern, "", composition_str).strip()
+        if unconsumed:
+            logger.warning(
+                "Unparsed text %r in glycan composition %r (expected only Symbol(count) tokens)",
+                unconsumed,
+                composition_str,
+            )
+            return None
```

```text
New tests with the fix removed:   3 failed, 8 passed
Full suite on the branch:         1291 passed, 8 skipped   (main: 1287 passed)
ruff check / ruff format --check / ty check: all passed

Ship-gate log:
## 2026-11-10 18:47 [manual] tacular-omics/tacular @ fix/54-gno-unconsumed-composition
- verdict: BLOCK. pr-precheck: standards-met [unclear]: ... the issue thread was unavailable
  (HTTP 403), so no-ignored-maintainer-direction cannot be verified.
## 2026-11-10 18:48 [manual] ... - verdict: PASS
```

### 5. Opening the PR, and the monitor saying wait

✍️ <!-- PR #55 opened 2026-11-10. CI 11/11 green. Your playbook's 5-day nudge rule (based on #51, the repo's only merged outside PR, 20.6 h) meant the monitor drafted a wait note, so you posted nothing. -->

### 6. The merge

✍️ <!-- What landed and when (2026-11-11 03:22 UTC, about 3.5 hours after opening). -->

> "Thank you @Jaed256, lgtm. All three acceptance items in #54 are met: `"Hex(6)NeuAc"` and a bare `"Hex"` are now skipped with a warning that names the full string and the unconsumed text, the existing tests are untouched, and the bundled GNOme data is not changed. ... The whitespace test is a good addition. Merging."
> (pgarrett-scripps, maintainer, on PR #55)

## Limitations and what's next

✍️ <!-- Pick real ones. Facts you could use:
- You never ran the 129 MB gen-gno byte-identical check (the download was blocked). The maintainer confirmed the data didn't change, but you didn't prove it yourself.
- The fix also accepts whitespace between tokens. The maintainer didn't object, but you didn't ask before building.
- Your environment had no authenticated gh and no git push, so you worked around the hook path (gh stand-in, web-upload commits) and the PR has 3 commits instead of 1.
- The scout's established-repo check passed tacular (1 star) only because one outside PR had just merged, and its ranking still favors brand-new tiny repos.
- The playbook threshold rests on a sample of one merged outside PR.
Then a next step that follows from one limit. -->
