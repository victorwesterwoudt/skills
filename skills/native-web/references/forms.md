# Custom-styled form inputs

**Use when:** you need inputs that match a design system without a component library. Style native elements — form participation, states, and keyboard accessibility are preserved.

## Checkbox with clip-path checkmark

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

## Range slider

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

**Free:** form participation, `:checked`/`:invalid`/`:disabled` states, keyboard accessibility — preserved because you're styling native elements.

## Form-associated Web Components

**Use when:** a Web Component needs to participate in `<form>` submission and `FormData`. Baseline 2023.

```js
class StarRating extends HTMLElement {
  static formAssociated = true;

  #internals;
  #value = '0';

  constructor() {
    super();
    this.#internals = this.attachInternals();
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
    this.#internals.setFormValue(v);
    this.shadowRoot.querySelectorAll('button').forEach(b => {
      b.dataset.on = b.dataset.v <= v;
    });
    this.dispatchEvent(new Event('change', { bubbles: true }));
  }

  formResetCallback() { this.value = this.getAttribute('value') ?? '0'; }
}
customElements.define('star-rating', StarRating);
```

```html
<form method="post">
  <label>Rate: <star-rating name="rating"></star-rating></label>
  <button type="submit">Submit</button>
</form>
```

**Free:** `FormData` inclusion, form reset, validity API, label association — all via `ElementInternals`.
