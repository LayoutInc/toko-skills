# Update an existing install

The user changed the palette in Toko and brought new files. Updating replaces the files and fixes what the change broke; everything else stays. With a `toko.json`, `toko pull` does steps 1 and 4 and lists what to fix (`sync.md`); do steps 2, 3 and 5.

1. **Replace the files in place.** Overwrite the existing Toko files where they are. If the new ones landed somewhere else, move them over the old ones and remove the extra copies; that's expected, no need to ask. Don't add a second import or folder.
2. **Map old names to new.** Before replacing the AGENTS.md section, compare its tables with the new section (from the prompt, or rebuilt from the new tokens.css). Rows keep their order and their "Use for" text, so a surface in the same row with a new attribute was renamed: `data-surface="raised"` → `data-surface="card"`.
3. **Fix what no longer exists.** Surfaces can be renamed, scales can gain or lose steps, and type roles can be added or removed. Look for:
   - `data-surface="…"` values that tokens.css no longer defines. Rename them using step 2.
   - `var(--…)` references that nothing declares any more (e.g. `--gray-12` after the scale shrank to 10 steps). Use the nearest step that keeps the same purpose.
   - Tailwind utilities for names that are gone (`text-title-5` after titles were reduced). Use the largest remaining title.

   `check.mjs` lists all three.
4. **Replace the AGENTS.md section** between `<!-- toko:start -->` and `<!-- toko:end -->` with the new one. Don't touch anything outside the markers.
5. Run the checks and the build, look at light and dark, and report what changed in plain words: "Cards now use the surface you renamed to Card", "Titles are a step smaller".

If the project has drifted since the last install (new hard-coded colors), migrate those too, in the same way as a first install.
