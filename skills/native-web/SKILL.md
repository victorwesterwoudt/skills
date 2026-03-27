---
name: native-web
description: Guides building UI components and interactions using modern native HTML, CSS, and browser APIs instead of framework components. Use when building frontend UI, implementing interactions, adding animations, creating modals/dropdowns/tooltips, styling components, building accessible components, avoiding React component libraries, building without dependencies, using the Popover API, using the dialog element, authoring Web Components, or when reaching for ShadCN, MUI, Radix, or similar component libraries. Also activates when writing CSS that could use modern layout, transitions, or selectors.
---

Before writing any UI code, ask: **does a native HTML element, CSS feature, or browser API already solve this?**

## The anti-pattern to avoid

The most common frontend mistake: take a native element (`<input type="radio">`, `<select>`, `<dialog>`), hide it, rebuild it in `<div>` + JS, and re-implement ARIA the browser already provides — shipping more bytes, more bugs, worse accessibility.

```html
<!-- Accessible, keyboard-navigable, form-compatible. Zero JS. -->
<input type="radio" name="beverage" value="coffee" id="coffee">
<label for="coffee">Coffee</label>
```

Style it with `appearance: none` + `::before` + `:checked`. No library needed.

## Decision rule

Work through this hierarchy before reaching for a library:

1. **Native HTML element** — does one already exist with the right semantics? (`<dialog>`, `<details>`, `<input type="range">`, etc.)
2. **CSS-only solution** — can styling alone achieve the desired result?
3. **Web Component** — need a reusable custom element without a framework dependency? Use the platform's own component model.
4. **Framework component library** — only when the above are genuinely insufficient.

Use a library only for: complex widgets with no native equivalent (combobox, calendar date picker, data table with virtual scroll, rich text editor), complex drag-and-drop, or an established design system where consistency outweighs the overhead.

## Web Components

When a native element doesn't fit but you'd otherwise reach for a React component, use a Web Component — part of the platform, no framework, no build step, works inside React/Vue/Svelte/plain HTML.

**Use when:** you need a reusable custom element with encapsulated styles across frameworks, a drop-in widget with no dependencies, or an extended native element (`class IconButton extends HTMLButtonElement`).

```js
class MyCard extends HTMLElement {
  connectedCallback() {
    const shadow = this.attachShadow({ mode: 'open' });
    shadow.innerHTML = `
      <style>p { color: red; }</style>  /* scoped */
      <slot></slot>                      /* like React children */
    `;
  }
}
customElements.define('my-card', MyCard);
```

Baseline in all modern browsers. No polyfills needed.

## Native element reference

| Instead of | Use | What you get for free |
|---|---|---|
| `<RadioGroup>` (ShadCN/Radix) | `<input type="radio">` + CSS | ARIA role, keyboard nav, grouping via `name`, form participation |
| `<Checkbox>` | `<input type="checkbox">` + CSS | ARIA role, `:checked` state, form participation |
| `<Switch>` | `<input type="checkbox" role="switch">` + CSS | Toggle semantics, no JS state |
| `<Slider>` | `<input type="range">` + CSS | ARIA, keyboard control, `min`/`max`/`step` |
| `<Dialog>` / `<Modal>` | `<dialog>` + `showModal()` | Focus trap, `::backdrop`, Escape key, `close` event |
| `<Accordion>` | `<details>`/`<summary>` | Toggle behaviour, accessible disclosure pattern |
| `<Select>` (custom) | `<select>` + `appearance: none` | Full keyboard nav, form participation, ARIA |
| `<Tooltip>` | CSS `[data-tooltip]::after` or `popover` API | Zero JS for simple cases |
| `<Progress>` | `<progress>`/`<meter>` | Semantic role, accessible value announcement |
| `<Tabs>` (simple) | Radio inputs + `:has()` CSS | CSS-only state, no JS |
| Sticky JS library | `position: sticky` | No JS, composable |
| Scroll listener for enter | `IntersectionObserver` | Performant, no scroll math |
| Truncation JS | `text-overflow: ellipsis` / `-webkit-line-clamp` | Pure CSS |
| Auto-resize textarea JS | `field-sizing: content` | One CSS property (Baseline 2024) |

## `<dialog>` pattern

Use `showModal()` for a true modal (focus-trapped, `::backdrop`, Escape key) or `show()` for a non-modal panel.

```html
<dialog id="confirm-dialog">
  <h2>Confirm action</h2>
  <form method="dialog">
    <button value="cancel">Cancel</button>
    <button value="confirm">Confirm</button>
  </form>
</dialog>
```

```js
dialog.showModal();
dialog.addEventListener('close', () => {
  if (dialog.returnValue === 'confirm') runAction();
});
```

Animate with `@starting-style` + `transition: opacity allow-discrete`. Style `::backdrop` with CSS. See [references/patterns.md](references/patterns.md) for full CSS.

**Free:** focus trap, Escape key, `::backdrop`, `returnValue`, `close` event, ARIA role.

## Popover API

Use `popover` (Baseline 2024) for **non-modal overlays** — tooltips, dropdowns, menus. Use `<dialog showModal()>` when focus must be trapped.

```html
<!-- Zero JS — button wires to popover by ID -->
<button popovertarget="my-menu">Open menu</button>
<ul id="my-menu" popover>
  <li><a href="/profile">Profile</a></li>
  <li><button>Log out</button></li>
</ul>
```

Position with CSS anchor positioning (`anchor-name` / `position-anchor`). Animate with `@starting-style`. See [references/patterns.md](references/patterns.md) for full CSS.

| Need | Use |
|---|---|
| Non-modal overlay | `popover` attribute |
| Modal (focus trap) | `<dialog>` + `showModal()` |
| Inline disclosure | `<details>`/`<summary>` |

**Free:** top-layer stacking, light-dismiss, Escape key, `:popover-open` pseudo-class.

## Modern CSS that replaces JS patterns

- **Fluid sizing without breakpoints**: `clamp(1rem, 2.5vw + 0.5rem, 1.5rem)`
- **Component-level responsive**: `@container` queries instead of media queries on the viewport
- **Parent selection**: `:has()` replaces many JS class-toggle patterns (e.g. `form:has(:invalid)`)
- **Entrance animations**: `@starting-style` replaces JS animation libraries for enter transitions
- **Dark mode without CSS-in-JS**: `light-dark()` or CSS custom properties + `prefers-color-scheme`
- **Auto-fill grid**: `grid-template-columns: repeat(auto-fill, minmax(240px, 1fr))` replaces grid components
- **Smooth scroll**: `scroll-behavior: smooth` on `html` or `element.scrollIntoView({ behavior: 'smooth' })`
- **View transitions**: `@view-transition { navigation: auto }` for route-change animations (Baseline 2024)

When a CSS feature isn't at full baseline, note it with a comment and provide a graceful fallback — but don't avoid it. Progressive enhancement is the right approach.

## Additional resources

- For detailed implementation patterns (dialog, popover, CSS-only tabs, form-associated elements), see [references/patterns.md](references/patterns.md)
