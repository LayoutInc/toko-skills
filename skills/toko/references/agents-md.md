# Leave rules for later sessions

Agents read AGENTS.md (or CLAUDE.md) at the start of every session. A short Toko section there is what keeps new code on the tokens after today.

**Where**

- `AGENTS.md` at the project root exists: add the section there.
- Only `CLAUDE.md` exists: add it to CLAUDE.md.
- Both exist: add it to AGENTS.md, and make sure CLAUDE.md contains `@AGENTS.md` on a line of its own (Claude Code only reads AGENTS.md when CLAUDE.md points to it).
- Neither exists: create `AGENTS.md`.

**How**

- When working from a Toko prompt, it contains the section inside `<agents-md>`. Copy it as it is, replacing only `<folder>` with the real path to the Toko files (for example `src/styles/toko`). It lists this palette's exact names.
- With a `toko.json`, `toko init` and `toko pull` write the section themselves. Don't edit it; check it names the real folder.
- The section starts with `<!-- toko:start -->` and ends with `<!-- toko:end -->`. If those markers already exist, replace everything between them. Never add a second section.
- Put it after the file's existing content, and don't reword the rest of the file.

**Without a prompt** (updating with only the new files), rebuild the section from tokens.css in the same shape, keeping what the old section says and replacing the names and values:

```markdown
<!-- toko:start -->
## Design tokens (Toko)

Colors, surfaces, shadows and type come from Toko. The values live in `<folder>/tokens.css` (and `<folder>/tailwind.css` with Tailwind v4), which are generated: don't edit them. To change a value, change the palette in Toko, export again and replace the files (with a `toko.json`: change it in Toko or with `npx @layoutinc/toko set`, then run `npx @layoutinc/toko pull`).

- Use tokens, never raw colors: no hex/rgb/oklch values, no Tailwind palette classes like `bg-blue-500`, no arbitrary values like `bg-[#…]` or `text-[13px]`.
- Pick a token by what the element is (text, border, main button), not by the color it looks like.
- Light and dark switch by themselves through `color-scheme`. Don't write `dark:` color overrides.
- Cards, menus, dialogs and navigation get `data-surface` plus `bg-surface` on their outer element; every token inside re-solves for that background.

### Surfaces
<one row per [data-surface="…"] in tokens.css: attribute, what it's for, shadow>

### Colors
<one row per semantic token: variable, Tailwind utility, what it's for; then status tokens and scales>

### Shadows
<every --shadow-* level in tokens.css (shadow-xs … shadow-xl, plus 2xs/2xl or 3xs/3xl if present) with their use>

### Type
<one row per type role: utility, size, what it's for>
<!-- toko:end -->
```
