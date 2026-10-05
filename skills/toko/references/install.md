# Install the files

**With the Toko CLI** (preferred when the project can reach npm): run `npx @layoutinc/toko init <palette>` with the saved palette's id or name, or `npx @layoutinc/toko init --from "<share link>"` with a Toko link. It picks the folder, writes the files and `toko.json`, and adds the AGENTS.md section, which later `toko pull` keeps up to date; it never touches the app's code. If it exits 3 (signed out), see `sync.md`. Then import the files once, as below. Without network, or when the CLI fails, save the files from the prompt or the user instead:

Put `tokens.css` next to the global stylesheet in a `toko/` folder, with `tailwind.css` beside it if the project uses Tailwind v4. `tailwind.css` imports `./tokens.css`, so they must stay together. Without Tailwind v4, don't add `tailwind.css` (delete it if the user dropped it in). If the files are already somewhere sensible, leave them there.

Import them **once**, from the global stylesheet or the app's entry, before the project's own styles, so the project can still override on purpose. Edit the existing stylesheet; don't create a new global one.

## Tailwind v4

Replace the project's `@import "tailwindcss";` with the Toko file, which already imports Tailwind and tokens.css:

```css
/* app/globals.css */
@import "./toko/tailwind.css";
/* the rest of the file stays: plugins, @custom-variant, the project's own @theme and rules */
```

If the old line had options: for `source("../src")`, add `@source "../src";` after the import (Tailwind also scans the project as usual, which only adds classes). `source(none)`, `prefix(…)` and `important` can't carry over this way, so tell the user before swapping.

What this changes, so you can plan the migration:

- `--color-*: initial` removes Tailwind's default palette: `bg-blue-500`, `text-gray-600` and friends stop existing. `transparent` and `current` stay; `white` and `black` still build, but they're hard-coded colors, so migrate them too. Tailwind drops unknown classes silently, so the build won't tell you; the inventory does.
- `--text-*: initial` replaces the font sizes: `text-sm`, `text-lg`, `text-2xl` stop existing. Use the type roles (`text-small`, `text-regular`, `text-title-3`).
- `--shadow-*: initial` replaces the shadows with Toko's levels: `shadow-xs` … `shadow-xl`, plus `shadow-2xs` and `shadow-2xl` (and `shadow-3xs`, `shadow-3xl`) when the palette has 7 (or 9) levels. Check `tokens.css` for which exist. The bare `shadow` class, `shadow-inner` and any level the palette doesn't have stop existing; `shadow-none` and `inset-shadow-*` stay.

The mapping is `@theme inline`, so utilities read the token where they're used. That's what lets a `data-surface` re-solve everything inside it; keep any mapping you add `inline` too.

## Tailwind v3

Import `tokens.css` only (not tailwind.css) from the global stylesheet, above the `@tailwind` lines, and point the config at the variables with `extend`, so existing classes keep working while you migrate them:

```js
// tailwind.config.js
theme: { extend: {
  colors: { // …one entry per color token in the AGENTS.md section, like these
    surface: "var(--surface)",
    text: { DEFAULT: "var(--text)", strong: "var(--text-strong)", muted: "var(--text-muted)", subtle: "var(--text-subtle)", link: "var(--text-link)" },
    border: { DEFAULT: "var(--border)", subtle: "var(--border-subtle)", strong: "var(--border-strong)" },
  },
  // …plus 2xs/2xl (3xs/3xl) when tokens.css defines them
  boxShadow: { xs: "var(--shadow-xs)", sm: "var(--shadow-sm)", md: "var(--shadow-md)", lg: "var(--shadow-lg)", xl: "var(--shadow-xl)" },
  fontFamily: { sans: "var(--font-sans)", mono: "var(--font-mono)" },
  fontSize: { // …one entry per type role, like these
    regular: ["var(--text-regular-size)", { lineHeight: "var(--text-regular-line-height)", letterSpacing: "var(--text-regular-letter-spacing)", fontWeight: "var(--text-regular-font-weight)" }],
    "title-3": ["var(--title-3-size)", { lineHeight: "var(--title-3-line-height)", letterSpacing: "var(--title-3-letter-spacing)", fontWeight: "var(--title-3-font-weight)" }],
  },
} },
```

v3 keeps Tailwind's own palette, so `bg-blue-500` still builds: rely on the inventory, not the build, to find what's left. Opacity modifiers (`bg-surface/50`) don't work with `var()` colors in v3; use the see-through scales (`gray-a-*`, `primary-a-*`) instead.

## Plain CSS, CSS Modules, Sass, CSS-in-JS

Import `tokens.css` once from the global stylesheet or the app's entry (`import "./styles/toko/tokens.css"`, or a `<link>` in plain HTML). Then use the variables directly: `color: var(--text-muted)`, `box-shadow: var(--shadow-md)`. In CSS-in-JS, use the same `var(--…)` strings.

## Type and fonts

tokens.css sets `font-family: var(--font-sans)` on the page, where `--font-sans` starts with "Inter Variable" (and `--font-mono` with "JetBrains Mono Variable"). The font has to be loaded:

- Default: add `@fontsource-variable/inter` (and `@fontsource-variable/jetbrains-mono` if the project shows code) with the project's package manager and import it once in the app's entry: `import "@fontsource-variable/inter/opsz.css"` (weights and optical sizes, which the tokens use).
- If the project already loads Inter with `next/font`, put its `variable` class on `<html>` (not `<body>`), then add both of these after the Toko import. The first sets the page font, the second the `font-sans` utility:

  ```css
  :root { --font-sans: var(--font-inter), ui-sans-serif, system-ui, sans-serif; }
  @theme inline { --font-sans: var(--font-inter), ui-sans-serif, system-ui, sans-serif; }
  ```
- If the project sets another font (shadcn's templates set Geist with `--font-sans: var(--font-geist-sans)` in their `@theme inline`), remove that line so the palette's type applies, and say so in the report.

Each type role is a set of variables: `--text-regular-size`, `--text-regular-line-height`, `--text-regular-letter-spacing`, `--text-regular-font-weight` (titles: `--title-3-…`). With Tailwind one utility sets all four: `text-regular`, `text-title-3`. In plain CSS, set all four where you use a role.
