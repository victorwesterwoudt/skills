# IntersectionObserver — replace scroll listeners

**Use when:** you need to react to elements entering the viewport without scroll event overhead.

## Lazy-loading images

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

## Entrance animations

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

## Infinite scroll sentinel

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

**Free:** runs off main thread (no jank), no scroll math, automatic cleanup via `disconnect()`, configurable `root`/`threshold`/`rootMargin`.
