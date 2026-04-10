# ONE Design System — Reference Guide

## Overview

This CSS file captures the shared visual language across OTN's Skuid custom components (Patient Messaging, Calendar Preference Selector, and future builds). Drop `one-design-system.css` into any project and wrap your root element with `class="one-ds"` to inherit the system.

## For AI Assistants (Claude, etc.)

When building a new ONE component mockup or React artifact, import or inline the design system CSS and follow these conventions:

1. **Wrap in `.one-ds`** — this sets base font, color, and box-sizing.
2. **Use CSS variables** (`var(--one-blue-600)`, `var(--one-radius-lg)`, etc.) instead of hardcoded hex/px values.
3. **Follow the component class patterns** documented in the CSS file's Section 3 (`one-btn`, `one-card`, `one-dialog`, etc.).
4. **Prefix any component-specific classes** with a short namespace (e.g., `pm-` for Patient Messaging, `cps-` for Calendar Pref Selector) to avoid collisions.

---

## Token Quick Reference

### Colors

| Token                  | Hex       | Usage                           |
| ---------------------- | --------- | ------------------------------- |
| `--one-brand-blue`     | `#3b9be5` | Patient-facing panel headers    |
| `--one-blue-600`       | `#2563eb` | Primary actions, links, focus   |
| `--one-blue-700`       | `#1d4ed8` | Gradient endpoint, hover        |
| `--one-green-700`      | `#059669` | Success, active language toggle |
| `--one-amber-600`      | `#d97706` | Warnings, segments count        |
| `--one-red-600`        | `#dc2626` | Destructive actions, errors     |
| `--one-purple-600`     | `#7c3aed` | Admin / shared visibility       |
| `--one-gray-900`       | `#111827` | Primary text                    |
| `--one-gray-500`       | `#6b7280` | Secondary / muted text          |
| `--one-gray-400`       | `#9ca3af` | Placeholder text, icons         |

### Radius Scale

| Token              | Value  | Usage                          |
| ------------------ | ------ | ------------------------------ |
| `--one-radius-sm`  | `4px`  | Tags, small badges             |
| `--one-radius-md`  | `6px`  | Compact buttons, chips         |
| `--one-radius-lg`  | `8px`  | Standard buttons, inputs       |
| `--one-radius-xl`  | `10px` | Cards, alert banners           |
| `--one-radius-2xl` | `12px` | Containers, panel wrappers     |
| `--one-radius-3xl` | `14px` | Dialogs                        |
| `--one-radius-pill`| `20px` | Pills, toggle groups           |

### Shadow Scale

| Token                  | Usage                              |
| ---------------------- | ---------------------------------- |
| `--one-shadow-xs`      | Subtle card resting state          |
| `--one-shadow-sm`      | Active pills, tab highlights       |
| `--one-shadow-md`      | Container wrapper                  |
| `--one-shadow-lg`      | Dropdown menus                     |
| `--one-shadow-xl`      | Dialogs                            |
| `--one-shadow-primary` | Primary button glow                |

---

## Two Header Flavors

ONE uses two header backgrounds depending on context:

- **`.one-header-brand`** — Flat `#3b9be5` sky blue. Used on patient-facing or clinical panels (Patient Messaging, etc.).
- **`.one-header-primary`** — Blue gradient (`#2563eb → #1d4ed8`). Used on admin/settings panels (Calendar Pref Selector, etc.).

Both use white text and translucent white icon buttons (`.one-btn-header-icon`).

---

## Discipline Color Map

Clinical disciplines each have a consistent color identity used for badges, filter chips, card accents, and group headers:

| Discipline | Background | Border    | Text      | Badge     |
| ---------- | ---------- | --------- | --------- | --------- |
| ABA        | `#fef3c7`  | `#fbbf24` | `#92400e` | `#f59e0b` |
| SLP        | `#dbeafe`  | `#60a5fa` | `#1e40af` | `#3b82f6` |
| OT         | `#dcfce7`  | `#4ade80` | `#166534` | `#22c55e` |
| PT         | `#fce7f3`  | `#f472b6` | `#9d174d` | `#ec4899` |
| MH         | `#ede9fe`  | `#a78bfa` | `#5b21b6` | `#8b5cf6` |

---

## Component Patterns at a Glance

### Buttons

```html
<button class="one-btn one-btn-primary">Save</button>
<button class="one-btn one-btn-secondary">Cancel</button>
<button class="one-btn one-btn-danger">Delete</button>
<button class="one-btn one-btn-primary one-btn--sm">Compact</button>
<button class="one-btn one-btn-primary one-btn--disabled" disabled>Disabled</button>
```

### Cards

```html
<div class="one-card">
  <div class="one-icon-box one-icon-box--md one-icon-box--blue">📋</div>
  <div class="one-card-info">
    <div class="one-card-title">Template Name</div>
    <div class="one-card-meta">2 segments · EN/ES</div>
  </div>
  <div class="one-card-actions">
    <button class="one-btn one-btn-icon">✏️</button>
  </div>
</div>
```

### Modal Dialog

```html
<div class="one-overlay">
  <div class="one-dialog">
    <div class="one-dialog-header one-dialog-header--amber">
      <div class="one-icon-box one-icon-box--md one-icon-box--amber">⚠️</div>
      <div>
        <div class="one-dialog-title">Unsaved Changes</div>
        <div class="one-dialog-subtitle">You have edits that haven't been saved.</div>
      </div>
    </div>
    <div class="one-dialog-body">
      <p>Would you like to save before leaving?</p>
    </div>
    <div class="one-dialog-footer">
      <button class="one-btn one-btn-secondary">Discard</button>
      <button class="one-btn one-btn-primary">Save</button>
    </div>
  </div>
</div>
```

### Alert Banner

```html
<div class="one-alert one-alert--red">
  <div class="one-icon-box one-icon-box--sm one-icon-box--red">🚫</div>
  <div>
    <div class="one-alert-title">SMS Opt-In Required</div>
    <div class="one-alert-detail">This patient has not opted in to receive text messages.</div>
  </div>
</div>
```

---

## Theme Variants

The design system supports two visual themes, activated by adding a class alongside `.one-ds`:

| Theme | Class | Primary | Font | Use For |
|-------|-------|---------|------|---------|
| **Base** | `.one-ds` | `#2563eb` | System stack | Default (Patient Messaging, Calendar) |
| **Theranet** | `.one-ds.one-theranet` | `#61a5d2` | System stack | Clinical/operational tools (ELM, future Theranet pages) |
| **Financial** | `.one-ds.one-financial` | `#2563EB` | Plus Jakarta Sans | Modeler, Pay Calculator, financial reporting |

**Theranet** aligns with the MaterialTheranet Skuid theme so custom components feel native alongside standard Skuid pages.

**Financial** uses a warm sand palette (`#F7F6F3` backgrounds, `#E8E5DE` borders) with Plus Jakarta Sans for brand identity.

### Brand Identity Tokens

| Token | Hex | Usage |
|-------|-----|-------|
| `--one-navy` | `#1C1A55` | ONE corporate navy |
| `--one-coral` | `#C95244` | ONE corporate coral accent |
| `--one-brand-blue` | `#3b9be5` | Patient-facing panel headers |

---

## Naming Convention

- **Design system classes**: `one-` prefix (shared across all components)
- **Component-specific classes**: Short namespace prefix unique to each component
  - `pm-` → Patient Messaging
  - `cps-` → Calendar Preference Selector
  - `ed-` → Employee Dashboard
  - `er-` → Employee Request
  - Future components pick their own 2-4 letter prefix

This avoids collision when multiple components coexist on the same Skuid page.

---

## JS-Driven State Classes

These classes are toggled at runtime by component JavaScript. They're the visual expression of behavioral state documented in `ONE-JS-PATTERNS.md`. The CSS lives in Section 8 of `one-design-system.css`.

### How to Read This Section

Each entry follows the pattern: **Class** → **Trigger** → **Visual Effect**. When building new components, follow the same pattern: define a state class in CSS, toggle it from JS, document it here.

### CPS — Drag & Drop

| Class | JS Trigger | Visual Effect |
|---|---|---|
| `.cps-card-dragging` | `dragstart` event → sets `draggedIndex` | 50% opacity, blue border + light blue fill |
| `.cps-card-drop-target` | `dragover` on a sibling card | Green border, green tint, slight scale-up |
| `.cps-scroll-zone-top` | Mouse within 60px of container top during drag | 3px blue top border on scroll container |
| `.cps-scroll-zone-bottom` | Mouse within 60px of container bottom during drag | 3px blue bottom border on scroll container |

### CPS — Selection & Filters

| Class | JS Trigger | Visual Effect |
|---|---|---|
| `.cps-card-selected` | Click on clinician card | Blue border + `#eff6ff` background |
| `.cps-clinic-selected` | Option matches `currentClinicId` | Blue tint on dropdown option |
| `.cps-filter-active` | Click on discipline filter chip | Bold text (colors applied inline by JS) |
| `.cps-sort-selected` | Sort option matches `sortMode` | Blue tint on sort dropdown option |

### CPS — Button & Indicator States

| Class | JS Trigger | Visual Effect |
|---|---|---|
| `.cps-btn-disabled` | `!hasUnsavedChanges` or `isSaving` | Translucent white, disabled cursor |
| `.cps-cancel-active` | `hasUnsavedChanges === true` | Amber background with pulsing glow animation |
| `.cps-change-dot` | Unsaved changes on displayed clinic | 8px yellow circle next to clinic name |

### PM — Send Flow

| Class | JS Trigger | Visual Effect |
|---|---|---|
| `.pm-send-ready` | All send conditions met | Blue gradient, white text, shadow lift on hover |
| `.pm-send-disabled` | Missing phone/message/opt-in | Gray `#e5e7eb`, muted text, disabled cursor |
| `.pm-send-success` | `showSuccess = true` (1800ms) | Green `#16a34a`, white text |
| `.pm-confirm-send-blocked` | Unresolved tokens in confirm dialog | Gray, disabled — blocks send |
| `.pm-token-unresolved` | `highlightUnresolvedTokens()` wraps `{{...}}` | Red background, red border, bold mono text |

### PM — Template Browser

| Class | JS Trigger | Visual Effect |
|---|---|---|
| `.pm-tpl-selected` | `tpl.id === selectedTemplateId` | Blue border + blue tint; icon inverts to white-on-blue |
| `.pm-tpl-unavailable` | `tpl.missingContext.length > 0` | 55% opacity, grayscale icon; hover restores to 75% |
| `.pm-nav-active` | Active template browser tab | White card with shadow on active tab |
| `.pm-lang-active` | `this.language` matches button | Green pill (`#059669`) |
| `.pm-lang-disabled` | No template content for language | Faded to 50% opacity, disabled cursor |
| `.pm-phone-selected` | `phone.key === selectedPhone` | Blue tint on phone dropdown option |

### PM — Manage View

| Class | JS Trigger | Visual Effect |
|---|---|---|
| `.pm-sidebar-active` | `this.manageFilter === item.key` | White card, bold text, subtle shadow |
| `.pm-manage-card-muted` | Template lacks content in `manageLang` | 55% opacity, dashed border, gray icon |
| `.pm-manage-tr-open` | Translated section expanded | Chevron points down (default rotation) |
| `.pm-manage-nt-open` | Needs-translation section expanded | Chevron rotates 180° |
| `.pm-action-pin-active` | `tpl.isPinned === true` | Amber icon + cream background |

### PM — Editor

| Class | JS Trigger | Visual Effect |
|---|---|---|
| `.pm-editor-save-active` | `isEditorDirty() && !isSavingTemplate` | Blue gradient, lift on hover |
| `.pm-editor-save-muted` | `!isEditorDirty()` | Translucent white on blue header |
| `.pm-editor-save-saving` | `isSavingTemplate === true` | Translucent, disabled cursor |
| `.pm-preview-active` | `editorPreview === true` | Blue tint + blue border on preview toggle |
| `.pm-toggle-shared` | `editorForm.isGlobal === true` | Purple pill with purple text |
| `.pm-toggle-system` | `editorForm.isSystem === true` | Amber pill with amber text |
| `.pm-icon-selected` | `iconKey === editorForm.icon` | Blue tint in icon picker grid |
| `.pm-pill-active` | Role/function included in form | Bold + shadow (colors set inline) |

### PM — Patient Info & Opt-In

| Class | JS Trigger | Visual Effect |
|---|---|---|
| `.pm-pi-optin-yes` | `textOptIn === 'Yes'` | Green text |
| `.pm-pi-optin-no` | `textOptIn === 'No'` | Red text |
| `.pm-pi-optin-unknown` | `textOptIn` is blank | Amber text |
| `.pm-pi-optin-saving` | `isSavingOptIn === true` | Blue text + spinning icon |
| `.pm-pi-optin-btn-active` | Button matches current value | Green fill + border |
| `.pm-pi-optin-btn-disabled` | `isSavingOptIn === true` | 50% opacity, no pointer events |
| `.pm-pi-phone-active` | `phone.key === selectedPhone` | Blue border + blue tint |
| `.pm-pi-phone-empty` | Phone field is blank | Dashed border, muted background |

### Pattern for New Components

When adding state classes to a new ONE component:

1. **Name**: `{prefix}-{element}-{state}` (e.g., `xyz-card-selected`, `xyz-btn-disabled`)
2. **Define in CSS**: Full rule in Section 8 of `one-design-system.css` with a comment noting the JS trigger
3. **Toggle in JS**: Add/remove class in the render function or event handler — never style inline for states
4. **Document**: Add a row to the appropriate table in this section
