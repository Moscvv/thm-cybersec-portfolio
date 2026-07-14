# Room 02 — Sudo Buffer Overflow
**Platform:** TryHackMe  
**URL:** https://tryhackme.com/room/sudovulnsbof  
**Difficulty:** Info (Introductory)   
**Series:** SudoVulns Series — Room 2  
**Status:** ✅ Completed

---

## Overview
This room focuses on **CVE-2019-18634**, a buffer overflow vulnerability in the Unix `sudo` program discovered by Joe Vennix. It affects sudo versions **earlier than 1.8.26** and is triggered by a specific configuration option called `pwfeedback`. The exploit allows a low-privilege user — even one with **no sudo permissions at all** — to escalate to root.

---

## Key Concepts Covered

### 1. What is `pwfeedback`
- A cosmetic option in `/etc/sudoers` that displays asterisks (`*`) as you type your password
- Disabled by default on most distros, but was **enabled by default** on ElementaryOS and Linux Mint
- When enabled, it opens the door for a buffer overflow attack on the `sudo` password input

### 2. What is a Buffer Overflow
- Programs store user input in a fixed-size memory buffer
- If more data is supplied than the buffer can hold, it **spills into adjacent memory**, overwriting it
- An attacker can carefully craft this overflow to inject malicious code or redirect execution
- In this case: overflowing the sudo password field leads to a **root shell**

### 3. CVE-2019-18634 — The Exploit
- Proof of concept: Perl is used to generate a large amount of data, piped into `sudo` as a password input
- A `Segmentation fault` confirms the overflow is possible (accessing restricted memory)
- The actual exploit was written in C by **Saleem Rashid** and is available on GitHub
- The exploit floods the password buffer, then overwrites critical memory to spawn a root shell
- **Key difference from CVE-2019-14287**: this exploit works with **zero sudo privileges** — no special sudo configuration needed on the user's side

### 4. Exploit Workflow
1. Check if `pwfeedback` is enabled in `/etc/sudoers`
2. Download or compile the exploit: `gcc -o exploit exploit.c`
3. Transfer to target machine (via `wget`, `SimpleHTTPServer`, etc.)
4. Run the exploit → root shell obtained
5. Retrieve flag from `/root/root.txt`

### 5. Access Method
- Connected via SSH on a non-standard port: `ssh -p 4444 tryhackme@<MACHINE_IP>`
- Non-standard ports are common in CTFs and real engagements — always check

---

## Key Takeaways
- **Default configurations matter** — a purely visual/cosmetic feature (`pwfeedback`) introduced a critical RootPrivEsc vulnerability
- **CVE awareness is essential** — knowing CVEs and their affected versions is a core pentesting and security analyst skill
- **Buffer overflows don't require sudo rights** — this exploit bypasses the normal permission requirement entirely
- **Always check the sudo version** (`sudo --version`) during enumeration — it can reveal exploitable CVEs immediately
- Affects sudo `< 1.8.26` — version checking is a quick and valuable enumeration step

---

## Tools & Commands Used
| Tool / Command | Purpose |
|---|---|
| `ssh -p 4444 user@ip` | Connect via SSH on a non-standard port |
| `sudo --version` | Check sudo version for known CVEs |
| `perl -e 'print(("A") x 100)'` | Generate overflow input (PoC test) |
| `gcc -o exploit exploit.c` | Compile the C exploit |
| `wget` | Download exploit to target machine |
| Pre-compiled C exploit (Saleem Rashid) | Execute CVE-2019-18634 for root shell |

---

## CVE Reference
| Field | Detail |
|---|---|
| CVE | CVE-2019-18634 |
| Affected Software | sudo < 1.8.26 |
| Vulnerability Type | Stack-based Buffer Overflow |
| Trigger Condition | `pwfeedback` enabled in `/etc/sudoers` |
| Impact | Local privilege escalation to root |
| Discovered By | Joe Vennix |
| Exploit Author | Saleem Rashid |

---

*Notes written for portfolio*
