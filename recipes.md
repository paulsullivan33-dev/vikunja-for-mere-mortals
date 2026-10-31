# Recipes

Outcome: four small jobs done the way the
earlier chapters taught — each one names
the chapters it leans on, so you can
re-derive it instead of memorising it.

## 1. A quick capture that lands in the Inbox

*Leans on chapters 5 and 6.*

The Inbox (chapter 5) is the landing
strip: a task created against project 1
is a captured thought, not yet filed. In
the web app this is the quick-add box set
to the Inbox; from a script it is one
`PUT`. File the captures properly once a
week: read the Inbox's tasks
(`GET /projects/1/tasks`), and for each,
decide its real project. Note that moving
a task between projects is a web-app
gesture; the API in this build has no
re-home operation, so a script files by
recreating the task whole in its project
(chapter 6's whole-object habit) and
deleting the capture with
`DELETE /tasks/{id}` (chapter 11's
phrasebook).

## 2. A Friday review from one filter

*Leans on chapters 6 and 7.*

Save a filter — *not done, due within
seven days, any project* — and it becomes
one of the negative-id pseudo-projects of
chapter 5, readable like any other list:
`GET /tasks` with the filter, or the saved
filter's own address. Print it, work it
top-down, and let the Gantt view
(chapter 7) show the week you just
promised yourself. The discipline is the
recipe: one list, every project, dates
only — which is why chapter 6 nagged
about keeping due dates honest.

## 3. A board for a recurring job

*Leans on chapters 7 and 8.*

For a job that repeats — a newsletter, a
monthly inspection — rename the buckets
to its real stages and give the busiest
middle bucket a `limit` of a small number.
The limit is the recipe's point: when the
board refuses another card in *Drafting*,
it is telling the truth about your week
before your week does. Archive the
project when the series ends (the archive
switch freezes it read-only) and start
next year's as a fresh project, so each
board stays one story.

## 4. A script that may look but not touch

*Leans on chapters 11 and 12.*

Mint an API token with `read_all` on
projects and tasks only, a near expiry,
and a title that says which script holds
it. The script's reads use the token;
its writes would earn chapter 11's
misleading 401 — which is the fence doing
its job. Keep the token out of the script
itself: an environment variable or a
file with owner-only permissions. And set
a calendar note for the expiry; a token
that dies mid-season fails as the same
cryptic 401, and chapter 16 knows its
face.

Check yourself for all four: each recipe
should feel like a rearrangement of
things you have already done once slowly.
That is the sign the model, not the
memorisation, is carrying you.
