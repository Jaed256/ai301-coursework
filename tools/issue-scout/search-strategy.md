# Search strategy: where your scout looks, and why

<!--
THIS IS THE PART YOU WRITE. The scout in SKILL.md executes whatever
queries and filters you define here. It ships empty on purpose: the
strategy is your judgment about where YOUR first contribution lives,
and a scan without authored queries is just someone else's defaults.

A filled strategy must contain:

1. At least one query in the Queries section. Each query is a real
   `gh search issues` invocation, and each should trace to a sentence
   of your fit profile in scope.md: if you cannot say which sentence
   produced a query, the query is probably not yours. Search breadth
   is the point at this stage: prefer generous limits and let the
   ranking filters do the narrowing.

2. At least one ranking filter in the Ranking filters section. A
   filter is a condition or an ordering over fields the search
   results already carry (last activity date, assignee presence,
   label set, comment count, repo). It must be checkable from the
   result fields alone: if deciding it would need opening the issue
   page, it belongs in your rubric, not here. Ranking is the free
   stage; keep it free. Make at least one filter an ordering, or
   state your tie-break: with drop conditions alone, the scout takes
   the top of whatever order the searches happened to return, and
   the graded candidates stop being traceable to you.

3. The top-N line, kept or deliberately changed.
-->

## Queries

<!-- Real gh search invocations, one per line, each with a comment
naming the fit-profile sentence it came from. Example SHAPE (write
your own; these values belong to nobody):

gh search issues --label "good first issue" --language python --state open --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,url
-->

# "I work mainly in Python ... a bug fix" + "data, scientific-computing ... projects"
gh search issues --label "good first issue" --label "bug" --language python --state open --limit 100 --sort updated --json title,repository,labels,assignees,commentsCount,updatedAt,url

# "a small feature, or tests/docs around Python code, in an actively maintained library or developer tool"
gh search issues --label "good first issue" --language python --state open --no-assignee --updated ">=2026-10-01" --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,url

# "CLI/developer-tooling projects" + "comfortable reading and running a Python test suite"
gh search issues "test" --label "help wanted" --language python --state open --no-assignee --limit 100 --json title,repository,labels,assignees,commentsCount,updatedAt,url

## Ranking filters

<!-- Conditions and orderings over the free result fields, applied in
the order written. State each so a stranger could apply it and get
your ordering. Example SHAPES (write your own):

- drop any result with an assignee
- drop any result not updated in the last 60 days
- prefer fewer comments over more
-->

Applied in this order, using only the fields the search returns:

1. Drop any result with one or more assignees.
2. Drop any result whose `updatedAt` is more than 60 days before the scan date.
3. Drop any result with more than 10 comments (long threads usually mean claims or design debate).
4. Drop any result whose labels include `frontend`, `css`, `ui`, `design`, `hardware`, `discussion`, `question`, `workshop`, `exercise`, or `tutorial` (profile: avoid frontend work, special hardware, non-code asks, and practice/workshop repos).
5. Drop any result whose repository name or owner matches the course sandbox (`codepath/pathreview*`).
6. Order the rest: fewest comments first; tie-break by most recently updated first; then keep at most 2 results per repository so one repo can't fill the top five.

## How many to grade

Grade the top **5** ranked results per scan.

<!-- Five is the default because grading is the only stage that costs
money: about $0.20 per candidate, so a scan is about $1, and you can
iterate the strategy several times for less than one full regression
run. You need one good issue, not ten. Raising this number is
allowed, but it is a decision you state here in a sentence, with the
cost you are accepting; it is not a number you bump because a scan
disappointed you once. -->
