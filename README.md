# ✋ Air Canvas

A browser-based drawing app that lets you **paint in the air using only your hand and a webcam** — no mouse, no touch, no stylus. Powered by [MediaPipe Hands](https://google.github.io/mediapipe/solutions/hands) for real-time hand tracking, with neon-glow strokes rendered on an HTML5 Canvas.

---

## ✨ Features

- **Gesture-controlled drawing** — point your index finger to paint; change gestures to pause, erase, or drag
- **Neon glow strokes** — every line renders with a layered glow + white highlight for a vibrant look
- **10 color swatches** — pick from a curated neon palette via the UI
- **4 brush sizes** — from fine lines to thick fills
- **Canvas drag** — pinch (thumb + index) to reposition your entire drawing
- **Undo & Clear** — full undo history and one-click canvas wipe
- **Save to PNG** — exports the drawing as a timestamped `.png` file
- **Stroke stabilizer** — 10-frame rolling average smooths out hand jitter
- **Live hand skeleton overlay** — see your hand landmarks and connections in real time
- **Zero dependencies to install** — runs entirely in the browser; MediaPipe loads from CDN

---

## 🖐️ Gesture Reference

| Gesture | Action |
|---|---|
| ☝️ Index finger extended | **Draw** |
| 🤏 Thumb + Index pinch | **Drag** the entire canvas |
| ✌️ Two fingers (peace) | **Pen Up** — pause drawing |
| 🖐️ Open hand | **Pen Up** — pause drawing |
| ✊ Fist | **Pen Up** — pause drawing |

---

## 🚀 Getting Started

No build step, no server required. Just open the file.

### Option 1 — Open directly in browser

```bash
# Clone the repo
git clone https://github.com/your-username/air-canvas.git
cd air-canvas

# Open in your browser
open hand_canvas.html        # macOS
start hand_canvas.html       # Windows
xdg-open hand_canvas.html    # Linux
```

### Option 2 — Serve locally (recommended for camera access on some browsers)

```bash
# Python 3
python -m http.server 8080

# Then open:
# http://localhost:8080/hand_canvas.html
```

> **Camera permission is required.** When prompted by your browser, click **Allow**.

---

## 🎨 UI Controls

| Control | Location | Description |
|---|---|---|
| ✏️ Draw mode | Toolbar (top-left) | Switch to drawing mode |
| 🧹 Erase mode | Toolbar (top-left) | Switch to eraser |
| ↩ Undo | Toolbar (top-left) | Step back one stroke |
| 🗑 Clear | Toolbar (top-left) | Wipe the entire canvas |
| 💾 Save | Toolbar (top-left) | Download canvas as PNG |
| Color swatches | Panel (top-right) | Select brush color |
| Brush size dots | Panel (top-right) | Select brush thickness |

The **gesture status bar** at the bottom center shows the detected gesture and a live hint in real time.

---

## 🛠️ How It Works

```
Webcam feed (hidden video element)
        │
        ▼
MediaPipe Hands  ──►  21 hand landmarks (x, y, z per frame)
        │
        ▼
Gesture Classifier
  • index extended?    → draw stroke on Canvas
  • thumb+index close? → drag canvas snapshot
  • 2+ fingers up?     → pen up
        │
        ▼
Stabilizer (10-frame rolling average)  →  smooth (x, y)
        │
        ▼
Canvas Renderer
  • Overlay canvas: hand skeleton + cursor ring
  • Draw canvas:    neon glow strokes (shadowBlur + white highlight)
```

---

## 📁 Project Structure

```
air-canvas/
└── hand_canvas.html    # Self-contained — all HTML, CSS, and JS in one file
```

---

## 🌐 Browser Compatibility

| Browser | Support |
|---|---|
| Chrome / Edge (latest) | ✅ Recommended |
| Firefox (latest) | ✅ Supported |
| Safari 16+ | ✅ Supported |
| Mobile browsers | ⚠️ Limited (camera angle matters) |

> MediaPipe Hands performs best in well-lit environments with a clear background.

---

## ⚙️ Customization

All key values are easy to tweak at the top of the `<script>` block:

```js
const BUFFER_SIZE = 10;      // Stabilizer window (higher = smoother, more lag)
const COLORS = [...];        // Palette — add or replace hex values
const SIZES  = [3,6,14,24]; // Brush sizes in pixels
```

MediaPipe confidence thresholds can also be adjusted:

```js
hands.setOptions({
  maxNumHands: 1,
  modelComplexity: 1,
  minDetectionConfidence: 0.75,  // Lower for dark/complex backgrounds
  minTrackingConfidence: 0.6
});
```

---

## 📄 License

MIT License — free to use, modify, and distribute.
