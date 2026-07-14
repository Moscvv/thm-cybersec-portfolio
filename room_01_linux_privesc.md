# Room 01 — Linux Privilege Escalation
**Platform:** TryHackMe  
**URL:** https://tryhackme.com/room/linprivesc  
**Difficulty:** Medium  
**Estimated Time:** 50 minutes  
**Status:** ✅ Completed

---

## Overview
This room covers the fundamentals of Linux privilege escalation — the process of going from a low-privilege user account to root. It walks through enumeration techniques and over 8 different escalation vectors hands-on.

---

## Key Concepts Covered

### 1. Enumeration
The first and most critical step after gaining initial access. Key commands practiced:
- `hostname`, `uname -a`, `/proc/version`, `/etc/issue` — system info gathering
- `ps aux`, `env`, `sudo -l`, `id`, `/etc/passwd` — user and process info
- `netstat -ano`, `ifconfig`, `ip route` — network information
- `find` with various flags — searching for sensitive files, SUID bits, writable directories
- `history` — checking for credentials left in command history

### 2. Automated Enumeration Tools
- **LinPEAS** — fast and comprehensive enumeration script
- **LinEnum** — another popular enumeration script
- **LES (Linux Exploit Suggester)** — suggests kernel exploits based on version
- **Linux Smart Enumeration**, **LinuxPrivChecker**

### 3. Kernel Exploits
- Identify kernel version → search CVE databases (cvedetails.com) → run exploit
- Risk: failed exploits can crash the system — always understand the code first
- Tools: `uname -a`, LES, searchsploit

### 4. Sudo Exploitation
- `sudo -l` lists commands a user can run as root
- **GTFOBins** (gtfobins.github.io) is the go-to reference for abusing sudo-allowed binaries
- **LD_PRELOAD** trick: if `env_keep` is set, a malicious shared library can be loaded to spawn a root shell

### 5. SUID / SGID Exploitation
- Files with the SUID bit run with the file owner's privileges (often root)
- Find them with: `find / -type f -perm -04000 -ls 2>/dev/null`
- Cross-reference results with GTFOBins SUID filter
- Can be used to read `/etc/shadow` or add a root user to `/etc/passwd`
- Tools: `unshadow` + John the Ripper for password cracking

### 6. Capabilities
- A more granular privilege control than SUID
- Find binaries with capabilities: `getcap -r / 2>/dev/null`
- Some binaries (e.g. vim, python) with certain capabilities can spawn root shells
- Not detectable via standard SUID enumeration — requires separate check

### 7. Cron Jobs
- Cron jobs run on a schedule, often as root
- Check `/etc/crontab` for scheduled tasks
- If a cron script is writable by our user → inject a reverse shell payload
- If a cron job references a deleted script → recreate it in the PATH to hijack execution

### 8. PATH Hijacking
- If a SUID binary calls another binary without an absolute path, it searches `$PATH`
- If a writable directory exists (e.g. `/tmp`) we can add it to PATH and place a malicious binary there
- Exploit chain: writable dir → add to `$PATH` → create fake binary → SUID script calls it as root

### 9. NFS (Network File Sharing) Misconfiguration
- Check `/etc/exports` for shares with `no_root_squash`
- This option lets a root-compiled SUID binary from an attacker machine retain root privileges on the target
- Mount the share, compile an SUID binary, execute on target → root shell

### 10. Capstone Challenge
- Combined all techniques in a real-world-style scenario (SSH access as low-privilege user `leonard`)
- Goal: escalate to root and retrieve two flags

---

## Key Takeaways
- **Enumeration is everything** — thorough manual and automated enumeration is the foundation
- **GTFOBins** is an essential reference for sudo, SUID, and capabilities abuse
- **Multiple vectors often exist** — always check all of them before picking one
- **Context matters** — real pentests require careful exploitation to avoid crashing systems
- Linux privesc is a critical skill for OSCP, penetration testing, and SOC/security analyst roles

---

## Tools Used
| Tool | Purpose |
|------|---------|
| `find` | File and permission enumeration |
| `sudo -l` | Check sudo privileges |
| `getcap` | Check Linux capabilities |
| GTFOBins | Reference for binary abuse |
| LinPEAS / LinEnum | Automated enumeration |
| LES | Kernel exploit suggester |
| `unshadow` + John the Ripper | Password hash cracking |
| `netcat (nc)` | Reverse shell listener |
| `gcc` | Compiling exploit/SUID binaries |

---

*Notes written for job portfolio *
