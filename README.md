# tidyterm

Clean up terminal output for pasting anywhere.

**Live:** [https://noahbrat.github.io/tidyterm](https://noahbrat.github.io/tidyterm)

## What it does

When you copy text from a terminal emulator (Claude Code, tmux, SSH sessions, Docker logs, etc.), the clipboard often includes:

- **Trailing whitespace** padding every line to the full terminal width
- **Leading whitespace** from terminal frame padding
- **Box-drawing characters** (`┌─┬─┐`) that look like garbage in non-monospace contexts
- **Excess blank lines** from terminal padding

tidyterm fixes all of that. Paste your terminal output in, get clean text out.

## Usage

1. Paste terminal output into the left panel
2. Cleaned output appears instantly on the right
3. Click **Copy** (or press `Cmd+Shift+C` / `Ctrl+Shift+C`)

## Cleaning rules

Each rule can be toggled on/off. Preferences are saved to localStorage.

| Rule | What it does |
| --- | --- |
| Strip trailing whitespace | Removes trailing spaces/tabs from every line |
| Remove leading padding | Strips common leading indent (terminal frame padding) |
| Fix box-drawing tables | Converts `│`/`─` grid tables to markdown pipe tables |
| Collapse excess blank lines | Reduces 3+ consecutive blank lines to 1, trims trailing blanks |

## Contributing

Contributions welcome! This is a simple single-file app — fork it, hack on it, send a PR.

## Technical details

- Single `index.html` — no frameworks, no npm, no build tools
- All processing is client-side — nothing leaves your browser
- Works in Chrome, Firefox, Safari
- Hosted on GitHub Pages

## License

[MIT](LICENSE) — do whatever you want with it.
