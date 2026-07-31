# Runway

A personal planner: quick-capture checklist, month calendar, and a 14-day runway that shows the shape of your workload before you read a single task.

One file, no build step, no dependencies beyond Google Fonts.

## Deploy to GitHub Pages

1. Create a new repo. Name it `yourusername.github.io` if you want it at the root domain, or anything else for a subpath.
2. Drop `index.html` in the root of the repo and push.
3. Go to **Settings → Pages**.
4. Under **Source**, pick **Deploy from a branch**, then `main` and `/ (root)`. Save.
5. Wait about a minute. It goes live at `https://yourusername.github.io` or `https://yourusername.github.io/reponame`.

## Using it

**Quick add.** Type in the box at the top and the parser pulls out the details:

| You type | It reads |
|---|---|
| `friday 5pm`, `tomorrow`, `today 9am` | due date and time |
| `in 3 days`, `next monday`, `dec 5`, `12/5` | due date |
| `every tuesday`, `every weekday`, `daily`, `every 2 weeks`, `monthly` | recurrence |
| `#classes`, `#side-project` | project tag |
| `!1` `!2` `!3` (or `!high` `!low`) | priority |

Example: `Algorithms pset every tuesday 11:59pm !1 #classes`

The preview strip under the box shows exactly what it parsed before you commit.

**Recurring tasks.** Check one off and the next instance appears with its date already set. Steps reset to unchecked. Monthly rules clamp to the shortest month, so a task due the 31st lands on the 28th in February. Weekday rules skip Saturday and Sunday. You can also set or change recurrence in the task detail panel.

**Command palette.** `Cmd+K` or `Ctrl+K`. Jump to a view, filter to a project, open a task by name, or run any action without leaving the keyboard. Arrow keys to move, Enter to run, Esc to close.

**Undo.** Deletes, bulk moves, and clearing completed tasks all show an Undo button in the toast for a few seconds. `Cmd+Z` works too.

**Move all to today.** When you have overdue tasks, the Overdue group header gets a button that reschedules the whole pile at once. Also in the command palette.

**Drag to reschedule.** Grab any task and drop it on a runway column or a calendar cell.

**Click a task** to open the detail panel: due date, time, priority, project, recurrence, steps, and notes.

**Keyboard.** `Cmd+K` palette, `N` to add, `/` to search, `Esc` to close.

## Colors mean something

The palette is the deadline system, not decoration:

- rose = overdue
- amber = due today
- periwinkle = due this week
- slate = later

## Schedule blocks

Tasks have deadlines. Schedule blocks are the other thing: work shifts, classes, practice, anything that occupies a stretch of time and repeats.

Open the **Week** view and click **Add a schedule block**, or click any empty spot in the grid to start one right there.

- **Which days** is a row of seven toggles, so any combination works. Mon Tue Thu Fri is two clicks, or use the shortcut button for it.
- **Single date** is for one-offs. Leave all seven day toggles off and pick a date instead.
- **Active from / until** bounds a block to a date range. Set a class to end in December and it stops appearing in January instead of repeating forever.

## Week view

Seven columns, one per day, with your blocks drawn as time slabs and any task that has a clock time pinned at its hour. Tasks with a date but no time sit in the all-day row across the top.

The dashed regions are your open stretches, meaning anything thirty minutes or longer between commitments. The row along the bottom totals free time per day. A red line marks the current time on today's column.

The grid auto-fits to whatever you actually have scheduled, so a 7am shift or a 10pm class pulls the window open rather than getting clipped.

Blocks appear in the month calendar too, and clicking a day there lists that day's schedule with its free total.

## Your data

By default everything lives in your browser's local storage on the device you're using. Nothing leaves the machine and there is no account. Use **Export** and **Import** to move a JSON file between devices.

## Optional: sync with Supabase

Click **Sync** in the sidebar to connect a free Supabase project. It talks to the REST and auth endpoints directly with `fetch`, so there is no SDK and no build step.

1. Create a project at supabase.com.
2. Open the SQL editor and run this:

```sql
create table public.tasks (
  id       text primary key,
  user_id  uuid not null default auth.uid()
           references auth.users on delete cascade,
  data     jsonb not null,
  updated  timestamptz not null default now(),
  deleted  boolean not null default false
);

alter table public.tasks enable row level security;

create policy "own rows" on public.tasks
  for all
  using (auth.uid() = user_id)
  with check (auth.uid() = user_id);
```

3. Under **Authentication → Providers → Email**, turn off "Confirm email" if you want to sign in right away without a confirmation step.
4. Copy your Project URL and anon public key from **Settings → API**, paste them into the Sync panel, and create an account.

After that it syncs on load, on window focus, a few seconds after any edit, and every two minutes.

**On the anon key:** it is designed to be public and is safe to commit to a public repo. Row level security is what actually protects the data, which is why the policy above matters. Without RLS enabled, the anon key would let anyone read the table.

**How conflicts resolve:** every task carries an `updated` timestamp and the newer version wins. Deletes leave a tombstone so they propagate instead of resurrecting on the next pull. This is last-write-wins, not a CRDT, so editing the same task on two devices while both are offline will keep whichever edit happened later.

## Customizing

Every color and radius is a CSS variable in the `:root` block at the top. `[data-theme="light"]` right below it overrides them for light mode. Change the accent in one place and the whole page follows.
