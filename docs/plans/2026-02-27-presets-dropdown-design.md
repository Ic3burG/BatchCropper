# Presets Section Redesign — Two Dropdown Design

**Date:** 2026-02-27
**Status:** Approved

## Problem

The current Presets section feels cluttered. It simultaneously shows:

- A wrapping row of built-in chip buttons
- A save-named-preset input row
- A scrollable list of user presets (each with apply + delete buttons)

All three layers compete visually and take up considerable vertical space.

## Design

Replace the chip row and user-preset list with two side-by-side `<select>` dropdowns.

### Layout

```
┌─────────────────────────────────────────┐
│ Presets                              ▾  │
├─────────────────────────────────────────┤
│ [— ratio —        ▾] [— saved —     ▾] │
│                                     [×] │  ← appears only when a saved preset is selected
│ [Name current crop…      ] [Save]       │
└─────────────────────────────────────────┘
```

### Built-in Dropdown

- Lists the 6 ratio presets: Square 1:1, Portrait 4:5, Story 9:16, Landscape 16:9, Twitter Header, Classic 4:3
- Selecting applies immediately via `change` event
- Resets to placeholder `— ratio —` after applying (stateless)

### My Presets Dropdown

- Lists user-saved presets by name
- Selecting applies immediately via `change` event
- After selecting, a `✕` button appears inline to the right
- Clicking `✕` deletes the currently-selected preset and resets the dropdown
- When no user presets exist: disabled, reads `— none saved —`

### Save Row

- Unchanged functionality: text input + Save button
- Minor style tightening to match the new aesthetic

### Styling

- Native `<select>` elements, custom-styled to match dark theme
- Uses existing CSS variables: `--surface2`, `--border`, `--accent`, `--mono`
- Custom CSS chevron replaces browser default arrow
- Both dropdowns equal width, small gap between them

## Files Changed

- `index.html` — HTML structure, CSS styles, JS logic (all in one file)
  - Remove: `.preset-chip`, `.user-preset-row` CSS classes and their HTML
  - Add: `.preset-select` CSS class, two `<select>` elements, delete `✕` button
  - Update: `renderUserPresets()` to populate `<select>` options instead of DOM rows
  - Update: built-in preset rendering to populate `<select>` options instead of chips
  - Add: `change` event handlers on both selects; delete handler on `✕` button
