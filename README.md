# TapVid · Dynamic Posters & LCD Showcase

Self-contained TapVid brand **dynamic posters** (real-time, looping 9:16 key visuals), plus an interactive 3D office scene that hangs them as swappable LCD wall art.

> **Anything to Motion Video** · [tapvid.ai](https://tapvid.ai)

## Live preview

| Page | Description | Link |
|---|---|---|
| Home | Three entries with live thumbnails | https://bobobobo-creator.github.io/tapvid-cover/ |
| 3D Showcase | Orbit a design studio with posters as LCD wall art | https://bobobobo-creator.github.io/tapvid-cover/office.html |
| Wheat | Letter wheat grows, ripens, then blooms | https://bobobobo-creator.github.io/tapvid-cover/TapVid-cover-wheat.html |
| Zongzi | Minimalist zongzi with natural physics and drifting smoke | https://bobobobo-creator.github.io/tapvid-cover/duanwu.html |
| SigmaZ Orb | A letter-sphere assembles in a dark porthole, ripening ink→blue→green | https://bobobobo-creator.github.io/tapvid-cover/TapVid-cover-sigmaz.html |

After every `git push` to `main`, GitHub Pages republishes automatically (about 1 to 2 minutes; do a hard refresh).

## Contents

```
index.html                 Navigation home (live thumbnail cards)
TapVid-cover-wheat.html     Wheat-themed dynamic poster
duanwu.html                 Zongzi-themed dynamic poster (Dragon Boat Festival, minimalist physics)
TapVid-cover-sigmaz.html    SigmaZ-themed dynamic poster (letter-sphere in a porthole, ink→blue→green)
office.html                 3D office showcase (Three.js CSS3DRenderer)
tapvid-dynamic-poster/      Reusable skill that generates these dynamic posters
  ├─ SKILL.md
  ├─ references/            Brand system and animation technique
  └─ assets/                Template and logo
```

## How it works

- **Posters:** generated procedurally with [p5.js](https://p5js.org/) on a `1080x1920` canvas that auto scales to fit. The artwork is made of moving letters (TapVid's ASCII art motif, JetBrains Mono) on the brand gradient and type.
- **3D showcase:** Three.js `CSS3DRenderer` embeds the real poster HTML as `iframe` panels inside a 3D room, so every frame is truly live and the content can be swapped at any time.
- **CTA:** the *Start Creating* button at the bottom of each poster links to [tapvid.ai](https://tapvid.ai).

### Brand palette
`Tappy Green #8FD71B` · `Ink #0A0A0A` · `Signal Purple #5B4FFF` · background gradient `#F0FAE0` to `#EFEEFF`

## Run locally

The posters reference each other through relative `iframe` paths and load p5.js / Three.js from a CDN, so they need a local server (opening `file://` will not work):

```bash
cd tapvid-cover
python3 -m http.server 8080
# open http://localhost:8080/
```

## Generate more posters

`tapvid-dynamic-poster/` is a reusable Claude Code skill. Give it a prompt or a reference image and it produces a new 9:16 dynamic poster in the TapVid design language, complete with the brand shell and a button that links to tapvid.ai. Copy `assets/template.html` and edit the **ANIMATION MODULE** following `references/technique.md`.

---
Generated with [Claude Code](https://claude.com/claude-code)
