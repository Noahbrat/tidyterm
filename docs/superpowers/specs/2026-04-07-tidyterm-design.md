# tidyterm — Clipboard Cleaning Tool for Terminal Output

**Date:** 2026-04-07
**Status:** Approved

## Purpose

When copying text from terminal emulators (Claude Code, tmux, SSH sessions, Docker logs), the clipboard often includes trailing whitespace padding, box-drawing characters, and excess blank lines. tidyterm cleans this up for pasting into non-monospace contexts.

## Architecture

Single `index.html` file with inline `<style>` and `<script>`. No frameworks, no npm, no build tools. Hosted on GitHub Pages as a static file.

Browser support: Chrome, Firefox, Safari.

## Layout

Option A — top controls, side-by-side panels.

- **Header row**: "tidyterm" left-aligned, one-liner description ("clean up terminal output for pasting anywhere") right-aligned. Small and unobtrusive.
- **Toggle row**: Three pill-style toggle switches, all on by default. Labels: "Strip trailing whitespace", "Fix box-drawing tables → markdown", "Collapse excess blank lines".
- **Two-panel area**: Input textarea (left), output textarea (right). CSS flexbox, stacks vertically below 768px breakpoint.
- **Action bar**: "Clear" button (left), diff stats center (e.g., "Removed 4,302 trailing spaces, 12 lines cleaned"), "Copy" button (right, visually prominent).
- **Footer**: Small GitHub repo link.

## Cleaning Rules

Applied in sequence on every input/paste event. Each rule can be toggled independently.

### 1. Strip trailing whitespace (on by default)
- Regex: `/ [ \t]+$/gm`
- Removes trailing spaces and tabs from every line.

### 2. Box-drawing → markdown tables (on by default)
- Simple implementation: detect lines that form a recognizable grid table using box-drawing characters (`┌ ┐ └ ┘ ├ ┤ ┬ ┴ ┼ │ ─`).
- For recognized grid tables: extract cell contents from `│`-delimited rows, output as markdown pipe tables with `|---|` separator after the header row.
- For non-tabular box-drawing characters (decorative borders, TUI frames): strip them.
- Scope: handles simple rectangular grids only. Nested/merged cells are out of scope for v1.

### 3. Collapse excess blank lines (on by default)
- Replace runs of 3+ consecutive blank lines with a single blank line.
- Trim trailing blank lines at end of content.

## Processing Flow

1. Listen to `input` and `paste` events on the input textarea.
2. On change: run each enabled cleaning rule in sequence (order: whitespace → box-drawing → blank lines).
3. Display result in output textarea.
4. Calculate and display diff stats: character count removed, lines cleaned. Animate stats on change (CSS opacity transition).

## Interactions

- **Copy button**: Uses `navigator.clipboard.writeText()`. Shows "Copied!" with checkmark, fades back to "Copy" after 2 seconds.
- **Clear button**: Resets both textareas and diff stats to empty/zero.
- **Keyboard shortcut**: Cmd/Ctrl+Shift+C copies the cleaned output to clipboard.
- **Toggle switches**: Flip cleaning rules on/off, immediately re-process input. Persist toggle state to localStorage.
- **Auto-process on paste**: The `paste` event listener triggers processing automatically.

## Visual Design

Dark theme, dev-tool aesthetic.

- **Backgrounds**: `#0d0d1a` (base), `#1a1a2e` (elevated surfaces)
- **Text**: `#e0e0e0` (primary), `#888` (muted/labels)
- **Accent**: `#7c8aff` (indigo/blue) for active toggles, primary buttons
- **Textareas**: Darker than surface, subtle `#333` border, monospace font
- **Font stack**: `'SF Mono', 'Cascadia Code', 'Fira Code', 'Consolas', monospace`
- **Border radius**: 8px for panels, 4px for buttons/toggles
- **Transitions**: Subtle fade on diff stats update, button hover states

## Out of Scope (v1)

- Nested/merged table cell detection
- ANSI color code stripping (potential future rule)
- File upload/drag-and-drop
- Any server-side processing
