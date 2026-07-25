# 2128 Bathroom Reno — contractor scope page

A single static page for the GC and the trades. Phone-first, bilingual (EN / ES),
with tap-to-check boxes so each trade can work their own list.

- **`index.html`** — the page. No build step, no dependencies, no framework.
- **`sketch.jpeg`** — the reference photo shown at the top of the page.
- **`SCOPE.md`** — the source scope text, EN + ES. Kept as the written record.
- **`vercel.json`** — static hosting config.

## Adding the sketch photo

The page expects the photo at the repo root, named exactly **`sketch.jpeg`**.

```
git add sketch.jpeg
git commit -m "Add reference sketch photo"
git push
```

Until that file exists the page shows a short note in the photo slot instead of a
broken image — everything else on the page still works.

If your file is a `.jpg` or `.png`, either rename it to `sketch.jpeg` or change the
two `src`/`href` references in `index.html` to match.

## Deploying on Vercel

1. Go to [vercel.com/new](https://vercel.com/new) and import this repo.
2. Framework preset: **Other**. Leave build command and output directory empty.
3. Deploy. Vercel serves `index.html` at the root.

Every push to the default branch redeploys automatically. Share the resulting URL
with the GC and the trades.

## How the page works

- **Three tabs** — *By Area* (shower / vanity / toilet / whole bathroom),
  *By Trade* (framing / plumbing / electrical / tile / general), and *Notes*.
  Same items, regrouped, so a plumber can open one list and see only their work.
- **EN / ES toggle** in the header. Every item is written in both languages.
- **Open questions** box at the top — the decisions still blocking work
  (arch depth, grout colors, neutral in the switch boxes, niche dry-lay,
  vanity stub-outs).
- **Checkboxes** save to the browser's `localStorage`. They are per-device and
  per-browser — they are a personal punch list, not shared state. Nobody sees
  anyone else's checkmarks.
- **Cancelled / Changed** badges mark everything that moved after demo.
- **Hide done** collapses finished items; **Reset** clears all checkmarks.
- **Print / Save as PDF** prints a clean, fully expanded copy with the header
  chrome stripped out.

## Editing the content

All content lives in the arrays at the top of the `<script>` block in `index.html`:

- `ITEMS` — the checklist. Each entry has an `id`, an area `a`, a trade `t`,
  optional `flag` (`"cancel"` or `"change"`), and `en` / `es` text.
- `CONSIDER` — the notes list. Entries with `open: true` also appear in the red
  Open Questions box at the top.
- `BENEFITS` — the "why it is built this way" list.
- `UI` — every other string on the page, in both languages.

Wrap dimensions in `d('48" × 17"')` so they render in the monospace chip style.

Item `id`s are the localStorage keys for the checkboxes. Changing an existing `id`
resets that item's checkmark for everyone, so prefer adding new ids over renaming.
