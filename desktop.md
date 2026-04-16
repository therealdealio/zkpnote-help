# ZKPnote Desktop — Install & Getting Started

**Status:** Public beta · macOS only (Apple Silicon: M1–M4) · Windows + Linux coming

ZKPnote Desktop is a native Mac app for your encrypted vault. It works offline, syncs with zkpnote.com when you want, and gives you the full note-taking experience without a browser tab.

---

## Download

Grab the latest build from the website:

**→ [zkpnote.com/download](https://zkpnote.com/download)**

You'll get `ZKPnote_0.0.2_aarch64.dmg` (about 9.2 MB).

**Verify the download** (optional but recommended) by running this in Terminal:

```bash
shasum -a 256 ~/Downloads/ZKPnote_0.0.2_aarch64.dmg
```

Expected hash:

```
897f10a6fec4eb4471bcd5ab5d4ac497cee448c9e5b063b75baaa342938f9a84
```

If the hashes match, the file is intact.

---

## Install — 3 Steps

### 1. Open the DMG

Double-click the `.dmg` file in Downloads. A small window pops up with the ZKPnote icon and an Applications folder shortcut.

### 2. Drag ZKPnote to Applications

Drag the ZKPnote app icon onto the Applications folder in the same window.

### 3. First-time open — right-click, not double-click

Because this beta isn't code-signed yet, macOS will complain if you just double-click. You'll see:

> _"ZKPnote cannot be opened because the developer cannot be verified."_

That's Gatekeeper being cautious about unsigned apps. It's not a real problem. To work around it **just the first time:**

1. Open Finder → Applications
2. **Right-click** (or Control-click) the ZKPnote app
3. Choose **Open** from the menu
4. A dialog appears: _"macOS cannot verify the developer. Are you sure you want to open it?"_
5. Click **Open**

After this first run, double-click works normally forever.

---

## Still seeing a "damaged" error?

Open the Terminal app (Applications → Utilities → Terminal) and run:

```bash
xattr -cr /Applications/ZKPnote.app
```

This clears the quarantine attribute macOS added to the downloaded file. It's safe — you're just telling Gatekeeper to trust the app.

---

## First Run

When ZKPnote opens for the first time, it walks you through setup:

1. **Create a vault.** Give it a name (e.g. "Personal") and pick a passphrase. Your notes will be encrypted with a key derived from this passphrase. We can't recover it if you forget — write it down.
2. **Optional: paste your 12-word recovery phrase** if you already have a zkpnote.com web account. This links your desktop vault to the web so the same notes appear on both.
3. **Start writing.** Press **⌘N** for a new note.

---

## Top Features

### Keyboard-first
- **⌘N** — new note
- **⌘⇧N** — new note from a template (daily, meeting, research, project plan)
- **⌘⇧D** — open today's daily journal
- **⌘⇧P** — command palette (every action is here)
- **⌘⇧Space** — global quick capture, even when ZKPnote isn't focused
- **⌘K** — focus search
- **⌘F** — find within the open note
- **⌘/** — keyboard cheat sheet

### Organization
- **Folders + nesting** — drag folders around to reorder siblings; drag to another folder to nest
- **Tags** — inline `#hashtag` in the editor or the Tags tab in the sidebar
- **Pin notes** — right-click any note → Pin. Pinned notes float to the top.
- **Favorites** — right-click a folder → ★ Add to favorites. Quick-jump bar appears above the folder tree.
- **Trash + Undo** — deletes move to trash. Click Undo in the toast within 6 seconds, or open Trash from the command palette to restore later.

### Writing
- **Markdown + Rich text** — flip between modes with **⌘E**. Both stay in sync.
- **Slash commands** — type `/` at the start of a line for a menu: headings, checklists, quote, divider, code block, table, today's date, current time.
- **Wikilinks** — type `[[` to autocomplete a link to any other note.
- **Auto-pair brackets** — `[ ( { " ' \`` close themselves; selection-wrap works; re-typing the closer smart-skips.
- **Smart list continuation** — Enter after `- item` / `1. item` / `> quote` carries the marker forward. Empty marker line exits the list.
- **Attachments** — drag images, PDFs, or any file into the editor. Stored encrypted alongside the note.

### Knowledge graph
- Visualize your whole vault as nodes (notes) and edges (wikilinks, shared tags, continuations, related-by-content, duplicates, shared-folder)
- Search bar dims non-matches; click legend items to hide/show node groups
- Color by tag or folder; hover a node or edge for details

### Proof of authorship
- Single **Prove on Solana** button (amber while you finalize with Phantom on zkpnote.com, green once on-chain)
- Publishes a SHA-256 of the note to Solana as a timestamp you can later verify publicly

### Sync
- Push your encrypted vault to zkpnote.com and pull updates
- Configurable auto-sync interval
- 3-way merge when the same note is edited in two places

### View modes
- **Focus mode (⌘.)** — hides sidebar, note list, status bar, tabs. Just you and the text. A floating "Exit focus" pill brings chrome back.
- **Typewriter mode (⌥⌘T)** — caret stays vertically centered as you scroll.
- **System theme** — follows your macOS Appearance setting in real time.
- **Show/hide chrome** — ⌥⌘1 sidebar, ⌥⌘2 note list, ⌥⌘S status bar, ⌥⌘I inspector.

---

## Built-in Diagnostic Bot

If something feels broken, run **⌘⇧P → "Run diagnostics"**. It runs 13 end-to-end tests and shows pass/fail for each layer (IPC, attachments, tiptap rendering, history replay, etc.). Paste the results into a bug report and we can usually pinpoint the cause from the output.

---

## Important Notes

- **Don't make the beta the only copy of critical data.** Periodically export: **⌘⇧P → "Export vault to directory"** writes encrypted `.md` files to a folder of your choosing.
- **Your passphrase is your key.** We can't reset it. Use a password manager.
- **No telemetry.** ZKPnote makes no network calls except the sync endpoint you configure (`zkpnote.com` by default). Everything else is local.

---

## Reporting Bugs

Please file issues at **[github.com/therealdealio/zkpnote-help/issues](https://github.com/therealdealio/zkpnote-help/issues)**.

Include:
- macOS version (About This Mac)
- What you were doing
- What you expected
- What actually happened
- Diagnostic bot output if possible

---

## Uninstalling

1. Quit ZKPnote
2. Drag `/Applications/ZKPnote.app` to Trash
3. (Optional) remove the vault data at `~/Library/Application Support/com.zkpnote.desktop`

That removes everything, including your encrypted notes. **Don't do this unless you've exported or pushed to the web first.**

---

## Roadmap

- Signed + notarized build (kills the Gatekeeper warning)
- Windows + Linux binaries
- Mobile apps (iOS/Android)
- Attachment sync with the web app

Thanks for trying the beta. Your feedback shapes the release.
