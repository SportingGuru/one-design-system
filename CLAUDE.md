# ONE Design System

## What This Is

The canonical design system for One Therapy Network (OTN). Contains design tokens (CSS custom properties), reusable component classes, icon catalog, and behavioral patterns for building custom Skuid components inside Salesforce.

## File Structure

```
css/one-design-system.css        ← CSS tokens + component classes + theme variants
js/one-icons.js                  ← Shared SVG icon library (future)
docs/ONE-DESIGN-SYSTEM.md        ← Reference guide: tokens, components, naming, state classes
docs/ONE-JS-PATTERNS.md          ← Behavioral patterns: icons, interaction flows, Skuid integration
docs/one-branding-instructions.md ← Brand identity: colors, logo usage
assets/one-logo.png              ← Brand logo
```

## How It's Consumed

This repo is NOT deployed directly. Consuming projects (e.g., `otn-salesforce`) copy the CSS into their static resources and deploy to Salesforce.

**Deployment flow:**
1. Copy `css/one-design-system.css` → consuming project's static resources
2. Deploy as Salesforce static resource (`OneDesignSystemCSS`)
3. Component stubs load it via `<link>` tag before component-specific CSS

## CSS Architecture

Three layers, one file:
- **Base** (`.one-ds` wrapper): Tokens + component classes. All components get this.
- **Theranet theme** (`.one-ds.one-theranet`): Clinical/operational tools. OTN blue `#61a5d2`, system fonts.
- **Financial theme** (`.one-ds.one-financial`): Modeler, PayCalc. Warm palette, Plus Jakarta Sans.

Components activate a theme via wrapper class:
```js
container.className = 'one-ds one-theranet my-component';  // clinical tool
container.className = 'one-ds one-financial my-component';  // financial tool
```

## Key Conventions

- **Class prefix:** `one-` for all shared design system classes
- **Component prefix:** Each component uses its own 2-4 letter prefix (`pm-`, `cps-`, `ed-`, `er-`, etc.)
- **CSS variables:** `--one-*` namespace for all tokens
- **Icons:** Feather-style inline SVGs, `stroke="currentColor"`, `stroke-width="2"`
- **State classes:** `{prefix}-{element}-{state}` pattern (e.g., `pm-tpl-selected`)

## For Claude Code

When building a new OTN component:
1. Read `docs/ONE-DESIGN-SYSTEM.md` for the full token + component reference
2. Read `docs/ONE-JS-PATTERNS.md` for behavioral patterns (icons, state management, Skuid integration)
3. Use `one-` classes from the design system before creating component-specific styles
4. Apply the appropriate theme class (`.one-theranet` or `.one-financial`)
5. Prefix component-specific classes with a unique 2-4 letter namespace
