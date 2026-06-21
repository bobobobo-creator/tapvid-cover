# TapVid · Dynamic Posters & LCD Showcase

自包含的 TapVid 品牌**动态海报**（实时渲染、循环播放的 9:16 关键视觉），以及一个把它们当作**可替换液晶挂画**展示的交互式 3D 办公室场景。
Self-contained, real-time animated TapVid key visuals (9:16, looping) plus an interactive 3D office that hangs them as swappable LCD wall art.

> **Anything to Motion Video** — [tapvid.ai](https://tapvid.ai)

## 🔗 在线预览 / Live preview

| 页面 | 说明 | 链接 |
|---|---|---|
| 导航首页 / Home | 三个入口 + 实时缩略图 | https://bobobobo-creator.github.io/tapvid-cover/ |
| 3D 办公室展示 / 3D Showcase | 可环视的设计工作室，海报作为液晶挂画 | https://bobobobo-creator.github.io/tapvid-cover/office.html |
| 麦穗海报 / Wheat | 字母麦穗生长→成熟→绽放 | https://bobobobo-creator.github.io/tapvid-cover/TapVid-cover-wheat.html |
| 粽子海报 / Zongzi (端午) | 极简粽子 + 物理漂浮 + 飘烟 | https://bobobobo-creator.github.io/tapvid-cover/duanwu.html |

> 每次 `git push` 到 `main` 后，GitHub Pages 会自动重新发布（约 1–2 分钟，记得强制刷新）。

## 📦 内容 / Contents

```
index.html                 导航首页（实时缩略图卡片）
TapVid-cover-wheat.html     麦穗主题动态海报
duanwu.html                 粽子主题动态海报（端午 / 极简物理风）
office.html                 3D 办公室展示（Three.js CSS3DRenderer）
tapvid-dynamic-poster/      生成动态海报的 Skill（可复用）
  ├─ SKILL.md
  ├─ references/            品牌系统 + 动效技法
  └─ assets/                模板 + Logo
```

## 🎨 设计要点 / How it works

- **海报 (posters):** 用 [p5.js](https://p5js.org/) 程序化生成，画布 `1080×1920`，自动缩放适配。
  画面由**流动的字母**（TapVid 的 ASCII art 母题，JetBrains Mono）构成，配合品牌渐变与字体。
- **3D 展示 (office):** 用 Three.js 的 `CSS3DRenderer`，把真实的海报 HTML 以 `iframe`
  嵌入 3D 空间作为液晶挂画 —— 因此画面是**真正实时动画**，并可在面板里**随时替换**内容。
- **CTA:** 每幅海报底部的 *Start Creating* 按钮链接到 [tapvid.ai](https://tapvid.ai)。

### 品牌色 / Brand palette
`Tappy Green #8FD71B` · `Ink #0A0A0A` · `Signal Purple #5B4FFF` · 背景渐变 `#F0FAE0 → #EFEEFF`

## 🖥️ 本地预览 / Run locally

海报间通过相对路径的 `iframe` 互相引用，并从 CDN 加载 p5.js / Three.js，**需用本地服务器打开**（不能直接 `file://`）：

```bash
cd tapvid-cover
python3 -m http.server 8080
# 打开 http://localhost:8080/
```

## 🧩 用 Skill 生成新海报 / Generate more

`tapvid-dynamic-poster/` 是一个可复用的 Claude Code Skill：输入一个提示词或参考图，
即可在 TapVid 设计语言下生成一幅新的 9:16 动态海报（自带品牌外壳 + 跳转 tapvid.ai 的按钮）。
复制 `assets/template.html`，按 `references/technique.md` 修改中间的 **ANIMATION MODULE** 即可。

---
🤖 Generated with [Claude Code](https://claude.com/claude-code)
