# Sync with Toko

`toko.json` links the project to a palette saved in Toko (`palette`) and says where the files go (`out`). Toko is the source of truth: a change goes into the palette first, then into the code. Below, `toko` means `npx @layoutinc/toko`; add `--json` to any command to read its result and, on failure, `error.fix`.

1. **Change the palette**, one request per call, with a short reason the user will see in Toko's history: a call may change several related settings, but don't mix unrelated requests. Toko works every token out from settings like `color.primary`, `surfaces.contrast` and `grays.tint`, so one token's color can't be set: change the setting behind it and say so. A wrong setting name gets a "did you mean".
   - With Toko's tools (`get_palette`, `update_palette`): read the palette toko.json names, then send only what changes, e.g. `{"color": {"primary": "#f2613a"}}` with the reason "Warmer primary color". The result lists what changed and an undo version.
   - Without them: `toko set color.primary=#f2613a -m "Warmer primary color"` saves the change in Toko and pulls, so skip step 2. Or edit `spec` in toko.json and run `toko push -m "Warmer primary color"`.
2. **Pull**: `toko pull` rewrites the files in `out` and the AGENTS.md section, and lists what changed. Act on every follow-up it lists (a renamed surface, a removed token) as `update.md` describes.
3. **Check** with `toko check` and the project's build, then **report** as `report.md` says: what looks different, the palette's link, how to undo.

If a command fails:
- **Exit 3, signed out**: run `npx @layoutinc/toko login --json --no-wait` and give the user the `url` and `code` it prints. After they approve in the browser, run `npx @layoutinc/toko login --json` again to finish, then run your command again.
- **Exit 4, `CONFLICT`**: the palette changed in Toko since the last pull. Run `npx @layoutinc/toko pull --force` (it drops the unpushed changes in toko.json), make the change again with `toko set`, then push.
- **Exit 4, `EDITED`**: a file in `out` was edited by hand. Move the edit into the palette (`update_palette` or `toko set`), then `npx @layoutinc/toko pull --force`.
- **Exit 1** from check: fix what it lists. **Exit 2**: read `error.fix`; `NOT_SET_UP` means there's no toko.json yet (`toko init <palette id>`). **Exit 5**: network; try again. **Exit 6**: `toko palettes` lists the palettes there are.

Never edit files in `out` or toko.json's `lock`. Without `palette` in toko.json the setup is local only: edit `spec`, then `toko build`. With no toko.json, change the palette, then offer to link the project with `toko init <palette>` (`install.md`).
