# Stream Deck Button Creator

Design animated button images for your Elgato Stream Deck — right in the
browser, in a single HTML file. No install, no build step, no server: download
`streamdeck-creator.html`, open it in Chrome or Edge, and start designing.

![version](https://img.shields.io/badge/version-4.4.0-e94560)

## What it does

Build up a button from layers, give each layer a chain of animations, preview
it live, then export a static PNG or an animated GIF/WebP sized for your deck.

### Layers
- **Text** — fonts, bold/italic, shadow, outline, alignment
- **Shapes** — rectangles (rounded), circles, plus a library of 40+ vector
  icons: arrows, media controls, stars, polygons, symbols, UI glyphs and more
- **Emoji** — 100+ including smilies and cat smilies
- **Images** — upload your own; placed at actual size or scaled to fit
- **Marching ants** — animated dashed borders with dash/square/dot styles,
  size, gap, speed, direction and transparency, snapped to loop seamlessly

Layers can be moved by dragging on the canvas, reordered, duplicated, rotated,
faded, mirrored and centred.

### Animation
Each layer gets a chain of animation steps played in sequence — fade, slide,
scale, bounce, rotate, pulse, shake, wiggle, typewriter, move and combined
move/scale/rotate steps — each with its own duration, easing and parameters,
plus a per-layer start delay. Quick presets cover common combos
(fade in → hold → fade out, etc.). A **Fit** button snaps the timeline length
to your longest chain.

### Canvas
Presets for Stream Deck hardware (Standard 72², XL 96², +/MK.2 144², Neo 128²)
plus generic sizes (64² – 512²) and free-form dimensions. Solid, linear or
radial gradient (any angle), or transparent backgrounds. Preview at fit-to-view
or actual pixel size.

### Export
- **PNG** — static, final frame
- **GIF** — animated, 5–60 fps, optional transparency
- **WebP** — animated with full 8-bit alpha (Chromium browsers)

### Projects
Save your work as a `.sdproj` file — a self-contained JSON snapshot of every
layer, animation and setting, with uploaded images embedded — and reopen it
later to keep editing. Name your project in the header; unnamed projects get
a random name like `SuspiciousCat`. Files are stamped
`Name_WxH_DDMMYYYYHHMMSS` so nothing overwrites.

## Running it

```
git clone https://github.com/Code-Yeti/StreamDeckButtonCreator.git
```

Open `streamdeck-creator.html` in Chrome or Edge. That's it.

> WebP export uses the browser's WebP encoder, so Firefox/Safari may only
> export PNG and GIF. An internet connection is needed on first load for the
> React/Babel CDN scripts.

## Using exports with your Stream Deck

In the Stream Deck software, right-click a key → *Set Custom Icon* and pick
your exported PNG/GIF. Animated GIFs play on the key.
