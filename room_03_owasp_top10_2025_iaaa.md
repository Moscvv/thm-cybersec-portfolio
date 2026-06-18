# Room 03 — OWASP Top 10 2025: IAAA Failures
**Platform:** TryHackMe  
**URL:** https://tryhackme.com/room/owasptopten2025one  
**Difficulty:** Easy  
**Estimated Time:** 30 minutes  
**Series:** OWASP Top 10 2025 — Room 1  
**Status:** ✅ Completed

---

## Overview
This room covers three categories from the **OWASP Top 10 2025** list, all centred around failures in the **IAAA model** — Identity, Authentication, Authorisation, and Accountability. These are among the most commonly exploited weaknesses in real-world web applications. The room includes hands-on challenges for each category.

---

## The IAAA Model

The IAAA framework describes the four sequential layers of user verification in an application. Each layer depends on the one before it — if one fails, the ones after it cannot be trusted.

| Layer | Meaning | Example |
|---|---|---|
| **Identity** | Who the user claims to be | User ID / Email address |
| **Authentication** | Proving that identity | Password, OTP, passkey |
| **Authorisation** | What that identity is allowed to do | Role-based access control |
| **Accountability** | Recording and alerting on actions | Logs, audit trails, alerts |

---

## Key Concepts Covered

### A01 — Broken Access Control
- Happens when the server fails to properly enforce **who can access what** on every request
- **IDOR (Insecure Direct Object Reference):** Manipulating a URL parameter (e.g. `?id=7` → `?id=6`) to access another user's data without permission
- **Horizontal privilege escalation** — same role, but accessing another user's resources
- **Vertical privilege escalation** — jumping to admin-level actions without the right role
- Root cause: the application trusts the client too much instead of enforcing checks server-side
- Hands-on: Modified `accountID` in a URL to access another user's account data

### A07 — Authentication Failures
- Happens when an application cannot reliably verify or bind a user's identity
- Common issues include:
  - **Username enumeration** — the app reveals whether a username exists
  - **Weak/guessable passwords** with no rate limiting or account lockout
  - **Logic flaws in login/registration** — e.g. case-insensitive username handling
  - **Insecure session or cookie handling**
- Hands-on: Registered an account as `aDmiN` to exploit a case-insensitive username check and gain access to the `admin` dashboard — a classic **authentication logic flaw**
- Fix: Enforce unique indexes on the **canonical (normalised) form** of usernames; rotate sessions on login/privilege changes

### A09 — Logging & Alerting Failures
- When applications fail to record or alert on security-relevant events, defenders cannot detect or investigate attacks
- Good logging is what makes **Accountability** (the "A" in IAAA) possible
- Common failures:
  - Missing authentication events (failed logins, successful logins)
  - Vague or incomplete error messages in logs
  - No alerting on brute-force attempts or privilege changes
  - Short log retention periods
  - Logs stored where attackers can tamper with them
- Hands-on: Investigated application logs to identify a brute-force attack — found the attacker's IP, the compromised account username, and the endpoint they accessed after gaining access

---

## Key Takeaways
- **IAAA is a chain** — a failure at any point breaks the entire security model for that user's session
- **Broken Access Control (A01) is the #1 OWASP vulnerability** — simple ID manipulation in URLs is still extremely common in real apps
- **Authentication logic flaws** are often overlooked — case sensitivity, registration bypasses, and session handling are all attack surfaces
- **Logs are your eyes** — without proper logging and alerting, attacks go undetected and unattributed; this is critical for both security teams and compliance
- These three categories together cover a huge portion of real-world web application breaches
- OWASP Top 10 knowledge is highly valued in IT security roles — especially in web app security, SOC, and penetration testing positions

---

## OWASP Top 10 2025 Reference (Categories Covered)
| # | Category | Core Issue |
|---|---|---|
| A01 | Broken Access Control | Server doesn't enforce who can access what |
| A07 | Authentication Failures | Identity cannot be reliably verified |
| A09 | Logging & Alerting Failures | Security events not recorded or alerted on |

---

## Techniques & Concepts Practised
| Technique | Description |
|---|---|
| IDOR via URL manipulation | Changed `accountID` parameter to access other users' data |
| Username case-sensitivity bypass | Registered `aDmiN` to impersonate `admin` account |
| Log analysis | Reviewed access logs to trace a brute-force attack and identify compromised account |

---

*Notes written for IT job portfolio — Tokyo, Japan*
