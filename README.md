<div align="center">

# 🦾 A.R.C. Hand Interface

### An Iron Man–style holographic HUD, controlled entirely with your bare hands.

*No controller. No keyboard. No mouse. Just a webcam and 21 tracked points on your hand.*

![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![MediaPipe](https://img.shields.io/badge/MediaPipe-Tasks--Vision-0F9D58?style=for-the-badge&logo=google&logoColor=white)
![Edge AI](https://img.shields.io/badge/Edge%20AI-On--Device-8A2BE2?style=for-the-badge)
![No Backend](https://img.shields.io/badge/Backend-None-success?style=for-the-badge)
![License](https://img.shields.io/badge/License-MIT-yellow?style=for-the-badge)

[**Live Demo**](https://iron-man-hud-eosin.vercel.app) · [Gesture Guide](#-gesture-guide) · [Getting Started](#-getting-started) · [Tech Stack](#-tech-stack--how-it-works)

</div>

---

## 📖 Overview

**A.R.C. Hand Interface** turns your browser into a full holographic control panel — the kind you've seen Tony Stark wave his hands through. Point your webcam at yourself, and a real-time hand-tracking model reads your gestures to rotate menus, lock targets, raise an energy shield, fire a repulsor blast, and cycle through fully holographic HUD views.

It's a **browser-native Edge AI project**: the entire hand-tracking model runs *on your device*, inside the tab, via WebAssembly and WebGL. Your camera feed is never uploaded anywhere — there is no backend, no server, and no account. Open the file, grant camera access, and the AI runs locally on your own GPU/CPU.

> **Category:** Edge AI · On-Device Computer Vision · Browser-Based Human-Computer Interaction

---

## ✨ Features

- 🖐️ **Real-time 21-point hand tracking** — no gloves, no markers, no sensors
- 🎯 **Gesture-driven UI** — pinch to lock, point to fire, peace-sign to switch views
- 🛡️ **Holographic shield projection** that follows your palm
- 🔫 **Repulsor blast** with a genuine charge-up beat before it fires
- 🌀 **Two-hand zoom** — pull apart / push together to scale and charge the whole HUD
- 🔊 **J.A.R.V.I.S. voice welcome** — plays your own custom audio clip the moment your hand enters frame
- 📊 **Live telemetry** — STATUS, POWER, TEMP, CHARGE, HANDS, and MODE, all driven by real tracking data, not fake placeholders
- 📜 **Live system log** — every gesture and view change streams into an on-screen console
- 🧩 **Zero dependencies to install** — a single self-contained HTML file; the only thing loaded from the network is the tracking model itself
- 🛠️ **Self-healing tracking** — automatically falls back from GPU to CPU inference if your hardware/browser can't run the GPU delegate

---

## 🎮 Gesture Guide

| Gesture | Effect | Works in |
|---|---|---|
| ☝️ Move index finger | Rotates the targeting ring toward it | Hand Interface |
| 🤏 **Pinch** (thumb + index touch) | Locks the targeted segment (turns orange) | All views |
| ✊ **Fist** | Raises a glowing energy shield at your palm | All views |
| 👉 **Point** (index only extended) | Fires a scan/laser beam from your fingertip | All views |
| ✋ **Open palm**, held ~0.4s | Charges up, then fires a repulsor blast | All views |
| ✌️ **Peace sign** | Cycles to the next HUD view | All views |
| 🙌 **Two hands**, pull apart / push together | Zooms the whole HUD in/out and charges the core | All views |
| 🖐️ Raise one hand on **System Online** | Plays the J.A.R.V.I.S. welcome line (once per session) | System Online only |

Prefer clicking? Every gesture that changes the view also has a manual **◄ ►** arrow / dot control at the bottom of the screen.

---

## 🖥️ The Four Views

Cycle through with a peace sign, or the arrow controls:

| # | View | Description |
|---|---|---|
| 1 | **Hand Interface** | Live camera feed + rotating radial targeting menu |
| 2 | **Arc Reactor** | Glowing holographic reactor emblem |
| 3 | **Iron Man Suit** | Front/back suit renders as a rotating holographic overlay |
| 4 | **System Online** | Clean view — raise a hand here to trigger the J.A.R.V.I.S. welcome line |

---

## 🚀 Getting Started

### Option A — Run it locally (zero setup)

1. Download `index.html` and `jarvis-welcome.mp3` from this repo, and keep them **in the same folder**.
2. Double-click `index.html`.
3. Use **Chrome** or **Edge** for the most reliable camera + GPU support.
4. Click **ENGAGE**, allow camera access, and wait for `SYSTEM ONLINE`.

No installs, no build step, no `npm install`. The only network activity is a one-time fetch of the tracking model and fonts on first load.

### Option B — Deploy your own copy

[![Deploy with Vercel](https://vercel.com/button)](https://vercel.com/new/clone?repository-url=https://github.com/<YOUR_USERNAME>/iron-man-hud)

1. Fork or clone this repo.
2. Import it into [Vercel](https://vercel.com/new) — no framework, no build command needed, it's a static file.
3. Deploy. You'll get a live HTTPS link in seconds.

> 💡 HTTPS is actually a plus here — browsers require a secure context for camera access, so a real deployment is more reliable than some local setups.

---

## 🧠 Tech Stack & How It Works

| Layer | Technology |
|---|---|
| Structure & styling | HTML5, CSS3 (custom properties, keyframe animations) |
| Logic | Vanilla JavaScript (ES modules) — no framework, no bundler |
| Hand tracking model | [MediaPipe Tasks Vision — HandLandmarker](https://ai.google.dev/edge/mediapipe/solutions/vision/hand_landmarker) (Google), pinned version |
| Inference runtime | WebAssembly + WebGL GPU delegate, with automatic CPU delegate fallback |
| Rendering | HTML5 Canvas 2D (effects, skeleton overlay, UI drawing) |
| Audio | Web Audio API (procedural UI tones — no sound files needed) + `<audio>` for the J.A.R.V.I.S. clip |
| Hosting | Any static host — Vercel, GitHub Pages, Netlify, or just the local filesystem |

**Why this counts as Edge AI:** the hand-landmark detection model — 21 3D keypoints per hand, 2 hands, in real time — is downloaded once and then runs entirely **on-device**, inside the browser tab, using your own CPU or GPU. There is no inference server, no API call per frame, and no video data ever leaves your machine. This is the same class of on-device ML that powers things like Google Lens gestures and on-phone face unlock, just running in a tab instead of a native app.

**Pipeline, at a glance:**

```
Webcam → HandLandmarker (WASM/WebGL) → 21 landmarks per hand
       → gesture math (pinch / fist / point / palm / peace / two-hand)
       → Canvas rendering (targeting ring, shield, laser, particles)
       → on-screen state (STATUS / POWER / TEMP / CHARGE / MODE)
```

If the GPU delegate fails to initialize — or even if it initializes but breaks mid-session on certain hardware — the app automatically rebuilds the tracker on the CPU delegate and keeps going, logging exactly what happened to the on-screen system log.

---

## 🌐 Browser Support

| Browser | Support |
|---|---|
| Chrome | ✅ Recommended |
| Edge | ✅ Recommended |
| Firefox | ⚠️ Untested |
| Safari | ⚠️ Untested |

Hand tracking relies on WebGL + WebAssembly camera pipelines that are most mature in Chromium-based browsers today.

---

## 🩺 Troubleshooting

| Problem | Likely cause / fix |
|---|---|
| No welcome audio | Confirm `jarvis-welcome.mp3` is named exactly that and sits next to `index.html` |
| ENGAGE does nothing / stuck on boot | Open DevTools (F12) → Console, check for a red error, and confirm camera permission wasn't blocked |
| "GPU DELEGATE UNAVAILABLE — RETRYING ON CPU" | Normal on some machines — it self-recovers, just takes a couple seconds longer |
| Gestures feel unresponsive | Keep your whole hand in frame and reasonably lit — tracking quality depends on the camera image |
| Something silently stops working | Check the **SYSTEM LOG** panel on the right — runtime errors are surfaced there, not just in DevTools |

---

## 📁 Project Structure

```
iron-man-hud/
├── index.html            # entire app — markup, styles, and logic in one file
├── jarvis-welcome.mp3    # your own J.A.R.V.I.S.-style welcome clip
└── README.md
```

---

## 🔒 Privacy

Everything runs client-side. Your camera feed is processed frame-by-frame in memory and is never recorded, stored, or transmitted anywhere — not to a server, not to analytics, not to the model provider. Closing the tab clears everything.

---

## ⚠️ Disclaimer

This is an unofficial, non-commercial fan project inspired by the *Iron Man* films. It is not affiliated with, endorsed by, or sponsored by Marvel Studios, Disney, or any related rights holders. All character names and references are used for descriptive/fan purposes only.

---

## 🙏 Acknowledgments

- [MediaPipe](https://ai.google.dev/edge/mediapipe) by Google — real-time, on-device hand landmark detection
- [jsDelivr](https://www.jsdelivr.com/) — CDN hosting for the tracking model runtime

---

## 📄 License

Released under the [MIT License](LICENSE) — do whatever you'd like with it, a credit back to this repo is always appreciated.

<div align="center">

*Built for fun. Powered by your own hand.* 🖐️⚡

</div>
