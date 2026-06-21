name: tapvid-dynamic-poster
description: >-
  Generate an animated 9:16 "dynamic poster" (a single self-contained HTML file
  using p5.js) in the TapVid brand design language, driven by a user prompt or a
  reference image. The poster renders a procedural, looping animation that
  visualizes the user's concept, built out of moving letters/text particles
  (TapVid's ASCII-art motif) in the brand palette and type, with the TapVid logo,
  a Display headline, and a shimmering "Start Creating" button that links to
  tapvid.ai. Use this skill whenever the user wants a TapVid poster, cover,
  thumbnail, key visual, hero animation, social or launch graphic,
  or asks to turn a prompt or reference image into a TapVid-branded animated
  visual, even if they don't say "poster" explicitly. Also use it to restyle an
  existing visual into the TapVid look or to change the poster's subject.
---

# TapVid Dynamic Poster

Turn a user's prompt or reference image into an **animated 9:16 poster** that looks
and feels like TapVid: one self-contained `.html` file, p5.js for the motion, the
brand's gradient + type + logo as fixed chrome, and a procedural animation in the
center that visualizes whatever the user described, rendered out of **moving
letters** (the brand's ASCII-art element) in the brand colors.

The output is always a poster that (a) is on-brand, (b) animates in a satisfying
loop, and (c) has a working **Start Creating → https://tapvid.ai** button.

## What stays fixed vs. what you design

The power of this skill is that the *brand chrome and the hard parts* (9:16 sizing,
gradient, logo, type scale, the shimmer CTA, the looping motion clock, the wind
field) are already solved in the template. **You design the central animation -
both its form and its motion**, so it matches the user's concept. This keeps every
poster correct and lets you focus creative energy where it matters.

- **Fixed (keep from the template):** canvas `1080×1920` auto-scaled to fit, the
  `linear-gradient(180deg, #F0FAE0, #EFEEFF)` background, the embedded TapVid logo,
  the `Plus Jakarta Sans` headline + tagline, the ShinyText CTA wired to
  `https://tapvid.ai`, and the timed loop clock.
- **You design:** the subject metaphor, the procedural geometry that builds it, the
  color treatment, and **the motion that brings it to life, inferred from what the
  concept is trying to say, not forced into one fixed pattern**, see
  `references/technique.md`.

## Workflow

1. **Read the references first.** Load `references/brand.md` (colors, type, logo,
   copy, CTA rules) and `references/technique.md` (the animation recipe and all
   tunable parameters). These are the source of truth, don't reinvent brand values
   from memory.

2. **Interpret the input into a visual concept, subject *and* motion.**
   - *Prompt only:* extract a concrete subject and a metaphor for it. TapVid is
     "Anything → Explainer/Motion Video," so the strongest concepts make *something
     come alive out of letters/text*. **Reason about how this particular concept
     naturally wants to move, and let the motion follow that logic**, don't impose
     a single canned animation on every subject. A wave flows and crashes; fireworks
     of glyphs burst and fall; a city skyline assembles upward; sound bars pulse to
     a beat; data streams across the frame; a crowd gathers or disperses; a sprout
     unfurls into a tree. Pick ONE clear subject and the ONE motion that best
     expresses its meaning; "one idea per screen" is a brand principle.
   - *Reference image provided:* read it. Identify its subject, composition
     (where the mass sits), motion implied, and palette. Reproduce the *structure
     and feeling*, then re-skin it into the TapVid palette/type and rebuild the
     texture out of letters. Let the implied motion of the reference drive your
     animation. Don't copy colors that fight the brand.
   - If the input is too vague to pick a subject or a motion, ask one short
     clarifying question rather than guessing blandly.

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
   CTA anchored at the bottom, with gaps so nothing overlaps. `references/technique.md`
   gives the exact vertical budget and the collision math (the moving letters can
   extend *beyond* each element's bounds, e.g. above its tip, which is the easy
   thing to get wrong).

6. **Verify visually, do not skip this.** The animation has a timed cycle, so a
   single screenshot usually catches a transitional frame. Serve the file and capture
   the *fully resolved / settled* state of the loop:
   - Start a local server from the output dir and open the file in a preview/browser
     (the file must be served over `http://`, not `file://`).
   - To reliably catch the peak frame, temporarily bias the loop so it reaches its
     settled state quickly and holds there for a long time (shorten the build phase,
     lengthen the hold), screenshot, confirm it reads well and on-brand, then
     **restore the real timing**.
   - Check: the subject is recognizable, the motion makes sense for the concept,
     letters are legible at the animation's peak frame, colors are brand-correct,
     nothing overlaps the headline/CTA, and the CTA resolves to `https://tapvid.ai/`
     (it's an `<a>`, `pointer-events:auto`).

7. **Deliver** the path to the `.html`. Mention it loops and that the CTA opens
   tapvid.ai. Offer to tweak subject, motion, palette accents, speed, or headline.

## Guardrails that keep it on-brand

- **Motion follows meaning.** Don't impose a single fixed animation arc on every
  poster. Infer the motion from what the subject *is* and what it's trying to say,
  then build the one motion that expresses it best. The only constants are the brand
  chrome and that the texture is built from letters.
- **Letters are the medium.** The signature TapVid texture is type-as-particles
  (ASCII art) in `JetBrains Mono`. Whatever the subject, render its detail out of
  characters, not generic dots, that's what ties any concept to the brand.
- **Palette discipline.** Use the documented tokens. Green↔gold is the brand's
  established color pairing; Signal Purple is a *highlight/accent*, never the whole
  field. Use color to support the concept, but don't introduce off-brand hues just
  because a reference image had them.
- **The logo never competes with the H1** (brand rule). Keep it small, up top.
- **Self-contained.** One HTML file, p5.js from CDN, fonts from Google Fonts,
  logo inlined as SVG. No build step, no local asset dependencies.

## Files in this skill

- `references/brand.md`, TapVid colors, type scale, logo SVG, voice, copy, CTA.
  Read before writing any poster.
- `references/technique.md`, the procedural-animation recipe: the looping motion
  clock, the wind field, the letter/particle math, vertical layout budget, the
  ShinyText CTA, and how to reason about and build a fitting motion for a new
  subject. Read before editing the template.
- `assets/template.html`, the ready-to-run scaffold (brand chrome fixed, with one
  worked example animation included). Copy and replace the ANIMATION MODULE.
