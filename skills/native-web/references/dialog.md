# `<dialog>` — Modal pattern

**Use when:** you need to block page interaction until the user makes a decision.

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

// fires on both Escape and form submission
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

**Free:** focus trap, Escape close, `::backdrop` top-layer overlay, `returnValue` from `<form method="dialog">`, correct ARIA semantics.
