# Troubleshooting

Outcome: the failures this guide's lab
actually met — and the common first-evening
ones its evidence explains — each with the
check that names it.

## "Method Not Allowed"

You used the wrong verb for a birth or a
read. In v2.7.1 creations are `PUT`
(projects, tasks, labels, tokens,
shares); a bucket's tasks are not read
bucket-by-bucket at all. **Check:** the
phrasebook in chapter 11, then the
server's own `/api/v1/docs` for your
version's routes. The lab met this twice:
creating labels with `POST`, and asking
bucket 7 for its tasks directly.

## 404 on an update or delete that "should" exist

Some operations live on top-level routes.
`POST /projects/2/tasks/5` and
`DELETE /projects/1/tasks/2` both answered
404 in the lab while `POST /tasks/5` and
`DELETE /tasks/2` worked. **Check:** when
a nested route 404s on a thing you can
*read*, try the flat route for the thing
itself.

## The task's due date (or priority) vanished

You sent a partial update. `POST /tasks/{id}`
takes the body as the whole truth for
scalar fields; what you omitted came back
as zero values (chapter 6). **Check:**
`GET` the task — a date of
`0001-01-01T00:00:00Z` is the fingerprint.
**Fix:** read-modify-write; send the whole
object every time. Labels surviving while
dates die is the same event, not a second
bug: labels live in a link table.

## Tasks landed somewhere unexpected

You created them against project 1, or
without a project, and the **Inbox**
collected them (chapter 5). **Check:**
`GET /api/v1/projects` and look at the
Inbox's tasks before searching anywhere
else. Negative ids in the list are
filters, not filing cabinets — a task
cannot be "in" one.

## The web page loads but shows nothing / cannot log in

The public URL and the listening address
have drifted apart (chapter 4). The web
app is calling the API at the public URL;
if that names the wrong host or port, the
page is a shop window with no shop.
**Check:** the startup log's CORS line
should repeat your public URL, and
`GET /api/v1/info` should answer at the
address in the browser's bar.

## A reminder never arrived

Follow the roads out (chapters 6 and 15).
**Check, in order:** the startup log for
`Mailer is disabled`; the mailer section
of the config; `vikunja testmail` once
SMTP is filled in; and whether any
calendar client holds the CalDAV feed,
where the reminder exists as an alarm
even when email is off.

## "Invalid token" — but the token is right there

Two different animals share this message
(chapter 11): a login JWT that expired
(three days is shorter than a holiday) or
was orphaned by a restart with an unset
`service.secret` (chapter 4 — set the
secret and restarts stop logging everyone
out); and an **API token doing something
outside its permissions**, which this API
also reports as an invalid token.
**Check:** the token's own record — its
expiry and its permission set — before
suspecting its spelling.

## A script reads an empty board that isn't

A bucket with no cards serialises as
`"tasks": null`, and a `jq` filter that
iterates it stops with an error that
looks like a broken board. **Check:**
guard with `// []` (transcript `t08`).

## The server will not start / the port is taken

The log names the cause: a config it
cannot parse, a database path it cannot
write, or `address already in use` when
another program — very possibly yesterday's
Vikunja — holds the port. **Check:**
`ss -tlnp` for the port, and read the
first error in the log, not the last.

## The dump "misses" a table

It doesn't — the dump spreads the database
across one JSON file per table
(chapter 12), so attachments are under
`files/`, link shares in
`link_shares.json`, and so on. Count the
zip's entries (the lab's had 45) before
concluding anything is absent.
