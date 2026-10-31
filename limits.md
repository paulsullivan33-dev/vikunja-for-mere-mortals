# Limits

Outcome: you can name Vikunja's edges
before they name themselves for you —
the paid shelf, the phone, the single
writer, the mail-shaped hole, and the
API's measured sharp edges.

**Some features are behind a licence
key.** Since the 2.x line, starting the
server logs it without embarrassment:
`No license key configured. Pro features
have been disabled.` Pro — described on
the project's page (vikunja.io/pro) as an
optional add-on for self-hosted
instances, in private beta as this guide
was written — brings an admin panel,
audit logs and time tracking, with guest
users and invoicing announced as coming.
The honest framing matters, so here it
is: the code is the same AGPL-3.0 code,
publicly auditable; the key activates the
Pro features out of the box; the core
task manager is complete without it, and
the project states nothing is being taken
away. What it means practically: three
organisation-grade features are a
subscription, and an instance *without*
a key makes no licence-server contact at
all. `/api/v1/info` reports the state as
`enabled_pro_features: []` — the lab ran
with the empty list throughout and never
missed a chapter to it.

**The phone story is a browser story.**
There are apps and a desktop program, and
the web app works on a phone — but
Vikunja is a client of its server, not an
offline-first notebook. No connection to
your instance means no lists, no capture,
no quiet syncing later; the
offline-tolerant tools (a notes app like
Joplin, covered elsewhere in this series)
solve a different problem. Relatedly, the
one door that works a treat from other
programs is the calendar door of
chapter 10.

**One server, one writer at a time.** The
whole design is a single program keeping
a single database coherent — the `/info`
field `concurrent_writes: false` is the
fact wearing a name tag. That is a
feature at home scale (nothing to
orchestrate, backups are one folder) and
a ceiling beyond it: there is no fleet of
Vikunjas behind a load balancer in this
story, and the Postgres option widens the
database, not the architecture.

**Reminders are only as good as their
road out.** Chapter 6's lesson restated:
a reminder is data until something
carries it — SMTP you configured, the web
app open, or a calendar program holding
the CalDAV feed. An install with the
mailer off and no calendar client is a
machine for remembering things nobody is
told. This is the single most likely way
a new install disappoints, and it is a
configuration fact, not a bug.

**The API has edges, measured in this
lab.** Updates replace rather than patch
(chapter 6). Creation and deletion live
on particular verbs and routes, and the
wrong guess is a flat 404 or *Method Not
Allowed* (chapter 11). Out-of-scope
tokens are refused with an "invalid
token" message (chapter 11). Empty
buckets serialise as `tasks: null`.
None of these is fatal; all of them are
cheaper to know than to find.

**What is simply not in the box.** No
sprints, story points or time estimates
with roll-ups (Pro's time *tracking* is
a clock, not an estimate). No custom
fields. No built-in chat or documents —
it links arms with calendars and files,
it does not replace them. Attachments cap
at 20 MB by default. And multi-hundred-user
performance is a claim this guide's small
lab neither made nor tested.

None of this is hidden by the project,
which is the real point of the chapter:
every limit above was learned from the
program's own log, its info endpoint, or
its own public pages.
