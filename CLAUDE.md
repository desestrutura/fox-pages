# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

`fox-pages` is a tiny set of static, self-contained HTML pages (no build step, no package manager, no framework). Each page is a single `.html` file with inline `<style>` and inline `<script>` — open it directly in a browser (`file://...`) to preview changes. There is no dev server, linter, or test suite in this repo.

Pages:
- `index.html` — home/directory page listing the other pages as cards.
- `roadmap-games-2026.html` — the main personal game roadmap for 2026 (large file).
- `roadmap-teddy-2026.html` — a second person's separate game roadmap, same format, independent color palette.
- `games-bucket-list.html` — "someday" games split out of the main roadmap; linked from both `index.html` and the roadmap's summary table.

## Editing workflow

- Edit the HTML file directly; there's nothing to compile. Verify visually in a browser before considering a change done (screenshot or open the file) — CSS/JS mistakes only show up visually, nothing will error at "build" time.
- Git: this is a personal repo pushed straight to `main` for most edits. `gh` CLI is not installed — when a PR is needed, create it via the GitHub API using the token from `git credential fill` (see recent commit history for the exact curl pattern) rather than assuming `gh pr create` works.
- Keep commit messages short: a one-line title plus 2-3 lines max. Double check the title actually matches the diff before committing.

## Architecture conventions shared across the roadmap pages

These conventions aren't enforced by any tooling — they're just the pattern the existing content follows, so match it by hand:

- **Two representations of the same data.** Each roadmap has a "Resumo Visual" `<table>`/row list near the top and a full `.game-card` per game further down. When you add, remove, or change status of a game, update both — they're not generated from each other.
- **Accordion cards.** Each `.game-card` has a `.card-toggle` button (`role="button" tabindex="0" aria-expanded="..."`) that expands a sibling `.card-body` via the shared `toggleCard(this)` function at the bottom of the file. Keyboard support (Enter/Space) is wired via a `keydown` listener over `.card-toggle` — preserve this when adding new cards instead of copy-pasting an inert `<div onclick>`.
- **One `<h2 class="section-label">` per month.** Don't create a second header for the same month (e.g. a separate "Clube do Game" header) — add the card under the existing month header instead. Clube do Game picks still get their own `.clube-badge` ("🎲 indicado por X") inside the card, they just don't get a duplicate section header.
- **Spoiler handling has two layers, and they protect different things.** `data-spoiler="1"` on a `.card-toggle` renders a "⚠ contém spoilers" hint next to the toggle, but that only warns about the hidden `.card-body`. The `.progress-label` (inside `.card-header`, always visible, never hidden) is a separate leak surface — keep it abstract on its own merits (no character deaths, boss names, or specific plot beats), regardless of whether the toggle is spoiler-flagged. E.g. "saindo da ilha dos Scars", not "mãe do Lev morta, fugindo da ilha dos Scars".
- **Cover images should come from `images.igdb.com`** (`/igdb/image/upload/t_cover_big/<hash>.webp`) for visual consistency with the rest of the page. Hashes are not guessable/derivable — get them from the user or search, and **always load the URL and visually confirm it's the right game before using it**; wrong hashes have silently pointed to a completely different game's cover in this repo before. When no correct IGDB hash is available, fall back to a Wikipedia box-art URL (`upload.wikimedia.org/wikipedia/en/...`) or the game's official CDN, and swap it for the IGDB version later if a correct hash turns up.
- **Per-page color palette.** Each file defines its own `:root` CSS custom properties (`--bg`, `--surface`, `--accent`, `--text-muted`, etc.) — they are not shared across files. When copying a component (like the `.crumbs` breadcrumb) from one page to another, re-derive colors from that page's own `--accent`/`--text-muted`, don't hardcode the source page's hex values.
- **Progress bars vs. static entries.** Games actively being played get `status-playing` on `.game-card`, plus a `.card-personal-row` (start date) and `.progress-bar-container` + `.progress-label`. Finished games get `status-done` and a `pill-done` status pill plus a FoxScore (`X.0 / 5` + tier `S/A/B/C/D`) instead of a progress bar. Keep the header stats block (`Total`, `Concluídos`, `Jogando`, `Pausados`, `Pendentes`, and the hours row) in sync when a game's status changes — these are hand-maintained counts, not computed.
- **Accessibility baseline established in this repo:** all interactive toggles are keyboard-operable, month/game headings use real `<h2>`/`<h3>` (not styled `<div>`s), decorative SVG icons carry `aria-hidden="true"`, and text/background color pairs are chosen to clear ~4.5:1 contrast — don't reintroduce opacity-based "dimming" for de-emphasized text (e.g. wishlist/paused entries), since stacking `opacity` on top of an already-muted color is what broke contrast before. Prefer a dedicated lighter color, border style, or desaturated image instead.
