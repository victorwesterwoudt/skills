# Popover API — Tooltip and dropdown

**Use when:** you need a non-modal overlay (tooltip, dropdown, menu) that light-dismisses on outside click.

## Tooltip with anchor positioning

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

## Dropdown menu

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

**Free:** top-layer stacking (no `z-index` wars), light-dismiss, Escape close, `:popover-open` state, `toggle` event.
