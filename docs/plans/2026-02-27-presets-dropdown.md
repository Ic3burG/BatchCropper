# Presets Dropdown Redesign Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Replace the cluttered built-in chip buttons and user-preset list with two side-by-side styled `<select>` dropdowns that apply immediately on selection.

**Architecture:** All changes are in `index.html` (single-file app). Three areas: CSS (remove old classes, add new ones), HTML (replace the preset DOM structure inside `#presets-body`), JS (replace chip/row rendering with `<select>` option rendering and `change` event handlers).

**Tech Stack:** Vanilla JS, HTML, CSS — no build step. Open `index.html` in browser to verify visually.

---

### Task 1: Remove old CSS, add new `.preset-select` styles

**Files:**

- Modify: `index.html:658–709` (the `.preset-chip` and `.user-preset-row` CSS blocks)

**Step 1: Remove these CSS blocks entirely**

Find and delete lines 658–709 — these seven rule blocks:

```css
.preset-chip { … }
.preset-chip:hover { … }
.user-preset-row { … }
.user-preset-row button.apply { … }
.user-preset-row button.apply:hover { … }
.user-preset-row button.del { … }
.user-preset-row button.del:hover { … }
```

**Step 2: In their place, insert the new CSS**

```css
.preset-select-row {
  display: flex;
  gap: 6px;
  margin-bottom: 8px;
  align-items: center;
}
.preset-select {
  flex: 1;
  font-family: var(--mono);
  font-size: 11px;
  padding: 4px 24px 4px 8px;
  background: var(--surface2);
  border: 1px solid var(--border);
  color: var(--text);
  border-radius: 2px;
  cursor: pointer;
  appearance: none;
  -webkit-appearance: none;
  background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' width='10' height='6'%3E%3Cpath d='M0 0l5 6 5-6z' fill='%23888'/%3E%3C/svg%3E");
  background-repeat: no-repeat;
  background-position: right 8px center;
  min-width: 0;
}
.preset-select:hover {
  border-color: var(--accent);
}
.preset-select:focus {
  outline: none;
  border-color: var(--accent);
}
.preset-select:disabled {
  opacity: 0.45;
  cursor: default;
}
.preset-delete-btn {
  flex-shrink: 0;
  padding: 4px 7px;
  font-size: 12px;
  background: none;
  border: 1px solid var(--border);
  color: var(--text-dim);
  border-radius: 2px;
  cursor: pointer;
  display: none;
}
.preset-delete-btn:hover {
  border-color: var(--accent2);
  color: var(--accent2);
}
```

**Step 3: Open `index.html` in browser, navigate to Presets section**

Verify the section still opens/closes with the toggle. No visual change yet — the new CSS classes aren't used yet.

**Step 4: Commit**

```bash
git add index.html
git commit -m "style: replace preset chip/row CSS with select + delete button styles"
```

---

### Task 2: Replace HTML structure inside `#presets-body`

**Files:**

- Modify: `index.html:1021–1050` (the `#presets-body` div contents)

**Step 1: Replace the contents of `#presets-body`**

Current contents (lines 1022–1050):

```html
<div
  id="builtin-presets"
  style="display: flex; flex-wrap: wrap; gap: 4px; margin-bottom: 8px"
></div>
<div style="display: flex; gap: 6px; margin-bottom: 6px">
  <input
    type="text"
    id="preset-name-input"
    placeholder="Name current crop…"
    style="flex: 1; font-size: 11px"
  />
  <button class="btn btn-ghost" id="save-preset-btn" style="font-size: 11px; white-space: nowrap">
    Save
  </button>
</div>
<div
  id="user-presets"
  style="display: flex; flex-direction: column; gap: 4px; max-height: 120px; overflow-y: auto"
></div>
```

Replace with:

```html
<div class="preset-select-row">
  <select id="builtin-preset-select" class="preset-select">
    <option value="" disabled selected>— ratio —</option>
  </select>
  <select id="user-preset-select" class="preset-select">
    <option value="" disabled selected>— saved —</option>
  </select>
  <button id="delete-preset-btn" class="preset-delete-btn" title="Delete preset">×</button>
</div>
<div style="display: flex; gap: 6px; margin-bottom: 4px">
  <input
    type="text"
    id="preset-name-input"
    placeholder="Name current crop…"
    style="flex: 1; font-size: 11px"
  />
  <button class="btn btn-ghost" id="save-preset-btn" style="font-size: 11px; white-space: nowrap">
    Save
  </button>
</div>
```

**Step 2: Open browser, verify**

Presets section should now show two small dropdowns side by side (both showing placeholder text) and the save row below. No JS wired yet — selecting options does nothing.

**Step 3: Commit**

```bash
git add index.html
git commit -m "refactor: replace preset chips and list HTML with two select dropdowns"
```

---

### Task 3: Update JS — built-in select rendering and change handler

**Files:**

- Modify: `index.html:3113–3121` (the built-in chip rendering block)

**Step 1: Replace the built-in chip rendering block**

Current code (lines 3113–3121):

```js
// Render built-in preset chips
const builtinContainer = document.getElementById('builtin-presets');
BUILTIN_PRESETS.forEach((preset) => {
  const chip = document.createElement('button');
  chip.className = 'preset-chip';
  chip.textContent = preset.name;
  chip.addEventListener('click', () => applyBuiltInPreset(preset.ratio));
  builtinContainer.appendChild(chip);
});
```

Replace with:

```js
// Populate built-in preset select
const builtinSelect = document.getElementById('builtin-preset-select');
BUILTIN_PRESETS.forEach((preset) => {
  const opt = document.createElement('option');
  opt.value = preset.ratio;
  opt.textContent = preset.name;
  builtinSelect.appendChild(opt);
});
builtinSelect.addEventListener('change', () => {
  applyBuiltInPreset(Number(builtinSelect.value));
  builtinSelect.value = '';
});
```

**Step 2: Open browser, verify built-in dropdown works**

- Click "Built-in" dropdown — should list 6 ratio options
- Select one — should apply the ratio crop and reset dropdown to `— ratio —`

**Step 3: Commit**

```bash
git add index.html
git commit -m "feat: wire built-in preset select with immediate apply + reset"
```

---

### Task 4: Update JS — user preset select rendering and handlers

**Files:**

- Modify: `index.html:3076–3111` (the `renderUserPresets` function)

**Step 1: Replace `renderUserPresets()` entirely**

Current function (lines 3076–3111) builds `div.user-preset-row` DOM rows.

Replace with:

```js
function renderUserPresets() {
  const sel = document.getElementById('user-preset-select');
  const delBtn = document.getElementById('delete-preset-btn');
  // Rebuild options, preserving placeholder
  while (sel.options.length > 1) sel.remove(1);
  if (userPresets.length === 0) {
    sel.options[0].textContent = '— none saved —';
    sel.disabled = true;
  } else {
    sel.options[0].textContent = '— saved —';
    sel.disabled = false;
    userPresets.forEach((preset, i) => {
      const opt = document.createElement('option');
      opt.value = i;
      opt.textContent = preset.name;
      opt.title =
        Math.round(preset.w) +
        ' × ' +
        Math.round(preset.h) +
        ' at (' +
        Math.round(preset.x) +
        ', ' +
        Math.round(preset.y) +
        ')';
      sel.appendChild(opt);
    });
  }
  // Hide delete button whenever list is re-rendered
  delBtn.style.display = 'none';
  sel.value = '';
}
```

**Step 2: Wire `change` and delete handlers after `renderUserPresets` definition**

Find the block starting at line ~3123 (`// Save current crop as named preset`). Just before that block, insert:

```js
// User preset select — apply on change, show delete button
const userPresetSel = document.getElementById('user-preset-select');
const deletePresetBtn = document.getElementById('delete-preset-btn');

userPresetSel.addEventListener('change', () => {
  const idx = Number(userPresetSel.value);
  if (isNaN(idx) || !userPresets[idx]) return;
  applyUserPreset(userPresets[idx]);
  deletePresetBtn.style.display = '';
});

deletePresetBtn.addEventListener('click', () => {
  const idx = Number(userPresetSel.value);
  if (isNaN(idx) || !userPresets[idx]) return;
  userPresets.splice(idx, 1);
  saveUserPresetsToStorage();
  renderUserPresets();
});
```

**Step 3: Open browser, verify user preset flow**

- Save a couple of presets using the input + Save button
- My Presets dropdown should update with those names
- Selecting one applies it immediately; a `×` button appears
- Clicking `×` deletes the preset; dropdown resets; `×` disappears
- When no presets exist, dropdown reads `— none saved —` and is disabled

**Step 4: Commit**

```bash
git add index.html
git commit -m "feat: wire user preset select with apply, delete button, and disabled state"
```

---

### Task 5: Final visual polish pass

**Files:**

- Modify: `index.html` (CSS tweaks only if needed)

**Step 1: Open browser, do a full visual check**

Walk through each scenario:

- [ ] No presets saved: My Presets shows `— none saved —`, disabled, no `×`
- [ ] One preset saved: My Presets enabled, select applies immediately, `×` appears, `×` deletes it
- [ ] Multiple presets: all listed, each works correctly
- [ ] Built-in dropdown: all 6 ratios apply and reset
- [ ] Save row: input + Save still works, trims whitespace, clears after save
- [ ] Collapse toggle: section collapses/expands correctly
- [ ] No horizontal overflow or wrapping issues in narrow sidebar

**Step 2: Fix any visual issues found**

Common things to check:

- Select arrow not showing (background-image SVG encoding issue) — re-encode if needed
- Dropdowns too narrow to read preset names — add `min-width` or adjust `flex`
- Color contrast on `option` elements (browser-native, limited control — acceptable)

**Step 3: Commit and push**

```bash
git add index.html
git commit -m "style: polish preset dropdown layout and visual edge cases"
git push
```
