<!--
SKELETON. Write the post yourself (Unit 9 rule), then delete this whole comment so the file is only the post.

Rules from the rubric: plain text (no headings, tables or code blocks); 60 to 250 words, where a link or a hashtag counts as one word; first person; complete sentences; fair to the maintainer, the project and the course.
- Open with two sentences that say what you did (LinkedIn hides the rest behind "...more").
- Say how it ended and why it matters: merged, and what it taught you about how you work.
- Use plain words: name what you fixed. Leave out course terms like eval harness and ship-gate, or explain them.
- End with one link: https://github.com/tacular-omics/tacular/pull/55 (hashtags can follow it).
- Only claim what your write-up backs up. It's merged, so "fixed" is accurate.

Facts you can use:
- Project: tacular, an open source Python library of lookups for mass-spectrometry proteomics (glycans, modifications, elements).
- Bug: a parser silently dropped part of a glycan description, so "Hex(6)NeuAc" came out about 291 Da too light and a bare "Hex" came out with zero mass, with no warning.
- Fix: the parser now refuses input it can't fully read and logs a warning naming it. 4 new tests, 3 of which fail without the fix.
- Opened 2026-11-10, merged 2026-11-11, about 3.5 hours later. The maintainer said all three acceptance items were met.
- How: you built AI tools to find an issue, check your drafts before posting, and run the project's checks before opening the PR. You used Claude Code and disclosed it.
- First merged open source contribution (say so only if it's true).
-->
