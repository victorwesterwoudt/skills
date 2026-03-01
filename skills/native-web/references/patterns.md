# Native Web Patterns — Deep Dive Reference

Copy-paste-ready patterns using native HTML, CSS, and browser APIs. No framework dependencies.

---

### 1. Modal dialog — full pattern

**Use this when:** you need to block page interaction until the user makes a decision.

```html
<button id="open-btn">Delete item</button>

<dialog id="confirm-dialog">
  <h2>Delete item?</h2>
  <p>This action cannot be undone.</p>
  <form method="dialog">
    <button value="cancel" autofocus>Cancel</button>
    <button value="confirm">Delete</button>
  </form>
</dialog>
```

```js
const dialog = document.getElementById('confirm-dialog');
document.getElementById('open-btn').addEventListener('click', () => dialog.showModal());

// close event fires on Escape AND form submission
dialog.addEventListener('close', () => {
  if (dialog.returnValue === 'confirm') deleteItem();
});
```

```css
dialog {
  border: none;
  border-radius: 12px;
  padding: 2rem;
  max-width: 400px;
  box-shadow: 0 8px 32px hsl(0 0% 0% / 0.2);
  transition: opacity 0.2s, translate 0.2s, display 0.2s allow-discrete, overlay 0.2s allow-discrete;
  opacity: 1;
  translate: 0 0;
}
@starting-style {
  dialog[open] { opacity: 0; translate: 0 -8px; }
}
dialog::backdrop {
  background: hsl(0 0% 0% / 0.5);
  backdrop-filter: blur(2px);
  transition: background 0.2s, display 0.2s allow-discrete, overlay 0.2s allow-discrete;
}
@starting-style {
  dialog[open]::backdrop { background: hsl(0 0% 0% / 0); }
}
```

**What you get for free:** focus trap, Escape close, `::backdrop` top-layer overlay, `returnValue` from `<form method="dialog">`, correct ARIA semantics.

---

### 2. Popover API — tooltip and dropdown

**Use this when:** you need a non-modal overlay (tooltip, dropdown, menu) that light-dismisses on outside click.

**a) Tooltip with anchor positioning**

```html
<button id="trigger" popovertarget="tip" popovertargetaction="show">What is this?</button>
<div id="tip" popover="hint" role="tooltip">Accepts ISO 8601: YYYY-MM-DD</div>
```

```css
#trigger { anchor-name: --trigger; }
#tip {
  position-anchor: --trigger;
  position-area: block-end center;
  margin-block-start: 0.5rem;
  background: hsl(220 10% 15%); color: #fff; border: none;
  border-radius: 6px; padding: 0.4rem 0.75rem; font-size: 0.875rem;
  transition: opacity 0.15s, display 0.15s allow-discrete;
  opacity: 1;
}
@starting-style { #tip:popover-open { opacity: 0; } }
```

**b) Dropdown menu**

```html
<button popovertarget="nav-menu" id="menu-btn">Menu</button>
<ul id="nav-menu" popover role="menu">
  <li><a href="/profile" role="menuitem">Profile</a></li>
  <li><a href="/settings" role="menuitem">Settings</a></li>
  <li><button role="menuitem">Log out</button></li>
</ul>
```

```css
#menu-btn { anchor-name: --menu-btn; }
#nav-menu {
  position-anchor: --menu-btn;
  position-area: block-end span-inline-end;
  list-style: none; padding: 0.5rem 0; margin: 0;
  border: 1px solid hsl(0 0% 80%); border-radius: 8px;
  background: #fff; box-shadow: 0 4px 16px hsl(0 0% 0% / 0.12);
  transition: opacity 0.15s, display 0.15s allow-discrete; opacity: 1;
}
@starting-style { #nav-menu:popover-open { opacity: 0; } }
#nav-menu li a, #nav-menu li button {
  display: block; width: 100%; padding: 0.5rem 1rem;
  text-decoration: none; background: none; border: none; text-align: left;
}
#nav-menu li :hover { background: hsl(0 0% 95%); }
```

**What you get for free:** top-layer stacking (no `z-index` wars), light-dismiss, Escape close, `:popover-open` state, `toggle` event.

---

### 3. CSS-only tabs using radio inputs + `:has()`

**Use this when:** you need tab UI with zero JavaScript.

```html
<div class="tabs">
  <div class="tab-list">
    <label><input type="radio" name="tabs" checked hidden><span>Overview</span></label>
    <label><input type="radio" name="tabs" hidden><span>Details</span></label>
    <label><input type="radio" name="tabs" hidden><span>Reviews</span></label>
  </div>
  <div class="tab-panels">
    <div class="tab-panel">Overview content</div>
    <div class="tab-panel">Details content</div>
    <div class="tab-panel">Reviews content</div>
  </div>
</div>
```

```css
.tab-list { display: flex; border-bottom: 2px solid hsl(0 0% 85%); }
.tab-list label { cursor: pointer; }
.tab-list label span {
  display: block; padding: 0.625rem 1.25rem;
  border-bottom: 2px solid transparent; margin-bottom: -2px;
  color: hsl(0 0% 45%); transition: color 0.15s, border-color 0.15s;
}
.tab-list label:has(:checked) span { color: hsl(220 90% 50%); border-bottom-color: hsl(220 90% 50%); }
.tab-panel { display: none; padding: 1.5rem 0; }
.tabs:has(input:nth-of-type(1):checked) .tab-panel:nth-child(1),
.tabs:has(input:nth-of-type(2):checked) .tab-panel:nth-child(2),
.tabs:has(input:nth-of-type(3):checked) .tab-panel:nth-child(3) { display: block; }
```

**What you get for free:** keyboard nav (arrow keys in radio group), no JS state, SSR-compatible. `:has()` is Baseline 2023.

---

### 4. CSS-only accordion using `<details>` / `<summary>`

**Use this when:** you need expandable content. The `name` attribute creates an exclusive accordion (Baseline 2024) — opening one closes others automatically.

```html
<div class="accordion">
  <details name="faq" open>
    <summary>What is your return policy?</summary>
    <div class="body"><p>We accept returns within 30 days.</p></div>
  </details>
  <details name="faq">
    <summary>How long does shipping take?</summary>
    <div class="body"><p>Standard shipping: 5–7 business days.</p></div>
  </details>
</div>
```

```css
.accordion { border: 1px solid hsl(0 0% 85%); border-radius: 8px; overflow: hidden; }
details { border-bottom: 1px solid hsl(0 0% 85%); }
details:last-child { border-bottom: none; }
summary {
  padding: 1rem 1.25rem; cursor: pointer; font-weight: 500;
  list-style: none; display: flex; justify-content: space-between; align-items: center;
}
summary::-webkit-details-marker, summary::marker { display: none; }
summary::after {
  content: ''; width: 0.75rem; height: 0.75rem;
  border-right: 2px solid currentColor; border-bottom: 2px solid currentColor;
  transform: rotate(45deg); transition: transform 0.2s;
}
details[open] summary::after { transform: rotate(-135deg); }
summary:hover { background: hsl(0 0% 97%); }
.body { padding: 0 1.25rem 1rem; }
@starting-style { details[open] .body { opacity: 0; } }
details[open] .body { transition: opacity 0.2s; opacity: 1; }
```

**What you get for free:** toggle behaviour, accessible disclosure semantics, keyboard accessible, `toggle` event. With `name`: exclusive accordion, zero JS.

---

### 5. Custom-styled form inputs

**Use this when:** you need inputs that match a design system without a component library.

**Checkbox with clip-path checkmark**

```html
<label class="check-label">
  <input type="checkbox" class="check"> I agree to the terms
</label>
```

```css
.check {
  appearance: none; width: 1.125rem; height: 1.125rem;
  border: 2px solid hsl(0 0% 60%); border-radius: 3px; background: #fff;
  cursor: pointer; display: inline-grid; place-content: center;
  transition: border-color 0.15s, background 0.15s;
}
.check::before {
  content: ''; width: 0.6rem; height: 0.6rem;
  clip-path: polygon(14% 44%, 0 65%, 50% 100%, 100% 16%, 80% 0%, 43% 62%);
  background: #fff; transform: scale(0); transition: transform 0.1s;
}
.check:checked { background: hsl(220 90% 50%); border-color: hsl(220 90% 50%); }
.check:checked::before { transform: scale(1); }
.check:focus-visible { outline: 3px solid hsl(220 90% 60%); outline-offset: 2px; }
.check-label { display: flex; align-items: center; gap: 0.5rem; cursor: pointer; }
```

**Range slider**

```css
.slider { appearance: none; width: 100%; height: 4px; background: hsl(0 0% 85%); border-radius: 2px; }
.slider::-webkit-slider-thumb {
  appearance: none; width: 1.25rem; height: 1.25rem; border-radius: 50%;
  background: hsl(220 90% 50%); border: 2px solid #fff;
  box-shadow: 0 0 0 1px hsl(220 90% 50%); transition: transform 0.1s;
}
.slider::-webkit-slider-thumb:hover { transform: scale(1.15); }
.slider::-moz-range-thumb {
  width: 1.25rem; height: 1.25rem; border-radius: 50%;
  background: hsl(220 90% 50%); border: 2px solid #fff;
}
```

**What you get for free:** form participation, `:checked`/`:invalid`/`:disabled` states, keyboard accessibility — all preserved because you're styling native elements.

---

### 6. Form-associated custom elements

**Use this when:** a Web Component needs to participate in `<form>` submission and `FormData`. Browser support: Baseline 2023.

```js
class StarRating extends HTMLElement {
  static formAssociated = true; // Step 1: declare form-associated

  #internals;
  #value = '0';

  constructor() {
    super();
    this.#internals = this.attachInternals(); // Step 2: attach internals
    this.attachShadow({ mode: 'open' });
    this.shadowRoot.innerHTML = `
      <style>
        :host { display: inline-flex; gap: 0.25rem; cursor: pointer; }
        button { background: none; border: none; font-size: 1.5rem; cursor: pointer;
                 color: hsl(0 0% 70%); transition: color 0.1s; }
        button[data-on="true"] { color: hsl(45 100% 50%); }
      </style>
      ${[1,2,3,4,5].map(n =>
        `<button type="button" data-v="${n}" aria-label="${n} stars">★</button>`
      ).join('')}
    `;
    this.shadowRoot.addEventListener('click', e => {
      const btn = e.target.closest('button');
      if (btn) this.value = btn.dataset.v;
    });
  }

  get value() { return this.#value; }
  set value(v) {
    this.#value = v;
    this.#internals.setFormValue(v); // Step 3: sync to form
    this.shadowRoot.querySelectorAll('button').forEach(b => {
      b.dataset.on = b.dataset.v <= v;
    });
    this.dispatchEvent(new Event('change', { bubbles: true }));
  }

  formResetCallback() { this.value = this.getAttribute('value') ?? '0'; } // Step 4
}
customElements.define('star-rating', StarRating);
```

```html
<form method="post">
  <label>Rate: <star-rating name="rating"></star-rating></label>
  <button type="submit">Submit</button>
</form>
<!-- FormData will include: rating=3 (or whichever star was clicked) -->
```

**What you get for free:** `FormData` inclusion, form reset, validity API, label association — all via `ElementInternals`.

---

### 7. IntersectionObserver — replace scroll listeners

**Use this when:** you need to react to elements entering the viewport without scroll event overhead.

**a) Lazy-loading images**

```js
const observer = new IntersectionObserver((entries, obs) => {
  entries.forEach(entry => {
    if (!entry.isIntersecting) return;
    const img = entry.target;
    img.src = img.dataset.src;
    img.classList.remove('lazy');
    obs.unobserve(img);
  });
}, { rootMargin: '200px' }); // load 200px before entering viewport

document.querySelectorAll('img.lazy').forEach(img => observer.observe(img));
```

**b) Entrance animations**

```css
.reveal { opacity: 0; translate: 0 24px; transition: opacity 0.5s, translate 0.5s; }
.reveal.entered { opacity: 1; translate: 0 0; }
```

```js
const revealObserver = new IntersectionObserver(entries => {
  entries.forEach(e => { if (e.isIntersecting) e.target.classList.add('entered'); });
}, { threshold: 0.1 });
document.querySelectorAll('.reveal').forEach(el => revealObserver.observe(el));
```

**c) Infinite scroll sentinel**

```js
let page = 1, loading = false;
const sentinel = document.getElementById('sentinel');

new IntersectionObserver(async ([entry], obs) => {
  if (!entry.isIntersecting || loading) return;
  loading = true;
  const items = await fetchPage(++page);
  if (!items.length) { obs.disconnect(); return; }
  renderItems(items);
  loading = false;
}).observe(sentinel);
```

**What you get for free:** runs off main thread (no jank), no scroll math, automatic cleanup via `disconnect()`, configurable `root`/`threshold`/`rootMargin`.
