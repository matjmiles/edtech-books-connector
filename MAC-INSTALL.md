# Installing the EdTech Books connector on a Mac

The short version is in the [README](README.md). This page is for when that
isn't enough — it covers every point macOS can stop you, and what to type at
each one.

Nothing here is risky. macOS is cautious about programs it hasn't seen before,
and this one has not been signed with a paid Apple certificate. Every warning
below is about that missing paperwork, not about the program.

## Before you start: which Mac?

**This build runs on Apple Silicon only** — any Mac with an M1, M2, M3 or M4
chip. That is every Mac sold since late 2020.

On an **Intel** Mac it will not run at all, and the error is unhelpful. To
check: **Apple menu → About This Mac**. If the Chip line says "Apple M-something"
you are fine. If it says "Processor: Intel", get in touch and we will sort
something out rather than have you fight it.

Also worth knowing before you spend time: the Mac file is built on a Windows
machine, and **nobody runs that exact file before it ships** — there is no Mac
in the build process. Earlier Mac builds were tested by hand, and the problems
found then are fixed and described below. But each new release goes out
unexercised on a Mac. If something misbehaves in a way this page does not
describe, it is more likely the build than your setup. Please say so.

## 1. Download it

Take **`byui-books-mcp.command`** from the
[latest release](https://github.com/matjmiles/edtech-books-connector/releases/latest).

Not the `.exe` — that one is Windows.

**Your browser may refuse to keep it.** Chrome and Edge say the file "is not
commonly downloaded" and hide the option to keep it behind a **⋯** menu beside
the warning. Choose **Keep**. Safari usually downloads it without comment.

This happens before macOS has said anything at all. It is the browser reacting
to an unsigned file.

## 2. Give it permission to run

Files arriving through a browser have no permission to run. One command fixes
that.

Open **Terminal** — press **Cmd+Space**, type `Terminal`, press Enter.

Then type this, **with a space after `+x`**, and don't press Enter yet:

```sh
chmod +x 
```

Now **drag `byui-books-mcp.command` from your Downloads folder onto the Terminal
window.** That fills in the file's location for you. *Now* press Enter.

Nothing visible happens. That is correct — the command succeeds silently.

> **Why drag it instead of typing the path?** Because dragging works wherever
> the file is, and copes with spaces in folder names. If you know the file is in
> Downloads and you would rather type it, this is the same thing:
>
> ```sh
> chmod +x ~/Downloads/byui-books-mcp.command
> ```

## 3. Open it — right-click, not double-click

In **Finder**, **right-click** (or Control-click) `byui-books-mcp.command` and
choose **Open**.

**Do not double-click it.** The first time, double-clicking gives a dead end:
macOS refuses and offers no way to continue. Right-clicking and choosing Open
offers a **Open** button on the warning. This is the step people miss, and it
looks like the program is broken when you hit it.

A Terminal window opens and the installer starts asking questions. Skip to
step 4.

### If macOS still refuses

Two ways through. Either works.

**The clicking route.** Open **System Settings → Privacy & Security**, scroll
down — it is below the list of permissions — and click **Open Anyway** next to
the message about `byui-books-mcp`. Then repeat the right-click → Open above.

**The Terminal route.** This removes the "downloaded from the internet" flag
that Gatekeeper is reacting to:

```sh
xattr -d com.apple.quarantine ~/Downloads/byui-books-mcp.command
```

It prints nothing if the flag was already gone, which is fine. Then
double-clicking works normally.

If the file is not in Downloads, use the drag trick from step 2: type
`xattr -d com.apple.quarantine ` (with the trailing space) and drag the file on.

## 4. Answer two questions

**Which site?** `books.byui.edu` or `edtechbooks.org`. Type the one you use.
There is no default on purpose — pointing it at the wrong site looks exactly
like working.

**An API key?** Press **Enter** to skip. Skipping is a real answer, not a
failure: you get fourteen read-only tools over published content, which is what
most people want. A key only matters if you are editing your own books.

## 5. Let it close Claude

If Claude Desktop is open, the installer quits it, writes the setting, checks
the setting survived, and opens Claude again. You do not need to do anything.

That is not politeness. **Claude Desktop keeps its settings in memory and
rewrites the whole file whenever you change a preference** — so an entry added
while it is running is discarded a few minutes later, after the installer has
already said it succeeded. Closing it first is the only way to make the change
stick.

**If the installer says it could not close Claude**, quit it yourself with
**Cmd+Q** and run the installer again. It deliberately writes nothing in that
case, because what it wrote would not survive. An honest refusal beats a setting
that silently disappears.

## 6. Check it worked

Ask Claude:

> What books are available on EdTech Books?

A good answer lists real book titles.

**If Claude says it has no way to look that up**, read the installer's own
output before anything else. It prints a **Settings check** line confirming the
entry was still there after it wrote it. If that line is missing, or says the
entry is gone, Claude Desktop was running and would not close — quit it fully
and run the installer again.

## Where things ended up

**The program** — one file, at:

```
~/Library/Application Support/byui-books-mcp/byui-books-mcp
```

That single file is the whole thing. The copy in Downloads is now disposable.

**A note telling Claude where it is** — one entry named `edtech-books` in
Claude's settings, with the site you chose. The first time only, your original
settings are saved alongside as `claude_desktop_config.json.bak`. Any other
connections you already had are untouched.

## If something is still wrong

**"Nothing happened when I opened it."** Check your antivirus, if you run one —
unsigned programs get quarantined by some of them. Also check you did step 2;
without it, nothing happens and nothing explains why.

**The connector worked and then stopped.** Most likely Claude Desktop rewrote
its settings and dropped the entry. This was a real bug in **v0.2.0** and is
fixed from **v0.3.0** onwards; if you are on v0.2.0, update. Otherwise, run the
installer again with Claude fully quit.

**macOS blocks the installed program, not the installer.** Only relevant if you
installed **v0.2.0**, which left the quarantine flag on the copy it made.
Every version from **v0.3.0** clears it automatically. To clear it by hand:

```sh
xattr -d com.apple.quarantine ~/Library/Application\ Support/byui-books-mcp/byui-books-mcp
```

Note the backslash before the space in `Application Support` — it is required.

You should not need to `chmod` that copy. It inherits permission to run from the
file you fixed in step 2.

**Still stuck.** [Open an issue](https://github.com/matjmiles/edtech-books-connector/issues)
describing what you saw, and paste anything the installer window printed. This
is a pilot; awkward moments are the point of running one.

## Updating, changing site, removing

**A newer version.** Quit Claude Desktop, then run the new file the same way —
steps 2 and 3 again, because it is a fresh download with the same flags. It
replaces the old one in place.

**A different site.** Run the installer again and pick the other one.

**Removing it.** Quit Claude Desktop, delete
`~/Library/Application Support/byui-books-mcp/`, then remove the `edtech-books`
entry from Claude's settings and leave the rest of that file alone.

Do not restore the whole `.bak` file unless you are certain nothing else has
changed since — that snapshot predates any other connections you have added.
