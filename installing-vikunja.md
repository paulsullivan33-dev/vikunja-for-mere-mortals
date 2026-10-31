# Installing Vikunja

Outcome: Vikunja is downloaded from the
project itself, its integrity is checked
the way the project provides for, and
`vikunja version` answers with a number.

## What you need

A Linux machine (the same program is
published for Windows, macOS and the
small ARM boards), a few dozen megabytes
of disk, and a folder the program may
own. Nothing else: no database server,
no runtime, no web server. The lab used
an ordinary shared Linux sandbox and the
program's whole working folder — database,
files, logs and all — stayed under a
couple of megabytes for a small instance.

## The download

Vikunja publishes its releases on its
GitHub project page, `go-vikunja/vikunja`,
and mirrors them at `dl.vikunja.io`. The
package you want for a server is the one
whose name ends in `-full`: it contains
both the program and the bundled web app.
The lab fetched
`vikunja-v2.7.1-linux-amd64-full.zip`
(49 MB) from the project's GitHub release
**v2.7.1**.

Inside the zip are four things: the
program itself, a licence file, a large
sample configuration — and a small file
named after the program with `.sha256`
after it. That file is Vikunja's own
checksum for the binary, and checking it
is the honest verification this download
offers:

```console
$ cat vikunja-v2.7.1-linux-amd64.sha256
517b6b3d43c0b472af69c01ab8a1b412c3bf4c45c20cfba357db3c7cd1200ff8  vikunja-v2.7.1-linux-amd64
$ sha256sum vikunja-v2.7.1-linux-amd64
517b6b3d43c0b472af69c01ab8a1b412c3bf4c45c20cfba357db3c7cd1200ff8  vikunja-v2.7.1-linux-amd64
```

The two lines agree, so the binary is the
one the project built. Note what this
does and does not prove, because the
guide's habit is to say it exactly: the
project publishes **no checksum for the
zip itself** — the check covers the
program inside it, which is the part that
will run. The lab's computed hash of the
zip, for the record, is in transcript
`t00-download-and-version.txt`.

Make the program executable if your
unzipper forgot (`chmod +x`), then ask it
who it is:

```console
$ ./vikunja version
Vikunja api version v2.7.1
Built with go1.27.3
```

That is the install. The program also
ships as a `.deb` and `.rpm` package (the
same release page carries them), as an
official **Docker image** — the route
most home servers take, where the same
config arrives as environment variables
and a volume holds the data folder — and
inside several app-store style catalogues
for home-server systems. Those routes are
documented by the project and were not
the lab's; the binary route is the one
this guide verifies, and everything after
this chapter behaves the same whichever
door you came in by.

## First start

The server starts with one command, given
a config file (next chapter writes it):

```console
$ ./vikunja --config /path/to/config.yml web
```

On first start it builds its database —
"log: Running migrations…" — finds no
licence key and says so, notes the mailer
is off because the config says so, and
ends with the line you are waiting for:
`HTTP server listening on 127.0.0.1:18105`
(the lab's address; yours is chapter 4's
business). Leave it running: every
following chapter talks to it. In
production you would let systemd or the
container runtime keep it alive and put a
reverse proxy with HTTPS in front; the
mechanics of the app do not change.

Check yourself: `vikunja version` answers,
and the startup log ends with a listening
line. If instead it complains about the
config, chapter 16 starts there.
