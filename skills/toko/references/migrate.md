# Replace hard-coded values

The goal: every color, shadow and font size in the code comes from a token, so the palette can change in Toko and the whole app follows, in light and dark. Change values only, never the design: no new spacing, radius or layout.

1. **Make a list.** Run `inventory.mjs`, or search as described under Checks. Skip the Toko files themselves.
2. **Map each value by purpose, not by looks.** Ask what the element *is*, then take that token: body text → `text`, a timestamp → `text-muted`, a card's edge → `border-subtle`, an input's border → `border-strong`, the main button → `action-primary`, an error message → `error-label`. The nearest-looking color is usually wrong in the other mode or on another surface.
3. **Work in batches** of a few files. Build (or typecheck) between batches, so a mistake shows up near its cause.
4. **When two tokens fit equally**, follow the default in the procedure and note the spot for the report rather than stopping.

| Hard-coded | Becomes |
|---|---|
| `color: #6b7280` on a caption | `color: var(--text-muted)` / `text-text-muted` |
| `bg-white` on a card | `data-surface="raised"` + `bg-surface` |
| `border-gray-200` on a divider | `border-border-subtle` |
| `bg-blue-600 hover:bg-blue-700` on the main button | `bg-action-primary hover:bg-action-primary-hover text-action-primary-text` |
| `text-red-600` on an error | `text-error-label` |
| `box-shadow: 0 4px 12px rgba(0,0,0,.1)` on a menu | `box-shadow: var(--shadow-md)` / `shadow-md` |
| `text-sm` / `font-size: 14px` | `text-small` / the `--text-small-*` variables |
| `text-2xl font-semibold` on a section heading | a middle title such as `text-title-3` (weight included); the page title takes the largest title |
| `outline: 2px solid #3b82f6` on focus | `outline: 2px solid var(--ring)` |
| `var(--text, #333)` | `var(--text)`: the token always exists, so drop the fallback |

Pick sizes by role too: body text → `text-regular`, secondary UI → `text-small`, headings by level. The title roles carry their own weight, so drop the separate `font-semibold` when you switch.

Leave alone, and mark the line with `/* toko-keep */` or `// toko-keep` so the checks skip it: images, logos and brand marks, syntax-highlighting themes, chart and data-visualization colors (unless the user asks; then use `--chart-1` … `--chart-8`, one per series), colors in emails and PDFs. Third-party CSS and anything in `node_modules` stay untouched and unmarked.
