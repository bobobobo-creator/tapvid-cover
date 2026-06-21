# TapVid Brand System (for dynamic posters)

Source of truth for colors, type, logo, voice, and copy. Distilled from the
*TapVid 品牌规范 v1.0* spec. Posters live on the **A-line (Marketing)** track:
bright, playful, hand-drawn-friendly, heavy mascot/ASCII use, big Display type.

## Positioning & voice

- **Tagline (master):** `Anything to Motion Video` (the brand also uses
  "Explainer Video"; for posters prefer **Motion Video** unless told otherwise).
- **Voice:** Confident (knows the answer, doesn't over-explain), Playful (hand-drawn
  feel, never childish), Concrete (says time/numbers/use-cases, no buzzwords),
  Creator-Native ("I / you", talks like a creator). Principle: **one idea per
  screen.**
- **Banned words:** 赋能 / 生态 / 解决方案 / 全链路 / 一站式, "Disrupt / Revolutionary /
  Game-changing", and "#1 / world-leading"-type superlatives. Keep copy plain and
  specific.

## Color tokens

Core palette:

| Token | Hex | Use |
|---|---|---|
| Tappy Green | `#8FD71B` | Primary brand color, the "alive/ripe" green |
| Ink Black | `#0A0A0A` | Main text, logo outline, primary button |
| Paper White | `#FFFFFF` | Main surface |
| Signal Purple | `#5B4FFF` | **Accent only** — highlights, interactive hotspots |

Surface / landing gradient (use as the poster background):

- Mist Green `#F0FAE0` (top) → Mist Purple `#EFEEFF` (bottom)
- `background: linear-gradient(180deg, #F0FAE0 0%, #EFEEFF 100%);`

Neutrals: Gray 900 `#0A0A0A`, Gray 700 `#404040` (secondary text), Gray 500
`#737373`, Gray 300 `#D4D4D4`, Gray 100 `#F5F5F5`, Gray 50 `#FAFAFA`.

Mascot-only (do **not** put in UI): Tappy Blue `#4F5BFF`.

### Palette recipes for the animation

These are the established "journeys" — reuse or adapt, don't invent off-brand hues:

- **Green → Gold ripening** (warm, harvest, "content matures into video"):
  stems green `#4A7023`→`#74af37` ripen to gold `#c89c40`→`#DAA520`; letter-bloom
  pool `["#e88c00","#f5b000","#ffe78a"]`.
- **Green → Brand-green** (fresh, energetic, most on-brand): stems
  `#4f7a1e`→`#6fae2e` ripen to `#8FD71B`→`#a8e84f`; letter-bloom in Signal Purple +
  Tappy Green + Ink Black: `["#5B4FFF","#5B4FFF","#8FD71B","#0A0A0A","#8b82ff"]`.
- **Purple accent** is great for the *bloom/highlight* (it's the "interactive
  hotspot" color) but should not dominate the whole field.

Pick the recipe that fits the concept's mood, or derive a new one *within* the
palette (e.g. a cool wave could go Ink → Signal Purple → Tappy Green).

## Typography

Load from Google Fonts. Roles:

- **Display / Headings:** `Plus Jakarta Sans` (700/800)
- **Body / UI:** `Inter` (400/500/600)
- **Mono / code / ASCII art:** `JetBrains Mono` (500/700) — **use this for the
  letter particles**, it's the designated ASCII font and the reason type-as-texture
  reads as TapVid.
- **CJK fallback:** PingFang SC / Noto Sans SC (only if Chinese is required —
  posters are usually English-only).

Type scale (scale up for a 1080-wide canvas; the template uses ~118px headline):
Display 72/80·800 · H1 56/64·700 · H2 40/48·700 · Body L 18/28 · Caption 14/20.

## Logo

Hand cursor + green circle + "Tapvid" wordmark. **Rules:** keep it small and at the
top so it never competes with the H1; place on light/solid backgrounds; don't recolor,
distort, or change the circle↔hand relationship. The horizontal SVG is embedded
inline in the template (and saved at `assets/logo-horizontal.svg`). Note the logo
file's green is `#78D701`; the brand's primary green token is `#8FD71B` — keep the
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
