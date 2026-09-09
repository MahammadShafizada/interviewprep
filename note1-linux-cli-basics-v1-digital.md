# Linux CLI Fundamentals — Digital Notes (Version 1)

**Domain tag:** Linux CLI Fundamentals — navigation, file search, system info

## 1. Big Picture
- CLI = how you control Linux via text commands instead of GUI clicks.
- Core skill for security work — most tools/servers only reachable via terminal.

## 2. Core Concepts
- **CLI (Command Line Interface)** — text-based OS control, faster + more powerful than GUI, required for most security tooling.
- **pwd** — "print working directory," shows current location in filesystem, first check when lost.
- **ls / ls -l / ls -al** — lists contents; `-l` adds detail (perms, size, date); `-al` also reveals hidden files.
- **Hidden files (dotfiles)** — filenames starting with `.`, hidden by default, not actually secret, just convention.
- **cd** — change directory; `cd <name>` moves in, `cd ..` moves up one level.
- **find** — searches filesystem for files; `find <start> -name <filename>`; returns full path; can be slow on large dirs.
- **cat** — prints file contents to screen; fastest way to read small text files.
- **uname -a** — one-line dump of kernel, hostname, architecture, OS type; `uname` alone = just OS name.
- **df -h** — disk usage in human-readable form (G/M not raw bytes).
- **/etc + os-release** — `/etc` holds system config files; `cat /etc/os-release` gives clean distro name/version, more readable than uname.

## 3. Terms That Get Confused
- **pwd vs cd** — pwd tells you where you ARE, cd MOVES you. ("print" = read-only, "change" = action)
- **ls vs -l vs -al** — plain = names, `-l` = detail columns, `-al` = detail + hidden. (more letters = more info)
- **uname -a vs os-release** — uname = kernel/hardware focus, os-release = distro/version focus.
- **cat vs find** — find LOCATES a file (path), cat READS a file (contents).
- **whoami vs uname** — whoami = which USER, uname = which SYSTEM.

## 4. Interview Drills

**Q: "What does CLI stand for, and why do security pros use it over a GUI?"**
→ "CLI stands for Command Line Interface — it's just typing commands instead of clicking through menus. Security folks lean on it because it's faster, scriptable, and honestly a lot of security tools only run in the terminal anyway, there's no GUI version."

**Q: "How would you find a file if you don't know where it lives on the system?"**
→ "I'd use `find`, something like `find ~ -name filename`, which searches recursively from your home directory and prints the full path once it locates it."
*Follow-up: "What if that search takes forever?"*
→ "I'd narrow the starting point instead of searching from root — searching from `/` on a big system can take a while, so pointing it at a more specific folder speeds things up a lot."

**Q: "How do you check disk space on a Linux machine?"**
→ "I'd run `df -h` — the `-h` gives you human-readable sizes like gigs and megs instead of raw byte counts, and it shows you each mounted filesystem and how full it is."

**Q: "What's the difference between the kernel and the distro?"**
→ "The kernel's the core engine of the OS — that's what `uname -a` shows you the version of. The distro is the whole packaged experience built around that kernel, like Ubuntu, with its own tools and defaults."
*Follow-up: "Where would you check that without any GUI?"*
→ "I'd just cat out `/etc/os-release` — it's a plain text file every distro ships with that spells out the name and version cleanly."

**Q: "Are hidden dotfiles a security feature?"**
→ "Not really, no — it's just a display convention so your home folder doesn't look cluttered. Anyone can reveal them with `ls -a`, so I wouldn't rely on that for anything actually sensitive."

## 5. One-Line Memory Anchors
- CLI → steering wheel vs autopilot, direct manual control.
- pwd → "you are here" map marker.
- ls -al → "show me everything, even the secrets."
- cd .. → go back up the stairs.
- find → GPS for files.
- cat → open the book and read it.
- uname -a → machine's ID card.
- df -h → gas gauge for disk space.
- /etc/os-release → machine's business card.
