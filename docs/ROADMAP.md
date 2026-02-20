# BatchCropper — Product Roadmap

## 🏁 Current Status

BatchCropper is a **stable, single-file web tool** for batch image cropping. It is fully functional and ready for everyday use.

---

## ✅ Completed

### v1.0 — Core Tool (Complete)

- [x] Single-file HTML/JS/CSS application (zero build step)
- [x] Drag-and-drop image loading (PNG, JPEG, WebP)
- [x] Visual crop editor with draggable handles (8-point: corners + edges)
- [x] Rule-of-thirds grid overlay on crop selection
- [x] Dark mask outside crop region for clear visual feedback
- [x] Numeric input fields (X, Y, Width, Height) with two-way sync
- [x] Zoom controls (zoom in, zoom out, fit-to-screen)
- [x] Coordinate display in canvas toolbar
- [x] File list sidebar with active/done status indicators
- [x] Batch export: all images cropped and bundled into a ZIP (JSZip)
- [x] Format selection: PNG, JPEG, WebP
- [x] Progress bar and status text during export
- [x] Drop additional images onto canvas while working
- [x] Reset crop selection button

---

## 📅 Upcoming Features

### v1.1 — Quality of Life

- [x] **Aspect Ratio Lock**: Constrain crop to common ratios (1:1, 4:3, 16:9, custom)
- [x] **Invert Aspect Ratio**: Quickly flip between landscape and portrait orientations (e.g., 16:9 ⇅ 9:16)
- [x] **Keyboard Shortcuts**: Arrow keys to nudge crop region by 1px; `R` to reset
- [x] **Direct Mouse Move**: Drag the selection box itself to reposition the crop area
- [x] **Per-file Crop Override**: Allow individual files to have a different crop than the batch default
- [x] **Image Info Display**: Show native resolution and file size in the file list

### v1.2 — Export Enhancements

- [x] **JPEG Quality Slider**: Control compression quality for JPEG/WebP exports
- [x] **Output Filename Template**: Customizable suffix/prefix for exported filenames (e.g., `_cropped`)
- [x] **Individual File Download**: Option to download a single file instead of the full ZIP

### v1.3 — UX Polish

- [x] **Undo/Redo**: Step back through crop region changes
- [x] **Touch Support**: Basic touch events for tablet use
- [x] **Persistent Settings**: Remember last-used format and zoom level via `localStorage`

---

## 🔮 Future Ideas

- **Preset Crop Regions**: Save and recall named crop presets
- **Batch Resize**: Optionally scale the output to a fixed resolution after cropping
- **PWA / Offline Mode**: Package as a Progressive Web App for offline use
- **Electron Wrapper**: Native desktop app with folder-level batch processing

---

## Architecture Philosophy

BatchCropper is intentionally a **single HTML file** with no build toolchain. This keeps it:

- **Instantly usable** — open and go, no install
- **Auditable** — all logic is visible in one file
- **Portable** — works on any device with a modern browser
- **Privacy-preserving** — no server, no telemetry, no dependencies to audit

Any new features should respect these constraints. If a feature requires a build step or server component, it belongs in a separate project.
