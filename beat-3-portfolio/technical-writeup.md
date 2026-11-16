# Fixing a silent data bug in tacular: my first merged open source contribution

## Overview

I fixed a bug in tacular that would have quietly produced wrong masses the next time someone ran `tacular update` to refresh the GNOme data. It wasn't actively breaking workflows yet, but it would have corrupted entries silently with no warning. My fix is [PR #55](https://github.com/tacular-omics/tacular/pull/55), for issue [#54](https://github.com/tacular-omics/tacular/issues/54), and it was merged on 2026-11-11, about 3.5 hours after I opened it. It's my first merged open source contribution. The tool part I wrote that mattered most is the AI-policy rule I added to my issue scout in Unit 5.

## What happened

### Picking the issue

The scout actually approved pants #15376, but I dropped it because someone else had claimed it back in July, and the bug didn't even reproduce on Pants 2.30. What the scout did catch was pycord #3437. It rejected it because pycord's AI policy blocks AI-assisted PRs from new contributors, which is exactly the rule I added to the scout in Unit 5.

The `ai-policy` row in my scout's rubric, before and after:

```text
Before: FAILS only on an outright ban on AI-generated or AI-assisted code or documentation ...
After:  FAILS on an outright ban ..., AND on a policy that tells new contributors not to use AI
        or warns their AI-assisted work will likely be closed (e.g. "use of AI tools by new
        contributors is highly discouraged and will likely result in closure") ...
```

The scout's verdict when I re-scanned:

```text
3. pycord #3437, AppInfo.integration_types_config round-trip: reject. It fails ai-policy.
   CONTRIBUTING.md says AI use by new contributors "is highly discouraged, and will likely
   result in the direct closure of your issue or pull request."
1. tacular #54, GNOme composition parser silently ignores text that isn't Symbol(count): accept.
   Difficulty: straightforward.
```

The bug, reproduced on tacular `main` (`4fa80f9`) with the issue's own reproducer:

```text
'Hex(6)NeuAc(1)'   -> [('C47H77NO38', 1263.4123570297998)]
'Hex(6)NeuAc'      -> [('C36H60O30', 972.3169405209001)]   <- NeuAc silently dropped
'Hex(6)Xenon'      -> [('C36H60O30', 972.3169405209001)]
'Hex'              -> [('', 0.0)]                          <- kept with an empty formula
Unknown glycan symbol 'Xenon' in composition 'Xenon(1)' (known symbols: [...])
'Xenon(1)'         -> []
```

### Claiming it: my voice guard stopped me

I jumped the gun. My first draft promised the exact fix before I had even reproduced the issue. The guard blocked it using my own rule: no fix promises before you've shown anything. I rewrote it to strip out the premature fix, keeping only the first step and adding, "I'll report back within two days."

```text
Draft 1 (stopped): "Then I'll add the full-string check to `_parse_glycan_composition`,
                    following the shape of the existing unknown-symbol branch."
Guard log: 2026-11-10 18:34 [manual] stop: rule broken: "Before I've fixed something, I only
           commit to the next thing I'll check and to reporting what I find. No fix promises,
           no dates." | target: claim-draft.md
Posted:    "I'd like to work on #54. My first step is to run your reproducer on current main
            and check both cases you listed ... I'll post what I find here within two days."
```

### The fix, and the ship-gate

The fix, in `src/tacular/_datagen/gno.py`:

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
```

The `pr-precheck` hit an HTTP 403 trying to read the issue thread, so it graded one of the checks as "unclear." Loosening the rubric defeats the whole purpose. Hacerlo bien o no hacerlo. Instead of lowering the bar, I just fed the gate the actual thread data so it could pass the strict check properly.

```text
## 2026-11-10 18:47 [manual] tacular-omics/tacular @ fix/54-gno-unconsumed-composition
- verdict: BLOCK. pr-precheck: standards-met [unclear]: ... the issue thread was unavailable
  (HTTP 403), so no-ignored-maintainer-direction cannot be verified.
## 2026-11-10 18:48 [manual] ... - verdict: PASS
```

### The merge

The maintainer merged it about 3.5 hours after I opened the PR. It felt awesome, and it validated my whole workflow. It tells me that my preference for concise, straightforward code works perfectly for open source: maintainers just want clean code that follows their standards from the jump.

> "Thank you @Jaed256, lgtm. All three acceptance items in #54 are met: `"Hex(6)NeuAc"` and a bare `"Hex"` are now skipped with a warning that names the full string and the unconsumed text, the existing tests are untouched, and the bundled GNOme data is not changed. ... The whitespace test is a good addition. Merging."
> (pgarrett-scripps, maintainer, on PR #55, 2026-11-11)

## Limitations and what's next

Limitation: I never ran the byte-identical data check, because the download was blocked.

Next step: figure out a workaround to fetch that data (or a valid subset), so I can run a strict byte-for-byte comparison and be 100% sure nothing else shifted.
