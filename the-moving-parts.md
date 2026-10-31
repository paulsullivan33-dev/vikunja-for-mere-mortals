# The Moving Parts

Outcome: given any Vikunja behaviour —
a page, a deadline in your calendar, a
backup file — you can name which part
produced it.

Vikunja's great convenience for the
learner is that there is barely any
plumbing. One program does almost
everything.

**The binary.** Vikunja ships as a single
executable program (with the web app
bundled into the same download — the
"full" package). Started with its `web`
command, that one process is:

- the **web application** your browser
  loads — the screens for lists, boards
  and settings;
- the **API server** underneath it,
  answering requests at `/api/v1/…` —
  everything the web app can do, it does
  by calling this same interface, which
  is why chapter 11 can drive the whole
  product from a script;
- the **CalDAV server** at `/dav/…`, the
  calendar dialect of the same tasks
  (chapter 10);
- and the housekeeper: background jobs
  that watch for due reminders live in
  the same process.

The same binary, run with other commands,
is also the toolbox: `vikunja dump` packs
the whole installation into a zip,
`vikunja restore` unpacks one, `vikunja
user` manages accounts from the command
line, and `vikunja migrate` and
`vikunja doctor` repair and check the
database. The lab ran `version` and
`dump` from that toolbox; the rest are
named here so you know they exist.

**The config file.** One YAML file —
`config.yml` — tells the binary where to
listen, what public address to call
itself, where its data lives and which
features are on. Almost everything has a
sensible default, so a working config is
short; chapter 4 reads a real one line by
line. Every setting can also come from an
environment variable spelled
`VIKUNJA_` + the setting's path, which is
how the container image is usually fed.

**The database.** Everything structured —
users, projects, tasks, labels, the
boards' columns, who may see what — lives
in one database. For a home or small team
that is **SQLite**: a single file,
`vikunja.db`, no database server to run.
(MySQL/MariaDB and PostgreSQL are the
other choices, for bigger installs; this
guide uses SQLite throughout, and the
[SQLite guide](https://paulsullivan33-dev.github.io/sqlite-for-mere-mortals/)
in this series explains that engine.) The
lab's database file, holding one user, two
projects and a handful of tasks, was
684 KB — most of it the empty rooms of a
fresh schema, not the data.

**The files folder.** Attachments — the
note pinned to a task, the photo of the
thing to fix — are not stuffed into the
database. They live as plain files in a
folder (the lab's was `files/` beside the
database), numbered, while the database
remembers their names and sizes.

**The log.** The server writes a running
diary — migrations applied, the mailer's
state, each warning — to its standard
output and, if configured, to log files.
Several chapters quote it, because the
log is where Vikunja confesses: that the
mailer is off, that no licence key is
set, that it is listening where you told
it to. Reading the log is a skill this
guide practises on purpose.

Nothing else is required. No separate web
server in front (a reverse proxy is a
production nicety, chapter 3), no cache,
no queue. When something misbehaves, it
is one of these five parts: binary,
config, database, files, log. The rest of
the guide is meeting them one at a time.
