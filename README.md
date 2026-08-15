# Your Tool, Built by Talking

This folder is a starter kit for building your own internal tool by talking to an AI
developer (Claude Code inside VSCode). You describe what you need in plain language; the
AI writes the software, keeps it tidy, documents everything, and asks you when something
needs your judgment.

## What you need

1. **Visual Studio Code** — free, from https://code.visualstudio.com
2. **The "Claude Code" extension** — open VSCode, click the Extensions icon in the left
   bar, search for "Claude Code", click Install. You will need a Claude account with a
   paid plan.

## Getting started

1. Get this folder onto your computer — download it, or clone it if someone showed you
   how. Put it somewhere plain and local (for example `C:\Tools\` on Windows, or a
   folder in your home directory). Avoid Desktop, Documents, or anything synced by
   OneDrive or Dropbox: sync tools fight with the change-history system this kit relies
   on and can quietly corrupt it.
2. Open the folder in VSCode (File → Open Folder…).
3. Open Claude Code and say hello — in whatever language you prefer.

**This folder is the setup, not your workspace.** Your tool never lives in here. The
assistant asks a few short questions and then puts everything your tool needs into a
separate folder of your choosing — after that you work there, and this one can sit
untouched or be deleted.

The first question it asks is which of these you want:

- **A new tool.** You say where it should live (a new folder — the assistant suggests
  one next to this kit), it sets everything up there, and you open that folder and carry
  on. Want a second tool later? Come back here and do it again.
- **A project you already have.** The assistant installs this way of working into that
  project instead — carefully, on a separate branch, keeping everything you already
  have. If anything of yours would be touched, it asks first and tells you honestly what
  each choice costs. Nothing is overwritten behind your back, and you can throw the
  whole thing away with one command until you are happy with it.

## What to expect

- **It asks before doing anything unusual.** When a permission box pops up, read it. If
  you are unsure what it means, just ask: "what does this mean?" — you'll get a plain
  answer before anything happens.
- **Everything is written down.** `CHANGELOG.md` tells you what changed for you, in your
  language. The `docs/` folder holds the manuals. The AI also keeps a work journal for
  itself so it never forgets why something was done.
- **Where this kit comes from.** The original starter kit, its full history and any
  newer version live at https://github.com/edrethardo/developer_agent — useful if you
  ever hand this tool to a professional developer.
- **YOUR OWN FILES ARE NOT SAVED BY THE TOOL.** The assistant keeps a full history of
  the tool it builds — every version, always recoverable. Your data files are
  deliberately kept OUT of that history, so nothing here can restore a spreadsheet you
  deleted or overwrote. **Keep your own copies of anything you care about**, exactly as
  you would with any other folder on your computer.
- **What leaves your computer.** The assistant runs on Anthropic's servers, so any
  file it reads for you is sent there to be processed — the same as pasting it into a
  chat. That is fine for most work files and not fine for some: if something is too
  sensitive to leave the building, don't hand it over. Say what it looks like instead
  ("a spreadsheet with date, amount, cost centre") and the assistant will work from the
  description. Your real files also stay out of the tool's saved history on purpose, so
  they are not carried along when the tool is copied or shared.
- **You never need the terminal.** If anything ever seems to require it, say so — that's
  a bug in this starter kit, not something you're supposed to do. One honest exception:
  if your computer doesn't yet have "git" (the change-history program this kit depends
  on), the assistant will walk you through installing that one program — a few clicks in
  a normal installer, no terminal.
