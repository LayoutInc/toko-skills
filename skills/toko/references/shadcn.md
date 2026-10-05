# shadcn and other theme variables

shadcn components read their own variables (`bg-background`, `text-muted-foreground`, `border-input`). Keep the components and their class names; make the variables point at Toko's tokens.

In the global stylesheet, replace shadcn's color values in `:root` with the aliases below, on a selector that also matches every surface, so they re-solve inside cards and menus. Then delete shadcn's `.dark { … }` color block: the tokens already switch.

```css
:root,
[data-surface] {
  --background: var(--surface); --foreground: var(--text);
  --card: var(--surface); --card-foreground: var(--text);
  --popover: var(--surface); --popover-foreground: var(--text);
  --primary: var(--action-primary); --primary-foreground: var(--action-primary-text);
  --secondary: var(--action-secondary); --secondary-foreground: var(--action-secondary-text);
  --muted: var(--surface-hover); --muted-foreground: var(--text-muted);
  --accent: var(--action-ghost-hover); --accent-foreground: var(--text);
  --destructive: var(--error-solid); --destructive-foreground: var(--error-solid-label);
  --input: var(--border-strong);
  --sidebar: var(--surface); --sidebar-foreground: var(--text);
  --sidebar-primary: var(--action-primary); --sidebar-primary-foreground: var(--action-primary-text);
  --sidebar-accent: var(--action-ghost-hover); --sidebar-accent-foreground: var(--text);
  --sidebar-border: var(--border-subtle); --sidebar-ring: var(--ring);
}
```

- **Delete** shadcn's own `--border` and `--ring`: Toko defines both under the same names, and shadcn's would win.
- **Delete** shadcn's own `--chart-1` … `--chart-5`: Toko defines `--chart-1` … `--chart-8` (one hue each, 3:1 on the page) under the same names.
- **Keep** `--radius` and the `@theme inline { --color-background: var(--background); … }` block as they are.
- **Tailwind v3 shadcn** stores colors as channels (`--background: 0 0% 100%`) and wraps them in `tailwind.config.js` as `hsl(var(--background))`. Toko's values are whole colors, so that wrapper turns them transparent. Change every entry to the bare variable (`background: "var(--background)"`, dropping any `/ <alpha-value>`), and replace opacity modifiers like `bg-background/50` with the see-through scales.
- **Surfaces**: add `data-surface` in the shared components so `--card` and `--popover` get the right background: `Card` → raised; `PopoverContent`, `DropdownMenuContent`, `SelectContent`, `HoverCardContent`, `DialogContent`, `SheetContent`, `DrawerContent` → overlay; `TooltipContent` and toasts → inverse (shadcn draws them in the foreground color); `Sidebar` → nav. Dialog and sheet overlays use `bg-scrim` in place of `bg-black/50`, and destructive buttons hover with `bg-error-solid-hover` in place of `hover:bg-destructive/90`. Use the attribute values from the AGENTS.md section.
- Leave shadcn's `dark:` variants that use its own variables (`dark:bg-input/30`) as they are.
- New components can use Toko's utilities directly (`bg-surface`, `text-text-muted`); existing ones keep shadcn's names. Both resolve to the same tokens.

## Other design systems

For any other set of theme variables, map each one by what it's used for, the same way: page background → `--surface`, body text → `--text`, secondary text → `--text-muted`, dividers → `--border-subtle`, input borders → `--border-strong`, the main button → `--action-primary`. Define the aliases on `:root, [data-surface]` so they follow surfaces.
