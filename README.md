# Local Circle

Small monthly walks. First twenty minutes silent. Then talk if you want.

This is the static site for [circle.pilgrimapp.org](https://circle.pilgrimapp.org). Sister project of [Pilgrim](https://pilgrimapp.org).

## Status: hibernating

The circle is resting. The Austin circle walked monthly around Lady Bird Lake from May to September 2026; the site is now an archive of those walks.

What hibernation turned off:

- **Domain**: moved from `walk.lc` to `circle.pilgrimapp.org`. `walk.lc` was not renewed, so there is no redirect from old links.
- **Upcoming walks**: the next-walk line, `.ics` download, cadence engine (`js/cadence.js`), and the recurring `Event` JSON-LD are gone.
- **Contact**: the leader's phone number and every `@walk.lc` address (`austin@`, `start@`, `hi@`) are removed from the site.
- **Starting new circles**: the `/start/` kit and the "Your city?" homepage tile are gone.

### Waking it up

Commit `4c81c1e` is the last active version of the site. Start there for anything listed above, including the full city runbook and the email operations notes (Listmonk, SES, the `plgrim` worker), which were written against `walk.lc` and will need a new email domain.

## Stack

Pure static HTML, CSS, and JS. No build step. Deployed to GitHub Pages from `main`.

```
/                        → index.html        master seal + manifesto + city grid
/austin/                 → austin/index.html city page + past walks
/austin/<date>/          → one page per walk (etegami, stats, Pilgrim embed)
/css/site.css                                shared styles, warm-rust palette, dark mode
/js/seal.js                                  procedural goshuin renderer (SVG, hash-derived)
/scripts/og.mjs                              per-walk OG card generator
/assets/og/*                                 OG images (1200×630)
/sitemap.xml, robots.txt                     SEO
```

Each city page reads an inline `#city-data` JSON block and `seal.js` renders the procedural seal from it. The seal's hash seed still uses the `walk.lc` prefix on purpose — changing it would redraw every seal.

## Local dev

```bash
python3 -m http.server 8000 --bind 127.0.0.1
```

Then open `http://127.0.0.1:8000`.

## OG images

Per-walk cards: `node scripts/og.mjs austin/<date>/walk.json` (needs `rsvg-convert` and `magick`).

The static cards in `assets/og/*.svg` use curved `textPath` text that `rsvg-convert` drops, so render them with Chrome:

```bash
"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" --headless --hide-scrollbars \
  --force-device-scale-factor=1 --window-size=1200,630 \
  --screenshot="$PWD/assets/og/austin.png" "file://$PWD/assets/og/austin.svg"
```

## License

MIT — see [LICENSE](LICENSE).
