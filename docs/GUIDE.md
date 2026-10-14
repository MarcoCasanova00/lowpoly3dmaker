# retro-qd3d studio — User Guide

A modern remake of Apple's **3D Movie Maker v1.0 (1998)** with a modeling room
homaging **Swivel 3D (1988)**. It makes tiny retro spinning-3D embeds for
Neocities-style sites. One file (`studio.html`), zero dependencies, everything
runs in your browser — your photos never leave your machine.

Background reading: `../README.md`, `../notes/REVERSE_NOTES.md`,
`../notes/SWIVEL_NOTES.md`.

## 5-minute quickstart

1. Click **Software Box** (section 1).
2. In section 2, click **Choose File** on *Front of Box*, upload any PNG/JPG photo.
3. Leave **Spin Y** selected (section 3). Drag the preview to inspect it.
4. Click **⧉ Build Neocities embed** (section 4), then **Copy embed**.
5. Paste into your page. Done — it spins forever.

## The workflow, section by section

### 1 · Model Select
Four built-ins: **Software Box**, **Hardcover Book**, **Compact Disc** (modeled
almost flat, like a real disc), **Picture Frame**. Click one to select it.
**Custom models:** drop in any Wavefront `.obj` (vertices + faces; quads and
n-gons are fan-triangulated). All faces share one `custom` texture slot.
Switching models resets textures and shape — deliberately, like the original
going back to the Model Select screen.

### 2 · Textures
Each model part has a slot (e.g. *Front Cover / Spine / CD Front Label* —
the names come from the original's resource fork). Per slot:
**Choose File** (PNG, JPG, GIF, WebP) **or drag-drop an image onto the row**.
Uploads are auto-converted the way 1998 did it: shrunk to power-of-two
(≤256px) and quantized to 16-bit RGB565 with nearest-neighbor sampling.
That keeps embeds tiny and the look authentic. ✕ removes a texture.
Parts marked *(fixed)* (pages, wood, CD edge) keep flat colors.

### 3 · Animation
Same four choices as 1998: **Spin Y / X / Z / Still**, at 10–180 °/s.
Output is always exactly one 360° turn.

### 4 · Movie options → Export
- **Size** (default 320×240), **Pixelation** (renders small, upscales chunky),
  **Background** color.
- **LOOP the 360° turn** (checked = repeat forever, like `LOOP=TRUE`; unchecked
  = play once and hold; **↻ Replay** restarts). This is the LOOP button.
- **Two-sided faces**: off = solid QD3D-style backface culling (correct for all
  built-ins); on = draw interiors too (useful for custom OBJs with messy
  windings, or X-ray effects).
- **⧉ Build Neocities embed** → self-contained `<canvas>` snippet (renderer +
  textures inlined, ~10–60 KB). Paste anywhere; needs no libraries, no network.
- **Copy / Download embed .html** helpers.
- **⧉ Copy `<video>` tag**: a `.webm` alone plays once — looping comes from the
  `loop` attribute, which this button sets from your LOOP checkbox. Upload the
  recorded `.webm` next to your page and paste the tag (muted+playsinline are
  included so autoplay works).
- **◉ Record 1 loop (WebM)**: records exactly one turn. **▣ PNG frame**: still.
- **⧉ Save offline studio .html**: snapshots this app into a file that runs
  from `file://` with no internet.

### 5 · Shape
Width / Height / Thickness sliders (resets on model switch). Travels into
embeds. Flatten anything into a card or stretch it into a totem.

### 6 · Model — Swivel room
Draw a 2D contour, turn it into a low-poly solid:
- Tools: **Rectangle** (drag), **N-gon** (click-drag radius, 6–48 sides),
  **Free poly** (click points; double-click/Enter closes; Backspace undoes;
  Esc cancels), **Arrow** (drag points; **Symmetric** mirrors ±x).
- **Snap** grid toggle. You start from a default cube outline, like Swivel.
- **Extrude** (Depth slider; concave-safe caps) or **Lathe ⤾** (segments 6–24 —
  this *is* the resolution control; fewer = chunkier = more retro).
- **⧉ Build & use model** → becomes the Custom model (one texture slot).
  **Download OBJ** saves it. Everything downstream (texture/spin/loop/export)
  works.

## Publishing on Neocities

1. Upload `retro-qd3d-<model>-<WxH>.html` *or* paste the canvas snippet into an
   existing page (Index → Edit). Canvas snippet is best: smallest, loops
   perfectly, scales without quality loss — the same three properties Apple
   bragged about in 1998.
2. For video: upload the `.webm`, paste the copied `<video>` tag.
3. Keep textures small (the app already caps at 256px) — texture bytes dominate
   file size, exactly as the 1998 ReadMe warned.

## IRL workflow (the point of the project)

Photograph something you own in flat, even light → pick the matching primitive
→ upload photos into its slots → Shape-tweak → spin → export. A book on your
shelf becomes a spinning book on your site.

## Troubleshooting

| Symptom | Fix |
|---|---|
| Page loads but nothing renders / red banner | Read the banner text; reload. The app needs JS enabled. |
| Image upload does nothing | Use PNG/JPG/GIF/WebP; status line reports problems. HEIC/TIFF won't decode — convert first. |
| `.webm` plays once on my site | Re-copy the tag with LOOP checked (adds `loop`), or use the canvas embed. |
| Custom OBJ looks inside-out | Tick **Two-sided faces**, or fix windings in your modeler. |
| Recording fails | Some browsers lack MediaRecorder — use PNG or canvas embed. |
| Page scrolled / cramped | Layout is 3-column on wide screens and stacks on narrow ones; preview sticks while scrolling. |

## What this is *not*

Not a general 3D suite: no scene graphs, skeletons, lighting rigs, physics, or
audio. No `.3df`/`.3dmf` import (OBJ instead), no quad live-previews on the
Animation screen, no video-quality slider (use Size/Pixelation). Deliberate:
stay as small and offline as the originals.
