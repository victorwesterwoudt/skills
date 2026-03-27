# Accordion — `<details>` / `<summary>`

**Use when:** you need expandable content. The `name` attribute creates an exclusive accordion (Baseline 2024) — opening one closes others automatically.

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

**Free:** toggle behaviour, accessible disclosure semantics, keyboard accessible, `toggle` event. With `name`: exclusive accordion, zero JS.
