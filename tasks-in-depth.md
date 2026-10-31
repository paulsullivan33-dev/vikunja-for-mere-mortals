# Tasks in Depth

Outcome: you can give a task every field
that matters — due date, priority, labels,
a reminder — and you know the one rule
that keeps the API from quietly eating
the fields you already set.

## Creating a task

A task is born into a project, again with
`PUT`:

```console
$ curl -X PUT http://127.0.0.1:18105/api/v1/projects/2/tasks \
    -H "Authorization: Bearer $TOKEN" \
    -d '{"title":"Take the old paint tins to the recycling centre",
         "description":"Check which tins are still usable first.",
         "due_date":"2026-11-05T09:00:00Z","priority":5}'
```

Dates travel as ISO timestamps
(`2026-11-05T09:00:00Z`). The reply is the
whole task, an object with more fields
than you sent: `done: false`, an
`identifier` like `#2` (the task's number
inside its project), `percent_done`,
`repeat_after`, room for assignees,
labels, attachments and relations. Fields
you never set wear their zero values
openly — an unset date reads
`0001-01-01T00:00:00Z`, an unset priority
`0` — which matters in a moment.

**Priority** is a small integer. In the
lab's build the scale ran 0 (unset) to 5;
the guide's fixture used 2 for ordinary
work and 5 for the job that gated the
rest. Treat the numbers as your team's
convention, not physics.

## Labels are made before they are worn

A label is its own object — a title and a
colour — shared across your projects. You
cannot invent one inside a task; you
create it first (another `PUT` birth):

```console
$ curl -X PUT http://127.0.0.1:18105/api/v1/labels \
    -d '{"title":"errand","hex_color":"1f8feb"}'
{"id":1,"title":"errand","hex_color":"1f8feb"}
```

then pin it on: `PUT /api/v1/tasks/5/labels`
with `{"label_id":1}`. The lab's transcript
`t04` shows both labels attached and read
back on their tasks. Two honest wrinkles
from the same transcript: creating a label
with `POST` instead of `PUT` earns a flat
`{"message":"Method Not Allowed"}`, and the
attach call's reply is the label-task link
record, not the label — read the task back
if you want to see the result dressed.

## Reminders

A reminder is a small object on the task:
an absolute time, or a period relative to
the due date (an hour before, a day
after). The lab set an absolute one —
`{"reminders":[{"reminder":"2026-11-07T08:00:00Z"}]}`
— and it was stored and returned exactly.

Now the honesty paragraph, because it is
the one readers most need. A stored
reminder is not a delivered reminder. On
this lab's server, with the mailer off,
the startup log said it plainly:
*Mailer is disabled, not sending reminders
per mail.* Nothing arrived anywhere; the
reminder existed as data, and reappeared
in exactly one other place — as an alarm
inside the calendar entry of chapter 10.
With a working SMTP config (chapter 4)
the same reminder would also leave as
email, and the web app can show its own
notifications; but "Vikunja will remind
me" is a claim about your *mail setup*,
not about the task. Check the log line
before you rely on it.

## The read-modify-write rule

Here is the chapter's real lesson, learned
by losing data. Updating a task is
`POST /api/v1/tasks/{id}` — and the body
you send is taken as **the whole truth**
about the editable fields, not as a patch.
The lab sent `{"reminders":[…]}` to one
task and `{"done":true}` to another, and
both updates worked — while silently
resetting each task's **due date and
priority to their zero values**, because
those fields were not in the body. The
labels survived (they live in a link table,
not on the task), the reminder survived
(it was the body), and everything else
scalar was re-zeroed. Transcripts `t08`
and `t09` keep the before, the damage and
the repair.

The repair is the rule: **read the task,
change your fields in the whole object,
send the whole object back.** Three lines
with `curl` and `jq`:

```console
$ curl -s …/api/v1/tasks/4 -H "Authorization: Bearer $TOKEN" \
    | jq '.due_date="2026-11-07T10:00:00Z" | .priority=2' > task.json
$ curl -X POST …/api/v1/tasks/4 -H "Authorization: Bearer $TOKEN" \
    -d @task.json
```

With that, both fixture tasks got their
dates and priorities back and kept their
reminder and done state. The web app, of
course, does this for you invisibly —
which is precisely why script writers
must know it happens.

## Marking done

`{"done":true}` — sent whole, per the
rule — stamps `done_at` with the moment
and, as chapter 8 shows, walks the card
into the board's Done column by itself.
Repeating tasks (a `repeat_after` period)
respawn their next copy when completed;
the lab did not exercise them and says so.

Check yourself: create a task with a date
and a priority, read it back, change only
its title *by the whole-object route*, and
read it again — date and priority intact.
If they are gone, you patched; the rule is
the fix.
