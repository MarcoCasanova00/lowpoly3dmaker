# Reverse notes — QD3D Movie Maker (Apple 1998, via Macintosh Repository)

Source: `QD3D-MovieMaker.sit.hqx` (485,276 bytes, BinHex 4.0) from
https://www.macintoshrepository.org/44089-quickdraw-3d-movie-maker (SHA1
`e10d97d8…` per the archive page).

## Decode chain (verified locally)

1. BinHex 4.0 decode (custom script): inner file `QD3D MovieMaker.sit`,
   type `SIT5`, data fork 356,999 bytes, no resource fork.
2. StuffIt 5 extract (unar): `QD3D MovieMaker/` containing
   - `3D Movie Maker` (47,876 B data + 4,684 B `.rsrc`)
   - `Data/Box.3df` (50,784), `CD.3df` (74,600), `Hardcover.3df` (67,740),
     `PictureFrame.3df` (36,872) — all `GLS_BINARY_MSB_FIRST` = binary 3DMF
   - `3D Movie Maker ReadMe.pdf` (8 pages, full UX spec)
   - `Untitled.mov` (31,897 B, valid QuickTime `moov`, sample output)

## Binary identity

- `3D Movie Maker` data fork starts `Joy!peffpwpc` → **PowerPC PEF**, Classic Mac OS.
- Imports (via `strings`): `Q3Initialize`, `Q3View_New`, `Q3MacDrawContext_New`,
  `Q3PixmapDrawContext_New`, `Q3TriMesh_GetData`, `Q3TextureShader_*`,
  `Q3MipmapTexture_New`, `Q3Matrix4x4_SetRotate_{X,Y}`, `Q3WindowPointPick_New`,
  `Q3File_{New,OpenRead,OpenWrite,ReadObject}`, QuickTime `CreateMovieFile`,
  `NewMovieTrack`, `FlattenMovieData`, `DataCodecCompress`, DragLib.
- Local (static, dot-prefixed) functions name the whole pipeline:
  `.QD3D_SetupWindow/.QD3D_DrawScene/.QD3D_PICTToTexture/.QD3D_GetTextureMap`,
  `.Load3DMFModel/.Save3DMFModel/.MyRead3DMFModel`,
  `.InitQuickTime/.Make3DMovie/.RecordNewMovie/.CreateMyMovieVideoTrack`,
  `.SetupTextureSelectScreen/.DoTextureSelectClick/.ExtractTriMeshTexture/.DropTexture`,
  `.AssignPickToModelTriMeshes/.DropCustomModel/.IsDrag3DMF/.IsDragPICT`,
  `.SetupAnimationSelectScreen/.DoAnimationSelectClick`,
  `.PickATemplate/.SelectATemplate`, plus error strings
  (`3QD3D must be installed…`, `QuickDraw 3D version 1.5 or better…`,
  `$$$temp$$$.3dmf`, `:data:Hardcover.3df`, `Select a Model/Apply Textures/Select An Animation`).
- Resource fork strings: menus (File/Quit, Edit/Undo…), window title `3D Window`,
  version `v1.0 ©1998 Apple Computer, Inc.`, texture-slot names
  (`Book Textures/Front Cover/Back Cover/Spine`, `Front of Box/Back of Box/Edges of Box`,
  `CD Front Label`). These slot names are reused in the remake.

## 3DMF models

- 24-byte header: `3DMF 00000010 00010005 00000000 00000000 00000000`.
- Recursive container walk finds `cntr` → `tmsh` (TriMesh) + `atar` (attributes) +
  `txsu`/`txpm` (texture shader + pixmap, the embedded textures — the bulk of the bytes)
  + `kdif` (diffuse colors). E.g. Box = 2 top-level `cntr` (9036 + 8562 B),
  first `tmsh` payload 268 B (≈ 8-vert box); CD = 4 `cntr` incl. a 10300 B `tmsh`
  (faceted cylinder) + 32 KB `txpm` texture.
- `Untitled.mov` parses as a real QuickTime file (`moov/mvhd/trak/tkhd/edts/tref/mdia`),
  i.e. a thin 3D-track movie, not pre-rendered frames.

## REA MCP

- `open_binary` on the PEF → `target_unavailable: unsupported binary format`.
- `inspect_binary_layout` (pwntools-elf provider) → `provider_unavailable`.
- No Ghidra/PEF/QD3D provider in this Linux container, so no decompilation was
  possible here. On a machine with REA + Ghidra and a PEF loader (or under
  SheepShaver with the QD3D SDK), the named functions above are the entry points
  to trace: `DropTexture` → `AssignPickToModelTriMeshes`, `Make3DMovie` →
  `RecordNewMovie`, `SetupAnimationSelectScreen` (the 4 spin modes).

## What the remake copies (and changes)

Copies: 3-screen flow, 4 built-ins at matching poly counts, click-slot texture
assignment with 16-bit power-of-2 quantization, drag-to-rotate preview, 4
animations (Y/X/Z/still), one-360°-loop output, size + bg options.
Changes: PICT→any image, `.3df`→`.obj` drag-drop, `.mov`-with-3D-track →
self-contained `<canvas>` embed + WebM/PNG (QD3D tracks don't play anywhere now).
