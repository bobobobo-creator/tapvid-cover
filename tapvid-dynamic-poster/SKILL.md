---
name: tapvid-dynamic-poster
description: >-
  Generate an animated 9:16 "dynamic poster" (a single self-contained HTML file
  using p5.js) in the TapVid brand design language, driven by a user prompt or a
  reference image. The poster renders a procedural, looping animation that
  visualizes the user's concept — built out of moving letters/text particles
  (TapVid's ASCII-art motif) in the brand palette and type — with the TapVid logo,
  a Display headline, and a shimmering "Start Creating" button that links to
  tapvid.ai. Use this skill whenever the user wants a TapVid poster, cover,
  thumbnail, key visual, hero animation, social/launch graphic, "动态海报", "封面",
  or asks to turn a prompt or reference image into a TapVid-branded animated
  visual — even if they don't say "poster" explicitly. Also use it to restyle an
  existing visual into the TapVid look or to change the poster's subject.
---

# TapVid Dynamic Poster

Turn a user's prompt or reference image into an **animated 9:16 poster** that looks
and feels like TapVid: one self-contained `.html` file, p5.js for the motion, the
brand's gradient + type + logo as fixed chrome, and a procedural animation in the
center that visualizes whatever the user described — rendered out of **moving
letters** (the brand's ASCII-art element) in the brand colors.

The output is always a poster that (a) is on-brand, (b) animates in a satisfying
loop, and (c) has a working **Start Creating → https://tapvid.ai** button.

## What stays fixed vs. what you design

The power of this skill is that the *brand chrome and the hard parts* (9:16 sizing,
gradient, logo, type scale, the shimmer CTA, the grow→ripen→bloom state machine,
the wind field) are already solved in the template. **You only design the central
animation** so it matches the user's concept. This keeps every poster correct and
lets you focus creative energy where it matters.

- **Fixed (keep from the template):** canvas `1080×1920` auto-scaled to fit, the
  `linear-gradient(180deg, #F0FAE0, #EFEEFF)` background, the embedded TapVid logo,
  the `Plus Jakarta Sans` headline + tagline, the ShinyText CTA wired to
  `https://tapvid.ai`, and the timed loop clock.
- **You design:** the subject metaphor, the procedural geometry that grows it, the
  color journey, and the letter "bloom" — see `references/technique.md`.

## Workflow

1. **Read the references first.** Load `references/brand.md` (colors, type, logo,
   copy, CTA rules) and `references/technique.md` (the animation recipe and all
   tunable parameters). These are the source of truth — don't reinvent brand values
   from memory.

2. **Interpret the input into a visual concept.**
   - *Prompt only:* extract a concrete subject and a metaphor for it. TapVid is
     "Anything → Explainer/Motion Video," so the strongest concepts show *something
     growing, assembling, or coming alive out of letters/text* — wheat from seeds,
     a tree from a sprout, a wave of characters, fireworks of glyphs, a city
     skyline typed into being, sound bars, a blooming flower. Pick ONE clear
     subject; "one idea per screen" is a brand principle.
   - *Reference image provided:* read it. Identify its subject, composition
     (where the mass sits), motion implied, and palette. Reproduce the *structure
     and feeling*, then re-skin it into the TapVid palette/type and rebuild the
     texture out of letters. Don't copy colors that fight the brand.
   - If the input is too vague to pick a subject, ask one short clarifying question
     rather than guessing blandly.

3. **Choose the headline + button text.** Default headline is `Create Your` /
   `Motion Video`; default CTA is `Start Creating`. Adapt the headline to the
   concept if it sharpens the message, but keep it short (Display/H1), concrete,
   and in the brand voice (see `references/brand.md` → Voice). The CTA always links
   to `https://tapvid.ai`.

4. **Copy the template and design the animation.** Copy `assets/template.html` to
   the output location, then edit only the marked **ANIMATION MODULE** and the
   color constants to realize your concept, following `references/technique.md`.
   Match the surrounding code's style. Keep it a single self-contained file.

5. **Place vertically with clear zones.** The composition must breathe: logo +
   headline + tagline in the top third, the animation filling the middle band, the
   CTA anchored at the bottom — with gaps so nothing overlaps. `references/technique.md`
   gives the exact vertical budget and the collision math (the letter "bloom"
   extends *above* each element's tip, which is the easy thing to get wrong).

6. **Verify visually — do not skip this.** The animation has a timed cycle, so a
   single screenshot usually catches a half-grown frame. Serve the file and capture
   the *fully bloomed* state:
   - Start a local server from the output dir and open the file in a preview/browser
     (the file must be served over `http://`, not `file://`).
   - To reliably catch the bloom, temporarily set the cycle to fast-grow + long-hold
     (e.g. `GROW_TIME=2000, COLOR_TIME=1000, HOLD_TIME=30000`), screenshot, confirm
     it reads well and on-brand, then **restore the real timing**.
   - Check: the subject is recognizable, letters are legible at the bloom, colors are
     brand-correct, nothing overlaps the headline/CTA, and the CTA resolves to
     `https://tapvid.ai/` (it's an `<a>`, `pointer-events:auto`).

7. **Deliver** the path to the `.html`. Mention it loops and that the CTA opens
   tapvid.ai. Offer to tweak subject, palette accents, speed, or headline.

## Guardrails that keep it on-brand

- **Letters are the medium.** The signature TapVid texture is type-as-particles
  (ASCII art) in `JetBrains Mono`. Whatever the subject, render its detail/"bloom"
  out of characters, not generic dots — that's what ties any concept to the brand.
- **Palette discipline.** Use the documented tokens. Green↔gold is the established
  "ripening" journey; Signal Purple is a *highlight/accent*, never the whole field.
  Don't introduce off-brand hues just because a reference image had them.
- **The logo never competes with the H1** (brand rule). Keep it small, up top.
- **Self-contained.** One HTML file, p5.js from CDN, fonts from Google Fonts,
  logo inlined as SVG. No build step, no local asset dependencies.

## Files in this skill

- `references/brand.md` — TapVid colors, type scale, logo SVG, voice, copy, CTA.
  Read before writing any poster.
- `references/technique.md` — the procedural-animation recipe: the grow→ripen→bloom
  state machine, the wind field, the letter-bloom math, vertical layout budget,
  the ShinyText CTA, and how to adapt the module to new subjects. Read before
  editing the template.
- `assets/template.html` — the ready-to-run scaffold (brand chrome fixed, a worked
  "wheat" animation as the default). Copy and modify the ANIMATION MODULE.
