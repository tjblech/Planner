# Runway

A fast, local-first personal planner for tasks, deadlines, and the next 14 days.

Runway combines a task checklist, calendar, project filters, natural-language task entry, and a visual two-week runway in a single static web app. It has no build step and stores data in the browser.

## Features

- Natural-language task creation
  - Dates such as `today`, `tomorrow`, `friday`, `in 3 days`, `Dec 5`, or `12/5/2026`
  - Times such as `5pm`, `5:30pm`, or `17:00`
  - Priorities using `!1`, `!2`, or `!3`
  - Projects using tags such as `#classes`
- Today, upcoming, calendar, all-task, and completed views
- Drag-and-drop scheduling onto runway or calendar dates
- Task details, notes, and subtasks
- Project filtering and task search
- Light and dark themes
- JSON import and export
- Browser-local persistence with `localStorage`
- Responsive mobile layout

## Try it locally

No installation is required. Open `index.html` in a modern browser.

For the most reliable browser behavior, serve the folder locally:

```bash
python -m http.server 8000
```

Then open `http://localhost:8000`.

## Example task syntax

```text
Finish OS lab friday 5pm !1 #classes
```

This creates a high-priority task due Friday at 5:00 PM in the `classes` project.

## Deploy with GitHub Pages

1. Push this repository to GitHub.
2. Open the repository's **Settings**.
3. Select **Pages**.
4. Under **Build and deployment**, choose **Deploy from a branch**.
5. Select the `main` branch and the `/ (root)` folder.
6. Save and wait for the Pages URL to appear.

Because this is a static app, no framework or build command is needed.

## Data and privacy

Task data stays in the current browser's local storage unless it is manually exported. Data does not automatically sync between browsers or devices.

## Project structure

```text
runway-planner/
├── index.html
├── README.md
├── LICENSE
├── .gitignore
└── .nojekyll
```

## License

MIT
