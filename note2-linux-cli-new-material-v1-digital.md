# Linux CLI — New Material — Digital Notes (Version 1)

**Domain tag:** Linux CLI — commands, operators, SSH, filesystem hierarchy, permissions/users

---

## TOPIC A — New Commands & Operators

### 1. Big Picture
- New commands let you create, search inside, and chain/redirect output of files — not just navigate.

### 2. Core Concepts
- **whoami** — shows current logged-in user, matters because your permissions depend on who you are.
- **echo** — outputs text you give it; multi-word text needs quotes; basis for redirecting output into files.
- **grep** — searches *inside* file contents for a string, e.g. `grep "password" file.txt` — different from finding files by name.
- **touch** — creates a blank empty file, no content until you add it.
- **mkdir** — creates a folder; can nest paths in one go, e.g. `mkdir Docs/HW/Assignment1`.
- **cp** — copies a file/folder to a new name/location; original stays untouched, both now exist.
- **mv** — moves or renames a file/folder; original is gone, only new name/location exists.
- **rm -R** — deletes files or folders; `-R` flag required specifically for folders.
- **& (background operator)** — runs command without waiting, frees up terminal immediately.
- **&& (chain operator)** — runs second command only after first finishes, like dominoes.
- **> (redirect, overwrite)** — sends command output into a file, wipes existing content.
- **>> (redirect, append)** — sends output into a file but adds to the bottom, keeps existing content.

### 3. Terms That Get Confused
- **cp vs mv** — cp duplicates (both files exist after), mv relocates/renames (only one exists after).
- **find vs grep** — find searches by *filename*, grep searches *inside* file content.
- **> vs >>** — overwrite vs append. (`>` = wipe & write, `>>` = add to end)
- **& vs &&** — & = run in background / don't wait, && = run in sequence / wait for first to finish.
- **rm vs rm -R** — plain `rm` only removes files, folders need the `-R` flag or it fails.

### 4. Interview Drills

**Q: "What's the difference between `find` and `grep`?"**
→ "Find searches for files by their name across the filesystem, while grep searches inside a file's actual content for a specific string — they solve different problems even though people mix them up."

**Q: "If I run `cp file1 file2`, what happens to file1?"**
→ "Nothing happens to file1, it stays exactly where it was — cp just creates a duplicate called file2 with the same content, so now you've got two copies."
*Follow-up: "What if I'd used mv instead?"*
→ "With mv, file1 would be gone — it gets renamed or relocated to file2, there's no duplicate left behind."

**Q: "When would you use `>>` instead of `>`?"**
→ "I'd use `>>` when I want to keep appending to a log or file without losing what's already there, like adding new output to the bottom each time. `>` would just overwrite everything that existed."

**Q: "Walk me through removing a directory and its contents."**
→ "I'd run `rm -R foldername` — the `-R` is required for folders, plain `rm` only works on individual files and will error out otherwise."

---

## TOPIC B — Arguments, Flags & Getting Help

### 1. Big Picture
- Commands have default behavior; flags change that behavior; Linux has built-in docs for every command.

### 2. Core Concepts
- **Argument/flag** — extra option added with a hyphen, e.g. `-a`, changes what a command does by default.
- **`--help`** — quick built-in cheat sheet listing a command's available flags with short descriptions.
- **`man <command>`** — full manual page, more detailed than `--help`; scroll with arrow keys, exit with `Ctrl+C`.
- **`ls -a`** — example of a flag in action, reveals hidden dotfiles that plain `ls` skips.

### 3. Terms That Get Confused
- **`--help` vs `man`** — `--help` = quick flag list, `man` = full detailed manual/documentation page.

### 4. Interview Drills

**Q: "How do you figure out what options a command supports?"**
→ "Most commands support `--help`, which gives you a quick rundown of the flags. If I need more detail, I'd check the man page with `man <command>` — that's the full documentation built into Linux."

**Q: "What does adding `-a` to `ls` actually do?"**
→ "It tells ls to also show hidden files — anything starting with a dot, which Linux hides by default just to reduce clutter, not for security."

---

## TOPIC C — Remote Access (SSH)

### 1. Big Picture
- SSH lets you securely log into and control another computer over a network, straight from your terminal.

### 2. Core Concepts
- **SSH (Secure Socket Shell)** — protocol that encrypts communication between two devices so it can't be read in transit.
- **`ssh user@ip`** — command format to connect to a remote machine; needs username + password (or key) for an account on that machine.
- **Password not visible when typing** — standard Linux security convention, not a glitch — just type blind and hit enter.
- **Once connected, commands run remotely** — after SSH login, everything you type executes on the *remote* machine, not your own.

### 3. Terms That Get Confused
- **SSH vs local terminal session** — SSH = controlling a different machine over the network (encrypted); local session = commands run on the machine physically in front of you.

### 4. Interview Drills

**Q: "What is SSH and why does it matter in security?"**
→ "SSH is a protocol for securely logging into another machine over a network — it encrypts everything so nobody snooping on the connection can read your commands or credentials. It's foundational because most servers you'll touch in this field don't have a GUI, so SSH is how you actually get in."

**Q: "Why can't I see my password when I type it into the terminal?"**
→ "That's intentional — Linux hides password input so it can't be shoulder-surfed or show up on screen recordings. You just type it blind and press enter, it's still registering the keystrokes."

---

## TOPIC D — Filesystem Hierarchy & Permissions/Users

### 1. Big Picture
- Linux's filesystem has fixed top-level folders with specific jobs, and access to files is controlled by permissions tied to users/groups.

### 2. Core Concepts
- **`/` (root of filesystem)** — the very top of the entire filesystem tree; everything else branches from here.
- **`/etc`** — system and program config files live here, e.g. the sudoers file (who can run commands as root).
- **`/var`** — "variable data" — logs (`/var/log`) and other frequently-changing app/service data.
- **`/root`** — home directory *for the root user specifically* — not inside `/home`.
- **`/tmp`** — temporary storage, cleared on reboot, and unusually any user can write here.
- **`ls -l` permission columns** — first char = file type (`-`=file, `d`=directory), next 9 chars = read/write/execute for owner, group, others.
- **Read / Write / Execute** — the three actions permissions control: open a file, change a file, run a file as a program/script.
- **Owner vs Group on a file** — a file has one owner, but can also have a separate group with its own distinct permission set.
- **`su -l <user>`** — switch to another user, `-l` gives a full login-like environment inheriting that user's settings; needs that user's password.

### 3. Terms That Get Confused
- **`/root` vs `/home/<user>`** — root's home is `/root`, NOT `/home/root` — common trip-up.
- **`/tmp` vs `/var`** — `/tmp` = short-term, wiped on reboot; `/var` = longer-term ongoing data like logs, survives reboot.
- **Owner permissions vs Group permissions** — owner = one specific user's rights; group = a separate set of rights for a defined group of users, independent of the owner's.
- **Root (the `/` directory) vs root (the user)** — same word, totally different things: one's a filesystem location, one's the superuser account.

### 4. Interview Drills

**Q: "Where would you look for application logs on a Linux system?"**
→ "I'd check `/var/log` — `/var` is where variable, frequently-written data lives, like logs and other application data that changes a lot."

**Q: "What's the difference between owner and group permissions on a file?"**
→ "A file has one owner with their own permission set, but you can also assign a group to that file with a completely separate set of permissions — so you could have the owner with full access and a group that can only read it, without changing who actually owns the file."
*Follow-up: "Why would that matter in a real environment?"*
→ "It lets you scope access without creating a ton of one-off rules — like giving a whole IT team read-write access via a group, while regular users in that same group only get read."

**Q: "How would you switch to another user on the system?"**
→ "I'd use `su -l username`, which logs me in as that user and sets up their environment properly, similar to if they'd logged in themselves directly. I'd need their password unless I'm already root."

### 5. One-Line Memory Anchors
- whoami → "who's driving right now?"
- grep → "search inside the book, not the shelf."
- touch → "blank sticky note."
- mkdir → "build a new shelf."
- cp vs mv → "clone vs relocate."
- `&` vs `&&` → "fire and forget vs wait your turn."
- `>` vs `>>` → "erase and write vs write at the end."
- `--help` vs `man` → "cheat sheet vs full instruction manual."
- SSH → "a locked tunnel to another computer."
- `/etc` → "the settings drawer."
- `/var` → "the diary that keeps growing."
- `/root` → "the boss's office, not in the regular hallway."
- `/tmp` → "sticky notes that vanish on reboot."
- Owner vs Group → "your stuff vs your team's shared stuff."
- `su -l` → "put on someone else's badge."

---

**Domain tag:** Linux CLI — new commands, operators, SSH, filesystem hierarchy, permissions/users
