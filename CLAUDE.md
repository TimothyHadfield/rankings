# Ultimate Rankings

A personal "which do you prefer?" ranking tool. You add options to a list, pick between two at a time,
and the app builds a ranked list from those choices. Used by Tim (and anyone he shares the link with)
on iPhone and laptop. No accounts, no backend: everything lives in the viewer's own browser.

- Live: https://timothyhadfield.github.io/rankings/
- Repo: TimothyHadfield/rankings (public), branch `main`, served by GitHub Pages from the repo root.

## File map
- `index.html`: the whole app (CSS in `<style>`, JS in one inline `<script>`; ~1,700 lines).
- `README.md`: public showcase page for GitHub (features + screenshots). Written for visitors, not Claude.
- `docs/screenshots/*.png`: images used only by README.md.
- `progress.md`: handoff notes (state, traps, decisions).

## How the app works (facts from the code)
- Tabs: Saves (create / load / rename / delete lists), Options & Rankings (add options, ranked list,
  expandable Beat / Lost-to history with per-comparison remove), Play (Fun) (close-ranked pairs, changes
  nothing), Train (every pair once, updates ratings, progress bar).
- Storage: `localStorage['wouldYouRatherV2']` = `{ allLists: {id: list}, currentListId }`. A legacy key
  `wouldYouRather` is migrated once into a "Migrated List" and then removed.
- A list: `{ name, options[], comparisons[], usedCombinations[], comparisonCount, createdAt }`.
  Option: `{ id, name, description, rating (starts 1000), wins, losses, createdAt }`.
  Displayed points = `round(rating - 1000)`.
- Rating rule (`updateRatings`): expected win (winner already higher) moves 0-5 points, scaled down to 0
  at a 500-point gap; an upset moves 20-100 points, scaling up to 100 at a 500-point gap. Each comparison
  stores its exact `winnerChange` / `loserChange` so `removeComparison` can undo it exactly.
- Pair keys in `usedCombinations` are `"<winnerId>-<loserId>"`; checks look at both orders.
- `deduplicateComparisons()` runs on every load and drops repeat comparisons of the same pair.
- IDs are `Date.now()` strings.

## Saving (important)
Adding/deleting options, training and removing comparisons change the in-memory list only. They reach
localStorage when the user presses **Update List**, or as a side effect of any action that calls
`saveAllData()` (create/load/delete/rename list, edit option, Reset Training). The README tells users to
press Update List. Don't add auto-save without Tim's OK (it's a behaviour change).

## Run locally
`python -m http.server 8108` from the repo root (background), then open http://localhost:8108/.

## Test
No test suite and no build step. Verify with `node ~/.claude/tools/phone-view.mjs <url>` (+ `--desktop`).
Seed data with `--local "wouldYouRatherV2=<json>"`, drive tabs with
`--do "click:.tabs>.tab-btn:nth-child(4)"` (1 Saves, 2 Options, 3 Play, 4 Train) and train with
`--do "click:#train-game-area>div>.option-card:last-child"`. Check logic with `--eval` calling the
global functions (`trainSelect`, `removeComparison`, ...).

## Deploy
Push to `main`; GitHub Pages rebuilds. Check with `python ~/.claude/hooks/live-check.py <repo path>`.

## Traps
- `--do "click:text=Train"` matches "Reset Training" first (opens a confirm). Use the nth-child selectors.
- Buttons don't inherit `font-family`, so in Playwright WebKit on Windows they render in a serif font.
  On a real iPhone they use the system font. Not a bug to "fix" without Tim.
- `.claude/` is gitignored (repo is public). No accounts, so no test account is needed.
