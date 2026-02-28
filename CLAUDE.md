# FaB Lower Third Generator

Browser-based lower-third overlay tool for live Flesh and Blood TCG tournament broadcasts. Single-file, no build step, no dependencies.

## Architecture

**One file only:** `fab-lower-third.html` — HTML + CSS + JS in a single file. Never split into separate CSS/JS files. Never add a build process or framework.

## Hard Rules

- **Vanilla JS only** — no jQuery, React, Vue, or any library
- **CSS variables for all colors** — every color must be defined in `:root` as a `--var-name`; never hardcode hex values elsewhere in the file
- **Player 1 = blue = left** always; **Player 2 = orange = right** always
- **Both lower-third sets update together** — `generateGraphic()` must always write to both `lt-*` and `slt-*` IDs in a single call
- **HEROES array is the single source of truth** — all hero data lives there; `heroLabel()` derives display strings; no duplication elsewhere
- **No network requests** — GEM Player IDs are display-only text; there is no public GEM API

## Element ID Conventions

| Prefix | Set | Description |
|--------|-----|-------------|
| `lt-`  | Main lower third | Full-height bar with hero info and info sub-bar |
| `slt-` | Secondary lower third | Slim bar for score/name overlays |

Both sets share the same data source (the form inputs); both are updated by a single `generateGraphic()` call.

## CSS Variable Conventions

```
--p1-*        Player 1 blue palette
--p2-*        Player 2 orange palette
--lt-*        Lower-third canvas colors (background, divider, score bg)
--bg-*        App chrome backgrounds
--text-*      Text hierarchy
--border*     Border colors
--accent*     CTA / highlight purple
--success     Green status / export button
--warning     Error / caution text
```

## Known Gaps (priority order)

1. **`exportGraphic()` is a stub** — needs `html2canvas` loaded to function. When adding: load the script (CDN or bundled), remove the `typeof html2canvas === "undefined"` guard, keep the rest of the implementation as-is.
2. **No round-to-round state persistence** — TO re-enters data each match. A future feature could serialize form state to `localStorage`.
3. **Dark theme only** — a light variant may be needed for venues with bright ambient light. Would be a second `:root` block toggled by a `<body class="light">`.

## Making Changes

- Modify only what is explicitly requested.
- Preserve all existing functionality.
- Follow the existing CSS variable and JS patterns.
- Test that both `lt-*` and `slt-*` previews update correctly after any change to `generateGraphic()`.
- If adding a new hero, append it to `HEROES` only — do not touch any other code unless `heroLabel()` needs updating.
