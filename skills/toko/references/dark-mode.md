# Light and dark

Every Toko color is written as `light-dark(light, dark)`. Which half shows depends on the CSS `color-scheme` of the element using it, and tokens.css sets it:

- `:root { color-scheme: light dark }`: follows the system setting.
- `[data-theme="light"]` / `[data-theme="dark"]`: forces a mode for that element and everything inside it.

So nothing needs to switch colors any more. Make the app's own switch set `color-scheme`:

| The project today | Do this |
|---|---|
| No dark mode | Nothing to wire: the app now follows the system setting, so it turns dark for people who use dark mode. Say so in the report, with the opt-out: `:root { color-scheme: light; }` after the Toko import keeps it light only. |
| `next-themes` | Leave `attribute` as it is (shadcn uses `"class"`). next-themes already writes `color-scheme` on `<html>` (its `enableColorScheme` option, on by default), and the tokens follow that. Make sure `enableColorScheme={false}` isn't set. Don't switch to `attribute="data-theme"` alone: it removes the `.dark` class and breaks `@custom-variant dark (&:is(.dark *))`. |
| Its own `.dark` class toggle | Add `.dark { color-scheme: dark; } .light { color-scheme: light; }` to the global stylesheet, after the Toko import. Keep the class and any `@custom-variant dark`. |
| A `data-theme` or `data-mode` attribute with other values | Add matching rules that set `color-scheme`, e.g. `[data-mode="night"] { color-scheme: dark; }`. |

Then remove what the tokens replace:

- The project's dark color block (shadcn's `.dark { --background: … }`, a `@media (prefers-color-scheme: dark)` block of colors). Only the colors go; other rules in the block stay.
- `dark:` variants that only change a color to a hard-coded one (`dark:bg-zinc-900`, `dark:text-white`). Keep `dark:` for things tokens don't cover, like swapping an image, and leave `dark:` variants that already use theme variables (shadcn's own `dark:bg-input/30`).

Test both modes after the change. If a color stays the same in both, something still overrides the token.
