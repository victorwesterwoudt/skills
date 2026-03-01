---
name: native-web
description: Guides building UI components and interactions using modern native HTML, CSS, and browser APIs instead of framework components. Use when building frontend UI, implementing interactions, adding animations, creating modals/dropdowns/tooltips, styling components, or when reaching for ShadCN, MUI, Radix, or similar component libraries. Also activates when writing CSS that could use modern layout, transitions, or selectors.
---

Before writing any UI code, ask: **does a native HTML element, CSS feature, or browser API already solve this?**

## The anti-pattern to avoid

The most common mistake in modern frontend:

1. Take a native element that already works (`<input type="radio">`, `<select>`, `<dialog>`)
2. Hide it and rebuild it using `<div>` and `<span>`
3. Re-implement focus, keyboard navigation, and ARIA in JavaScript
4. Ship more bytes, more bugs, and worse accessibility than the browser gave you for free

**Styling is not a JavaScript concern.** The W3C ARIA spec is explicit: *"If you can use a native HTML element with the semantics and behavior you require already built in, do so."*

The canonical example: a ShadCN radio button is three libraries deep (ShadCN → Radix UI → custom React), imports the entire Lucide icon library to draw the selection dot, and re-implements ARIA that the browser already provides. That dot is `border-radius: 50%`. The whole abstraction exists to achieve styling.

```html
<!-- This is a radio button. Accessible, keyboard-navigable, form-compatible. Zero JS. -->
<input type="radio" name="beverage" value="coffee" id="coffee">
<label for="coffee">Coffee</label>
```

Style it with CSS using `appearance: none` + `::before` + `:checked`. No library needed.

## Decision rule

Work through this hierarchy before reaching for a library:

1. **Native HTML element** — does one already exist with the right semantics? (`<dialog>`, `<details>`, `<input type="range">`, etc.)
2. **CSS-only solution** — can styling alone achieve the desired result?
3. **Web Component** — need a reusable custom element without a framework dependency? Use the platform's own component model.
4. **Framework component library** — only when the above are genuinely insufficient.

Use a library only for: complex widgets with no native equivalent (combobox, calendar date picker, data table with virtual scroll, rich text editor), complex drag-and-drop, or an established design system where consistency outweighs the overhead.

## Web Components

When a native element doesn't fit but you'd otherwise reach for a React component, consider a Web Component. They're part of the platform: no framework, no build step, and they work inside React, Vue, Svelte, or plain HTML.

```js
class ToastMessage extends HTMLElement {
  connectedCallback() {
    this.innerHTML = `<p>${this.getAttribute('message')}</p>`;
    setTimeout(() => this.remove(), 3000);
  }
}
customElements.define('toast-message', ToastMessage);
```
```html
<toast-message message="Saved!"></toast-message>
```

**Use a Web Component when:**
- You need a reusable custom element with encapsulated styles (Shadow DOM) that works across frameworks or projects
- You're building a widget that should be dropped in anywhere without dependencies: media players, code blocks, custom inputs, embeddable UI
- You want to extend a native element and keep its built-in accessibility: `class IconButton extends HTMLButtonElement`

**Shadow DOM** keeps styles encapsulated — no class name collisions, no leaking in or out:
```js
class MyCard extends HTMLElement {
  connectedCallback() {
    const shadow = this.attachShadow({ mode: 'open' });
    shadow.innerHTML = `
      <style>p { color: red; }</style> <!-- scoped to this element only -->
      <slot></slot>
    `;
  }
}
```

**Slots** let consumers provide content, just like React children:
```html
<my-card>
  <span slot="title">Hello</span>
  <p>Body content here</p>
</my-card>
```

Web Components are universal Baseline (all modern browsers). No polyfills needed.

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
