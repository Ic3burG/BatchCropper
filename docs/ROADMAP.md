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
- [x] **Rotation Handle**: Deferred to v1.8 (complete)

### v1.8 — Rotation (Complete)

- [x] **Rotation Handle**: Rotate the crop box (not the image) before export using canvas transform

### v1.5.2 — Preset UI Redesign (Complete)

- [x] **Dropdown Presets UI**: Replaced the chip-button row and scrollable preset list with two compact side-by-side `<select>` dropdowns — one for built-in ratios, one for saved user presets — each applying immediately on selection
- [x] **Preset Name Shown After Selection**: Built-in dropdown retains the selected preset name instead of resetting to placeholder, providing clear feedback on the active preset
- [x] **Ratio Lock from Preset**: Selecting a built-in preset simultaneously locks the aspect ratio in the Crop section (sets it to Custom with the preset's exact W:H values)
- [x] **Free Unlock in Preset Dropdown**: "Free" option in the built-in dropdown unlocks the aspect ratio and resets the dropdown to its placeholder — no need to use the Crop section separately
- [x] **Edge Handles Respect Ratio Lock**: Vertical edge handles (top/bottom) and horizontal edge handles (left/right) now enforce the locked aspect ratio, matching the behaviour of corner handles; the constrained dimension is recentered automatically
- [x] **Corner Handle Fix (Free Mode)**: The `tr` and `bl` corner handles now move freely in both axes when no ratio is locked; previously they were stuck to single-axis movement due to an assumption baked in for ratio-locked mode

### v2.0 — Sessions & Persistence

This version introduces stateful sessions as a first-class concept, allowing users to pause and resume large batch jobs.

- [ ] **Session Export/Import**: Download a `.json` "Session File" containing all current file names, metadata, crop coordinates, rotations, and export settings.
- [ ] **Intelligent File Re-linking**: A guided UI to "Reconnect Files" when loading a session, matching local files by name and size to restore the workspace pointers.
- [ ] **Project Auto-Save**: Periodically backup the active session to `IndexedDB` or `localStorage` to prevent data loss from accidental refreshes or browser crashes.
- [ ] **Multi-Selection Sidebar**: Shift-click or Ctrl-click to select multiple files in the sidebar and apply crops or settings to the subset.

### v2.1 — Scripted Workflow

Shifts the tool from purely visual to hybrid-scripted, enabling deterministic results for power users.

- [ ] **Expression Language**: A text-based "Crop Script" bar (e.g., `center(1:1)`, `top(16:9)`, `pad(10%)`) that can be applied to the whole batch.
- [ ] **Dynamic Batch Variables**: Support for variables like `w` (width), `h` (height), and `aspect` within scripts to allow responsive crops that adapt to different resolutions.
- [ ] **Conditional Logic**: Apply different scripts automatically based on image orientation or size (e.g., `if (landscape) rule_a else rule_b`).
- [ ] **Bulk Metadata Editing**: Ability to rename files based on a scriptable pattern or strip/inject specific EXIF tags during export.

### v2.3 — Intelligent Content Processing (Local AI)

Using client-side libraries like **Transformers.js** (running via WebAssembly/WebGPU), we could add:

- [ ] **Background Removal**: One-click background removal for "sticker" creation.
- [ ] **Client-Side Upscaling**: Simple super-resolution to clean up low-res images before cropping.
- [ ] **Smart Subject Tracking**: Automatically move the crop box to follow the primary subject across a series of similar photos.

### v2.4 — The "Social Media" Multi-Crop

Specialized workflows for cross-platform content creation.

- [ ] **Multi-Region Export**: Define multiple crop regions on a _single_ image and have them all exported as separate files in the ZIP.
- [ ] **Safe Zone Overlays**: Specialized overlays for Instagram/TikTok UI elements to avoid cropping content into interactive zones.

### v2.5 — Advanced Output & Color

- [ ] **Layered PSD/TIFF Export**: Export a ZIP of layered files where the crop, watermark, and original are on separate layers (via `ag-psd`).
- [ ] **Batch Color Correction**: Simple sliders for Brightness/Contrast/Saturation that apply to the whole batch.
- [ ] **LUT Support**: Support for uploading and applying `.cube` (3D LUT) files for cinematic color grading.

### v2.6 — Automation & Extensibility

- [ ] **Hot-Folder Monitoring**: (Requires v2.2 Desktop Shell) Automatically process and export any images dropped into a specific folder.
- [ ] **Custom Post-Processing Hooks**: Small JavaScript snippet execution on the `CanvasRenderingContext2D` after crop but before export.

---

## Architecture Philosophy

BatchCropper is intentionally a **single HTML file** with no build toolchain. This keeps it:

- **Instantly usable** — open and go, no install
- **Auditable** — all logic is visible in one file
- **Portable** — works on any device with a modern browser
- **Privacy-preserving** — no server, no telemetry, no dependencies to audit

Any new features should respect these constraints. If a feature requires a build step or server component, it belongs in a separate project.
