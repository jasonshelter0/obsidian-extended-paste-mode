## Extended Paste Mode

An [Obsidian](https://obsidian.md/) plugin to paste and manage text, including block quotes, at any level of indentation.

> **This is a fork** of [obsidian-paste-to-current-indentation](https://github.com/jglev/obsidian-paste-to-current-indentation) by [Jacob Levernier](https://github.com/jglev). All credit for the original plugin design, features, and years of maintenance goes to Jacob. This fork applies bug fixes and code optimizations.

### What this fork changes

**Bug fixes (v1.0.0):**
- **Fixed**: Default paste (Passthrough mode and disabled plugin) was completely broken — `evt.preventDefault()` was called *before* checking the mode, blocking native paste even in Passthrough mode.
- **Fixed**: The `editor-paste` event handler was not registered with the plugin's component lifecycle, so disabling the plugin did not actually stop it from intercepting paste — paste stayed broken until restarting Obsidian.

**Code optimizations (v1.0.0):**
- Removed `moment` (replaced with native `Date`) and `lodash.clonedeep` (unused) — **50% bundle size reduction** (282 KB → 142 KB).
- Extracted shared helpers: `ensureFolder()`, `resolveLocation()`, `escapeBlockquoteCharacters()`.
- Replaced 15+ repeated `Object.values(Mode)` / `Object.keys(Mode)` calls with module-level `MODE_VALUES` / `MODE_ENTRIES` constants.
- Hoisted regexes out of loops in `toggleQuote()`; made `toggleQuote` synchronous.
- Simplified `PasteModeModal.getItems()`, `cycle-paste-mode`, command registration loops, `checkCallback`, and 20+ other redundant patterns.

---

### Installation

#### Via BRAT (recommended)

This plugin is not in the Obsidian community plugin store, but you can install and auto-update it using the [BRAT](https://github.com/TfTHacker/obsidian42-brat) (Beta Reviewers Auto-update Tester) plugin:

1. Install **BRAT** from the Obsidian community plugin store and enable it.
2. Open the Command Palette (`Ctrl/Cmd + P`) and run **`BRAT: Add a beta plugin for testing`**.
3. Paste the repository URL:  
   `https://github.com/jasonshelter0/obsidian-extended-paste-mode`
4. BRAT will download `main.js`, `manifest.json`, and `styles.css` into your vault's `.obsidian/plugins/obsidian-extended-paste-mode/` folder.
5. Go to **Settings → Community plugins**, find **Extended Paste Mode**, and enable it.

BRAT will check for updates automatically whenever you restart Obsidian.

#### Via GitHub Release

1. Download `main.js`, `manifest.json`, and `styles.css` from the [latest release](https://github.com/jasonshelter0/obsidian-extended-paste-mode/releases).
2. Create a folder in your vault: `.obsidian/plugins/obsidian-extended-paste-mode/`
3. Place the three files inside that folder.
4. Restart Obsidian (or reload plugins via the Command Palette).
5. Go to **Settings → Community plugins**, find **Extended Paste Mode**, and enable it.

To update later, repeat the steps above with the new release files.

---

### Paste modes

Paste Mode takes over paste functionality within Obsidian. It has seven paste modes, which determine what happens when pasting text within a file. **All modes honor the cursor's current indentation when pasting, except "Passthrough" mode, which uses Obsidian's default paste behavior.**

![Demonstration of paste modes](img/all-paste-modes.gif)

1. **Text** — Paste clipboard text as-is.
1. **Text (Blockquote)** — Paste with blockquote prefix (default `> `, configurable).
1. **Markdown** — Convert HTML to Markdown before pasting.
1. **Markdown (Blockquote)** — Convert HTML to Markdown, then wrap in blockquote.
1. **Code Block** — Paste within ` ``` ` code fences.
1. **Code Block (Blockquote)** — Paste within code fences, then wrap in blockquote.
1. **Passthrough** — Use Obsidian's default paste behavior.

![Status bar](img/status-bar.png)

#### Switching modes

1. Click the status bar indicator to open a searchable mode picker.
1. `Paste Mode: Cycle Paste Mode` in the Command Palette cycles through all modes.
1. `Paste Mode: Set Paste Mode to <mode>` commands for direct switching (bindable via, e.g., Quick Add).
1. Plugin settings tab.

#### Limitations

- In Obsidian Mobile, "Markdown" and "Markdown (Blockquote)" one-time paste commands are disabled due to clipboard API restrictions.
- Similarly, images/screenshots cannot be pasted from the clipboard on mobile.

### Additional commands

- **`Paste Mode: Toggle blockquote at current indentation`** — toggles blockquote markers on the selected text.  
  ![Toggle blockquote](img/toggle-blockquote.gif)

### Additional features

- **Dynamic attachment saving** — route pasted files to different folders based on the current note's location.  
  ![](img/attachment_location_overrides.png)
- **Download linked files** — when pasting Markdown, files referenced via `http://` or `file://` URLs can be downloaded locally.
- **Automatic character escaping** — escape Markdown-sensitive characters (`==`, `<`, etc.) in blockquotes.

### Developing

```bash
npm install
npm run dev    # watch mode
npm run build  # production build
```

### Releasing

1. Update `manifest.json` and `versions.json` with the new version number.
2. Run `npm run build` to produce `main.js`.
3. Create a new [GitHub release](https://github.com/jasonshelter0/obsidian-extended-paste-mode/releases) with the version number as the tag (e.g., `1.0.0`).
4. Upload `main.js`, `manifest.json`, and `styles.css` as binary attachments to the release.
5. Publish. BRAT users will pick up the update on next restart.

### License & Credits

This project is a fork of [obsidian-paste-to-current-indentation](https://github.com/jglev/obsidian-paste-to-current-indentation) by **Jacob Levernier** (<j@adunumdatum.org>), who designed and maintained the original plugin through 5 major versions. All credit for the plugin concept, feature design, and years of development belongs to him.

This fork is distributed under the same **BSD 3-Clause License** as the original. See [LICENSE](LICENSE) for the full text.

```
Copyright 2021 Jacob Levernier <j@adunumdatum.org>
```
```
Modifications (2026) Jason Shelter
```
