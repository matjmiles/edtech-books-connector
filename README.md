# EdTech Books Connector

Connects Claude directly to [EdTech Books](https://edtechbooks.org), so it can
search the catalogue and read any book, chapter, or page while you work.

It works with **any EdTech Books site** — [books.byui.edu](https://books.byui.edu),
[edtechbooks.org](https://edtechbooks.org), or another instance. The installer
asks which one you want.

It **reads only**. It cannot change, publish, or delete anything on the site,
and it touches no student data.

> **This is a pilot.** Windows is the better-tested path. The Mac build is
> newer and has had far less use — if you are on a Mac, expect the odd rough
> edge and please say so. Awkward moments are the point of running a pilot.
>
> **Already have an earlier version?** Install the current one over it. Nothing
> needs uninstalling first, and the installer tidies up the old entry itself.
> See [Upgrading from an earlier version](#upgrading-from-an-earlier-version)
> for what changed.

## Install

**[Download from the latest release →](../../releases/latest)**

| Your computer | File to download |
| --- | --- |
| Windows | `byui-books-mcp.exe` |
| Mac (Apple Silicon: M1/M2/M3/M4) | `byui-books-mcp.command` |

**You need at least one of Claude Desktop or Claude Code**, and the installer
sets up whichever it finds — both, if you have both. You do not need Claude
Desktop if you work in Claude Code; see
[Using it with Claude Code](#using-it-with-claude-code). Nothing else is
required — the file is entirely self-contained, about 86 MB on Windows and
62 MB on a Mac.

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

### 3. Choose your site

The installer asks which EdTech Books site to connect to:

```
Which EdTech Books site should this connect to?

  1. BYU-Idaho  (https://books.byui.edu)
  2. EdTech Books  (https://edtechbooks.org)
  3. Something else -- type the address

Enter 1-3, or paste a site address:
```

Type **1** or **2** and press Enter. For any other instance, type its address —
`example.edu` is enough, you do not need the `https://`.

There is no default here on purpose. EdTech Books runs several sites, and
picking one for you would quietly decide whose books you are reading. If you
pick nothing, the installer stops without changing anything.

You can change your mind later by running the installer again.

### 4. Let it finish

A window lists what it set up. It waits for you — press **Enter** to close it.
If it seems to sit there doing nothing, that is the window waiting, not a
freeze.

### 5. Nothing — Claude restarts itself

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

A good answer lists real titles from the site you chose.

If Claude says it has no way to look that up, check what the installer printed.
It ends with a **Settings check** line confirming the entry was still there
after it wrote it. If that line says the entry is missing, Claude was running
and would not close — quit it fully and run the installer again.

There is nothing to click and no command to remember. Ask ordinary questions
and Claude will reach for the books when they help:

> Pull up the table of contents for Invertebrate Life.
>
> Read me the review questions from chapter 24 and tell me what they miss.
>
> Find where this book explains outer joins.

## Using it with Claude Code

**Claude Code in VS Code needs no separate setup.** The installer registers the
connector with Claude Code at *user scope*, which means one registration covers
every way you run it:

- Claude Code in **VS Code**
- Claude Code in a **terminal**
- Claude Code inside the **Claude Desktop** app
- Claude Code in **JetBrains** IDEs

It also covers every folder you open. You do not register it per project, and
you do not need a `.vscode/mcp.json` — that file is for VS Code's own MCP
support, which is a different thing from Claude Code and does not read this.

**You do not need Claude Desktop.** If you only use Claude Code, the installer
prints `Claude Desktop not found -- skipped.` and finishes normally. That line
is information, not a failure.

### Making it appear

**Claude Code reads its connector list once, at startup.** So after installing:

1. Close VS Code completely — every window, not just the Claude panel.
2. Open it again.
3. Start a new Claude Code conversation.

An existing conversation will not pick it up, which is the single most common
reason someone thinks the install failed.

### Checking it worked

In a Claude Code conversation, type:

```
/mcp
```

You should see **edtech-books** listed as connected. Or just ask a question and
watch what happens:

> What books are available on EdTech Books?

If you have a terminal handy, this lists every registered connector and whether
each one is answering:

```sh
claude mcp list
```

Look for `edtech-books: ... ✔ Connected`.

### If it does not appear

**Check you restarted.** Genuinely the usual cause. A new conversation in an
already-open window is not enough — VS Code has to start again.

**Look for an older entry fighting it.** If you registered a copy by hand at
any point, an out-of-date entry may still be there and failing. List them:

```sh
claude mcp list
```

Anything named `byui-books` is from before the connector was renamed, and will
not start. Remove it:

```sh
claude mcp remove byui-books --scope user
```

If a stale entry was added for one project rather than globally, `claude mcp
list` shows it while you have that folder open, and removing it needs
`--scope local` instead. A failing entry is harmless beyond the error message,
but it is worth clearing so a real failure is not hidden among them.

**Check the binary is still where it was installed.** The registration records
a path. If you moved or deleted the downloaded file after installing, run the
installer again — it is safe to re-run and refreshes the path.

## Where things went

Two separate things happened, and it helps to keep them apart.

**The program.** One file, copied somewhere permanent so the download becomes
disposable:

- **Windows** — `C:\Users\<you>\AppData\Local\byui-books-mcp\`
- **Mac** — `~/Library/Application Support/byui-books-mcp/`

That single file is the whole thing. There is no folder of parts to look after,
and nothing was added to Claude's own program folder.

**A note telling Claude where it is.** Claude has to be told where that file
lives, so one entry named `edtech-books` was added to Claude's settings, along
with the site you chose. The first time only, your original settings were saved
alongside as `claude_desktop_config.json.bak`. Any other connections you already
had were left alone.

That entry is only a location, not a second copy of the program. When you ask
about a book, Claude reads the location, starts the program, and puts the
question to it.

## If it didn't work

**Claude says it can't look that up.** Claude Desktop was not fully quit and
restarted. Quit it from the system tray, not the window, and reopen.

**The installer said "Claude Desktop not found."** Either it is not installed,
or it is installed but has never been opened. Launch Claude Desktop once, quit
it, then run the installer again.

**The installer said no site was chosen.** It needs to know which EdTech Books
site to use and will not guess. Run it again and pick one.

**Claude finds the connector but every question fails.** Check the site is
right — run the installer again and choose. If you pointed it at an address
that is not an EdTech Books site, nothing will answer.

**Nothing happened when you ran the file.** Your antivirus may have quarantined
it, since it is unsigned. Check the quarantine list, or ask IT to allow it.

**Still stuck.** [Open an issue](../../issues) describing what you saw —
especially anything the installer window printed. This is a pilot, and awkward
moments are the point of running one.

## Upgrading from an earlier version

**Just run the current installer.** It replaces the program, asks which site to
use, and removes the old settings entry for you.

Two things changed in v0.4.0 that matter if you installed anything earlier:

- **The settings entry was renamed** from `byui-books` to `edtech-books`, because
  the connector works with every EdTech Books site rather than one of them. The
  installer removes the old entry; left behind it would sit there pointing at a
  program that no longer runs.
- **The site is now recorded explicitly.** Earlier versions always used
  books.byui.edu. The installer asks, and writes your answer into the settings
  entry.

**An earlier install will stop working until you re-run the installer.** That is
expected, and re-running is the whole fix.

If you would rather repair it by hand, quit Claude completely first — editing
the file while Claude is running is what causes entries to vanish — then open:

- **Mac** — `~/Library/Application Support/Claude/claude_desktop_config.json`
- **Windows** — `%APPDATA%\Claude\claude_desktop_config.json`

Remove any `byui-books` entry, and add this inside `mcpServers`, leaving
everything else alone:

```json
{
  "mcpServers": {
    "edtech-books": {
      "command": "PUT THE PATH BELOW HERE",
      "args": [],
      "env": { "BOOKS_BASE_URL": "https://books.byui.edu" }
    }
  }
}
```

Set `BOOKS_BASE_URL` to the site you want. The program path is:

- **Mac** — `/Users/YOURNAME/Library/Application Support/byui-books-mcp/byui-books-mcp`
- **Windows** — `C:\Users\YOURNAME\AppData\Local\byui-books-mcp\byui-books-mcp.exe`

Write the path out in full — a `~` shortcut will not work here. On Windows, the
backslashes must be doubled in JSON: `C:\\Users\\YOURNAME\\...`.

Save, then open Claude.

**On a Mac, one extra step may be needed.** Files downloaded through a browser
carry a flag that can stop macOS running them, and versions before v0.3.0 did
not clear it. In Terminal:

```sh
xattr -d com.apple.quarantine ~/Library/Application\ Support/byui-books-mcp/byui-books-mcp
```

It prints nothing if the flag was already gone, which is fine.

## Updating and removing it

**A newer version.** Quit Claude Desktop first, then run the new file the same
way. If you forget, the installer says so and leaves your working setup alone.

**Changing which site it uses.** Run the installer again and pick a different
one.

**Removing it.** Quit Claude Desktop, then delete the folder above. Then remove
the `edtech-books` entry from Claude's settings, leaving the rest of the file
alone. Nothing else is left behind.

Don't replace the whole settings file with the `.bak` copy unless you are sure
nothing else has changed since — that snapshot predates any other connections
you may have added.

## What it can do

Fourteen read-only tools, covering the catalogue and its contents:

| | |
| --- | --- |
| Find things | Search books, chapters, pages and authors by title; search keywords |
| Search inside a book | Find a phrase in a book's chapters, with an excerpt of each match |
| Books | List the catalogue, read a book's contents and its glossary |
| Read | Fetch any chapter or page by id, or resolve one from its web address |
| Authors | Read an author's public profile and the books they have written |

Searching **by title** and searching **inside a book** are different tools, and
the difference catches people out. A phrase from the middle of a chapter will
not be found by the catalogue search — ask about a book by name first, then
search within it.

## About

Built by [Mat Miles](https://github.com/matjmiles) in collaboration with Royce
Kimmons, who created and maintains EdTech Books.

This repository holds the installer and its documentation. The server is built
on the [Model Context Protocol](https://modelcontextprotocol.io), an open
standard for connecting AI assistants to external systems.
