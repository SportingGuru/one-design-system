# ONE Branding — Claude Code Instructions

---

## Pre-step — Upload logo as Salesforce Static Resource

1. In Salesforce Setup, go to **Static Resources → New**
2. Name it exactly: `OTN_Logo`
3. Upload the `one-logo.png` file
4. Cache Control: **Public**
5. Save

The logo URL in Skuid JS will then be: `/resource/OTN_Logo`

---

## Change 1 — `Css_for_modeler`: Add ONE brand variables

### 1a. Replace the accent color variables

Find this exact block (around line 20–21):
```css
    --acc: #2563EB;
    --acc-bg: #EFF6FF;
```

Replace with:
```css
    --acc: #1C1A55;
    --acc-bg: #EEEDF8;
    --one-navy: #1C1A55;
    --one-coral: #C95244;
    --one-blue: #1C1A55;
```

---

### 1b. Add brand bar CSS at the bottom of the file

Append this block at the very end of `Css_for_modeler`:

```css
/* ── ONE Brand Bar ─────────────────────────────────────────── */
.mod-brand-bar {
    display: flex;
    align-items: center;
    margin-bottom: 16px;
    padding-bottom: 12px;
    border-bottom: 1px solid var(--brd);
}

.mod-brand-logo {
    height: 24px;
    width: auto;
    display: block;
    opacity: 0.75;
}
```

---

## Change 2 — `javascript_for_modeler`: Insert brand bar into header HTML

There are **two** locations that build the modeler header. Both have this pattern:

```js
var html = '<div class="mod-header">';
```

**In both locations**, immediately after that line, insert the following block:

```js
html += '<div class="mod-brand-bar">';
html += '<img class="mod-brand-logo" src="/resource/OTN_Logo" alt="One Therapy Network" />';
html += '</div>';
```

The two locations are approximately:
- **Line ~2895** — header for a loaded model (has clinic name logic)
- **Line ~2972** — header for the empty/list state (`ONE Model` fallback)

---

## Notes

- The logo PNG already contains the full "One Therapy Network" wordmark, so no separate text element is needed.
- `opacity: 0.75` on `.mod-brand-logo` softens it slightly so it reads as a brand mark rather than competing with the clinic name headline. Adjust or remove if the team prefers full opacity.
- If the static resource URL doesn't resolve, check Setup → Static Resources and confirm the name is exactly `OTN_Logo` (case-sensitive in the URL).
