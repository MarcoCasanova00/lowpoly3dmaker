# retro-qd3d — QuickDraw 3D Movie Maker remake (1998 → modern web)

Make tiny retro spinning-3D embeds for Neocities, the way Apple's **3D Movie Maker v1.0**
(©1998 Apple, by Brian Greenstone, from the QuickDraw 3D SDK) did for the early web —
but running on any modern computer, with zero dependencies.

## What the original did (from reversing `QD3D-MovieMaker.sit.hqx`)

Downloaded from https://www.macintoshrepository.org/44089-quickdraw-3d-movie-maker
(`QD3D-MovieMaker.sit.hqx`, 473.9 KiB, BinHex 4.0 → StuffIt 5 archive).

- App: `3D Movie Maker` — 47 KB **PowerPC PEF** binary (`Joy!peffpwpc`), Mac OS 7.5–9.2,
  needs QuickDraw 3D 1.5+. Imports `Q3TriMesh_*`, `Q3TextureShader_*`, `Q3View_*`,
  `Q3Matrix4x4_SetRotate_*`, QuickTime `CreateMovieFile`/`FlattenMovieData`, DragLib.
- 3 screens, no menus except Quit:
  1. **Model Select** — 4 built-ins (`Box.3df`, `Hardcover.3df`, `CD.3df`,
     `PictureFrame.3df`, binary `3DMF` / `GLS_BINARY_MSB_FIRST`) or drag-drop your own `.3df`.
  2. **Texture Mapping** — drag PICT files onto model parts (pick-based:
     `Q3WindowPointPick_New`, `AssignPickToModelTriMeshes`, `DropTexture`).
     Textures auto-converted to **16-bit, power-of-two**; smaller textures = smaller movies.
  3. **Animation** — 4 choices only: spin on Y, Z, X, or still. Output = **one 360° loop**,
     tiny `Untitled.mov` with a QuickTime 3D track (real-time render, not pre-rendered
     video) + optional QuickTime 3.0 compression + bg color. Loop it on your page
     (`LOOP=TRUE`) and resizing costs nothing.
- Models are very low-poly TriMeshes (`tmsh` objects, a few hundred bytes – ~8 verts
  for the box; CD is a low-seg cylinder). Textures dominate file size.

### REA status (honest)

REA MCP was attempted (`open_binary`, `inspect_binary_layout` on the PEF and `.3df`
files). Result: **`unsupported binary format` / `provider_unavailable`** — this
environment has no Classic-Mac PowerPC/PEF or QD3D-3DMF provider (would need Ghidra
+ a PEF loader / 68k+PPC + Mac resource-fork handling, ideally in a Mac emulator like
SheepShaver). So the findings above come from manual static analysis instead:
PEF header bytes, `strings` import/function names (`.QD3D_*`, `.DropTexture`,
`.SetupAnimationSelectScreen`, …), resource-fork strings (menus, texture-slot names
like "Front of Box", "CD Front Label"), the 8-page ReadMe.pdf, and a partial
recursive 3DMF container/object walk (`cntr/tmsh/atar/txsu/txpm/kdif`). No execution,
no decompilation — conclusions are observations + documented inferences, cited to
byte offsets in the notes above.

## The remake: `studio.html`

One self-contained file (no CDN, no build — drop it on Neocities as-is).
Mirrors the 3 screens, keeps the retro constraints on purpose:

| Original | Remake |
|---|---|
| 4 built-in `.3df` TriMeshes | 4 built-in low-poly meshes (Box, Book, CD, Frame), same ballpark poly counts |
| Drag PICT onto a part | Click a part slot → upload any image (e.g. **a photo of the real object you own**) → auto-quantized to **16-bit + power-of-two + nearest-neighbor** |
| Drag to rotate preview | Drag to rotate preview (same) |
| Y / Z / X / still | Y / X / Z / still (same 4) |
| Movie options: size, compression, bg color | Export options: size, pixelation (render scale), bg color, **LOOP checkbox + ↻ Replay**, two-sided toggle |
| Output: `.mov` w/ QD3D track | Output: **self-contained Neocities HTML snippet** (inline `<canvas>` software renderer, textures as data-URLs, LOOP honored) + **`<video>` tag snippet (loop attribute)** + **WebM/PNG via captureStream** + PNG frame |

### IRL → Neocities workflow (the point of this project)

1. Photograph something you own (book cover, CD, framed print, game box) in flat light.
2. Open `studio.html`, pick the matching primitive, click each face slot, upload your photos.
3. Pick a spin axis, set size/bg, preview the chunky pixels.
4. **Copy embed** → paste into your Neocities page. The snippet is dependency-free and
   loops one 360° turn, just like the 1998 movies with `LOOP=TRUE`.

## Files

- `studio.html` — the whole app + exporter. Open in any browser, or host it.
  It is one file with zero dependencies: click **Save offline studio .html** inside
  the app (or copy the file) and it runs from `file://` with no internet.
  Layout is 3-column on wide screens (model+textures | sticky preview |
  animation→export→shape→swivel) and stacks on narrow ones.
- `docs/GUIDE.md` — user guide: quickstart, every section, Neocities
  publishing, IRL workflow, troubleshooting.
- `models/` — hand-rebuilt `.obj` equivalents of the 4 built-ins + texture-slot notes.
- Section 5 (Shape) adds Width/Height/Thickness sliders the original never had —
  the CD is modeled almost flat (rim ±0.02) like a real disc; flatten/thicken at will.
- Section 6 (Swivel room, homage to Swivel 3D 1988 — see `notes/SWIVEL_NOTES.md`):
  draw a contour (Rectangle / N-gon 6–48 / Free-poly / Arrow point-drag with
  Double-Arrow symmetric edit + snap grid), then Extrude (concave-safe caps)
  or Lathe it into a low-poly model that feeds the whole pipeline
  (texture → spin → loop → embed/WebM/OBJ).
- `notes/` — byte-level reverse notes (PEF header, strings hits, 3DMF walk).

## Use

```sh
# just open it
xdg-open studio.html
# or serve + host the same file on neocities
```

No accounts, no tracking, your photos never leave the browser.
