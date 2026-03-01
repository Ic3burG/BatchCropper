# Design: Keyboard Shortcuts, Rulers, and Guides

## Overview

This enhancement adds professional-grade alignment tools (Rulers and Guides) and a robust keyboard shortcut system to BatchCropper. It focuses on improving precision for power users while maintaining the "client-side, no-build" philosophy of the project.

## 1. Rulers & Guides

### Visuals

- **Rulers**: 20px wide/high bars at the top and left of the image canvas.
- **Units**: Pixel-based, relative to the image's native resolution.
- **Tick Marks**: Small notches every 10px, larger notches with labels every 100px.
- **Guides**: Cyan (#00ffff) thin lines that span the workspace.
- **Toggle**: A new toolbar button to show/hide the ruler system.

### Behavior

- **Pull-down**: Dragging from a ruler creates a new guide (horizontal or vertical).
- **Global**: Guides are global across the entire batch session.
- **Deletion**: Dragging a guide back into its ruler area removes it.
- **Zoom-aware**: Rulers and guides scale and move with the image as the user zooms and pans.

## 2. Snapping System

### Precision Alignment

- **Snapping Threshold**: 10 viewport pixels.
- **Scope**: The crop box and its individual handles will snap to guides and image edges.
- **Toggle**: `S` key or a checkbox to enable/disable snapping globally.
- **Feedback**: Guides will change color slightly (to `--accent`) when a snap occurs to provide visual confirmation.

## 3. Keyboard Shortcut System

### Core Shortcuts

- `G`: Toggle Rulers/Guides visibility
- `S`: Toggle Snapping (On/Off)
- `[` / `]`: Navigate to Previous / Next image in batch
- `Enter`: Trigger "Download All" (Batch Export)
- `Shift + Enter`: Download Active File
- `T`: Toggle Rule of Thirds Grid
- `L`: Toggle Golden Ratio Grid
- `C`: Toggle Comparison View
- `+` / `-`: Zoom In / Out
- `0`: Fit to Screen
- `R`: Reset Crop (Existing)
- `Arrow Keys`: Nudge crop region (Existing)
- `Ctrl/Cmd + Z`: Undo (Existing)
- `Ctrl/Cmd + Y / Shift+Z`: Redo (Existing)

### Discoverability

- **Shortcut Tooltips**: Custom immediate tooltips that appear on hover for any UI element with an associated shortcut.
- **Styling**: Minimalist mono-font bubbles, e.g., "Export All `[Enter]`".

## 4. Technical Architecture

- **State Management**:
  - `globalGuides = { horizontal: [], vertical: [] }`
  - `isSnappingEnabled = true`
  - `showRulers = true`
- **Persistence**: Save these settings to `localStorage` under the existing `batchcrop_settings` key.
- **Event Handling**: Consolidate `keydown` listeners into a single clean handler. Use a single `mouseover` listener on the document body to manage tooltip lifecycle.
- **Canvas Rendering**: Rulers will be drawn using SVG or standard DOM elements (divs) to avoid complex canvas redraws for the UI layer. Guides will be DOM elements overlaying the canvas wrap.

## 5. Success Criteria

- Users can pull and place guides precisely.
- Crop box snapping feels "magnetic" and predictable.
- Navigation via `[` and `]` is fast and responsive.
- Shortcuts are easily discovered without reading a manual.
