# Sharing and Teams

Outcome: you know the three ways another
human gets to a project — a named share,
a team, and a public link — and you have
watched the third one work end to end.

A project starts private to its owner.
Vikunja has three doors out of that
privacy, and they differ in who carries
the key.

## Door one: a public link (run live)

Anyone with the link can open the
project — no account, no login. Links are
created per project, at a permission you
choose: **read**, **write**, or **admin**.
The lab minted a read link:

```console
$ curl -X PUT …/api/v1/projects/2/shares \
    -H "Authorization: Bearer $TOKEN" -d '{"permission":0}'
{"id":1,"hash":"<43 characters>","permission":0,
 "sharing_type":1,"password":"",…}
```

The `hash` is the link's secret — a
43-character random string that is the
whole credential — and the address is
your public URL plus `/share/` plus that
hash. The lab then proved the door opens
from outside: the share page answered a
request with **no login at all**, and the
API exchanged the hash for its own scoped
token (transcript `t13`), a pass that
names project 2 and read permission
inside itself and nothing else. A link
can carry a name (so you remember which
link you gave the neighbours) and a
**password**, making the hash a two-key
door.

The discipline follows from the design:
a link is a **bearer credential**. Whoever
holds it, holds it — a forwarded email
forwards the access. Treat hashes like
passwords, delete links from the
project's sharing settings when their job
is done, and prefer read links unless
writing is truly the point.

## Door two: named users (documented)

Sharing with a person who has an account
on your instance gives them the project
in their own list, under their own name,
at read, write or admin level — the same
three permissions as links, but attached
to an identity you can remove without
disturbing anyone else. The lab did not
exercise named sharing against a second
account; it is described here from the
project's documentation and labelled
documented-only in `criteria.md`, as the
guide's rule requires.

## Door three: teams (documented)

A **team** is a named group of accounts,
and projects are shared with teams the
same way as with people. The permission
model deserves one plain paragraph,
because it is where shared systems
usually surprise: access is granted per
project, the levels are the same three,
and a person in two teams with different
levels on one project effectively gets
the higher one — access adds up, it does
not cancel out. There is no "deny" that
overrides a grant. The lab did not create
teams; this section rests on the
documentation.

One feature the lab *could* verify
without exercising it: `/info` reports
`link_sharing_enabled: true` from the
configuration, and link sharing can be
switched off instance-wide
(`service.enablelinksharing`) for
installations where a bearer link is a
door too many.

Check yourself: mint a read link for a
throwaway project, open it in a private
browser window where you are not logged
in, and see your board as a stranger
sees it. Then delete the link and watch
the same address stop answering. Both
halves of that experiment are the feature.
