# linux-study-vault

Personal notes on Linux internals, written while studying for the LFCA certification. Not a tutorial. Not a wiki. Just notes - the kind you write when you're trying to actually understand something, not just memorize it.

Built as an Obsidian vault. Open the `linux-notes/` folder as a vault.

## What's written so far

One section: **01 - big picture**, six notes in `linux-notes/01-big-picture/`.

| Note | What it covers |
| --- | --- |
| [Levels and Layers of Abstraction in a Linux System](linux-notes/01-big-picture/levels-of-abstraction.md) | The stack from user programs down to hardware, and the system call line between user space and the kernel. |
| [Kernel: Process Management, Memory Management, Device Drivers](linux-notes/01-big-picture/kernel-overview.md) | The kernel's three jobs, and why Linux is a monolithic kernel with loadable modules. |
| [User Space vs Kernel Space](linux-notes/01-big-picture/user-space-vs-kernel-space.md) | Memory layout, what isolation means in practice, kernel panics. |
| [System Calls and Support](linux-notes/01-big-picture/system-calls.md) | What happens during a syscall, the ones worth knowing, glibc, strace. |
| [Users](linux-notes/01-big-picture/users-and-root.md) | UIDs, root as UID 0, file permissions, sudo. |
| [UNIX, MINIX, Distros](linux-notes/01-big-picture/unix-history.md) | Bell Labs, Minix and the 386, what a distribution is, POSIX. |

New notes start from [`linux-notes/_templates/note-template.md`](linux-notes/_templates/note-template.md).

Links between notes are Obsidian wikilinks (`[[system-calls]]`), so they work inside the vault but not on GitHub.

## Planned sections

Not written yet. Each one becomes a folder under `linux-notes/` when its first note lands.

```
00-index/                    entry points and maps, once there is more than one section
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
```

## Plugins

Two community plugins are enabled in the vault (`linux-notes/.obsidian/community-plugins.json`):

- **Templater** - smarter templates
- **Style Settings** - theme customization

Manifests for five more are versioned, but they are not enabled yet:

- **Dataview** - query notes as a database
- **Excalidraw** - diagrams
- **Advanced Canvas** - whiteboard / flowcharts
- **Mermaid Tools** - diagram toolbar
- **Spaced Repetition** - flashcard review from exam-note sections

Install them through Obsidian → Settings → Community Plugins → Browse. The manifests tell Obsidian which plugins and versions to install; the compiled plugin files are gitignored, so you'll need to reinstall them after cloning.

## Notes format

Each note has minimal frontmatter (title, tags, related), prose where the topic has explanation to it, Mermaid diagrams where the concept genuinely needs a visual, an `exam-note` section flagging anything specifically tested on the LFCA, and a closing list of related notes.

## License

These notes are licensed under [Creative Commons Attribution 4.0 International (CC BY 4.0)](LICENSE):
you can share and adapt them as long as you give credit.

## Dónde más viven estas notas

La sección **01 - big picture** de estas notas se adaptó y tradujo al español
en Open Security Labs, como el lab
[Del comando al kernel: capas, syscalls y strace](https://securitylabs.valentorassa.com/labs/linux-real/del-comando-al-kernel/)
(fuente en
[`Open-Security-Labs`](https://github.com/ValentinTorassa/Open-Security-Labs/blob/main/src/content/labs/linux-real/del-comando-al-kernel.mdx)).
Ahí también está la [página de la certificación LFCA](https://securitylabs.valentorassa.com/certificaciones/linux-foundation-lfca/),
que enlaza los labs que preparan cada dominio del examen.

Este repo sigue siendo el vault de estudio original. Las notas están bajo
CC BY 4.0 y el lab mantiene la atribución.
