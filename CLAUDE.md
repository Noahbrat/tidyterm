# tidyterm

A single-page static web app that cleans terminal output for pasting into non-monospace contexts. Hosted on GitHub Pages.

## Architecture

- Single `index.html` file with inline CSS and JS — no frameworks, no build tools, no dependencies.
- All processing is client-side. Nothing leaves the browser.

## Cleaning Rules

Applied in order: strip trailing whitespace → dedent leading padding → convert box-drawing tables to markdown → collapse excess blank lines. Each rule can be toggled independently by the user.

## Development

Open `index.html` directly in a browser. No build step, no server needed.

## Conventions

- Keep everything in one file unless there's a strong reason to split.
- No npm, no frameworks, no external dependencies.
- Must work in Chrome, Firefox, Safari.
- Dark theme, monospace font throughout.
