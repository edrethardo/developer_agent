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
3. Open Claude Code and say hello — in whatever language you prefer. The assistant asks
   a few short questions and sets everything up. That's it.

The first question it asks is which of these you want:

- **This folder becomes your tool.** Best if you are starting something new — the
  assistant sets it up here and you work in this folder from now on. Want a second tool
  later? Get a fresh copy of the kit for it.
- **You already have a project, and you want to work this way in it.** The assistant
  installs the setup into that project instead — carefully, on a separate branch,
  keeping everything you already have. If anything of yours would be touched, it asks
  first and tells you honestly what each choice costs. Nothing is overwritten behind
  your back, and you can throw the whole thing away with one command until you are
  happy with it.

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
- **You never need the terminal.** If anything ever seems to require it, say so — that's
  a bug in this starter kit, not something you're supposed to do. One honest exception:
  if your computer doesn't yet have "git" (the change-history program this kit depends
  on), the assistant will walk you through installing that one program — a few clicks in
  a normal installer, no terminal.
