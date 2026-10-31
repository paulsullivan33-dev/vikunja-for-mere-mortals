# Configuring Vikunja

Outcome: you have a config file you can
read aloud, and you know which three
lines do the heavy lifting — the address,
the public URL that must match it, and
the folder that holds everything.

Start the server without a config and it
still runs: Vikunja's defaults are a
complete opinion (listen on port 3456,
SQLite in the working folder, mailer on,
registration open). The sample file in
the download runs to some 39 KB of
commented possibilities, but a deliberate
config for a small instance is short. The
lab's — secret redacted, kept whole in
`vikunja-lab/config-example.yml` — is the
text of this chapter:

```yaml
service:
  secret: "<redacted>"
  interface: "127.0.0.1:18105"
  publicurl: "http://127.0.0.1:18105/"
  rootpath: "/home/hatch/workspace/vikunja-labwork/run"
  enableregistration: true
  enablecaldav: true
  timezone: "UTC"
database:
  type: "sqlite"
  path: "/home/hatch/workspace/vikunja-labwork/run/vikunja.db"
files:
  basepath: "/home/hatch/workspace/vikunja-labwork/run/files"
mailer:
  enabled: false
log:
  enabled: true
  path: "/home/hatch/workspace/vikunja-labwork/run/logs"
  standard: "stdout"
  database: "file"
```

Section by section:

**service** is the program's public face.

- `secret` signs the login tokens. If you
  omit it, Vikunja invents a fresh one at
  every start — which quietly logs
  everyone out at every restart. Set it,
  keep it private.
- `interface` is where the server listens:
  an address and port. The lab bound the
  loopback address only, because the lab
  machine is shared; a home server would
  usually listen on all its addresses
  (`":3456"` is the default shape) behind
  your network's usual care.
- `publicurl` is the address Vikunja
  *calls itself* — printed into links,
  used by the web app to find its own API,
  and used to decide which cross-origin
  requests to allow. The startup log shows
  the marriage working: `CORS enabled with
  origins: …, http://127.0.0.1:18105`. When
  the two disagree — the server listening
  somewhere the public URL doesn't point —
  the web app loads and then cannot talk
  to its own backend, the classic
  first-evening failure. Keep them in
  step, port included, trailing slash
  included.
- `rootpath` names the home folder:
  database, files and default config
  search all live relative to it.
- `enableregistration` is exactly what it
  says; turn it off once your household
  has its accounts. `enablecaldav` opens
  the calendar door of chapter 10.
  `timezone` names the zone timestamps are
  stored against — use a real tz name
  (the sample warns that bare offsets like
  `UTC+2` will not work; `UTC` will).

**database** picks the engine. `sqlite`
plus a `path` is the whole story for this
guide. `mysql` and `postgres` add user,
password, host and database name lines in
the obvious places.

**files** says where attachments live and
implies their ceiling: the default size
limit is 20 MB per file, which the `/info`
endpoint repeats to every visitor.

**mailer** is the honest switch of this
guide. With `enabled: false`, Vikunja
says so twice in its startup log —
`Mailer is disabled, not sending reminders
per mail` — and reminders still exist as
data (chapter 6 sets one and chapter 10
sees it in the calendar feed), they simply
have no way out of the building. A real
install fills in `host`, `port`,
`username`, `password` and `fromemail`
for an SMTP account and flips this to
true; the binary's `testmail` command
checks the result.

**log** decides where the diary goes. The
lab kept the standard log on the console
and let database chatter go to a file;
either way, chapter 2's advice stands:
when in doubt, read the log.

Two last mechanics. Settings can also
arrive as environment variables —
`VIKUNJA_SERVICE_PUBLICURL`, and so on,
one per config path — which is the normal
way to feed the Docker image. And the
file's location is your choice: the
`--config` flag pins it, otherwise
Vikunja searches its rootpath and the
usual system places. Check yourself: the
log's CORS line should name your public
URL back at you. If it doesn't, the two
addresses have drifted apart already.
