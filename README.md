# EdTech Books Connector

Connects Claude directly to [EdTech Books](https://books.byui.edu), so it can
search the catalogue and read any book, chapter, or page while you work.

It **reads only**. It cannot change, publish, or delete anything on the site,
and it touches no student data.

> **This is a pilot.** Windows is the tested path. The Mac build is new and
> has had far less use — if you are on a Mac, expect the odd rough edge and
> please say so. Awkward moments are the point of running a pilot.

## Install

**[Download from the latest release →](../../releases/latest)**

| Your computer | File to download |
| --- | --- |
| Windows | `byui-books-mcp.exe` |
| Mac (Apple Silicon: M1/M2/M3/M4) | `byui-books-mcp.command` |

You need Claude Desktop installed and opened at least once. If you also use
Claude Code, it will be set up automatically. Nothing else is required — the
file is entirely self-contained, about 86 MB on Windows and 62 MB on a Mac.

There is no Intel Mac build. If you have one, get in touch.

### 1. Download the file

Your browser may warn that the file is not commonly downloaded. Choose
**Keep** — you may need the **…** menu beside the warning to find it.

### 2. Open it

**On Windows,** double-click it.

Windows shows a blue box reading *"Windows protected your PC."* Click
**More info**, then **Run anyway**.

**On a Mac,** there is one command first, because downloaded files arrive
without permission to run.

1. Open **Terminal** (press Cmd+Space, type "Terminal").
2. Type `chmod +x` followed by a space, then drag `byui-books-mcp.command`
   from your Downloads folder onto the Terminal window — that fills in its
   location for you. Press **Enter**. Nothing visible happens, which is right.
3. Now **right-click** the file in Finder and choose **Open**. Do not just
   double-click it; the first time, that gives a dead end with no way through.
4. If macOS still refuses, open **System Settings → Privacy & Security**,
   scroll down to the message about `byui-books-mcp`, and click
   **Open Anyway**. Then repeat step 3.

These warnings appear because this program has not been signed with a paid
commercial certificate. That is a statement about paperwork, not about safety —
both systems show the same warning for any unsigned program. If you would
rather not click through it, get in touch and we will set you up another way.

### 3. Let it finish

A window opens and lists what it set up. It waits for you — press **Enter** to
close it. If it seems to sit there doing nothing, that is the window waiting,
not a freeze.

### 4. Nothing — Claude restarts itself

If Claude was open, the installer closes it before changing its settings and
opens it again when it is done. You do not have to quit anything.

That step exists for a real reason. Claude keeps its settings in memory and
rewrites the whole settings file every time you change a preference, so an entry
added while Claude is running is discarded a few minutes later, with nothing to
tell you it happened. Closing Claude first is the only way to make it stick.

**If the installer says it could not close Claude,** quit it yourself — Cmd+Q on
a Mac, or right-click the system-tray icon and choose **Quit** on Windows — and
run the installer again. It deliberately writes nothing in that case, because
what it wrote would not last.

## Check it worked

Ask Claude:

> What books are available on EdTech Books?

A good answer lists real titles — *BIO 180*, *Invertebrate Life*, *Advanced
Writing*, and others.

If Claude says it has no way to look that up, check what the installer printed.
It ends with a **Settings check** line confirming the entry was still there
after it wrote it. If that line says the entry is missing, Claude was running
and would not close — quit it fully and run the installer again.

**If you installed v0.2.0 or earlier,** you likely hit a bug: the installer
wrote its entry while Claude was running, and Claude discarded it minutes later.
The program installed correctly; only the setting was lost. Download the current
version and run it — the installer now closes Claude first.

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

Built by [Mat Miles](https://github.com/matjmiles) in collaboration with Royce
Kimmons, who created and maintains EdTech Books.

This repository holds the installer and its documentation. The server is built
on the [Model Context Protocol](https://modelcontextprotocol.io), an open
standard for connecting AI assistants to external systems.
