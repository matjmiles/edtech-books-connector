# EdTech Books Connector

Connects Claude directly to [EdTech Books](https://books.byui.edu), so it can
search the catalogue and read any book, chapter, or page while you work.

It **reads only**. It cannot change, publish, or delete anything on the site,
and it touches no student data.

> **This is a pilot.** Windows only for now; macOS is being tested and will
> follow. If something goes wrong, that is useful — please say so.

## Install

**[Download `byui-books-mcp.exe` from the latest release →](../../releases/latest)**

You need Claude Desktop installed and opened at least once. If you also use
Claude Code, it will be set up automatically. Nothing else is required — the
file is entirely self-contained, about 86 MB.

### 1. Download the file

Your browser may warn that the file is not commonly downloaded. Choose
**Keep** — you may need the **…** menu beside the warning to find it.

### 2. Double-click it

Windows shows a blue box reading *"Windows protected your PC."* Click
**More info**, then **Run anyway**.

Both warnings appear because this program has not been signed with a paid
commercial certificate. That is a statement about paperwork, not about safety —
Windows shows the same warning for any unsigned program. If you would rather
not click through it, get in touch and we will set you up another way.

### 3. Let it finish

A window opens and lists what it set up. It waits for you — press **Enter** to
close it. If it seems to sit there doing nothing, that is the window waiting,
not a freeze.

### 4. Quit Claude completely, then reopen it

Closing the Claude window is **not** enough. It keeps running in the background.

Find the Claude icon in the system tray, near the clock at the bottom-right of
your screen — you may need to click the small arrow to reveal hidden icons.
Right-click it, choose **Quit**, then open Claude again from the Start menu.

**Nearly every report of "it didn't work" turns out to be this step.** Claude
only notices the new connection when it starts up fresh.

## Check it worked

Ask Claude:

> What books are available on EdTech Books?

A good answer lists real titles — *BIO 180*, *Invertebrate Life*, *Advanced
Writing*, and others. If Claude says it has no way to look that up, go back to
step 4.

There is nothing to click and no command to remember. Ask ordinary questions
and Claude will reach for the books when they help:

> Pull up the table of contents for Invertebrate Life.
>
> Read me the review questions from chapter 24 and tell me what they miss.

## Where things went

Two separate things happened, and it helps to keep them apart.

**The program.** One file, copied somewhere permanent so the download becomes
disposable:

```
C:\Users\<you>\AppData\Local\byui-books-mcp\
```

That single file is the whole thing. There is no folder of parts to look after,
and nothing was added to Claude's own program folder.

**A note telling Claude where it is.** Claude has to be told where that file
lives, so one entry named `byui-books` was added to Claude's settings. The
first time only, your original settings were saved alongside as
`claude_desktop_config.json.bak`. Any other connections you already had were
left alone.

That entry is only a location, not a second copy of the program. When you ask
about a book, Claude reads the location, starts the program, and puts the
question to it.

## If it didn't work

**Claude says it can't look that up.** Claude Desktop was not fully quit and
restarted. Quit it from the system tray, not the window, and reopen.

**The installer said "Claude Desktop not found."** Either it is not installed,
or it is installed but has never been opened. Launch Claude Desktop once, quit
it, then run the installer again.

**Nothing happened when you ran the file.** Your antivirus may have quarantined
it, since it is unsigned. Check the quarantine list, or ask IT to allow it.

**Still stuck.** [Open an issue](../../issues) describing what you saw —
especially anything the installer window printed. This is a pilot, and awkward
moments are the point of running one.

## Updating and removing it

**A newer version.** Quit Claude Desktop first, then run the new file the same
way. If you forget, the installer says so and leaves your working setup alone.

**Removing it.** Quit Claude Desktop, then delete the folder above. Then remove
the `byui-books` entry from Claude's settings, leaving the rest of the file
alone. Nothing else is left behind.

Don't replace the whole settings file with the `.bak` copy unless you are sure
nothing else has changed since — that snapshot predates any other connections
you may have added.

## What it can do

Thirteen read-only tools, covering the catalogue and its contents:

| | |
| --- | --- |
| Find things | Search books, chapters, pages and authors; search keywords |
| Books | List the catalogue, read a book's contents and its glossary |
| Read | Fetch any chapter or page by id, or resolve one from its web address |
| Authors | Read an author's public profile and the books they have written |

## About

Built by [Matt Miles](https://github.com/matjmiles) in collaboration with Royce
Kimmons, who created and maintains EdTech Books.

This repository holds the installer and its documentation. The server is built
on the [Model Context Protocol](https://modelcontextprotocol.io), an open
standard for connecting AI assistants to external systems.
