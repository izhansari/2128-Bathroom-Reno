# 2128 Bathroom Reno — contractor scope page

A single static page for the GC and the trades. Phone-first, bilingual (EN / ES),
with tap-to-check boxes so each trade can work their own list.

- **`index.html`** — the page. No build step, no dependencies, no framework.
- **`sketch.jpeg`** — the reference photo shown at the top of the page.
- **`SCOPE.md`** — the source scope text, EN + ES. Kept as the written record.
- **`vercel.json`** — static hosting config.

## Adding the sketch photo

Put the photo in the **repo root**, next to `index.html`, named `sketch` with any
common image extension:

```
sketch.jpg    sketch.jpeg    sketch.png    sketch.webp
```

The page tries each in turn and uses the first one that loads, so it does not
matter which format your phone exported.

```
git add sketch.jpg
git commit -m "Add reference sketch photo"
git push
```

Or upload it straight through the GitHub web UI: **Add file → Upload files**,
drag it in, commit.

Until that file exists the page shows a short note in the photo slot instead of a
broken image — everything else on the page still works.

To use a different filename entirely, edit the `SHOT_NAMES` array near the bottom
of the `<script>` block in `index.html`.

## Per-area photos

Each area section in the **By Area** view has photo slots at the top. Drop these
files in the repo root and they appear automatically; until then each slot shows a
labelled placeholder (`Photo — vanity1.jpeg`).

| Area    | Files |
|---------|-------|
| Shower  | `shower1.jpeg` |
| Vanity  | `vanity1.jpeg`, `vanity2.jpeg` |
| Toilet  | `toilet1.jpeg` |

To add, remove, or rename slots, edit the `AREA_IMAGES` map near the top of the
`<script>` block in `index.html`:

```js
var AREA_IMAGES = {
  shower: ["shower1.jpeg"],
  vanity: ["vanity1.jpeg","vanity2.jpeg"],
  toilet: ["toilet1.jpeg"]
};
```

These names are exact (unlike the top sketch, which tries several extensions), so
match the filename to what is listed here. Empty slots are hidden when printing.

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
