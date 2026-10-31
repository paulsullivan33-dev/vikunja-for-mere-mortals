# Vikunja for Mere Mortals

Most to-do apps live on someone else's
computer. Your projects, your deadlines
and your team's habits sit in a rented
database, behind a subscription, and the
export button — if there is one — decides
what you may take with you. **Vikunja**
does the same job the other way round. It
is a **to-do and project manager** you
install on a machine you control. One
small program serves the web app, the
programming interface and a calendar
endpoint; your tasks live in a database
file in a folder you can name. No account
with the makers, no subscription for the
core program, and the whole thing can be
packed into a single backup file with one
command.

This guide explains how Vikunja organises
work — **projects** holding **tasks**,
looked at through **views**: a list, a
table, a Gantt chart and a **kanban
board**. It installs the real program,
configures it with a file you can read,
and then does the everyday jobs: a
project, tasks with due dates and
priorities, coloured labels, a card moved
across the board, a reminder, a shared
link, and a calendar program reading the
deadlines back as its own to-dos.

The lab behind this guide ran genuine
**Vikunja 2.7.1**, the official release
binary, on a small Linux machine. The
binary's checksum was verified against
the checksum file shipped inside the
download itself. The server ran only on
the machine's loopback address, with its
database, files and logs in one lab
folder. Against it the lab registered a
user, built a small invented project — a
garden shed tidy-up, three tasks — and
exercised the programming interface end
to end: labels created and attached,
cards moved between board columns, a
task marked done, an API token with
deliberately narrow rights, a public
share link exchanged for a read-only
session, and a CalDAV request answered
with the tasks as calendar entries, the
reminder included as an alarm.

The lab also reports what surprised it,
because that is where a tool's real shape
shows. A brand-new account arrives with
an **Inbox** project already in it, so
the lab's first tasks landed there and
had to be remade in the right project.
Some operations accept one verb and not
another, and answer a polite "method not
allowed" until you find the right one.
Most important: sending a half-filled
task back to the API **erases** the
fields you left out — the lab watched two
due dates vanish that way, restored them
by sending the whole task, and wrote the
lesson into chapter 6 and chapter 11.

What the lab could not do, it did not
fake. It had no mail server, so reminders
were stored and seen in the calendar
feed, but never arrived as email — the
server's own log says it is not sending
them, and chapter 6 quotes it. It never
used the web app's screens beyond
fetching the page itself; everything else
was done through the API, and the guide
says so where it matters. Sharing with
named users and teams, and the paid Pro
features, are taught from the project's
documentation and labelled as such.
`criteria.md` keeps the full ledger of
what was run and what was only read.

## What this guide covers

- [**What Vikunja is**](what-vikunja-is.md) —
  the mental model: projects, tasks and
  views on a server of your own, and the
  licence it travels under
- [**The moving parts**](the-moving-parts.md) —
  one binary, three doors in (web app,
  API, calendar), one database, and a
  folder of files
- [**Installing Vikunja**](installing-vikunja.md) —
  the release download, the checksum that
  travels with it, and the version check
  that proves it runs
- [**Configuring Vikunja**](configuring-vikunja.md) —
  the config file read line by line: the
  address it listens on, the public URL
  that must agree with it, and where the
  data lives
- [**Your first project**](your-first-project.md) —
  registering, logging in, the Inbox
  every account is born with, and making
  a project of your own
- [**Tasks in depth**](tasks-in-depth.md) —
  due dates, priorities, labels made
  before they are worn, reminders, and
  the read-modify-write rule the lab
  learned the hard way
- [**Views — list, Gantt and table**](views.md) —
  the four views every project is born
  with, and what each one is for
- [**Kanban boards**](kanban-boards.md) —
  buckets, moving a card, and the done
  bucket that swallows finished tasks on
  its own
- [**Sharing and teams**](sharing-and-teams.md) —
  public links proven live, and sharing
  with named people and teams from the
  documentation
- [**Calendars — the CalDAV endpoint**](caldav.md) —
  your deadlines as calendar to-dos,
  fetched and read in this lab, alarm
  included
- [**The API and tokens**](the-api-and-tokens.md) —
  the interface the web app itself uses,
  its built-in documentation page, and
  tokens with narrow rights
- [**Backups and moving house**](backups-and-moving.md) —
  one command that packs the whole
  installation into a zip, and the
  importers for other apps' data
- [**Recipes**](recipes.md) — a project
  template worth copying, a weekly review
  from the filters, and driving Vikunja
  from a script safely
- [**The neighbours**](the-neighbours.md) —
  Todoist and Trello in the cloud,
  Nextcloud's tools, and the other
  self-hosted boards: what each does that
  Vikunja doesn't, and vice versa
- [**Limits**](limits.md) — the Pro
  features behind a licence key, no
  offline story on the phone, one server
  writing at a time, and the API's sharp
  edges, measured
- [**Troubleshooting**](troubleshooting.md) —
  the 404 that means "wrong door", the
  vanished due date, the reminder that
  never arrives, and the traps this
  guide's lab fell into
- [**Glossary**](glossary.md) — the
  terms, collected in one place

## What it doesn't cover

Running Vikunja at company scale — many
hundreds of users, the Postgres database
behind a fleet of servers — is pointed
at, not taught; this guide's one binary
and one database file are the home and
small-team shape. The web app's every
screen is not walked through click by
click: the lab drove the API, and the
screens follow the same shapes. The paid
Pro features (an admin panel, audit logs,
time tracking) were not licensed in the
lab, so they are described from the
project's own pages, never presented as
run.

## How to use this guide

Read chapter 1 for the model and
chapter 2 before installing anything —
knowing that one program is the web app,
the API and the calendar server at once
turns later confusion into a named
problem. Chapters 3 and 4 get you a
running server; chapter 5 makes an
account and a project in the same
sitting. Chapter 6 is the one to read
slowly: its read-modify-write rule is
the difference between an API that
serves you and one that quietly eats a
due date. The lab's transcripts live in
`vikunja-lab/transcripts/` and its
configuration, secret redacted, is
`vikunja-lab/config-example.yml`.

---

## Part of the series

This guide is one of the [For Mere Mortals](https://paulsullivan33-dev.github.io/mere-mortals/) guides: plain-language guides to the tools programmers and makers use.

Related guides:

- [Radicale for Mere Mortals](https://paulsullivan33-dev.github.io/radicale-for-mere-mortals/)
- [Nextcloud for Mere Mortals](https://paulsullivan33-dev.github.io/nextcloud-for-mere-mortals/)
- [NocoDB for Mere Mortals](https://paulsullivan33-dev.github.io/nocodb-for-mere-mortals/)
- [SQLite for Mere Mortals](https://paulsullivan33-dev.github.io/sqlite-for-mere-mortals/)

## License

The writing in this guide is licensed under [CC BY 4.0](https://github.com/paulsullivan33-dev/vikunja-for-mere-mortals/blob/main/LICENSE). Code examples and lab scripts are licensed under the [MIT License](https://github.com/paulsullivan33-dev/vikunja-for-mere-mortals/blob/main/LICENSE-CODE).
