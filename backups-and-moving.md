# Backups and Moving House

Outcome: you can pack a whole Vikunja —
users, projects, tasks, files, even the
config — into one zip, say what a backup
must contain and why, and name the doors
through which other apps' data comes in.

## What has to survive

An instance is three things on disk
(chapter 2): the **database file**, the
**files folder** of attachments, and the
**config**. The lab's whole installation,
spread across its folder (transcript
`t15`):

```console
run/
  config.yml
  vikunja.db          (684 KB)
  files/1             (the one attachment, stored by number)
  logs/
```

Back up those three and you have
everything except the program itself,
which you can download again. The one
trap: copy the database while the server
is stopped, or use the tool below — a
SQLite file copied mid-write can be a
photograph of half a sentence. (The
`-wal` and `-shm` companions beside
`vikunja.db` are the database's working
papers; a stopped server folds them in.)

## The one-command way

Vikunja brings its own:

```console
$ vikunja --config /path/to/config.yml dump
… msg="Dump file saved at …/vikunja-dump_2026-10-31_01-01-36.zip"
```

The zip the lab made is instructive to
open. The database travels as a set of
JSON files, one per table — `tasks.json`,
`projects.json`, `users.json`,
`link_shares.json`, even
`api_tokens.json` — alongside the config
and the raw attachment files. Two
consequences follow. The dump is
**database-agnostic**: a dump from a
SQLite instance can be restored into a
PostgreSQL one, because JSON does not
care. And it is **inspectable**: you can
read your own tasks out of it with a text
editor, which is what an exit door should
feel like.

Restoring is the mirror command,
`vikunja restore`, aimed at the zip. The
lab did not perform a restore —
`criteria.md` lists it as documented-only
— but every ingredient of one was
produced and listed in this chapter, and
the shape leaves little to mystery.

A backup habit that fits this tool: a
nightly `dump` to another folder (or
another machine) keeps history as a row
of dated zips, each one a complete
instance. Dumps contain the config,
secret included — store them with the
care that implies.

## Moving in: the migrators

The door in the other direction is the
set of importers. The lab asked its
server which it carried — `/api/v1/info`
answers with `available_migrators`, and
for v2.7.1 the list is exact:

- **vikunja-file** — another instance's
  dump (the zip above);
- **ticktick** — an export from TickTick;
- **wekan** — the self-hosted Wekan
  boards;
- **planka** — the self-hosted Planka
  boards;
- **csv** — a spreadsheet-shaped file,
  the universal adapter.

That list is version-specific and worth
checking on your own server rather than
trusting any guide — the lab's transcripts
show the query, and the project's site
has described a wider set across versions
(Todoist and Trello among them in other
builds and routes). What a migration
brings over is the structure — projects,
tasks, dates, labels where the source has
them; attachments and comment histories
are the usual casualties in every tool's
importer, so keep the old export until
the new home is proven.

Check yourself: run a dump, unzip it in a
scratch folder, and find your fixture
project inside `projects.json` and your
tasks in `tasks.json`. When you can read
your data with the server off, you own
it.
