# Kanban Boards

Outcome: you can run a board the way the
API does — read it as buckets with tasks
inside, move a card, and explain why a
finished task walks to the last column by
itself.

A **kanban board** shows a project's work
as cards standing in columns. The columns
are **buckets**, and the card's column is
its answer to "where has this got to?" A
new project's board is born with three
buckets (transcript `t05`):

```console
$ curl …/api/v1/projects/2/views/12/buckets -H "Authorization: Bearer $TOKEN"
[{"id":7,"title":"To-Do","limit":0},
 {"id":8,"title":"Doing","limit":0},
 {"id":9,"title":"Done","limit":0}]
```

`limit` is a work-in-progress cap: 0 means
none, and a bucket with a limit will
refuse extra cards in the web app —
the board's polite way of saying *finish
something first*. Buckets can be renamed,
added and reordered; projects that grow
up often trade *To-Do/Doing/Done* for
their own stages (*Intake, Drafting,
With the editor, Published*). The view
also carries a switch, seen in chapter 7,
for how buckets are configured — `manual`,
as here, or derived from a task field
such as its labels or priority.

## Reading a board

The natural first guess — ask a bucket
for its tasks at
`GET …/views/12/buckets/7/tasks` — is
refused: `Method Not Allowed`, all three
buckets, transcript `t06`. A board is
read whole, from the view:

```console
$ curl …/api/v1/projects/2/views/12/tasks -H "Authorization: Bearer $TOKEN"
```

and the answer is the buckets again, this
time with their tasks nested inside each.
That nesting is why the answer to "what
is in Doing?" is one request, and also
why an empty bucket arrives as
`"tasks": null` rather than an empty
list — a small sharp edge for script
writers, kept in transcript `t07`.

## Moving a card

Moving is a small post to the destination
bucket:

```console
$ curl -X POST …/api/v1/projects/2/views/12/buckets/8/tasks \
    -H "Authorization: Bearer $TOKEN" -d '{"task_id":4}'
```

The reply is the placement itself: the
task, the bucket, and the bucket's new
count. The lab moved *Sweep the floor* to
**Doing** and read the board back to find
it there (transcript `t08`). A card sits
in exactly one bucket per board; moving
is re-filing, not copying.

## The Done bucket is not just a column

Two behaviours set the last bucket apart.
First, one bucket per board is designated
the **done bucket**, and the designation
works from both directions: when the lab
marked the paint-tin task done through
the task itself (chapter 6), the card
**moved into the Done bucket on its own**
— nobody dragged it. Second, dragging a
card the other way — into the done
bucket — is how the web app lets you
finish a task with your thumb. Either
door, same room: `done` is a fact about
the task, and the board arranges itself
around the fact.

Cards that are done but left standing in
earlier columns are the smell this design
prevents; if your board shows some, an
API client moved them without the flag —
or the flag without the move — and the
task's own `done` field is the truth to
reconcile against.

Check yourself: create a task, drag it
(or post it) to Doing, then mark it done
from the task's page and reopen the
board. The card is in Done, and it got
there without you. That small act of
housekeeping is the board earning its
keep.
