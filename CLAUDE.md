# Stream Deck Button Creator

Single-file browser app for designing animated Stream Deck button images.
Everything lives in `index.html` — no build step, no dependencies
to install. Open the file in a browser (Chrome/Edge preferred; WebP export
needs a Chromium browser) to run and test it.

## Architecture

One HTML file containing a React 18 app written in JSX, transpiled in-browser
by Babel standalone (all loaded from cdnjs). Rendering is pure canvas 2D —
no DOM layers.

Rough map of the `<script type="text/babel">` block (top to bottom):

- **Constants**: `APP_VERSION`, `PRESETS` (Stream Deck hardware sizes),
  `GENERIC_SIZES`, `ANIM_TYPES` + `ANIM_PARAM_DEFAULTS`, easing functions
  (`EF`), `FONTS`, `EMOJIS`, `SVG_ICONS` (canvas draw functions, see below),
  `AC` (per-animation-type timeline colors), random project-name helpers.
- **Animation engine**: `applySingle` (one animation step's canvas transform),
  `drawChain` (walks a layer's animation timeline), `drawContent` (per-layer-type
  drawing), `renderFrame` (full frame: background + all layers).
- **Encoders**: `GIFEncoder` (palette quantise + LZW, hand-rolled),
  `buildAnimWebP` (muxes browser-encoded WebP frames into an animated RIFF).
- **`App` component**: all state, layer add/edit functions, save/load/export,
  then the JSX: header (project name, Save/Open/Reset), left panel (canvas
  size, background, add-layer, layer list), centre (canvas preview, playback,
  export buttons), right panel (selected layer properties + animation chain).

## Key concepts

- **Layers** are plain objects `{id, type, x, y, ...}`. Types: `text`, `rect`,
  `circle`, `emoji`, `image` (holds a live `HTMLImageElement` in `.img`),
  `svg` (references `SVG_ICONS` by `iconId`), `ants` (marching-ants border).
  `text`/`emoji` store x,y as the draw centre; all others as top-left.
- **Animations** are per-layer chains of steps `{id, type, duration, easing,
  ...params}` played sequentially, plus a per-layer `animStartDelay`.
- **SVG_ICONS** entries draw with canvas paths. The `draw` fn may return a
  mode string ("stroke-only", "stroke-mic", "cutout", "eye") that tells the
  caller how to fill/stroke — check existing icons before adding new ones.
- **Marching ants** use `setLineDash` + animated `lineDashOffset`. The speed
  is snapped so ants travel a whole number of dash periods per loop duration
  (seamless loop) — `loopDur` is threaded through
  `renderFrame → drawChain → drawContent` for this.
- **Project files** (`.sdproj`) are JSON: `{app, appVersion, format, name,
  canvas, background, timeline, layers}`. Image layers are serialised as PNG
  data URLs (`imgSrc`) at original resolution and rebuilt on load. Loaded
  layers are merged over defaults so older files stay loadable; bump `format`
  only on breaking changes.
- **Filenames**: project name (user-set or random `AdjectiveAnimal` like
  `SuspiciousCat`) + `_WxH` (images only) + `_DDMMYYYYHHMMSS` timestamp.

## Conventions

- On every release: bump `APP_VERSION` **and** the `<title>` tag (two places),
  minor bump for features, patch for fixes. Commit message starts
  `vX.Y.Z: summary`.
- Code style is dense/minified-ish single-line JS; match it rather than
  reformatting.
- The user tests manually in the browser before versions are committed —
  there is no test suite and no Node.js on this machine (so no linting;
  double-check JSX syntax by eye).
- Original file history: started as `streamdeck-creatorV3.html` on the
  desktop; renamed here with the version tracked inside the file.
