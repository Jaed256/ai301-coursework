# Voice guide: how I talk upstream

<!--
THIS IS THE PART YOU WRITE (new this week). Live mode reads this file
before any comment of yours goes out the door; eval mode ignores it
entirely, because your voice is yours and carries no gold labels.

This is not etiquette. "Be polite and concise" is advice for everyone
and therefore rules for no one. Write rules YOU need, in your own
words, each one concrete enough that the skill can hold a draft
against it and say which rule it breaks.

Three sections. Fill all three.
-->

## Who I am in threads

<!-- DRAFT written with Claude from Jaed's course context. Reword in your own voice. -->

I'm a CS student at FIU doing my first open-source contributions through CodePath AI301. I'm comfortable in Python but new to this codebase. In a thread I say what I checked, show the output, and say what I'll look at next. I don't guess about things I haven't run.

## Rules I write by

### Rule: Name the exact thing

Every comment names the function, file, test, or error it is about, so nobody has to guess which part of the issue I mean.

- Wrong: "I'd like to work on this, it looks like a good first issue for me."
- Right: "I'd like to work on #72: `verify_password()` in `core/security.py` raising `UnknownHashError` on a malformed hash."

### Rule: Promise investigation, not results

Before I've fixed something, I only commit to the next thing I'll check and to reporting what I find. No fix promises, no dates.

- Wrong: "I'll have a PR up by Friday that fixes this."
- Right: "Next I'll run the covering test with the xfail disabled and post what I see, whether or not it reproduces."

### Rule: Show it, then say it

A claim like "reproduced" or "confirmed" goes right next to the pasted output that shows it. If I didn't paste it, I don't claim it.

- Wrong: "Confirmed, it's definitely broken on my machine."
- Right: "Reproduced on `main` at `2f4e82f`: the call raises `passlib.exc.UnknownHashError: hash could not be identified` (output below)."

### Rule: Say where I ran it

I record the environment I actually used (OS, Python, key package versions, commit) and I don't describe it as anything else.

- Wrong: "Works the same on every setup."
- Right: "Ubuntu 24.04, Python 3.11.15, passlib 1.7.4, bcrypt 4.3.0, fork at `2f4e82f`."

### Rule: Be plain with classmates on shared issues

If others have claimed the issue, I say so once, without apology or competition, and post my own work.

- Wrong: "Sorry, I know others are on this, but can I please have it?"
- Right: "Several classmates have claimed this too; per the house rules I'm posting my own claim and reproduction."

## Things I never post

- "+1", "same here", or "same as above, can confirm" without my own output
- A fix deadline or "guaranteed" anything
- Requests to assign or reserve the issue for me
- A root cause I haven't shown evidence for
- Anything I haven't run myself, and output I didn't actually get
