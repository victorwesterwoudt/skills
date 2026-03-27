# CSS-only tabs — radio inputs + `:has()`

**Use when:** you need tab UI with zero JavaScript. `:has()` is Baseline 2023.

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

**Free:** keyboard nav (arrow keys in radio group), no JS state, SSR-compatible.
