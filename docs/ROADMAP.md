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

### v1.4 — Crop Intelligence (Complete)

- [x] **Smart Crop Suggestions**: Use the browser's `ShapeDetection` API (no server, no library) to detect faces/subjects and auto-propose a centered crop region
- [x] **Crop by Percentage**: Express crop dimensions as `%` of image size instead of pixels — useful when images vary in resolution but you want the "middle 80%"
- [x] **Mirror/Flip on Export**: Apply horizontal or vertical flip at export time via canvas compositing; common for social media variants

### v1.5 — Workflow (Complete)

- [x] **Named Crop Presets**: Save and recall named crop regions (e.g., "Instagram Square", "Twitter Header")
- [x] **Sort Sidebar**: Sort the file list by filename, file size, or export status
- [x] **Clear Completed Files**: Remove already-exported files from the list without restarting the session
- [x] **Batch Resize on Export**: Optionally scale the output to a fixed resolution after cropping

### v1.5.1 — Collapsible Sidebar (Complete)

- [x] **Collapsible Sections**: Crop, Presets, and Export sections collapse/expand via a shared helper; state persists across reload

### v1.6 — Export Enhancements (Complete)

- [x] **AVIF Format Support**: Add `image/avif` as an export format — natively supported by Canvas API in Chrome/Firefox, no new dependency needed
- [x] **Watermark / Overlay Stamp**: Draw text or a logo at a fixed position on export using canvas compositing
- [x] **Copy to Clipboard**: Single-file "Copy cropped image" button using the browser Clipboard API

### v1.7 — Visual Aids (Complete)

- [x] **Golden Ratio Grid Overlay**: Alternative to rule-of-thirds, togglable alongside existing grid
- [x] **Crop Comparison View**: Side-by-side before/after preview within the canvas
- **Rotation Handle**: Deferred to v1.8

### v1.8 — Rotation

- **Rotation Handle**: Rotate the crop box (not the image) before export using canvas transform

### v2.0 — New Paradigm (Breaking Changes)

These ideas change the mental model of the tool and warrant a major version bump:

- **Session Save/Restore**: Export current file list + crop settings as a `.json` file and reload it later — introduces stateful sessions as a first-class concept
- **Scripted Crop Rules**: A small expression language (e.g., `center(1:1)`, `top(16:9)`) for describing crops programmatically, applied to the whole batch; moves BatchCropper from visual-first to scripted-first for power users
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
