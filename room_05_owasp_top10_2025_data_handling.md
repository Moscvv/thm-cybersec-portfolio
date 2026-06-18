# Room 05 — OWASP Top 10 2025: Insecure Data Handling
**Platform:** TryHackMe  
**URL:** https://tryhackme.com/room/owasptopten2025three  
**Difficulty:** Easy  
**Estimated Time:** 30 minutes  
**Series:** OWASP Top 10 2025 — Room 3  
**Status:** ✅ Completed

---

## Overview
This room covers three OWASP Top 10 2025 categories all tied to how applications handle data — whether that's sensitive information being poorly protected, user input being unsafely processed, or trusted data being accepted without verification. Each category was explored with a hands-on practical challenge on a live target machine.

---

## Key Concepts Covered

### A04 — Cryptographic Failures
- Occurs when sensitive data is not adequately protected due to **missing encryption, weak algorithms, or faulty implementation**
- Common mistakes include: storing passwords without hashing, using outdated algorithms (MD5, SHA1, DES), exposing encryption keys, or failing to secure data in transit
- A particularly dangerous pattern is **"rolling your own cryptography"** — creating custom encryption instead of using well-established, vetted libraries
- **Prevention:**
  - Hash passwords using strong, slow functions: **bcrypt**, **scrypt**, or **Argon2**
  - Use trusted, industry-standard encryption libraries — never invent your own
  - Never embed credentials or keys in source code, config files, or repositories
  - Use dedicated secret management systems (e.g. HashiCorp Vault, AWS Secrets Manager)
- Hands-on: Exploited a note-sharing web app that used a **weak, shared derivative key** to protect notes — decrypted all notes to retrieve the flag

### A05 — Injection
- One of the longest-standing and most critical categories on the OWASP list — appeared in both 2021 and 2025
- Occurs when **user input is passed directly into a system that can execute it** — database, shell, templating engine, or API — without proper sanitisation
- Classic injection types:
  - **SQL Injection** — inserting SQL queries into login forms or search fields
  - **Command Injection** — injecting OS commands via unsanitised input
  - **Server Side Template Injection (SSTI)** — abusing a templating engine's ability to render dynamic content to execute code on the server
  - **AI Prompt Injection** — manipulating LLM system prompts via user input
- **Prevention:**
  - Always treat user input as **untrusted**
  - Use **prepared statements and parameterised queries** for databases (never string concatenation)
  - Avoid functions that pass input directly to the system shell
  - Validate input strictly — enforce data types, escape dangerous characters, filter before processing
- Hands-on: Performed an **SSTI attack** on a web application — injected a template payload to achieve server-side code execution and read `flag.txt` from the server's filesystem

### A08 — Software or Data Integrity Failures
- Occurs when an application **trusts code, updates, or data without verifying their authenticity, integrity, or origin**
- Covers a broad range of failures: trusting unsigned software updates, loading scripts from untrusted CDNs, accepting serialised data without validation, or failing to verify CI/CD pipeline artefacts
- A key attack in this category is **insecure deserialization** — where an application deserialises data (e.g. a Python pickle object) from an untrusted source, allowing an attacker to embed malicious code inside the serialised payload that executes on the server
- **Prevention:**
  - Never automatically trust code, updates, or data — always verify integrity using **cryptographic checksums**
  - Ensure only trusted sources can modify critical artefacts
  - Harden CI/CD pipelines — restrict who can push changes to build processes
  - Avoid deserialising data from untrusted or user-controlled sources
- Hands-on: Crafted a **malicious Python pickle payload** that, when deserialised by the vulnerable web application, read the contents of `flag.txt` on the server and returned it

---

## Key Takeaways
- **Injection is timeless** — SQL injection, command injection, and SSTI remain actively exploited in 2025 despite decades of awareness; input sanitisation is non-negotiable
- **SSTI is often underestimated** — it looks like a display issue but gives full remote code execution if exploited; any templating engine that renders user input is a potential target
- **Cryptography must be used correctly** — weak shared keys or homemade encryption can be just as bad as no encryption at all
- **Deserialization is a hidden attack surface** — many developers don't realise that accepting serialised objects from users is essentially accepting code; Python's `pickle` module is a well-known example
- **Data integrity is a trust issue** — applications must verify what they receive before acting on it, whether it's an update package, a script, or a serialised object
- Together, these three categories cover a huge portion of web application exploitation seen in real-world penetration tests and bug bounty reports

---

## OWASP Top 10 2025 Reference (Categories Covered)
| # | Category | Core Issue |
|---|---|---|
| A04 | Cryptographic Failures | Sensitive data unprotected or weakly encrypted |
| A05 | Injection | Unsanitised user input executed by the application |
| A08 | Software or Data Integrity Failures | Trusting unverified code, updates, or serialised data |

---

## Techniques & Concepts Practised
| Challenge | Technique | Target Port |
|---|---|---|
| A04 — Note sharing app | Weak shared key cryptanalysis / decryption | :8001 |
| A05 — Template engine | Server Side Template Injection (SSTI) | :8000 |
| A08 — Deserialisation app | Malicious Python pickle payload crafting | :8002 |

---

## Tools & Concepts Used
| Tool / Concept | Purpose |
|---|---|
| Python `pickle` module | Craft malicious serialised payload for deserialization attack |
| SSTI payload syntax | Inject template code to achieve server-side code execution |
| Cryptographic key analysis | Identify weak shared key and decrypt protected data |
| Parameterised queries (theory) | Correct prevention method for SQL injection |
| bcrypt / Argon2 (theory) | Correct password hashing algorithms to prevent crypto failures |

---

*Notes written for IT job portfolio — Tokyo, Japan*
