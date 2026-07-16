# Open Graph / social share card

Generates `../images/og-card.png` (1200×630) — the image used for `og:image` /
`twitter:image` in `../index.html`. It's a before/after of the fixture-repo PR
list: bare rows → rows enriched by Workvane (Jira status, assignees, merge
blockers, one-click transitions).

## Files
- `og-card.html` — the 1200×630 template (self-contained, dark theme).
- `before.png` — plain GitHub PR list of the public fixture repo, captured with
  **no extension** (the "Jira-blind" state).
- `pro-list-transition-buttons-dark.png` — the "after" (Pro), pulled from the e2e
  screenshots published to Supabase. Refresh it any time with:
  ```bash
  curl -o pro-list-transition-buttons-dark.png \
    "https://xwbcjrtrfzkyafniskjk.supabase.co/storage/v1/object/public/Assets/e2e/pro-list-transition-buttons-dark.png"
  ```

## Regenerate the card
Render the template at 2× and downscale to a crisp 1200×630:
```bash
CHROME="$(find ~/Library/Caches/ms-playwright -name 'Google Chrome for Testing' -type f | head -1)"
"$CHROME" --headless=new --disable-gpu --hide-scrollbars \
  --force-device-scale-factor=2 --default-background-color=00000000 \
  --screenshot=../images/og-card.png --window-size=1200,630 \
  "file://$PWD/og-card.html"
sips -z 630 1200 ../images/og-card.png
```
(Any headless Chromium works; Playwright's cached binary is just convenient.)

## Recapture the "before"
```bash
"$CHROME" --headless=new --disable-gpu --hide-scrollbars \
  --force-device-scale-factor=1 --virtual-time-budget=9000 \
  --screenshot=before.png --window-size=1280,720 \
  "https://github.com/Workvane/jira-github-wind-e2e-fixtures/pulls"
```
GitHub renders logged-out in dark mode, which matches the dark "after" variant.
