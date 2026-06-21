# TapVid Brand System (for dynamic posters)

Source of truth for colors, type, logo, voice, and copy. Distilled from the
*TapVid Brand Spec v1.0*. Posters live on the **A-line (Marketing)** track:
bright, playful, hand-drawn-friendly, heavy mascot/ASCII use, big Display type.

## Positioning & voice

- **Tagline (master):** `Anything to Motion Video` (the brand also uses
  "Explainer Video"; for posters prefer **Motion Video** unless told otherwise).
- **Voice:** Confident (knows the answer, doesn't over-explain), Playful (hand-drawn
  feel, never childish), Concrete (says time/numbers/use-cases, no buzzwords),
  Creator-Native ("I / you", talks like a creator). Principle: **one idea per
  screen.**
- **Banned words:** empower / ecosystem / solution / end-to-end / one-stop,
  "Disrupt / Revolutionary / Game-changing", and "#1 / world-leading"-type
  superlatives. Keep copy plain and
  specific.

## Color tokens

Core palette:

| Token | Hex | Use |
|---|---|---|
| Tappy Green | `#8FD71B` | Primary brand color, the "alive" green, also the **accent / highlight** |
| Ink Black | `#0A0A0A` | Main text, logo outline, primary button, high-contrast accent |
| Paper White | `#FFFFFF` | Main surface |

Posters are **green-dominant**. Green is both the field color and the highlight; use
Ink Black for contrast. Do **not** introduce purple or blue into posters, the accent
that used to be Signal Purple is now carried by Tappy Green / Ink Black.

Surface / landing gradient (use as the poster background):

- Mist Green `#F0FAE0` (top) → Mist Light `#FAFEF2` (bottom), a soft green-to-near-white
  wash, no purple
- `background: linear-gradient(180deg, #F0FAE0 0%, #FAFEF2 100%);`

Neutrals: Gray 900 `#0A0A0A`, Gray 700 `#404040` (secondary text), Gray 500
`#737373`, Gray 300 `#D4D4D4`, Gray 100 `#F5F5F5`, Gray 50 `#FAFAFA`.

### Palette recipes for the animation

**Pick the color from the concept, not from a default.** Read the prompt (and the
uploaded reference image, if any) and choose the recipe that matches what the visual
is *trying to say*, its mood, subject, and energy. Green is the dominant family; only
shift colors when the meaning calls for it (e.g. warmth/harvest, energy, calm). Don't
invent off-brand hues, and **don't use purple or blue**.

These are the established green-led "journeys", reuse or adapt:

- **All-green (fresh, energetic, the default, most on-brand):** stems
  `#4f7a1e`→`#6fae2e` brightening to `#8FD71B`→`#a8e84f`; letter detail pool in Tappy
  Green + Ink Black: `["#8FD71B","#a8e84f","#6fae2e","#0A0A0A","#74af37"]`.
- **Green → Gold (warm, harvest, only when the concept is warm/"ripening"):**
  stems green `#4A7023`→`#74af37` shift to gold `#c89c40`→`#DAA520`; letter detail
  pool `["#e88c00","#f5b000","#ffe78a"]`.
- **Green accent / highlight:** the brightest detail and any "hotspot" should be
  Tappy Green `#8FD71B`, with Ink Black `#0A0A0A` for contrast. Highlights stay green.

Color shifts are **optional**, many concepts read best in a steady green field. If you
do derive a new recipe, stay *within* the palette and let the concept decide (e.g. a
calm/cool subject could go Ink Black → Gray 500 → Tappy Green; a warm one Green → Gold).

## Typography

Load from Google Fonts. Roles:

- **Display / Headings:** `Plus Jakarta Sans` (700/800)
- **Body / UI:** `Inter` (400/500/600)
- **Mono / code / ASCII art:** `JetBrains Mono` (500/700), **use this for the
  letter particles**, it's the designated ASCII font and the reason type-as-texture
  reads as TapVid.
- **CJK fallback:** PingFang SC / Noto Sans SC (only if Chinese is required -
  posters are usually English-only).

Type scale (scale up for a 1080-wide canvas; the template uses ~118px headline):
Display 72/80·800 · H1 56/64·700 · H2 40/48·700 · Body L 18/28 · Caption 14/20.

## Logo

Hand cursor + green circle + "Tapvid" wordmark. **Rules:** keep it small and at the
top so it never competes with the H1; place on light/solid backgrounds; don't recolor,
distort, or change the circle↔hand relationship. The horizontal SVG is embedded
inline in the template (and saved at `assets/logo-horizontal.svg`). Note the logo
file's green is `#78D701`; the brand's primary green token is `#8FD71B`, keep the
logo's own green as-is, use `#8FD71B` everywhere else.

## Copy bank (drop-in, on-brand)

- Headlines: `Create Your Motion Video` · `Anything to Motion Video` ·
  `Turn anything into motion`
- Taglines (sub): `The AI Motion Video Engine for Creators` ·
  `From a single prompt, a whole video.`
- Primary CTA: `Start Creating` (→ `https://tapvid.ai`). Alternatives from the brand
  CTA bank: `Create Your First Video`, `Use This Template`.

## The CTA (must-have)

Every poster ends with a button that links to **https://tapvid.ai**. In the
template it's an `<a class="cta" href="https://tapvid.ai" rel="noopener">` styled as
an Ink-Black pill, with the **ShinyText** shimmer on the label and hover/press
micro-interactions. Keep it an anchor (so the click navigates) and keep
`pointer-events:auto` on it (the overlay above the canvas is otherwise
click-through).
