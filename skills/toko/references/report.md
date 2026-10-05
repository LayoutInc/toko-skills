# Report back

The person reading this may not be a developer. Lead with the outcome, in plain words, then the detail. Keep it short enough to read in a minute.

```markdown
**Your Toko palette is set up.** Colors, shadows and text sizes now come from Toko, and dark mode follows it.

**What changed**
- Added the Toko files in `src/styles/toko/` and connected them to the app.
- Your existing components now use the palette (they were built with shadcn; their colors now point at Toko).
- Dark mode: the theme switch now changes Toko's colors too.
- Cards, menus and the sidebar now know which surface they are, so text stays readable on each.
- Text now uses your Toko type, in Inter.
- Replaced 48 colors, 6 shadows and 12 text sizes that were typed in by hand, across 17 files.
- Added a Toko section to AGENTS.md, so future changes keep using the palette.

**Worth a look**
- `components/Chart.tsx`: left the chart colors as they were.
- `app/pricing/page.tsx:42`: this gray could be a border or muted text; I used muted text.

**Checked**: the build passes, and I looked at the home page in light and dark.

**To undo everything**: discard everything since commit `a1b2c3d`: "Discard all changes" in your editor, or in a terminal `git restore .` and then delete the new files `git status` lists (the Toko folder and, if it's new, AGENTS.md).

**To change the palette later**: edit it in Toko, export again, and paste the new prompt.
```

- After a palette change (the Sync job), lead with what looks different and give the palette's link. For undo, tell the user: in Toko, open **Version history** in the palette menu at the bottom of the sidebar and pick the version from before the change, or ask you to restore it; then `toko pull` brings the code back. With a `toko.json`, changing the palette later means changing it in Toko or asking an agent, then running `toko pull`.
- Say what you verified and what you couldn't.
- On an update, say what looks different now, not only which files changed.
- If the app had no dark mode before, say it now follows the system setting, and how to keep it light only.
- If you stopped early or skipped something, say so and why.
