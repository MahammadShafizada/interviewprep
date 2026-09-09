# Linux CLI Basics — Spoken Practice Guide

This material actually splits into two related but distinct topics: **navigating the filesystem** and **gathering system information**. Treating them separately will make the concepts stick better.

---

# Topic A: Linux Filesystem Navigation

## 1. The Big Picture

Before you can do *anything* on a Linux machine — defend it, investigate it, break into it during a pentest — you need a way to move around it and see what's there. The terminal is just a text-based way of giving the computer instructions instead of clicking icons, and the filesystem is basically a giant tree of folders and files that you need to be able to walk through, search, and read. If you forget every detail, remember this: **the terminal is how you talk to Linux directly, and a handful of commands (where am I, what's here, move, search, read) cover 90% of what you do at first.**

## 2. Core Concepts, Explained for Speaking Aloud

**The Terminal / CLI**
It's a text window where you type commands instead of clicking through menus. Security folks live in it because it's faster, gives way more control, and honestly a lot of security tools *only* work from the command line — there's no GUI version. It feels intimidating for about a day, and then it just becomes normal.
*Why it matters:* almost every real security tool — scanners, exploit frameworks, log analyzers — is either terminal-only or way more powerful from the terminal.

**pwd (print working directory)**
This one just answers "where am I right now?" It prints the full path of the folder you're currently sitting in, like `/home/ubuntu`. Super simple, but it's the first thing you run when you're lost, which happens a lot.
*Why it matters:* when you're jumping between folders during an investigation, losing track of your location wastes time and can cause you to run commands in the wrong place.

**ls (and its variants)**
`ls` lists what's inside the folder you're in — files and subfolders. Add `-l` and you get a "long" view with details like permissions, size, and dates. Add `-a` (or combine as `-al`) and you also see hidden files — anything starting with a dot, which Linux hides by default but doesn't actually protect.
*Why it matters:* attackers and defenders both stash things in hidden dotfiles, so knowing to check for them is a habit you want early.

**cd (change directory)**
This moves you from one folder to another — `cd Documents` takes you into Documents, and `cd ..` bumps you back up one level. It's literally just "walk into this folder" or "walk back out."
*Why it matters:* you can't inspect logs, configs, or evidence sitting in some folder if you can't get to it.

**find**
`find` searches for a file by name starting from a given location — like `find ~ -name mission_brief.txt` searches your whole home directory for that exact filename and prints the full path once it locates it. It can be slow because it's checking every folder underneath your starting point.
*Why it matters:* during an incident you often know *what* file you're looking for (a log, a suspicious script) but not *where* it is — find is how you track it down instead of manually digging through folders.

**cat (concatenate / read file)**
`cat` dumps the contents of a file straight to your screen. Simple as that — you point it at a file, it shows you what's inside.
*Why it matters:* it's the fastest way to peek at a config file or log without opening a full text editor, which matters when you're moving quickly.

## 3. Terms That Get Confused With Each Other

**`ls` vs `find`** — people mix these up because both "show you files." The difference is basically: `ls` shows you what's *in the folder you're already standing in*, while `find` goes *searching* through folders on your behalf when you don't know exactly where something is. Keep it straight with: "ls looks around the room, find sends out a search party."

**`cd ..` vs `cd ~`** — both move you, but `cd ..` steps back *one level* from wherever you currently are, while `cd ~` (or just `cd` with nothing after it) teleports you straight back to your home directory no matter how deep you've wandered. Think: "`..` is one step back, `~` is a reset button."

## 4. Interview Simulation

**Q1: What's the difference between the terminal and the shell, or is that the same thing? And why would a security person use it over a GUI?**
*Model answer:* "So the terminal is really just the window or program you're typing into, and the shell is the program running inside it that actually interprets your commands — but in casual conversation people use the words pretty interchangeably, and that's fine. The reason security folks live in it is that it's faster once you know the commands, it gives you way more precise control, and a lot of the actual security tooling — scanners, exploitation frameworks, log parsers — either doesn't have a GUI at all or the GUI version is way more limited."

**Q2: Walk me through how you'd locate a specific file on a Linux system if you didn't know where it was.**
*Model answer:* "I'd use the find command, pointing it at a starting location — usually the home directory with the tilde shortcut — and give it the exact filename with the -name flag. So something like find ~ -name filename.txt. It'll search recursively through every folder underneath that starting point, which can take a bit on a big filesystem, but once it's done it prints the full path to wherever it found the file. From there I'd cd into that directory and confirm it's there with ls before actually reading it."

**Q3 (follow-up/pushback): That works if you already know the exact filename. What if you only have a rough idea, like 'some log file with today's date in it'?**
*Model answer:* "In that case I wouldn't use -name with an exact string — find actually supports wildcards, so I could do something like find ~ -name '*.log' or use a partial date pattern to catch anything close. If I genuinely had no idea about the name at all, I'd probably switch to something like grep combined with find, or just manually browse with ls into the likely folders like /var/log. The exact command matters less than knowing find can take patterns, not just exact matches."

**Q4: What are hidden files in Linux, and why would you care about them as a security person?**
*Model answer:* "Hidden files are just any file or folder whose name starts with a dot, and by default ls won't show them unless you add the -a flag. They're not actually secure or protected in any way, it's purely a display convention — but that's exactly why they're interesting from a security angle. Both attackers and legitimate config tools like to tuck things away in dotfiles because casual browsing won't reveal them, so checking with ls -la is basically a habit you want to build early so you're not missing something sitting in plain sight."

**Q5: If a coworker told you they were 'lost' in the filesystem and didn't know where their terminal session was, what would you tell them to run?**
*Model answer:* "Just pwd — print working directory. It'll spit out the full path of exactly where they're sitting right now, like /home/username/Documents. It's the simplest command there is, but it's the first thing I run any time I've been cd-ing around for a while and lose track."

## 5. One-Line Memory Anchors
- **Terminal:** "Typing instead of clicking, and way more powerful for it."
- **pwd:** "Where am I? — pwd answers it."
- **ls / -al:** "ls looks around the room; -al also checks behind the curtains."
- **cd:** "Walk in, walk back out."
- **find:** "When you don't know where — send find to go look."
- **cat:** "Fastest way to peek inside a file."

---

# Topic B: Linux System Information Gathering

## 1. The Big Picture

Once you can move around a Linux system, the next basic skill is figuring out *what* that system actually is — who you're logged in as, what version of Linux it's running, how much storage is available. This matters because security work almost never starts with full context; you land on an unfamiliar machine and need to quickly size it up before you can decide what to do next. If you remember nothing else: **a handful of quick commands let you fingerprint any Linux box in under a minute — who am I, what OS/kernel is this, how much disk space is there.**

## 2. Core Concepts, Explained for Speaking Aloud

**whoami**
This just tells you the username you're currently logged in as. It's the simplest command that exists, but it matters because on a real system you might be several logins deep, or working on someone else's session, and you want to confirm your identity before you do anything else.
*Why it matters:* running commands as the wrong user — especially with elevated permissions — is a classic way to cause damage or miss the point of an investigation.

**uname -a**
This prints a full line of system details in one shot: the kernel name, the hostname, the kernel version, the hardware architecture, and the OS type. Basically it's a system fingerprint. Running plain `uname` with nothing after it just tells you the kernel name, like "Linux," which is far less useful.
*Why it matters:* knowing the exact kernel version matters a lot in security — certain vulnerabilities only affect certain kernel versions, so this is often one of the first things you check during recon or an assessment.

**df -h**
This shows disk usage across all the mounted filesystems on the machine, and the `-h` flag makes it "human readable" — so you see 2G or 500M instead of a huge raw byte number. It breaks down what's the real physical disk versus temporary RAM-based storage like tmpfs.
*Why it matters:* if a system's disk is nearly full, that can be a red flag for something like runaway logging, a malware dropping large files, or just an operational problem you need to flag.

**Reading /etc config files (like os-release)**
The `/etc` directory is where Linux keeps most of its system-wide configuration files. One of the most useful ones for quick recon is `os-release`, which you read with `cat` — it spells out the exact distribution name and version in a much cleaner, more explicit way than uname does.
*Why it matters:* different distros and versions patch vulnerabilities on different timelines, so accurately identifying the distro is part of understanding what you're actually dealing with.

## 3. Terms That Get Confused With Each Other

**`uname -a` vs `cat /etc/os-release`** — both tell you "what system is this," but they answer slightly different questions. `uname -a` is more about the kernel and hardware — the low-level guts of the machine — while `os-release` tells you specifically about the distribution flavor, like "this is Ubuntu 24.04" in plain, unambiguous language. A simple way to keep them straight: "uname talks about the engine, os-release talks about the car model."

**Disk space (`df`) vs memory (RAM)** — this one isn't in the material directly but trips people up constantly: `df -h` reports *disk/storage* space, not RAM. The tmpfs entries you see in df's output actually live in RAM, not on the physical disk, which is a bit of a curveball — they show up in a disk command but they're not really "disk" in the traditional sense.

## 4. Interview Simulation

**Q1: If you SSH into an unfamiliar Linux box during an assessment, what commands would you run first to get your bearings?**
*Model answer:* "I'd start really basic — whoami to confirm exactly which user context I'm in, since that affects what I'm even allowed to do. Then I'd run uname -a to get the kernel version and architecture, and probably cat /etc/os-release right after to get a clean read on the actual distro and version, since uname alone can be a little cryptic. If disk space seems relevant to what I'm investigating, I'd throw in df -h too. It's basically a 30-second fingerprint of the machine before I go any deeper."

**Q2: Why would the kernel version specifically matter to you as a security person?**
*Model answer:* "Because a lot of privilege escalation exploits and vulnerabilities are tied to specific kernel versions — a bug that lets you escalate to root might only exist in kernel versions up to a certain patch level. So once I know the exact kernel version from uname -a, I can cross-reference it against known vulnerabilities for that version, which tells me what's actually exploitable on this specific machine versus what's just theoretical."

**Q3 (follow-up/pushback): Okay, but isn't uname -a kind of redundant if you're also going to check os-release? Why run both?**
*Model answer:* "Fair pushback — there's overlap, but they're not identical. uname -a gives you the kernel version and hardware architecture, which os-release doesn't cover at all. And os-release gives you the clean distro name and version number in plain language, which uname's output doesn't spell out as clearly — sometimes the kernel string is genuinely ambiguous about which distro it belongs to. In practice I run both because they're each fast and they cover slightly different angles, so together you get a much more complete picture than either alone."

**Q4: You run df -h and see the main disk is at 95% capacity. What would that make you think about, from a security angle?**
*Model answer:* "That's a flag worth digging into rather than ignoring. It could be something totally mundane like unrotated log files piling up, but it could also point to something more concerning — like a compromised system where an attacker is staging or storing large files, or a logging or monitoring tool that's been disabled so logs never got cleaned up. Either way, I wouldn't just note it and move on, I'd want to find out specifically what's eating the space before deciding whether it's benign or not."

**Q5: What's the difference between running plain uname and uname -a?**
*Model answer:* "Plain uname with no flags just prints the kernel name, so on basically any Linux box it just says 'Linux' — not very useful on its own. Adding the -a flag, which stands for 'all,' gives you the full picture in one line: hostname, kernel version, build date, architecture, and OS type. In practice I basically never run plain uname, I always just default to uname -a because the extra info costs nothing and is almost always what I actually want."

## 5. One-Line Memory Anchors
- **whoami:** "Confirm your identity before you touch anything."
- **uname -a:** "One command, full system fingerprint."
- **df -h:** "Human-readable disk health check."
- **os-release:** "The car model, not just the engine."

---

**Domain/Topic Tag:** Linux Fundamentals — CLI Navigation & System Reconnaissance
