# Your First Project

Outcome: an account exists, you hold a
login token, and a project of your own is
on the server — plus you know about the
project you were given before you asked.

Everything in this chapter is done through
the API, the interface under the web app,
because it can be shown exactly. In the
browser the same steps are a registration
form and a *New project* button; the
shapes underneath are these.

## Register and log in

Registration is one request (the fixture
password is the lab's, shown because the
account is the lab's too):

```console
$ curl -X POST http://127.0.0.1:18105/api/v1/register \
    -d '{"username":"ada","email":"ada@example.org","password":"correct-horse-battery"}'
{"id":1,"username":"ada","email":null,"created":"2026-10-31T01:48:10Z"}
```

Two things to notice. The answer carries
no email address — Vikunja does not echo
it back even to its owner — and there is
no confirmation step in this lab, because
email confirmation rides on the mailer,
which chapter 4 switched off. Logging in
trades the password for a **token**:

```console
$ curl -X POST http://127.0.0.1:18105/api/v1/login \
    -d '{"username":"ada","password":"correct-horse-battery"}'
{"token":"<a JWT — three dot-separated parts, redacted>"}
```

That token is the key to every later
request, sent as `Authorization: Bearer
<token>`. It is a signed pass with an
expiry (three days by default; thirty if
the login asks to be remembered), not a
password — but while it lasts it opens
everything the account can open, so the
transcripts redact it and so should you.

Before making anything, ask the server
about itself. `GET /api/v1/info` needs no
login and reports the version, whether
registration is open, whether CalDAV is
on, which importers exist, and — hold
this for chapter 15 — `enabled_pro_features:
[]`. It is the health page scripts check
first, and this guide quotes it twice.

## The project you already have

Now list the projects:

```console
$ curl http://127.0.0.1:18105/api/v1/projects -H "Authorization: Bearer $TOKEN"
```

The lab expected an empty list. It got
three entries: **Inbox** (id 1), the
project it was about to create (id 2), and
**My Open Tasks** (id −2). Both surprises
are permanent features, so learn them now:

- Every account is born with an **Inbox**
  project. It is the landing place for
  tasks created without a chosen project —
  quick captures, calendar imports, the
  lab's own first three tasks, which were
  aimed at "project 1" out of habit and
  landed in the Inbox before being remade
  in the right place (transcripts `t02`
  and `t03` keep the whole detour).
- Some projects are **filters wearing a
  project's clothes**: negative ids, like
  *My Open Tasks*, are saved searches that
  gather matching tasks from everywhere.
  You cannot file a task into one; it is a
  window, not a room.

## Make the project

Creating is a `PUT` — keep that verb in
mind, it is this API's habit for births:

```console
$ curl -X PUT http://127.0.0.1:18105/api/v1/projects \
    -H "Authorization: Bearer $TOKEN" \
    -d '{"title":"Shed Tidy-Up","description":"A small invented project: clear, sort and label everything in the garden shed."}'
{"id":2,"title":"Shed Tidy-Up",…}
```

(The fixture is invented, as every example
in this series is: any small real job of
yours — a room, a trip, a newsletter —
shapes a project the same way.) A project
arrives with more than a name: four views
and a fresh kanban board are created with
it, as chapter 7 shows. It can be given a
colour, a background, an archive switch
that freezes it read-only when the job is
over, and a place under another project.

Check yourself: `GET /api/v1/projects`
shows your project with a positive id of
its own, distinct from the Inbox's 1. If
your tasks later seem to vanish, the
first question is always chapter 16's:
*did they land in the Inbox?*
