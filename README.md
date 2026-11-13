# 🧠 BraTS Brain Tumor — Free-Rotation WebXR AR Viewer

A lightweight, no-build **WebXR augmented reality viewer** for a 3D brain tumor segmentation mesh. Place a translucent brain with a highlighted tumor on your table or floor using an Android phone, then rotate it freely on every axis, pinch to scale, and inspect it from any angle.

> ⚠️ **Disclaimer:** Research and educational visualization only. **Not a medical device and not intended for clinical diagnosis or decision-making.**

---

## ✨ Features

- **Markerless AR** via the WebXR Device API (`immersive-ar`) with surface detection (hit-test) and a placement reticle
- **Fallback placement** in front of the camera if no surface is detected
- **Free rotation on all axes** — one-finger drag spins/tilts relative to the camera view
- **Pinch to scale** (0.2× – 6×) and **two-finger twist to roll**
- **Move** and **Reset rotation** controls
- Translucent brain shell (light gray-blue) with the **predicted whole tumor (WT)** rendered in opaque red
- Runs fully client-side: a single `index.html` + one `.glb` file, Three.js loaded from a CDN

---

## 📁 Repository Structure

```
.
├── index.html            # AR viewer (Three.js + WebXR, single file)
├── brain_tumor_ar.glb    # 3D mesh (~2.6 MB)
└── README.md
```

### Model contents (`brain_tumor_ar.glb`)

| Mesh node              | Description                                  | Vertices |
|------------------------|----------------------------------------------|----------|
| `translucent_brain`    | Brain surface shell (rendered semi-transparent) | 30,004 |
| `predicted_tumor_WT`   | Predicted whole-tumor segmentation surface   | 37,021   |

The GLB stores only positions and vertex colors (no normals or materials). The viewer computes normals and applies its own materials at load time.

---

## 📱 Requirements

| Requirement | Details |
|---|---|
| Device | Android phone with **ARCore** support |
| Browser | **Chrome for Android** (WebXR `immersive-ar`) |
| Connection | **HTTPS** (required by WebXR; `localhost` also works with port forwarding) |
| iOS | Not supported (Safari lacks WebXR `immersive-ar`) |

---

## 🚀 Quick Start

### Option 1 — GitHub Pages (recommended, gives HTTPS for free)

1. Push the three files to a GitHub repo (e.g. `brain-tumor-ar`).
2. Go to **Settings → Pages**.
3. Under **Build and deployment**, choose **Deploy from a branch**, select `main` and `/ (root)`, then **Save**.
4. After a minute your app is live at:
   ```
   https://adarshanandiitbhu.github.io/brain-tumor-ar/
   ```
5. Open that URL in **Chrome on your Android phone** and tap **Start AR**.

### Option 2 — Run locally and test on your phone

```bash
# from the project folder
python -m http.server 8000
```

WebXR needs a secure context, so either:

- **USB port forwarding:** connect the phone with USB debugging on, open `chrome://inspect/#devices` on your desktop Chrome, add the rule `8000 → localhost:8000`, then open `http://localhost:8000` on the phone; **or**
- **Tunnel:** use `ngrok http 8000` (or Cloudflare Tunnel) and open the HTTPS URL on your phone.

---

## 🕹️ How to Use

1. Open the page and wait for the status to show **Ready**.
2. Tap **Start AR** and grant camera permission.
3. Slowly move the phone to scan a floor or table until the green reticle appears.
4. Tap **Place here** — the model floats 20 cm above the surface.
5. Interact:

| Gesture | Action |
|---|---|
| 1-finger drag | Rotate freely (left/right = spin, up/down = tilt) |
| Pinch | Scale |
| 2-finger twist | Roll around the view direction |
| **Move** button | Re-place the model |
| **Reset rotation** | Restore default orientation and scale |
| **Exit AR** | End the session |

---

## ⚙️ Configuration

Tweak these constants at the top of the `<script type="module">` block in `index.html`:

```js
const MODEL_URL = "./brain_tumor_ar.glb";
const MODEL_SIZE_METERS = 0.3;   // longest side of the model in AR
const FLOAT_HEIGHT = 0.2;        // height above detected surface (m)
const ROTATE_SPEED = 0.008;      // radians per pixel of drag
const MIN_SCALE = 0.2, MAX_SCALE = 6;
const TUMOR_COLOR = 0xe61919;    // red
const SHELL_COLOR = 0xb9c3d2;    // light gray-blue
const SHELL_OPACITY = 0.25;      // 0 = invisible, 1 = solid
```

---

## 🛠️ Tech Stack

- [Three.js](https://threejs.org/) r160 (via jsDelivr import map) — `GLTFLoader`, `RoomEnvironment`
- [WebXR Device API](https://immersiveweb.dev/) — `immersive-ar`, `hit-test`, `dom-overlay`
- Vanilla HTML/CSS/JS, no bundler

---

## 🧪 Data & Model Provenance

The mesh is derived from an AI-based brain tumor segmentation (whole tumor, WT) on a case from the **BraTS** (Brain Tumor Segmentation) benchmark, converted from a voxel segmentation to a surface mesh and exported to GLB.

> Add your details here: dataset/year (e.g. BraTS 20XX), case ID, segmentation model used, and mesh-generation steps (e.g. marching cubes + smoothing). Please also follow the BraTS data usage terms and cite the dataset accordingly.

---

## 🩹 Troubleshooting

| Problem | Fix |
|---|---|
| "WebXR not available" | Use Chrome on Android over HTTPS |
| "Immersive AR is not supported" | Install/update **Google Play Services for AR (ARCore)** and Chrome |
| Start AR button stays disabled | Model still loading or AR unsupported; check the status text |
| Model failed to load | Make sure `brain_tumor_ar.glb` sits next to `index.html` with the exact filename |
| Reticle never appears | Scan a textured, well-lit surface, or tap **Place here** to place in front of the camera |
| Blank page via `file://` | Serve over HTTP(S); browsers block module/GLB loading from `file://` |

---

## 🗺️ Possible Future Work

- Add enhancing tumor / necrotic core sub-regions (ET, TC) with a toggle
- Slice-by-slice MRI plane overlay
- Standard (non-AR) 3D orbit viewer fallback for desktop and iOS
- Opacity slider and measurement tools

---

## 📜 License

Add a license of your choice (e.g. MIT for the code). Note that data derived from BraTS is subject to the dataset's own terms.

---

## 👤 Author

**Adarshanand** — [@AdarshanandIITBHU](https://github.com/AdarshanandIITBHU)

---

## ⚠️ Medical Disclaimer

This project is for research and educational visualization only. Outputs have not been clinically validated and must not be used for diagnosis, treatment planning, or any clinical decision-making.