# Handoff: publish Bedroom Layout Planner to GitHub Pages

## Goal
Publish the room planner (single static HTML file) to GitHub Pages under the user's account so anyone can open it via a public link with no login.

Target URL: `https://ridgelabs.github.io/room-planner/`

## What exists
- **`index.html`** (attached alongside this handoff): the finished, standalone planner, already wrapped with `<!doctype html>`, `<html>`, `<head>` and charset/viewport metas. Ready to commit as-is. No build step, no dependencies (only Google Fonts loaded from fonts.googleapis.com).
- `room-planner.html` is the same page without the doctype/html wrapper (that's the Claude artifact source). Use `index.html` for Pages.
- Live Claude artifact version: https://claude.ai/artifact/DCjwD4v7PEEtbsvU6pqtks (private to the user).

## About the planner (context only)
- To-scale 2D top view, units in cm. Room polygon: top wall 288 cm, length 420 cm, right wall steps out 11 cm (to 299 cm) at 125 cm from the top wall. The 125 cm step position was estimated from the user's sketch; it is editable in the page under "Room size".
- Drag/rotate/resize furniture (Indian standard sizes), wall-gap readouts, overlap/out-of-room warnings, layout saved in each viewer's localStorage.
- Wall outline was thinned to 1.5 cm on user request.

## Decisions already confirmed by the user
- GitHub account: **ridgelabs** (user confirmed "yes, I'm sure").
- Repo name: **room-planner**, **public** (required for free Pages). User accepted that the page and code will be public.

## Steps for the next session
1. Put `index.html` in a new folder, e.g. `room-planner/`.
2. Create and push the repo:
   ```bash
   cd room-planner
   git init -b main
   git add index.html
   git commit -m "Add bedroom layout planner"
   gh repo create ridgelabs/room-planner --public --source . --push \
     --description "Bedroom layout planner – drag furniture in a to-scale top view"
   ```
3. Enable Pages from `main` / root:
   ```bash
   gh api -X POST repos/ridgelabs/room-planner/pages \
     -f "source[branch]=main" -f "source[path]=/"
   ```
   (If it returns 409, Pages is already on — fine.)
4. Wait ~1 minute, then verify: `gh api repos/ridgelabs/room-planner/pages --jq .html_url` and open the URL; check the plan renders and furniture drags.

## What went wrong in the cloud session (why this is being handed off)
The cloud session's git proxy only allowed already-existing, session-attached repos, so `gh repo create` and the push returned 403. Nothing was created on GitHub. A local Claude Code session with the user's own `gh` auth won't have that restriction.

## Possible follow-up the user showed interest in
A "Copy layout code" button: encode the room + items (`S.room`, `S.items` in the script) into a short base64 string the user can share, plus a "Load layout code" field (or read it from `location.hash`, e.g. `#l-<code>`) so others see the same arrangement. Not started; ask the user before building.

## Suggested skills
- None required for publishing (plain git/gh).
- `frontend-design` if the follow-up layout-code feature or further UI changes are requested.
