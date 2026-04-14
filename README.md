# linux-study-vault

Personal notes on Linux internals, written while studying for the LFCA certification. Not a tutorial. Not a wiki. Just notes — the kind you write when you're trying to actually understand something, not just memorize it.

Built as an Obsidian vault. Open the `linux-notes/` folder as a vault.

## Structure

```
linux-notes/
  00-index/               entry points and maps
  01-big-picture/         abstraction layers, kernel overview, Unix history
  02-commands-and-shell/
  03-devices/
  04-disks-and-filesystems/
  05-kernel-boot/
  06-user-space/
  07-system-configuration/
  08-processes-and-resources/
  09-networking/
  10-network-services/
  11-shell-scripting/
  12-moving-files/
  13-user-environments/
  14-linux-desktop/
  15-development-tools/
  16-compiling-software/
  17-advanced-topics/
  _templates/             note template
  _meta/                  local style guide (gitignored)
```

## Plugins

The vault uses a few community plugins. Install them through Obsidian → Settings → Community Plugins → Browse:

- **Dataview** — query notes as a database
- **Templater** — smarter templates
- **Excalidraw** — diagrams
- **Advanced Canvas** — whiteboard / flowcharts
- **Mermaid Tools** — diagram toolbar
- **Style Settings** — theme customization
- **Spaced Repetition** — flashcard review from exam-note sections

Plugin manifests are versioned so Obsidian knows what to install. The compiled plugin files are gitignored — you'll need to reinstall them after cloning.

## Notes format

Each note has minimal frontmatter (title, tags, related), prose where the topic has explanation to it, diagrams where the concept genuinely needs a visual, and an `exam-note` section at the bottom flagging anything specifically tested on the LFCA.
