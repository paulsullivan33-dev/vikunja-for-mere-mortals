# Views — List, Gantt and Table

Outcome: you know the four views every
project is born with, what each is for,
and why they can never disagree with
each other.

Ask a new project for its views and the
answer is always the same four
(transcript `t05`):

| View | Kind | What it shows |
| --- | --- | --- |
| List | `list` | Tasks one under another, in order |
| Gantt | `gantt` | Tasks as bars along a calendar |
| Table | `table` | Tasks as rows, fields as columns |
| Kanban | `kanban` | Cards in columns — next chapter |

```console
$ curl http://127.0.0.1:18105/api/v1/projects/2/views -H "Authorization: Bearer $TOKEN"
[{"id":9,"title":"List","view_kind":"list",…},
 {"id":10,"title":"Gantt","view_kind":"gantt",…},
 {"id":11,"title":"Table","view_kind":"table",…},
 {"id":12,"title":"Kanban","view_kind":"kanban",
  "bucket_configuration_mode":"manual",…}]
```

A view is a stored configuration — a
filter, a sort, a choice of columns —
with its own id and its own address. It
holds **no task data**. That is the fact
this whole chapter hangs on: the list and
the board cannot disagree about whether
the paint tins went to the recycling
centre, because both read the same tasks
from the same database at the moment you
look. When the lab marked a task done
(chapter 6), the list view and the board
both showed it, in the same second,
because there was only ever one task.

**The list** is the home view: tasks in
order, done ones struck through, the
quick-add box at the top in the web app.
Its API face is `GET /projects/{id}/tasks`,
which the lab used as its ground truth
throughout.

**The Gantt chart** draws each task as a
bar from its start date to its end date
along a timeline. It is the view with an
opinion about your data: a task with no
dates has nothing to draw. This is the
practical reason chapter 6's due dates
matter beyond reminding — they are what
the Gantt view eats. The lab verified the
Gantt view exists on every project and
what fields it needs; it did not render
the chart (the lab drove the API, not the
browser), and will not pretend otherwise.

**The table** is the spreadsheet face:
one row per task, one column per field —
priority, due date, labels, assignees —
sortable and the natural place to scan a
big project for the task whose date is
wrong. If you arrive from a spreadsheet
tool, this is your door in; the
[NocoDB guide](https://paulsullivan33-dev.github.io/nocodb-for-mere-mortals/)
in this series covers the tool that is
*only* this idea.

**Filters and extra views.** A view's
filter can narrow it — only tasks with
this label, only what is due this week —
and you can save further views of your
own beside the four. The whole family's
extremes are the negative-id "projects" of
chapter 5: a saved filter with no project
under it at all.

Check yourself: in the web app, open one
project and click through its four view
tabs. Nothing is copied between them —
finish a task in one and watch the
others agree. That agreement is the
design, and chapter 8's board is just the
most theatrical of the four windows.
