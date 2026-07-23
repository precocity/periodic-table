# The Periodic Table of Software Development

A single-page visualization that arranges the roles on a software delivery team as
elements in a periodic table — from Product Owner to Customer Success — complete with
tongue-in-cheek RPG-style stats (`1d20 • Vision`, `1d10 • Deploys`, …). Built by
[Precocity](https://github.com/precocity).

**Live view:** open `index.html` in any modern browser.

![The Periodic Table of Software Development](.thumbnail)

## Features

- **Two layouts.** Toggle between **Grouped** (roles bracketed by delivery phase) and
  **Periodic** (a compact grid with a color legend).
- **Responsive scale-to-fit.** On wide screens the table renders on one row and scales to
  fit; below 1300px it reflows and wraps automatically.
- **JPG export.** The **Download JPG** button renders the table (UI chrome excluded) to a
  high-resolution JPEG.
- **Deep-linkable content.** Roles, phases, colors, and stats are defined declaratively in
  a single `renderVals()` method — no build step required to edit them.

## Running locally

This is a static site with no build or install step. Either:

```bash
# Option A — open directly
open index.html            # macOS

# Option B — serve over HTTP (recommended; required for JPG export to embed assets)
python3 -m http.server 8000
# then visit http://localhost:8000
```

> **Note:** Serving over HTTP (Option B) is recommended. Opening via `file://` can prevent
> the JPG exporter from embedding the logo and web fonts, which is a common cause of blank
> or partial exports.

## Project structure

| Path                              | Purpose                                                                 |
| --------------------------------- | ----------------------------------------------------------------------- |
| `index.html`                      | The app: markup, styles, and the `Component` logic (layout + export).   |
| `support.js`                      | Generated `dc-runtime` framework (`x-dc`, `sc-for`, `sc-if`, `ref`). **Do not hand-edit** — it is built from `dc-runtime/src/*.ts`. |
| `assets/precocity-logo-white.svg` | Precocity logo shown in the table title.                                |
| `.thumbnail`                      | Static preview image of the rendered table.                             |

## How it works

The page uses a small custom component runtime (`support.js`, generated from `dc-runtime`)
that supports declarative templating: `sc-for` (loops), `sc-if` (conditionals), `ref`
bindings, and `{{ ... }}` expression interpolation.

All table content lives in `Component.renderVals()` in `index.html`. Each delivery phase is
an object with a `label`, an OKLCH `hue`, and an array of `cells`:

```js
{ label: "Development", hue: 160, cells: [
  { n: 26, sym: "Fe", name: "Frontend Developer", stat: "1d8 • CSS" },
  // ...
]}
```

To add or change a role, edit the relevant band array (`band1`, `band2`, `band3`, or
`overflow`). Colors are derived from each group's `hue` — no per-cell styling needed.

### JPG export

`downloadJpg()` (in `index.html`) uses
[`html-to-image`](https://github.com/bubkoo/html-to-image) to rasterize the table. Elements
marked `data-noexport="true"` (the header controls) are filtered out. The exporter waits for
web fonts to load, primes asset embedding with a warm-up pass, caps resolution to avoid
oversized canvases, and surfaces any failure to the console.

## License

The source code is licensed under the [Apache License 2.0](./LICENSE).

The "Precocity" name and logo are trademarks of Precocity LLC and are **not** covered by
that license. See [TRADEMARK.md](./TRADEMARK.md) for how the Precocity marks may be used.
