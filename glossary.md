# Glossary

**AGPL (GNU Affero General Public License)**
The licence Vikunja is published under —
version 3, or later at your option. Free
to run, study and modify; if you offer a
modified version to others over a network,
you must offer them the source.

**API (application programming interface)**
The machine-facing door of a program.
Vikunja's lives at `/api/v1/…` and does
everything the web app does.

**API token**
A long-lived key for scripts, beginning
`tk_`, shown once at creation, carrying a
chosen set of permissions and an expiry
date (chapter 11).

**Attachment**
A file pinned to a task. Stored as a file
in the files folder; the database
remembers its name, size and owner.

**Bucket**
A column on a kanban board — *To-Do*,
*Doing*, *Done*. A card sits in exactly
one bucket per board (chapter 8).

**CalDAV**
The standard protocol calendar programs
use to exchange calendars and to-dos.
Vikunja's endpoint at `/dav/…` presents
projects as calendars of tasks
(chapter 10).

**Done bucket**
The one bucket per board designated as
the finish line. Marking a task done
moves its card there, and moving a card
there marks it done.

**Dump**
The single zip produced by
`vikunja dump`: config, the database as
one JSON file per table, and all
attachment files (chapter 12).

**Filter project**
A saved search wearing a project's shape,
with a negative id (*My Open Tasks*,
id −2). A window onto tasks, not a place
they live.

**Inbox**
The project every account is born with
(id 1 on a fresh instance). Tasks created
without a chosen project land here.

**JWT (JSON Web Token)**
The signed pass a login returns. Three
dot-separated parts; carries its own
expiry; invalidated at once if the
server's `service.secret` changes.

**Kanban**
A way of running work as cards in columns
named for stages, with limits on how many
cards a column may hold.

**Label**
A coloured tag of your own making, shared
across projects. Labels are created
first, then attached to tasks
(chapter 6).

**Link share**
A public address for a project:
public URL + `/share/` + a 43-character
hash, at read, write or admin permission.
The hash is a bearer credential
(chapter 9).

**Migrator**
An importer for another tool's export.
This build's `/info` lists vikunja-file,
ticktick, wekan, csv and planka
(chapter 12).

**Priority**
A task's urgency number: 0 unset, then a
small scale (the lab used 2 and 5). Not
the same ruler as the calendar world's
1–9 priority, as chapter 10 shows.

**Pro features**
The optional paid shelf of the 2.x line —
admin panel, audit logs, time tracking —
activated by a licence key; the code is
the same AGPL code (chapter 15).

**Project**
A named collection of tasks, possibly
nested inside another project. The middle
noun of the whole model (chapter 1).

**Public URL**
The address Vikunja calls itself
(`service.publicurl`). Must agree with
where it actually listens, or the web app
cannot reach its own API (chapter 4).

**Reminder**
A time attached to a task: absolute, or a
period relative to its due date. Data
until something carries it — email, the
web app, or a calendar alarm
(chapters 6, 10).

**View**
A stored way of looking at a project's
tasks — list, Gantt, table or kanban.
Views hold no data and so can never
disagree (chapter 7).

**VTODO**
The calendar format's to-do entry: what a
task becomes at the CalDAV endpoint,
due date and alarm included.
