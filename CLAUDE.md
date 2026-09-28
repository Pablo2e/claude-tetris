# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project overview

Vanilla JavaScript Tetris: a single-page browser game with no dependencies, build step, package manager, tests, or linter. It ships as three files:

- `index.html` — DOM, two `<canvas>` elements (board and next-piece preview), score panel, pause/game-over overlay.
- `style.css` — dark retro arcade styling using flexbox and backdrop filters.
- `game.js` — all game logic (~300 lines), executed as a plain script loaded by `index.html`.

## How to run

Open `index.html` directly in a browser, or serve the folder with any static HTTP server:

```bash
# Windows (PowerShell)
start index.html

# Python 3
python3 -m http.server 8000
# then open http://localhost:8000

# Node / npx
npx serve .

# PHP
php -S localhost:8000
```

There are no `npm test`, `npm run build`, or lint commands available because the project has no `package.json`.

## Architecture and key facts

- Entry point: `init()`, called at the bottom of `game.js`.
- All state is module-level (`board`, `current`, `next`, `score`, `lines`, `level`, `paused`, `gameOver`, `lastTime`, `dropAccum`, `dropInterval`, `animId`). There are no classes or modules.
- The board is a `ROWS x COLS` matrix where `0` is empty and `1–7` are piece color indices.
- Game loop is driven by `requestAnimationFrame`, accumulating time until `dropInterval` elapses before moving the piece down or locking it.
- Core systems: `collide`, `rotateCW`/`tryRotate` (wall kicks), `ghostY` (ghost piece), `clearLines`, scoring with `LINE_SCORES * level`, and level progression every 10 lines (`max(100, 1000 - (level - 1) * 90)` ms).
- Input is handled by a single `keydown` listener: `←`/`→` move, `↑`/`X` rotate, `↓` soft drop, `Space` hard drop, `P` pause.

## Customization

Tunable constants are at the top of `game.js`: `COLS`, `ROWS`, `BLOCK`, `COLORS`, `PIECES`, `LINE_SCORES`, and `dropInterval`.

If you change `COLS`, `ROWS`, or `BLOCK`, also update `width` and `height` on `<canvas id="board">` in `index.html` so they match `COLS * BLOCK` and `ROWS * BLOCK`.

## Git convention

When creating commits in this repo, append the Claude Code attribution footer as required by project policy:

```
Co-Authored-By: Claude Code <noreply@anthropic.com>
```
