# Sidebar Cleanup Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Clean up the sidebar by relocating misplaced controls, pinning the primary actions, and improving visual hierarchy in the Export section.

**Architecture:** All changes are in `index.html` (single-file app). Five independent edits to HTML structure and minimal JS changes. No new CSS classes are required except `.sidebar-footer`. Changes must be made in order because line numbers shift after each edit.

**Tech Stack:** Vanilla HTML/CSS/JS. Prettier runs as a pre-commit hook (`npm run format:check`).

**Design doc:** `docs/plans/2026-03-14-sidebar-cleanup-design.md`

---

### Task 1: Move grid toggles to canvas toolbar

**Files:**

- Modify: `index.html` (toolbar ~line 826, crop body ~line 1099)

The two grid toggle buttons (`⊞ ⅓` and `⊞ φ`) currently sit at the bottom of the Crop section body. They are view controls and belong in the canvas toolbar alongside Ruler and Compare.

**Step 1: Remove grid buttons from Crop body**

Find and delete this entire block (~lines 1099–1117):

```html
<div style="display: flex; gap: 4px; margin-top: 4px">
  <button
    class="btn btn-ghost"
    id="grid-thirds-btn"
    style="flex: 1; font-size: 11px; font-family: var(--mono)"
    title="Toggle rule-of-thirds grid"
    data-shortcut="T"
  >
    ⊞ ⅓
  </button>
  <button
    class="btn btn-ghost"
    id="grid-golden-btn"
    style="flex: 1; font-size: 11px; font-family: var(--mono)"
    data-shortcut="L"
  >
    ⊞ φ
  </button>
</div>
```

**Step 2: Add grid buttons to toolbar**

Find the ruler button in the toolbar (~line 826):

```html
<button class="btn btn-ghost" id="ruler-btn" ...>📏 Ruler</button>
<button class="btn btn-ghost" id="compare-btn" ...>◫ Compare</button>
```

Insert the two grid buttons between `ruler-btn` and `compare-btn`:

```html
<button
  class="btn btn-ghost"
  id="grid-thirds-btn"
  style="padding: 4px 8px; font-size: 11px; font-family: var(--mono)"
  title="Toggle rule-of-thirds grid"
  data-shortcut="T"
>
  ⊞ ⅓
</button>
<button
  class="btn btn-ghost"
  id="grid-golden-btn"
  style="padding: 4px 8px; font-size: 11px; font-family: var(--mono)"
  title="Toggle golden ratio grid"
  data-shortcut="L"
>
  ⊞ φ
</button>
```

**Step 3: Verify**

Open in browser. The ⊞ ⅓ and ⊞ φ buttons should appear in the canvas toolbar between Ruler and Compare. The bottom of the Crop section should no longer have those buttons. Clicking them should still toggle the grids. Shortcut keys T and L should still work. Hovering should show tooltips with shortcuts.

**Step 4: Commit**

```bash
git add index.html
git commit -m "refactor: move grid overlay toggles to canvas toolbar"
```

---

### Task 2: Move px/% toggle into crop body

**Files:**

- Modify: `index.html` (crop section header ~line 900, crop body ~line 917)

The `px` mode toggle button currently lives in the Crop section header, making the header asymmetric with Presets and Export. Move it into the crop body as a `[px] [%]` two-button strip.

**Step 1: Remove px button from header**

Find the Crop section header buttons (~lines 900–915):

```html
<div style="display: flex; gap: 4px; align-items: center">
  <button
    class="btn btn-ghost"
    id="crop-mode-btn"
    style="padding: 2px 8px; font-size: 10px; font-family: var(--mono)"
  >
    px
  </button>
  <button
    class="btn btn-ghost"
    id="crop-collapse-btn"
    style="padding: 2px 8px; font-size: 10px; font-family: var(--mono)"
  >
    ▾
  </button>
</div>
```

Replace it with just the collapse button:

```html
<button
  class="btn btn-ghost"
  id="crop-collapse-btn"
  style="padding: 2px 8px; font-size: 10px; font-family: var(--mono)"
>
  ▾
</button>
```

**Step 2: Add [px] [%] toggle strip at the top of crop body**

Find the opening of `#crop-body` and the input-grid that follows it (~line 917):

```html
<div id="crop-body">
  <div class="input-grid"></div>
</div>
```

Insert a toggle strip between them:

```html
<div id="crop-body">
  <div style="display: flex; gap: 4px; margin-bottom: 8px">
    <button
      class="btn btn-ghost"
      id="crop-mode-px"
      style="flex: 1; font-size: 11px; font-family: var(--mono)"
    >
      px
    </button>
    <button
      class="btn btn-ghost"
      id="crop-mode-pct"
      style="flex: 1; font-size: 11px; font-family: var(--mono)"
    >
      %
    </button>
  </div>
  <div class="input-grid"></div>
</div>
```

**Step 3: Update JS — new button IDs**

The existing JS listens on `#crop-mode-btn` and toggles `cropMode` between `'px'` and `'pct'`. Search for `crop-mode-btn` in the JS (~line 1620 area) and replace the single-button pattern with two-button logic.

Find:

```js
const cropModeBtn = document.getElementById('crop-mode-btn');
```

Replace with:

```js
const cropModePxBtn = document.getElementById('crop-mode-px');
const cropModePctBtn = document.getElementById('crop-mode-pct');
```

Find the existing click listener on `cropModeBtn` that toggles between px/pct. Replace it with two separate listeners:

```js
cropModePxBtn.addEventListener('click', () => {
  if (cropMode === 'px') return;
  cropMode = 'px';
  updateCropModeUI();
  updateInputs();
  saveSettings();
});

cropModePctBtn.addEventListener('click', () => {
  if (cropMode === 'pct') return;
  cropMode = 'pct';
  updateCropModeUI();
  updateInputs();
  saveSettings();
});
```

**Step 4: Update `updateCropModeUI` (or equivalent)**

The function that updates the button's label/active state needs to set active style on the correct button. Find the existing function that updates `cropModeBtn.textContent` and adapt it to add/remove an active class or `btn-active` style on the two new buttons instead.

```js
function updateCropModeUI() {
  cropModePxBtn.style.background = cropMode === 'px' ? 'var(--accent)' : '';
  cropModePxBtn.style.color = cropMode === 'px' ? 'var(--bg)' : '';
  cropModePctBtn.style.background = cropMode === 'pct' ? 'var(--accent)' : '';
  cropModePctBtn.style.color = cropMode === 'pct' ? 'var(--bg)' : '';
}
```

Call `updateCropModeUI()` wherever `cropMode` is set (including on settings load).

**Step 5: Verify**

Open in browser. The Crop header should show only "Crop" and the collapse arrow — no `px` button. The crop body should start with a `[px] [%]` toggle strip. Clicking px/% should highlight the active one and update the coordinate inputs accordingly. Reload should restore the last-used mode.

**Step 6: Commit**

```bash
git add index.html
git commit -m "refactor: move px/pct mode toggle into crop body"
```

---

### Task 3: Merge save/clear override into single button

**Files:**

- Modify: `index.html` (~lines 1071–1080 HTML, ~lines 2085–2088 and 2757–2772 JS)

Two mutually exclusive buttons are replaced by one button that changes label and behaviour based on state.

**Step 1: Replace the two HTML buttons with one**

Find (~lines 1071–1080):

```html
<button class="btn btn-ghost btn-full" id="save-override" style="margin-top: 4px">
  Save crop for this file
</button>
<button class="btn btn-ghost btn-full" id="clear-override" style="margin-top: 4px; display: none">
  Clear file override
</button>
```

Replace with:

```html
<button class="btn btn-ghost btn-full" id="override-btn" style="margin-top: 4px">
  Save Override
</button>
```

**Step 2: Update `updateOverrideButtons` JS function (~line 2085)**

Find:

```js
function updateOverrideButtons() {
  const hasOverride = !!files[activeIndex]?.crop;
  document.getElementById('clear-override').style.display = hasOverride ? 'block' : 'none';
}
```

Replace with:

```js
function updateOverrideButtons() {
  const hasOverride = !!files[activeIndex]?.crop;
  const btn = document.getElementById('override-btn');
  btn.textContent = hasOverride ? 'Clear Override' : 'Save Override';
  btn.style.color = hasOverride ? 'var(--accent)' : '';
}
```

**Step 3: Merge the two event listeners (~lines 2757–2772)**

Find and remove both separate listeners:

```js
document.getElementById('save-override').addEventListener('click', () => { ... });
document.getElementById('clear-override').addEventListener('click', () => { ... });
```

Replace with one combined listener:

```js
document.getElementById('override-btn').addEventListener('click', () => {
  if (!files[activeIndex]) return;
  const hasOverride = !!files[activeIndex].crop;
  if (hasOverride) {
    files[activeIndex].crop = null;
    activeCrop = batchCrop;
    updateInputs();
    updateCropBoxVisual();
  } else {
    if (!activeCrop.w || !activeCrop.h) return;
    files[activeIndex].crop = { ...activeCrop, rotation: cropRotation };
    activeCrop = files[activeIndex].crop;
  }
  updateOverrideButtons();
  renderFileList();
});
```

**Step 4: Verify**

Open in browser with an image loaded. The button should read "Save Override". Click it — it should save the per-file crop and change to "Clear Override" (with accent colour). Click again — it should clear the override and return to "Save Override". Switch files and back — state should reflect the correct label.

**Step 5: Commit**

```bash
git add index.html
git commit -m "refactor: merge save/clear override into single toggle button"
```

---

### Task 4: Pin download buttons to sidebar footer

**Files:**

- Modify: `index.html` (~lines 1458–1482 HTML to remove, ~line 1541 HTML to add footer, CSS to add `.sidebar-footer`)

Move the three download buttons, progress bar, and status text out of the collapsible Export section into a permanent footer at the bottom of the sidebar.

**Step 1: Remove download buttons and progress from export-body**

Find and delete these lines from inside `#export-body` (~lines 1458–1482):

```html
<button
  class="btn btn-ghost btn-full"
  id="download-active-btn"
  disabled
  style="margin-bottom: 8px"
  data-shortcut="Shift+Enter"
>
  ▼ Download Active File
</button>
<button class="btn btn-ghost btn-full" id="copy-clipboard-btn" disabled style="margin-bottom: 8px">
  ⧉ Copy to Clipboard
</button>
<button class="btn btn-primary btn-full" id="export-btn" disabled data-shortcut="Enter">
  ▼ Download All Cropped
</button>
<div class="progress-bar" id="progress-bar" style="display: none">
  <div class="progress-fill" id="progress-fill"></div>
</div>
<div class="status-text" id="status-text"></div>
```

**Step 2: Add sidebar footer after file-list**

Find the closing of the sidebar (~line 1541):

```html
    </div>  <!-- file-list -->
  </div>    <!-- sidebar -->
```

Insert the footer between them:

```html
    </div>  <!-- file-list -->
    <div class="sidebar-footer">
      <button
        class="btn btn-ghost btn-full"
        id="download-active-btn"
        disabled
        data-shortcut="Shift+Enter"
      >
        ▼ Download Active File
      </button>
      <button
        class="btn btn-ghost btn-full"
        id="copy-clipboard-btn"
        disabled
      >
        ⧉ Copy to Clipboard
      </button>
      <button class="btn btn-primary btn-full" id="export-btn" disabled data-shortcut="Enter">
        ▼ Download All Cropped
      </button>
      <div class="progress-bar" id="progress-bar" style="display: none">
        <div class="progress-fill" id="progress-fill"></div>
      </div>
      <div class="status-text" id="status-text"></div>
    </div>
  </div>    <!-- sidebar -->
```

**Step 3: Add `.sidebar-footer` CSS**

Find the `.sidebar` CSS block (~line 429) and add after it:

```css
.sidebar-footer {
  flex-shrink: 0;
  padding: 12px 16px;
  border-top: 1px solid var(--border);
  background: var(--surface);
  display: flex;
  flex-direction: column;
  gap: 6px;
}
```

The sidebar already has `display: flex; flex-direction: column` and `.file-list` already has `flex: 1; overflow-y: auto`, so no further CSS changes are needed.

**Step 4: Verify**

Open in browser. The three download buttons, progress bar, and status text should appear pinned at the bottom of the sidebar at all times — even when Export is collapsed. Collapsing the Export section should hide only the settings (Format, Quality, Filename, Watermark, Resize). Export and copy functionality should work as before.

**Step 5: Commit**

```bash
git add index.html
git commit -m "refactor: pin download buttons to sidebar footer"
```

---

### Task 5: Watermark and Resize as visual sub-sections

**Files:**

- Modify: `index.html` (~lines 1261 and ~line 1390, CSS for `.sub-section-header`)

Add a visual divider and sub-heading row to both the Watermark and Resize blocks inside the Export section body, making the hierarchy explicit.

**Step 1: Add `.sub-section-header` CSS**

Add this CSS class near the `.sidebar-section h2` block:

```css
.sub-section-header {
  display: flex;
  align-items: center;
  justify-content: space-between;
  padding-top: 10px;
  margin-top: 10px;
  border-top: 1px solid var(--border);
  font-family: var(--mono);
  font-size: 10px;
  font-weight: 600;
  letter-spacing: 0.12em;
  text-transform: uppercase;
  color: var(--text-dim);
  margin-bottom: 6px;
}
```

**Step 2: Replace Watermark `input-group` wrapper with sub-section**

Find the Watermark block (~line 1261):

```html
<div class="input-group" style="margin-top: 8px">
  <label style="display: flex; align-items: center; justify-content: space-between">
    Watermark
    <input type="checkbox" id="watermark-enabled-chk" />
  </label>
  <div id="watermark-inputs" style="display: none; margin-top: 6px">...</div>
</div>
```

Replace with:

```html
<div>
  <div class="sub-section-header">
    Watermark
    <input type="checkbox" id="watermark-enabled-chk" />
  </div>
  <div id="watermark-inputs" style="display: none">...</div>
</div>
```

**Step 3: Replace Resize Output `input-group` wrapper with sub-section**

Find the Resize block (~line 1390):

```html
<div class="input-group" style="margin-top: 8px">
  <label style="display: flex; align-items: center; justify-content: space-between">
    Resize Output
    <input type="checkbox" id="resize-enabled-chk" />
  </label>
  <div id="resize-inputs" style="display: none; margin-top: 6px">...</div>
</div>
```

Replace with:

```html
<div>
  <div class="sub-section-header">
    Resize Output
    <input type="checkbox" id="resize-enabled-chk" />
  </div>
  <div id="resize-inputs" style="display: none">...</div>
</div>
```

**Step 4: Verify**

Open in browser. In the Export section, the Watermark and Resize Output rows should have a visible top divider and use the same uppercase monospace label style as section headers. Checking either checkbox should expand its sub-form as before.

**Step 5: Commit**

```bash
git add index.html
git commit -m "refactor: watermark and resize as visual sub-sections in export"
```

---

### Final: Push

```bash
git push
```
