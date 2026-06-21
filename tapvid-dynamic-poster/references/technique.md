# Animation Technique: the grow → ripen → bloom recipe

This is the procedural pattern behind the TapVid dynamic poster. It's general:
"wheat" is just one subject. The same machine — *elements that grow from a base,
shift color when mature, then sprout a cluster of letters at their tip, all swaying
in a shared wind field, on a timed loop* — visualizes almost any "something coming
alive out of text" concept (trees, waves, fireworks, sound bars, a skyline, a
flower). Read this before editing `assets/template.html`.

## Table of contents
1. The loop clock (state machine)
2. Element model & growth
3. Wind field (organic motion)
4. Color journey (ripening)
5. The letter "bloom" (the brand texture)
6. Vertical layout budget + collision math
7. The ShinyText CTA
8. Adapting the module to a new subject
9. Verifying

## 1. The loop clock

One global timeline drives everything via `millis()`:

- `t = constrain((millis()-t0)/GROW_TIME, 0, 1)` — growth progress 0→1.
- When `t>=1`, start `colorStart`; `colorProg` ramps 0→1 over `COLOR_TIME` (ripening).
- When `colorProg>=1`, start `bloomStart`; after `BLOOM_DELAY`, each element spawns
  its letter cluster, which fades in over `BLOOM_FADE_MS`.
- After `BLOOM_FADE_MS + HOLD_TIME`, call `resetCycle()` (new `t0`, rebuild the
  scene) so it loops forever.

Real timing (ship these): `GROW_TIME≈4800, COLOR_TIME≈2000, BLOOM_DELAY≈400,
BLOOM_FADE_MS≈1100, HOLD_TIME≈5200`. The restart is a clean snap (acceptable for a
looping poster). The background is the CSS gradient; the sketch calls `clear()` each
frame so the gradient shows through.

## 2. Element model & growth

The subject is built from N "elements" (e.g. wheat stalks). Each element is densely
tiled from many tiny rectangles ("joints") so it can grow smoothly and bend.

Per element: base `x`, base `y` (`by`, near the bottom), length `h`, thickness `w`,
lean `ang`, wind `phase`, and a `green`/`gold` color pair. Growth is gated by
`currentVisibleDots = floor(t * TOTAL_DOTS)`: the render loop tiles joints from base
upward and `break`s once `j > currentVisibleDots`, so the element appears to shoot up
from the ground. `TOTAL_DOTS≈90`.

A **tied-bunch** look: cluster the bases in a narrow band (`BASE_SPREAD`) and fan the
tops via `ang = bias*FAN + jitter`, so stalks converge low and spread high.

## 3. Wind field

A shared continuous sine wave gives life: `wind = sin(millis()*WIND_SPEED + phase)`.
Per joint, sway scales with relative height to a power so the **base stays anchored
and the tip moves most**: `sway = wind * WIND_AMP * pow(tt, SWAY_EXP)` where
`tt = j/TOTAL_DOTS`. Each element's own `phase` desynchronizes the field into a
natural wave. `WIND_SPEED≈0.0016, WIND_AMP≈30, SWAY_EXP≈2.2`.

## 4. Color journey (ripening)

Each element interpolates `lerpColor(green, gold, colorProg)` — so the whole field
shifts color the instant growth finishes. Use a palette recipe from
`references/brand.md`. The two colors per element are themselves random lerps within
a pair, which gives depth instead of a flat fill.

## 5. The letter "bloom" — the brand texture

This is what makes it unmistakably TapVid: the tip of each element sprouts a **dense
cluster of letters** (in `JetBrains Mono`) that fade in. The letters can spell a word
(cycle through `"tapvid"`), or be random glyphs for pure ASCII-art texture.

Mechanics (`makeDenseBloom` / `drawDenseBloom`):
- Bloom length = `h * random(BLOOM_RATIO_MIN, BLOOM_RATIO_MAX)`. Step along it by
  `BLOOM_STEP`, emitting a particle each step with: offset `off`, side-scatter `rad`,
  position `jitter`, a character `ch`, and a color from the bloom pool.
- At draw time, take the tip's **tangent direction** (delta of last two joints),
  normalize it, get its perpendicular, and lay particles back along the tangent
  (`off`) and out to both sides (`rad * perp`). This makes the cluster hug the
  element's direction and sway with it.
- Alpha fades 0→255 over `BLOOM_FADE_MS` from each element's `bloomBorn`.
- For legibility on a 1080 canvas: `BLOOM_FONT_SIZE≈26, BLOOM_STEP≈2.2,
  BLOOM_RAD≈22`. To spell a word, assign `ch` by `index % word.length` (sequential),
  not random, so it reads.

To change the texture to a different subject, you mostly change *where elements are*
and *what shape they grow in* (§8) — the bloom code is reusable as the "detail" layer
on any tip/edge.

## 6. Vertical layout budget + collision math

Canvas is `1080×1920`. Keep three zones with gaps:

- **Top (~0–740):** logo (~120 top margin) + H1 (~118px ×2 lines) + tagline.
- **Middle (~760–1540):** the animation.
- **Bottom (~1560–1920):** the CTA pill.

The easy mistake: the bloom extends **above** each element's tip, so the *highest
point* is `by - h*(1 + bloomRatioMax)`, not `by - h`. Keep that above the tagline.
Worked values that clear the headline: `BASE_Y≈1520`, `LEN_MIN≈470, LEN_MAX≈600`,
`BLOOM_RATIO_MAX≈0.30` → highest bloom ≈ `1520 - 600*1.30 = 740`, just under the
tagline; bases sit above the CTA. If you make elements taller, lower `BASE_Y` or
shrink the bloom ratio to keep the top clear.

The whole `1080×1920` stage is scaled to the viewport with
`transform: scale(min(vw/1080, vh/1920))` (see `fit()`), so it always fits.

## 7. The ShinyText CTA

The label uses a vanilla port of React Bits' **ShinyText**: a `background-clip:text`
gradient (`linear-gradient(120deg, #b5b5b5 0 35%, #fff 50%, #b5b5b5 65% 100%)`,
`background-size:200% auto`) animated `background-position: 150% → -50%` over 2s
linear, infinite — a white highlight sweeping a grey base. The pill is an Ink-Black
`<a href="https://tapvid.ai">` with hover-lift + arrow-nudge + press, and a subtle
ambient shadow pulse. Keep it an anchor and keep `pointer-events:auto`.

If asked for a different shine: `speed`→animation-duration, `spread`→gradient angle,
`color`/`shineColor`→the gradient stops, `direction:'right'`→swap keyframe `from/to`.

## 8. Adapting the module to a new subject

Keep the loop clock, wind, color, bloom, and CTA. Reshape `buildScene()` and the
per-element draw so the geometry reads as the user's subject. Patterns:

- **Wheat / grass / reeds:** near-vertical stalks, tied base, fanned tops. (default)
- **Tree / branching:** a few trunks; recursively spawn child elements at angles up
  the parent; bloom = leaves/letters at branch tips.
- **Wave / ocean of text:** elements are short and many, bases along a sine baseline;
  bloom rides the crest; wind amplitude high, phases offset left→right for a
  traveling wave.
- **Fireworks / burst:** elements radiate from a point; growth = radius; bloom at the
  far end; color ripens to Signal Purple/gold sparks.
- **Sound bars / equalizer:** vertical bars on a grid, heights driven by
  `sin(time + i)`; bloom optional; very "motion/video" coded.
- **Skyline / mountains:** elements are columns of varying height forming a silhouette;
  bloom = windows/stars as letters.

Rule of thumb: change *position + growth direction + how tips connect*; reuse the
joint-tiling, wind, ripen, and bloom helpers. Always render the fine detail as
letters so it stays TapVid.

### Figurative subjects (a face, a mascot, a product) via silhouette letter-fill

When the subject is a recognizable *figure* rather than abstract strands, swap the
"stalk" model for a **silhouette letter-fill**: define the figure as a union of
simple regions (circles/ellipses/triangles + point-in-shape tests), scatter dense
letter-particles inside on a jittered grid, assign each a reveal `order` so the
figure *assembles* over `GROW_TIME`, sway each lightly (edge particles more) for a
breathing feel, and `lerpColor(base, mature, colorProg)` to ripen. Add a ragged
**fringe** of particles just outside the silhouette so it reads as fluffy/organic
rather than a hard mask.

Recognizability comes from a few **landmark features**, not the blob: make them
*solid* (don't thin them) and *high-contrast*. For an animal: clear triangular
ears poking above the head, two eyes (great place for the Signal-Purple accent),
a small nose/mouth, and a couple of whisker rows. Reveal the eyes slightly later
so they "pop." Keep the subject's body fill lighter/mid-grey and the accents dark
— that value contrast is what makes it legible on the pale background (don't render
a light subject in near-white; it vanishes). You can still pair the figure with a
strand-based element (e.g. rising steam that blooms a word) so the "motion out of
letters" story stays.

The canonical worked example to study first is the **wheat field** that ships as the
default in `assets/template.html`: stalks tied at the base and fanned at the top,
growing up under the wind field, ripening green→gold, then sprouting dense
letter-heads that spell "tapvid." It's the cleanest illustration of the full
grow→ripen→bloom machine — start there, then reshape it toward your subject.

When a reference image is given, map: its main mass → where you cluster bases; its
implied motion → wind direction/amp and growth direction; its focal accents → where
the brightest bloom/Signal-Purple goes; its palette → the nearest brand recipe.

## 9. Verifying

Because of the loop, a random screenshot usually catches mid-growth. To inspect the
payoff frame: temporarily set `GROW_TIME=2000, COLOR_TIME=1000, HOLD_TIME=30000`,
reload, screenshot the fully-bloomed state, confirm subject legibility + brand
colors + no overlap + the CTA resolves to `https://tapvid.ai/`, then **restore real
timing**. Serve over `http://` (a local static server), not `file://`.
