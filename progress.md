# Ultimate Rankings: progress

## START HERE
Last updated 2026-10-16. Read order: this file, then `CLAUDE.md` (file map, data model, how to test).

## Goal right now
Nothing in progress. The app is published and working (https://timothyhadfield.github.io/rankings/).

## Status
| Area | State |
|---|---|
| App (`index.html`) | Working on iPhone and laptop (phone-view, 2026-10-16) |
| README showcase | Published with screenshots |
| Tests | None (single HTML file); verified with phone-view `--do` / `--eval` |
| 2026-10-16 sweep | Removing a comparison now restores the exact rating (was drifting by fractions). See sweep report. |
| 2026-10-17 fixes | Buttons/inputs use the page font in WebKit (were serif); the 3 browser confirm boxes (Reset Training, delete option, delete list) are now an in-page confirm (`appConfirm`), Enter/Escape/backdrop work. Verified phone-view WebKit. |
| 2026-10-17 site icon | `icon.svg` (tab) + `apple-touch-icon.png` 180px (iPhone home screen): gold podium with a star on the navy background |

## Authorized next steps
None.

## Standing instructions
- None project-specific yet. Tim's global rules apply.

## Traps
- Changes are only written to localStorage on Update List or other actions that call `saveAllData()` (see CLAUDE.md).
- phone-view: `click:text=Train` hits "Reset Training". Use `.tabs>.tab-btn:nth-child(N)`.
- No accounts and no backend, so there is no `claude-test` account.

## Decisions
- 2026-10-16: comparisons store exact (unrounded) rating changes so undo is exact. Old saved comparisons keep their rounded values.

## NOT verified
- Real iPhone Safari (only Playwright WebKit at iPhone size).

## Rejected / parked
- (none)
