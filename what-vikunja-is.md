# What Vikunja Is

Outcome: you can say what Vikunja holds,
how it organises it, and what "self-hosted"
costs and buys you — before you install
anything.

**Vikunja** is a to-do list and project
manager that runs on a server you control.
Where a cloud to-do app keeps your tasks
in a rented database, Vikunja keeps them
in a small program on your own machine:
your home server, a spare laptop, a cheap
rented virtual machine. You and your team
reach it through a web browser, and the
data never has to leave the building.

The organisation is three nouns deep.

- A **project** is a named collection of
  work: *Shed Tidy-Up*, *Kitchen Refit*,
  *The Newsletter*. Projects can hold
  other projects, so a big job can be a
  folder of smaller ones.
- A **task** is one thing to do, belonging
  to exactly one project. A task carries a
  title and, as you need them, a longer
  description, a due date, a start and end
  date, a priority, coloured **labels**,
  reminders, attachments, comments and
  relations to other tasks (this one
  blocks that one, and so on).
- A **view** is a way of looking at a
  project's tasks: a **list**, a **table**,
  a **Gantt chart** (tasks as bars along a
  timeline) or a **kanban board** (cards in
  columns — *To-Do*, *Doing*, *Done*).
  Views change nothing about the tasks;
  they are four windows onto the same
  room.

On top of that sit the people parts.
Accounts register on your instance. You
can share a project with named users or a
**team**, at read, write or admin level —
or mint a **public link** that shows the
project, read-only or writable, to anyone
who has the link. And because deadlines
are calendar-shaped, Vikunja also speaks
**CalDAV**, the standard calendar
protocol, so a calendar program can show
your due tasks as its own to-dos.

The licence is part of the model, so here
it is plainly. Vikunja is free software
under the **AGPL-3.0-or-later** licence:
the source code is public, you may run it
for any purpose, and if you offer a
modified version to others over a network
you must offer them the source too. The
program itself says this at startup; the
lab's copy of its help text is in
`vikunja-lab/transcripts/t00-download-and-version.txt`.
Since version 2 there is also an optional
paid add-on, **Vikunja Pro** — an admin
panel, audit logs and time tracking —
which a licence key switches on. It is
the same AGPL code; the key activates it.
The core program this guide teaches needs
no key and no payment, and without a key
the instance makes no contact with the
project's licence server at all (the
project documents this, and the lab ran
keyless throughout). Chapter 15 returns
to what Pro means in practice.

Who is it for? A household that wants a
shared list off the big platforms. A
small team that wants a kanban board
without renting one. A maker whose tasks
mention things a cloud provider does not
need to know. What it is not is a
corporate programme-management suite:
there are no sprints, story points or
portfolio roll-ups, and chapter 15 is
honest about the edges.

Check yourself: if you can finish the
sentence "a task lives in a ___, and a
___ is just a way of looking at it", you
have the model. The next chapter names
the pieces of the machine itself.
