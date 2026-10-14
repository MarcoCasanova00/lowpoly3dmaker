# Swivel 3D Pro 2.0.4 Demo — reverse notes (for retro-qd3d)

Source: https://www.macintoshrepository.org/1496-swivel-3d
File: `Swivel_Pro_2.0.4_Demo.sit_.bin` (533,245 bytes, MacBinary II + StuffIt).
SHA1 `f3055726dba5c850c227376bd0c0248dd938f54b` — matches the Repository page.

## Decode chain

MacBinary II header (filename `Swivel Pro 2.0.4 Demo.sit`) → 128-byte header
stripped → StuffIt archive → single file `Swivel Pro 2.0.4 Demo`
(1,179,361 bytes, **all resource fork** — classic 68K app, data fork empty).

## What it is

Swivel 3D v1.00 by Young Harvill, Paracomp Inc., 1988 (this demo: Pro 2.0.4).
68K, runs on a Mac Plus with 1 MB RAM, System 6.2+. A 3D **modeling** +
animation package — the creation half that QD3D Movie Maker lacks:

- Start from a default cube outline; Design Object View = Object / Cross
  Section / Top / Side views; world space ±455 inches.
- 5 contour tools (confirmed in strings): Single Arrow (drag points),
  Double Arrow (symmetric contour edit), Free Poly (click-series contour),
  N-gon/Circle (12–80 points in this version; 6–48 per the 1988 review),
  Rectangle (four-sided sections).
- Construction: extrude with numeric depth, MakeLathe, polymesh over
  selection; triangular/rectangular facets toggle; Fine→Very Coarse resolution.
- Linking: Free / BallJoint / Locked links, parent-child trees, Duplicate Link.
- Animation: keyframes + tweening, fast wireframe preview, Effects Render.
- Rendering: wireframe, hidden-line, shaded, shaded+outlines, contour;
  dithered on 1-bit; single movable light + shadows; direct/specular material
  params; PICT environment maps; MacRenderMan shaders (Pro); Scrapbook
  PICT in/out; Swivel Command Script language (pastes from Clipboard).
- Workspace sizes 64×48 … 1024×768.

## REA status (honest)

Same boundary as the QD3D binary: 68K Mac app, no 68K provider in this
environment (`open_binary` unsupported). Findings are static `strings`
evidence only — tool names, dialog texts, menu/option literals, error
strings. No execution (would need Mini vMac + ROM + System 6) and no
decompilation. Feature list corroborated by the contemporary Macworld
review quoted on the Repository page.

## What the remake borrows (section 6, "Swivel room")

- Contour tools: Rectangle / N-gon (6–48 sides, as the original) /
  Free-poly / Arrow point-drag + symmetric (Double Arrow) + snap grid.
- Construction: Extrude with depth, Lathe with segment count (= the
  original's facet/resolution control, kept low-res on purpose).
- Single-slot texture wrap ≈ Swivel projecting a PICT around an object.
- Everything downstream (spin, loop, embed, WebM, OBJ) works on built models.
- Deliberately NOT copied: linking/trees, tweening, RenderMan, scripts.
