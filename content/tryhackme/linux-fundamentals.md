+++
date = '2025-06-15T07:23:04+01:00'
draft = false
title = 'Linux Fundamentals'
+++

### 🐧 Linux Fundamentals Part 1

1. Introduction
2. A Bit of Background on Linux
3. Interacting With Your First Linux Machine (In-Browser)
4. Running Your First few Commands
5. Interacting With the File-system!
6. Searching for Files
7. An Introduction to Shell Operators

### 🐧 Linux Fundamentals Part 2

1. Introduction
2. Accessing Your Linux Machine Using SSH (Deploy)
3. Introduction to Flags and Switches
4. File-system Interaction Continued
5. Permissions 101
6. Common Directories

### 🐧 Linux Fundamentals Part 3

1. Deploy Your Linux Machine
2. Terminal Text Editors
3. General/Useful Utilities
4. Processes 101
5. Maintaining Your System: Automation
6. Maintaining Your System: Package Management
7. Maintaining Your System: Logs

---

## 📝 Notes

### 🐧 Linux Fundamentals Part 1

- Accessing Your Linux Machine Using SSH (Deploy)
  - ssh tryhackme@MACHINE_IP
- Introduction to Flags and Switches
  - ls -a, ls --help, man, -h (human-readable)
- File-system Interaction Continued
  - touch, mkdir, cp, mv, rm, file
- Permissions 101
  - ls -lh, read, write, execute, su, 
- Common Directories
  - /etc, /var, /root, /tmp
- Conclusions and Summaries
- 
### 🐧 Linux Fundamentals Part 2

 - Introduction
 - Accessing Your Linux Machine Using SSH (Deploy)
  - practice exercises
  - 
### 🐧 Linux Fundamentals Part 3

- Deploy Your Linux Machine
- Terminal Text Editors
  - nano, vim
- General/Useful Utilities
  - wget, cp, scp, 
- Processes 101
  - ps, ps aux, kill, SIGTERM, SIGKILL, SIGSTOP
- Maintaining Your System: Automation
  - crontab require 6 specific values (MIN, HOUR, DOM, MON, DOW, CMD)
  `0 *12 * * * cp -R /home/cmnatic/Documents /var/backups/`
- Maintaining Your System: Package Management
  - Repositories (Adding and Removing)
  - apt, dpkg, 
- Maintaining Your System: Logs
  - /var/log, access log, error log
- Conclusions & Summaries

## A little extra info
This is just an important additional note, it wasn't included in the curriculum. The next command is very rarely needed, but it may be needed.

### Safe shutdown of a frozen Linux system

Linux has a special kernel‑level emergency mechanism called the Magic SysRq key.
It works even when:

- the graphical interface is frozen
- the shell is unresponsive
- most of the system appears dead

As long as the kernel is still alive, you can safely shut down or reboot the machine.

#### Safe reboot sequence
This performs a clean, controlled reboot.  
**Alt + SysRq + REISUB**

#### Safe shutdown sequence (the one you asked for)
This performs a clean, controlled shutdown without rebooting.  
**Alt + SysRq + REISUO**

| Key | Action                                           |
|-----|--------------------------------------------------|
| R   | Switch keyboard from raw mode back to XLATE mode |
| E   | Send SIGTERM to all processes (polite stop)      |
| I   | Send SIGKILL to all processes (force stop)       |
| S   | Sync all mounted filesystems                     |
| U   | Remount filesystems read‑only                    |
| B   | Reboot                                           |
| O   | Power off                                        |

---

🐧 **Conclusions**  
Although I have been a Linux user since 2008, these rooms have been a valuable refresher. They have helped me re-learn basic commands and system concepts, while also introducing me to some tools and practices that I have not or rarely encountered before. From basic file system navigation to SSH access to permissions and system maintenance, the content has reinforced and updated my practical Linux knowledge. It has reinforced my core skills in a well-structured and practical way.
