# Calendars — the CalDAV Endpoint

Outcome: you can point a calendar program
at Vikunja and see your dated tasks as
calendar to-dos — and you know what that
door is, and is not, for.

**CalDAV** is the standard protocol by
which calendar programs (Thunderbird,
Apple Calendar, DAVx⁵ on Android, and
many others) read and write calendars on
a server. Vikunja speaks it for tasks: a
project becomes a calendar, and each
task with a life in time becomes a
**VTODO** — the calendar format's to-do
entry, cousin of the meeting's VEVENT.
The [Radicale guide](https://paulsullivan33-dev.github.io/radicale-for-mere-mortals/)
in this series covers a server whose
whole job is this protocol; Vikunja wears
it as a second hat.

The endpoint is on when
`service.enablecaldav` is true (the lab
set it, and `/api/v1/info` confirmed
`caldav_enabled: true`). It lives under
`/dav/` on the same address as everything
else, and it authenticates the plain old
way — your username and password, as
HTTP basic auth (or an API token where
the client allows). No bearer token; this
door predates that fashion.

The lab knocked, with the protocol's own
verbs (transcript `t11`):

```console
$ curl -X PROPFIND -u ada:<password> -H 'Depth: 0' http://127.0.0.1:18105/dav/
→ 207 Multi-Status, a WebDAV answer naming the collection

$ curl -X PROPFIND -u ada:<password> -H 'Depth: 1' http://127.0.0.1:18105/dav/projects/2/
→ the project, and one .ics file per task
```

Notice the address: `/dav/projects/2/` —
the project id from chapter 5 is the
calendar's name here. Fetching one of
those files gives the task as a calendar
client receives it (transcript `t12`):

```ics
BEGIN:VTODO
UID:0a3596f1-69d4-41d9-a59b-07ea714d98ad
SUMMARY:Sweep the floor and lay the mats
STATUS:NEEDS-ACTION
DUE:20261107T100000Z
PRIORITY:5
BEGIN:VALARM
TRIGGER;VALUE=DATE-TIME:20261107T080000Z
ACTION:DISPLAY
END:VALARM
END:VTODO
```

Read it as a checklist of chapter 6's
fields, translated: the title is the
summary, the due date is `DUE`, the undone
state is `STATUS:NEEDS-ACTION` — and the
**reminder became a VALARM**, an alarm the
calendar program itself can fire. That is
the answer to chapter 6's honesty
paragraph from the other direction: with
the mailer off, the reminder still had
somewhere real to go. Two translation
notes, observed rather than assumed:
Vikunja's internal task ids are replaced
by fresh UUIDs as calendar UIDs, and the
priority came back as `5` on the
iCalendar 1–9 scale for a task the API
called priority 2 — the two scales are
not the same ruler, and the endpoint
converts without comment.

In daily use the flow is: add the
account to your calendar program (server
address, username, password), and your
projects appear as to-do calendars. The
protocol allows writing as well as
reading — clients can complete a VTODO or
move its date, and Vikunja accepts such
changes — but the lab verified the read
direction only and says so;
`criteria.md` marks client-side editing
as documented-only.

One boundary to hold clearly: this is a
**task** calendar. It carries due dates
and alarms, not your meetings; Vikunja is
not a calendar server for events, and a
project with dateless tasks shows a
client an empty-looking calendar, because
a VTODO without dates has nothing to
stand on.

Check yourself: the PROPFIND above with
your own credentials should answer 207
and list an `.ics` per dated task. If it
answers 401, the password is wrong or the
client stripped the credentials; if 404,
CalDAV is off in the config.
