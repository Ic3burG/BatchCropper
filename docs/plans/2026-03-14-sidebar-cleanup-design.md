# Sidebar Cleanup Design

**Date:** 2026-03-14
**Status:** Approved

## Problem

The sidebar has grown organically and is starting to feel cluttered. Five specific issues:

1. Grid overlay toggles (⊞ ⅓ and ⊞ φ) live at the bottom of the Crop section but are view controls, not crop parameters.
2. Download buttons are buried at the bottom of a long, collapsible Export section — users must scroll past settings to reach the primary action.
3. "Save crop for this file" and "Clear file override" are two mutually-exclusive buttons, only one ever visible at a time.
4. The `px` mode toggle sits in the Crop section header where it looks like a collapse control.
5. The Watermark sub-form is deeply nested inside Export with no visual separation from the core settings above it.

## Changes

### 1. Grid toggles → canvas toolbar

Remove the `⊞ ⅓` (rule-of-thirds) and `⊞ φ` (golden ratio) buttons from the Crop section body. Add them to the canvas toolbar between the Ruler and Compare buttons. They are view toggles, not crop parameters, and belong alongside the other view controls. Their `data-shortcut` attributes (`T` and `L`) carry over, so tooltip behaviour is unchanged.

### 2. Pinned download footer

Add a `.sidebar-footer` element below the file list. Move the three download buttons (Download Active File, Copy to Clipboard, Download All Cropped), the progress bar, and the status text into it. The footer is always visible regardless of section collapse state or scroll position. The Export section becomes settings-only.

The sidebar requires `display: flex; flex-direction: column` with the file list set to `flex: 1; min-height: 0; overflow-y: auto` so the footer pins to the bottom.

### 3. Single override toggle button

Replace the two buttons (`#save-override`, `#clear-override`) with a single `#override-btn`. JavaScript sets the label to "Save Override" when no per-file override exists, and "Clear Override" (with a distinct active style) when one does. The `display: none` toggling pattern is removed.

### 4. px/% toggle moves into crop body

Remove the `px` button from the Crop section header. The header becomes "Crop" + collapse arrow only, symmetric with Presets and Export. Add a compact `[px] [%]` two-button toggle strip as the first element inside the crop body, above the X/Y/W/H input grid.

### 5. Watermark and Resize as visual sub-sections

Give both Watermark and Resize a `border-top` divider and a sub-header row (label left, checkbox right) styled to match the existing section header pattern. This makes the hierarchy explicit: Export → core settings (Flip, Format, Quality, Filename) → Watermark sub-section → Resize sub-section. No new UI patterns introduced.

## Structure After Changes

**Canvas toolbar:** `[Zoom] [Ruler] [⊞ ⅓] [⊞ φ] [Compare] [coords]`

**Crop section:**

```
[Crop]                              [▾]
[px] [%]
X ___ Y ___
W ___ H ___
Ratio [select] [⇅] [custom inputs]
Rotation [___°] [slider] [⟳]
[Reset Selection]
[Save Override | Clear Override]
[◉ Detect Faces]   (conditionally visible)
```

**Export section:**

```
[Export]                            [▾]
[⇄ Flip H] [⇅ Flip V]
Format [select]
Quality [slider]   (conditionally visible)
Filename [___]

── Watermark ──────────── [checkbox]
  text / logo / corner / opacity / fontsize
  (conditionally visible)

── Resize Output ──────── [checkbox]
  W / H / mode
  (conditionally visible)
```

**Sidebar footer (always visible):**

```
[▼ Download Active File]
[⧉ Copy to Clipboard]
[▼ Download All Cropped]   (primary)
[progress bar]
[status text]
```
