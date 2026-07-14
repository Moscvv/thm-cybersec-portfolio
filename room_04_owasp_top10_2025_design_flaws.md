# Room 04 — OWASP Top 10 2025: Application Design Flaws
**Platform:** TryHackMe  
**URL:** https://tryhackme.com/room/owasptopten2025two  
**Difficulty:** Easy  
**Series:** OWASP Top 10 2025 — Room 2  
**Status:** ✅ Completed

---

## Overview
This room covers four OWASP Top 10 2025 categories that all stem from the same root problem: **weak foundations at the architecture and design level**. Unlike runtime bugs, these vulnerabilities are baked into how an application is configured, built, and structured — making them harder to patch retroactively. Each category was paired with a hands-on challenge on a live target machine.

---

## Key Concepts Covered

### AS02 — Security Misconfigurations
- Occurs when systems are deployed with **unsafe defaults, incomplete settings, or unnecessarily exposed services** — not code bugs, but setup mistakes
- Even a single exposed admin panel, open cloud storage bucket, or verbose error message can compromise an entire system
- **Real-world example:** Uber (2017) — a publicly accessible AWS S3 backup bucket exposed sensitive rider and driver data; no credentials needed to access it
- Common patterns: default credentials left unchanged, unnecessary APIs exposed, misconfigured cloud permissions, stack traces visible in error messages, outdated containers
- **Prevention:** Harden defaults, enforce least privilege, segment networks, patch regularly, audit cloud configs, hide system details from error outputs
- Hands-on: Explored a User Management API that left too many developer traces exposed — found and retrieved a flag from an insufficiently secured endpoint

### AS03 — Software Supply Chain Failures
- Happens when applications depend on **compromised, outdated, or unverified third-party components** — libraries, packages, APIs, or AI models
- Attackers exploit these weak links to inject malicious code or bypass security without ever touching the core application code
- **Real-world example:** SolarWinds (2021) — malicious code was inserted into a trusted software update, affecting thousands of organisations that automatically installed it
- Also applies to AI/ML: unverified third-party models or fine-tuned datasets can embed hidden behaviours or backdoors
- Common patterns: unmaintained dependencies, auto-installing updates without verification, insecure CI/CD pipelines, no provenance tracking
- **Prevention:** Verify all components before use, sign and audit updates, lock down build pipelines, monitor runtime behaviour of dependencies
- Hands-on: Exploited an outdated `lib/vulnerable_utils.py` component imported by the application — debugged it to extract the flag

### AS04 — Cryptographic Failures
- Occurs when **encryption is used incorrectly or not at all** — weak algorithms, hard-coded keys, poor key management, or unencrypted sensitive data
- Enables attacks such as man-in-the-middle, brute-force on weak keys, or simply reading secrets that were never protected
- Common patterns: using deprecated algorithms (MD5, SHA-1, ECB mode), hard-coded secrets in source code or config files, no TLS or self-signed certs, poor key rotation, unencrypted data at rest
- **Prevention:** Use modern algorithms (AES-GCM, ChaCha20-Poly1305, TLS 1.3), use key management services (AWS KMS, Azure Key Vault, HashiCorp Vault), rotate keys regularly, maintain a certificate/key inventory, never expose secrets in AI prompts
- Hands-on: Discovered a hard-coded or exposed key within the application and used it to decrypt a file — retrieved the flag from the decrypted content

### AS06 — Insecure Design
- Happens when **flawed logic or architecture is built into the system from the start** — cannot be patched later, must be rethought at the design level
- Root causes: skipped threat modelling, no security requirements defined before development, faulty trust assumptions about users or systems
- **Real-world example:** Clubhouse (early days) — backend API had no proper authentication because designers assumed users would only interact via the mobile app; researchers could directly query user data and private conversations
- **AI-era specific risks:** Prompt injection (user input blended with system prompts to hijack context), blind trust in model output, poisoned models pulled from unverified sources
- Common patterns: weak business logic in recovery/approval flows, test/debug bypasses left in production, no abuse-case review, AI components with unchecked authority
- **Prevention:** Threat model at every stage, define security requirements before coding, apply least privilege, validate all AI inputs and outputs, separate system prompts from user content, require human review for high-risk AI actions
- Hands-on: Bypassed a mobile-device assumption in the application design — accessed an endpoint that was incorrectly assumed to be mobile-only to retrieve the flag

---

## Key Takeaways
- **Security cannot be added at the end** — all four categories stem from decisions made during setup or design, not during coding
- **Defaults are dangerous** — every service, framework, and cloud resource should be hardened before deployment; assume defaults are insecure
- **The supply chain is an attack surface** — every library, package, and AI model you import is a potential entry point; verify and monitor all of them
- **Cryptography done wrong is worse than no cryptography** — using weak algorithms or hard-coded keys creates a false sense of security
- **Insecure design is the hardest to fix** — architecture flaws require rethinking systems from scratch; secure design must come first
- AI introduces entirely new categories of design failure (prompt injection, blind model trust, poisoned datasets) that are now part of OWASP's 2025 scope

---

## OWASP Top 10 2025 Reference (Categories Covered)
| # | Category | Core Issue |
|---|---|---|
| AS02 | Security Misconfigurations | Unsafe defaults and exposed services at deployment |
| AS03 | Software Supply Chain Failures | Compromised or unverified third-party components |
| AS04 | Cryptographic Failures | Weak, missing, or misused encryption |
| AS06 | Insecure Design | Flawed logic and architecture baked in from the start |

---

## Techniques & Concepts Practised
| Challenge | Technique Used |
|---|---|
| AS02 — User Management API | Explored exposed API endpoints left by developers |
| AS03 — Vulnerable Python lib | Identified and exploited an outdated imported component |
| AS04 — Encrypted file | Located exposed cryptographic key and decrypted file |
| AS06 — Mobile assumption bypass | Accessed an endpoint by bypassing a flawed design assumption |

---

*Notes written for portfolio*
