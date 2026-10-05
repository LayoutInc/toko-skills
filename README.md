# Toko skills

Skills that let coding agents set up, sync and check [Toko](https://toko-neon-rho.vercel.app) design tokens.

```bash
npx skills add LayoutInc/toko-skills
```

Then ask your agent, for example: "set up my Toko tokens", "pull the latest palette from Toko", "make the accent warmer in Toko and update the code" or "does the app use the Toko tokens everywhere?".

- `toko`: installs the tokens, connects shadcn and dark mode, puts surfaces on cards and menus, replaces hard-coded colors, shadows and font sizes, leaves a Toko section in AGENTS.md, and brings palette changes into the code. `scripts/check.mjs` and `scripts/inventory.mjs` check the result.

The skill works on its own with files exported from Toko, and does more with Toko's other agent tools:

- **The Toko CLI**, `npx @layoutinc/toko`: `init` links a project to a saved palette and writes the files, `pull` brings in changes, `set` and `push` change the palette, and `check` runs in CI with no account.
- **The Toko MCP server**: lets Claude, Cursor, VS Code and other apps read and change your saved palettes. Toko keeps the version before every change, so one click undoes it.

Setup for both: [llms-full.txt](https://toko-neon-rho.vercel.app/llms-full.txt), or Export → Connect AI app in Toko.
