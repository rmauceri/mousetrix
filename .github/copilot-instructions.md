# Mousetrix — Copilot Instructions

Mousetrix is a Tetris-inspired block puzzle game with a mouse/cheese/cat theme. Single HTML file (`index.html`), zero dependencies, runs entirely in the browser.

## Running

Open `index.html` directly in a browser. No build step, no server required.

## Architecture

Flat structure with a top-level `game` state object and a `requestAnimationFrame` game loop:

```
CONFIG / constants  — grid dimensions, gravity table, color palette, timing
Game state object   — board grid, active piece, score, level, cheese/cat state
Input handlers      — keyboard + pointer events (unified touch/mouse)
Game loop           — update physics → update entities → render canvas
Render functions    — board, active piece, ghost piece, cheese, cat overlays, HUD
```

## Grid and pieces

- **Grid**: `COLS = 10`, `ROWS = 20`, base cell size `CELL = 28px` (scales responsively)
- **7 piece types**: I, O, T, S, Z, L, J — colored with a warm mouse-fur palette (browns, creams, tawny)
- **Rotation**: standard SRS (Super Rotation System) with wall kicks

## Core mechanic — cluster clear (not line-clear)

**This is the key deviation from standard Tetris.** Lines do NOT clear when full. Instead:
- After a piece locks, flood-fill to find groups of ≥6 connected cells of the same piece type
- Any qualifying cluster clears with a shimmer animation (400 ms), then cells above fall
- This means vertical stacking of same-type pieces is strategically important

## Scoring and progression

- Cluster clear points scale with cluster size and current level
- **20-level difficulty ramp**: gravity interval drops from ~800 ms (level 1) to ~1 ms (level 20)
- Lock delay: **400 ms** after a piece touches down before it locks
- DAS (Delayed Auto Shift): held left/right auto-repeats after initial delay

## Cheese system

- Every 3 piece locks, a random cheese spawns on the board (max 5 at a time)
- Cheese types: **Cheddar** 75 pts, **Swiss** 150 pts, **Golden** 500 pts
- Cheese is collected by clearing the cluster/row it sits in

## Cat attack system

- A cat attack triggers every **15–25 s** (random interval)
- Attack patterns: **cross** (horizontal + vertical sweep), **pounce** (direct grab), **lurk** (paw from side)
- Effect: shoves the active piece in the attack direction; can displace it off its intended drop path

## Conventions

- **No external dependencies** — never add a CDN link, import, or network call.
- **Canvas rendering** — board and all gameplay elements draw on `<canvas>`; start screen and overlay UI use DOM/CSS.
- **Responsive canvas** — canvas scales to fill the viewport; all coordinates are in cell units, converted at render time.
- **Touch + keyboard** — swipe gestures map to move/rotate/drop; on-screen buttons for mobile; keyboard for desktop.
- **Cozy aesthetic** — warm brown/blue palette ("cozy burrow"). Maintain this tone for any new visual elements.
- **Offline-first** — no network calls, no CDN links.
- **LocalStorage** — high scores and settings are persisted via LocalStorage.
- **Config first** — gravity table, color palette, timing constants, and cheese/cat parameters all live in the top-level config, not inline.

## Commits

All commits in this repo include:
```
Co-authored-by: Copilot <223556219+Copilot@users.noreply.github.com>
```
