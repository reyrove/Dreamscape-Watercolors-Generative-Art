# Dreamscape Watercolors

**A seed-based generative system for soft watercolor compositions.**

A catalogue of computational textile compositions for fashion, textile and surface design — algorithmically drawn, seed-documented, and ready for production.

---

## Overview

Dreamscape Watercolors is a generative design system rather than a single artwork. Each composition is built from a set of soft elliptical strokes, each one layered into a feathered bloom with invisible ink bleeds. Where the strokes overlap, colour deepens and light glows from within. Twenty glowing orbs are scattered across the surface, resting on a segmented gradient and a subtle paper texture.

The system is designed for:

- **Fashion houses** adapting painterly ornament for apparel and accessories
- **Textile studios** developing repeat patterns and yardage
- **Surface designers** working across print, wallpaper, and interior applications

Every composition can be licensed, adapted, or commissioned to a brief.

---

## Concept

A wash, when it is *layered*, becomes a dreamscape — soft, glowing, quietly yours.

The watercolor wash — fluid, layered, endlessly soft — has always carried the mark of a hand guided by water. From the wet-on-wet tradition of classical painting to the abstract color fields of the twentieth century, translucent layers are one of the oldest forms of painterly computation we have. Dreamscape Watercolors translates that fluidity into code. Each composition begins with a set of brush strokes and unfolds through layered ellipses, ink bleeds, and glowing orbs, until the frame fills with a soft, dreamlike field.

The palette, the number of strokes, the number of orbs, and the paper texture are all derived from a single numeric seed.

Like the other still volumes in this series (Girih, Arachne, Celestial Grove, ChaotiColor, Citrus Mosaic, Crazy Knight Curve, Crazy Knight Line, Crazy Letter, cyPollock, Digital Pollen, Draconic Fractals), **Dreamscape Watercolors is a static composition.** The plate, the framed plate, the surfaces, and the archive are all static frames. A watercolor wash is something you read; its character is stillness, not motion.

---

## Features

- **Seed-based generation** — every composition is defined by a numeric seed and can be regenerated exactly
- **Deterministic output** — the same seed always produces the same composition
- **Layered watercolor blooms** — 60 strokes, each built from 7 feathered ellipses at decreasing size and alpha
- **Ink bleeds** — each stroke is ringed with concentric faint circles, mimicking real watercolor bleed
- **Glowing orbs** — 20 soft elements with shadow-blur glow, scattered across the surface
- **Segmented gradient background** — the canvas is divided into 30 colour bands, one from each background palette
- **Paper texture overlay** — a sampled grain that reads as watercolor paper, without per-pixel noise
- **Complementary palette** — brush colours and background colours generated independently, so contrast is high
- **Adaptive surfaces** — one seed applied across print, scarf, textile, and wall formats
- **Archive** — eight curated seeds available for immediate loading
- **Download** — export the composition as a high-resolution PNG
- **Keyboard shortcuts** — `R` for new seed, `S` to save

---

## Project Structure

```
.
├── index.html          # Main catalogue page
├── images/
│   ├── fav.svg         # Favicon
│   ├── tote.png        # Mockup: tote bag
│   ├── tee.png         # Mockup: t-shirt
│   └── cushion.png     # Mockup: cushion
└── README.md
```

---

## How It Works

### The Seed

A numeric seed (a large integer) initializes a deterministic pseudo-random generator. From this seed, the system derives:

- 30 brush colours (RGB + alpha)
- 30 background colours (RGB)
- 60 brush stroke positions, sizes, and layer seeds
- 20 dreamy element positions, sizes, and glow values
- A paper texture noise seed set

Because the generator is deterministic, the same seed always produces the same composition — on any device, at any time.

### The Brush Strokes

Each stroke is a layered bloom, not a single shape. The system draws:

1. **Seven feathered ellipses** at decreasing size, each one at slightly different rotation and aspect ratio, with alpha fading from full to near-zero.
2. **A blurred core ellipse** with a soft shadow, giving the centre of the stroke its weight.
3. **An ink bleed ring** — 10 concentric circles at decreasing radius, all at very low alpha, producing the soft halo that real watercolor leaves on paper.

The result is a single brush stroke that reads as a wet, translucent bloom rather than a flat painted shape.

### The Glowing Orbs

Twenty soft glowing elements are scattered across the composition. Each one is a filled circle with a shadow-blur glow, drawn in the pastel range (150–255 on each channel) with alpha between 50 and 100. Where the orbs overlap the brush strokes, they add depth and quiet light.

### The Background

The canvas is divided into 30 horizontal bands, each one filled with a colour from the background palette. Because the palette is generated separately from the brush colours, the background has its own rhythm — sometimes warm, sometimes cool, sometimes in direct contrast to the strokes above it.

### The Paper Texture

A sampled grid of soft alpha rectangles is overlaid on top of the whole composition, giving it the subtle grain of watercolor paper. Because the texture is sampled rather than computed per pixel, it runs in a fraction of the time of a full noise pass.

### The Surfaces

The same seed is rendered across four surface formats. These are static frames — they represent the print-ready composition.

| Surface  | Aspect | Material          |
|----------|--------|-------------------|
| Print    | 1 : 1  | Cotton rag        |
| Scarf    | 3 : 1  | Twill silk        |
| Textile  | 4 : 3  | Fabric yardage    |
| Wall     | 2 : 3  | Wallpaper         |

Each surface uses the same underlying seed and structural logic — only the repeat, orientation, and scale change.

### Stillness

Like the rest of the still volumes, Dreamscape Watercolors does not animate. The plate is a single frozen frame — the composition is complete the moment it is generated.

This is a deliberate design choice. A watercolor wash is not a swarm. It is not a rotation. It is a bloom, laid down once and left. Its stillness is what makes it print-ready in the strictest sense: what you see is what you get.

---

## Usage

### In the browser

1. Open `index.html` in any modern browser.
2. Click **New Seed** to generate a new composition.
3. Click **Download** to save the composition as a PNG.
4. Scroll to the **Archive** section and click any plate to load it into Plate 001.

### Keyboard shortcuts

| Key | Action          |
|-----|-----------------|
| `R` | New seed        |
| `S` | Save as PNG     |

### Reproducing a composition

Each composition is identified by an 8-digit seed label displayed in the metadata panel. To reproduce a specific composition, note the seed and regenerate it programmatically:

```js
const rng = new RandomGenerator(seed);
const features = buildFeatures(rng);
renderComposition(canvas, features, rng);
```

Because the generator is deterministic, this will produce the identical composition on any device.

---

## Technical Notes

- **No build step.** The system is a single HTML file with inline CSS and JavaScript.
- **No dependencies.** All drawing is done with the native Canvas 2D API.
- **Deterministic.** The `RandomGenerator` class uses a xorshift-based PRNG seeded by an integer, so identical seeds produce identical outputs.
- **Static rendering.** Every canvas renders a single frame. There is no animation loop.
- **Feature isolation.** Cover, framed plate, surfaces, and archive thumbnails each derive their own feature set from their own local RNG, without disturbing the main plate's state.
- **Pre-computed layer seeds.** The feathered ellipse layers and the stroke rotations are pre-computed in `buildFeatures` rather than drawn on the fly — so the same seed always produces the same bloom across resizes.
- **Sampled paper texture.** The paper texture is drawn as a coarse grid of soft alpha rectangles, not a full per-pixel noise pass — so the overlay is fast even at large canvas sizes.
- **Responsive.** The layout adapts from large desktop down to very small mobile devices (tested at 360px viewport width).
- **Accessible.** Supports `prefers-reduced-motion`. Pinch-zoom is enabled.

### Browser support

Tested in current versions of:

- Chrome / Edge
- Firefox
- Safari (desktop and iOS)

---

## Licensing

All Dreamscape Watercolors compositions are **seed-documented** and available for licensing across textile, surface, and print applications.

- **Standard licenses** cover single-product production runs.
- **Commercial use, custom editions, or exclusive rights** are available on request.

Each license is issued against a specific seed ID. Regeneration of the same seed produces the identical composition — ensuring reproducibility between artist, studio, and manufacturer.

For licensing enquiries: [reyhanehdaneshdoost@gmail.com](mailto:reyhanehdaneshdoost@gmail.com)

---

## Commission

Dreamscape Watercolors is a generative design system, not a fixed artwork. It can be adapted for specific briefs:

| Service     | Description                                                       |
|-------------|-------------------------------------------------------------------|
| Licensing   | Existing seeds from the archive, licensed for production use      |
| Commission  | New compositions designed to your palette, repeat, and product    |
| Systems     | A private generative tool built for your studio's ongoing use     |

To begin a conversation: [reyhanehdaneshdoost@gmail.com](mailto:reyhanehdaneshdoost@gmail.com)

---

## Series

Dreamscape Watercolors is part of a computational textile series. Each volume approaches ornament from a different structural angle:

| Volume                     | Structure                    | Motion                     |
|----------------------------|------------------------------|----------------------------|
| Girih 1                    | Islamic geometric            | Static                     |
| Arachne                    | Rotating rings               | Static                     |
| Baroque Me Baby            | Baroque frames               | Static                     |
| Bezier 1                   | Concentric curves            | Static                     |
| Bezier 2                   | Single rotating curve        | Animated (plate)           |
| Brownian Graphe            | Graph networks               | Animated + interactive     |
| Celestial Grove            | Recursive branch trees       | Static                     |
| ChaotiColor                | Cellular automata            | Static                     |
| Citrus Mosaic              | Arc-and-triangle tiles       | Static                     |
| Crazy Knight Curve         | Knight's-tour smooth path    | Static                     |
| Crazy Knight Line          | Knight's-tour gradient       | Static                     |
| Crazy Letter               | Framed wavy lines            | Static                     |
| cyPollock                  | Scattered branch field       | Static                     |
| Digital Pollen             | Noise-driven texture         | Static                     |
| Draconic Fractals          | Tiled dragon curve           | Static                     |
| **Dreamscape Watercolors** | **Layered watercolor blooms**| **Static**                 |

The series is designed as a coherent whole — same page structure, same seed logic, same licensing and commission terms — so that each volume can be presented individually or as part of a larger body of work.

---

## Credits

- **Design & Generative System** — Reyhaneh Daneshdoost
- **Typefaces** — Cormorant Garamond · DM Mono
- **Platform** — Reyrove Studio
- **Edition** — Dreamscape Watercolors, Autumn 2026

### On AI tools

Where technical obstacles were encountered, AI tools were used for debugging and code optimization. Every structural, aesthetic, and conceptual decision remained the artist's own.

---

## Links

- Website — [reyrove.github.io](https://reyrove.github.io/)
- Instagram — [@rey._.rove](https://www.instagram.com/rey._.rove/)
- LinkedIn — [Reyhaneh Daneshdoost](https://www.linkedin.com/in/reyhaneh-daneshdoost-730481160/)
- X — [@reyrove](https://x.com/reyrove)

---

© Dreamscape Watercolors · All compositions reproducible by seed · Computational Textile Design