# Writing criteria for this guide

Every "for Mere Mortals" guide is written to the same nine-point
standard, so a reader who trusts one can trust the rest. Here it
is, with the verification notes for this particular guide below.

1. **Fix it first.** Reader outcomes lead, not action items: each
   chapter opens by naming the outcome it delivers, shows a check
   the reader can run to confirm they got it, and a reasonable
   reader finishes able to state what was done, what it means,
   and what to try next.
2. **Plain language.** Short sentences, everyday vocabulary,
   technical terms defined on first use. No slang, and no
   idioms that only make sense in one country — the reader may
   be translating as they go.
3. **No dead ends.** A guide never tells the reader to do
   something it cannot show them how to verify. Every claim a
   reader relies on — an install, a command, an output — is
   something this guide demonstrates or clearly labels
   documented-only below.
4. **Live-verified where it matters.** Procedures are run for
   real in the guide's lab wherever the build machine allows it,
   and the transcripts kept in the repository are the evidence.
   What could not be run is listed honestly below, never
   presented as if it had been.
5. **General audiences.** Nothing in this guide — examples,
   fixtures, names, addresses — is modelled on any particular
   reader or author's real life, network, or organisations.
   The lab's project is an invented shed tidy-up.
6. **Standalone.** A reader needs this guide, its lab, and
   nothing else. Where another guide would genuinely help, it
   is pointed to once, by name, and the chapter still completes
   without it.
7. **Honest about limits.** Chapter 15 names what the tool
   cannot do and what sits outside the free core: the Pro
   features behind a licence key, reminders that go nowhere
   without a mail or calendar road out, a single writer
   architecture, no offline phone story, and the API's
   replace-don't-patch update rule, all measured in the lab.
8. **The ethics rule is in the text, early.** A share link is
   a bearer credential, so chapter 9 states the discipline
   where the feature is taught: treat hashes like passwords,
   prefer read links, delete links when their job is done.
   The lab's own secrets were handled the same way — its
   config is kept with the secret redacted, its tokens are
   redacted in every transcript, and the one credential that
   appears in full, the fixture user's password, belongs to a
   throwaway account on a server that no longer exists.
9. **Builds clean.** The site builds with MkDocs in strict mode
   with no warnings, and a fresh reader following the chapters
   in order reaches the stated outcome of each.

## Verification notes for this guide

The build machine is a small Ubuntu 24.04 sandbox, shared with
other work while the lab ran (timings are therefore ranges, and
the guide says so). Vikunja **2.7.1** — the current release,
dated 2026-10-20 — was downloaded as the official
`vikunja-v2.7.1-linux-amd64-full.zip` from the project's GitHub
release page. The project publishes **no checksum for the zip
itself**; the zip carries a `.sha256` sidecar for the binary
inside it, and that check was run and matched
(`517b6b3d…1200ff8`); the lab's own computed hash of the zip is
recorded in transcript `t00` for the record. The binary ran
unmodified, from a lab folder, listening only on the loopback
address (port 18105), with SQLite as its database, the mailer
disabled, and CalDAV enabled. Its config is kept, secret
redacted, as `vikunja-lab/config-example.yml`. The server was
shut down at the end of the lab and the port verified free
(transcript `t16`). All requests were made with `curl` against
the API on that loopback address; no Vikunja instance belonging
to anyone else was involved, nothing was published anywhere,
and no account exists outside the lab folder.

Run for real, with transcripts as evidence:

- Version, the startup sequence (migrations, SQLite path, the
  no-licence-key warning, both mailer-disabled lines, the CORS
  origins line, the listening line) (t00, t00b).
- `GET /api/v1/info` in full: version, registration and CalDAV
  flags, the five available migrators, `enabled_pro_features:
  []`, `concurrent_writes: false` (t01).
- Registration and login of the fixture user; the register
  reply's null email; the login JWT, redacted in the
  transcript (t01).
- The pre-existing projects of a fresh account: the Inbox
  (id 1) and the filter project *My Open Tasks* (id −2) (t02).
- Project creation by `PUT`, arriving as id 2 (t02); three
  tasks first created into the Inbox by mistake and deleted —
  the nested delete route answering 404, the flat
  `DELETE /tasks/{id}` succeeding (t02, t03, t04).
- Labels: `POST` refused with "Method Not Allowed", creation
  by `PUT`, attachment via `PUT /tasks/{id}/labels`, read back
  on their tasks (t02, t03, t04).
- The four default views of a project and the kanban view's
  three default buckets, with the bucket limit field (t05).
- Reading a bucket's tasks directly: 405 for all three
  buckets; reading the whole board via the view's task list,
  buckets with tasks nested, empty buckets as `"tasks": null`
  (t06, t07, t08).
- Moving a task to the Doing bucket by `POST`, verified by
  reading the board back (t06, t08).
- An absolute reminder stored on a task (t08); the startup
  log's statement that no reminder mail is sent with the
  mailer off (t00b, quoted in t07).
- Partial-update data loss, twice: a reminders-only update and
  a done-only update each reset the task's due date and
  priority to zero values; both repaired by read-modify-write
  of the whole task object (t08, t09).
- Marking a task done, and the done card appearing in the
  Done bucket without being moved there (t08).
- API tokens: creation refused until both `expires_at` and
  `permissions` were supplied (error 2002 naming both fields);
  a read-only token created (value redacted) and used to read
  projects; the same token refused a task creation with a 401
  "invalid token" message (t10).
- A public read link for the project: created by `PUT`, its
  page answering with no login, and the hash exchanged at
  `/shares/{hash}/auth` for a scoped token (t13).
- A file attachment uploaded to a task (multipart `PUT`,
  76-byte text fixture) (t13).
- CalDAV: `PROPFIND` on `/dav/` (207) and on
  `/dav/projects/2/` listing one `.ics` per task; one `.ics`
  fetched whole — the task as a VTODO with its due date,
  converted priority, and the reminder as a VALARM (t11, t12).
- The web app's root page fetched (t14); the API's built-in
  documentation endpoints `/api/v1/docs` and
  `/api/v1/docs.json` confirmed answering, and two guessed
  specification paths confirmed *not* (t14).
- `vikunja dump` run for real; the zip's 45 entries listed,
  including the database as per-table JSON, the config, and
  the attachment file; the installation's on-disk layout
  listed and measured (t15).

Documented only — described from the project's documentation
or its public pages, not run, and the text does not claim
otherwise:

- The web app used as a person uses it. The lab fetched the
  page and drove the API under it; click-by-click screen
  tours, drag-and-drop on the board, and the Gantt chart
  rendered are described, not performed.
- Sharing a project with named users, and teams — the model
  (three permission levels, grants adding up) is taught in
  chapter 9 from the documentation; only link sharing was
  exercised live, because it needed no second account.
- `vikunja restore`. The dump it consumes was produced and
  dissected; the restore itself was not performed.
- Installation routes other than the release binary: the
  `.deb`/`.rpm` packages, the Docker image, catalogue
  installs, and running under systemd with a reverse proxy
  and TLS in front.
- Email delivery of reminders (no mail server exists in the
  lab), repeating tasks respawning, task relations, comments,
  reactions and time entries; LDAP and OpenID Connect login;
  CalDAV *writes* from a calendar client; MySQL and
  PostgreSQL backends.
- The Pro features (admin panel, audit logs, time tracking)
  and their licensing mechanics, characterised in chapter 15
  from the project's own Pro pages (vikunja.io/pro and
  vikunja.io/docs/pro, fetched 2026-10-31) and from what the
  lab's server itself reported: the startup warning and
  `enabled_pro_features: []`.
- The neighbours of chapter 14 — characterised, not run or
  benchmarked.

Fixtures are this guide's own and deliberately generic: one
invented user, an invented garden-shed project with three
tasks, two labels, and a four-line shopping note as the
attachment — no real people, organisations or events.
