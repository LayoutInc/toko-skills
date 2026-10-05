# Surfaces

A surface is a background that other things sit on: the page, a card, a menu, the sidebar. Toko solves every token again for each surface, so `--text-muted` is readable and `--border` visible on whichever background it lands on. That's why tokens are named by purpose (`--surface`, `--text`, `--border`) rather than by color.

To make an element a surface, give it the attribute and its background:

```html
<div data-surface="raised" class="bg-surface shadow-sm">…</div>
<!-- plain CSS: background: var(--surface); box-shadow: var(--shadow-sm); -->
```

Everything inside it now uses that surface's tokens. Surfaces nest: a menu inside a card, a selected row inside a menu. The page itself is a surface already, via `:root`.

The AGENTS.md section lists this palette's surfaces with their exact attribute values; the user can rename them in Toko, so read the names from there or from tokens.css, not from memory. By role:

| Role | Use for | Shadow |
|---|---|---|
| Page | the main background, already on `:root` | none |
| Inset | wells and panels set into the page: code blocks, sidebars inside content, empty states | none |
| Raised | cards, tiles, list groups that sit on the page | `shadow-sm` (a button on it: `shadow-xs`) |
| Overlay | popovers, dropdown menus, selects, tooltips, dialogs, sheets | `shadow-md` for menus, `shadow-lg` for dialogs, `shadow-xl` for windows |
| Nav | persistent navigation: sidebar, top bar | none |
| Selected | the chosen item in a list or group; pair it with a check or bar | none |
| Focused | the item the keyboard is on; the focus ring still shows | none |

Put the attribute on the component's outermost element, once, in the shared component (the `Card`, the `PopoverContent`), not at every call site. Popovers and menus usually render in a portal at the end of `<body>`; the attribute still works there because it sits on the content element itself. For selected and focused, set the attribute only while the state is true, from the state the component already has (`aria-selected`, `data-state="checked"`, `data-highlighted`).
