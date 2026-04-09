# ONE Design System

Design system for One Therapy Network. Contains CSS tokens, reusable component classes, and documentation for building consistent UI across the OTN platform.

## Quick Start

1. Copy `css/one-design-system.css` into your project
2. Wrap your root element with `class="one-ds"` (add `one-theranet` or `one-financial` for theme)
3. Use `one-*` CSS classes and `--one-*` custom properties

## Documentation

- [Design System Reference](docs/ONE-DESIGN-SYSTEM.md) — Tokens, components, naming conventions
- [JS Patterns](docs/ONE-JS-PATTERNS.md) — Icons, interaction flows, Skuid integration
- [Branding](docs/one-branding-instructions.md) — Brand colors, logo usage

## Themes

| Theme | Class | Use For |
|-------|-------|---------|
| Base | `.one-ds` | Default tokens + all components |
| Theranet | `.one-ds.one-theranet` | Clinical/operational tools |
| Financial | `.one-ds.one-financial` | Modeler, Pay Calculator |
