# API Security Labs and Findings

## Overview

This repository documents my hands-on learning and security testing of common **API security vulnerabilities** using authorized and intentionally vulnerable lab environments.

Each finding includes vulnerability analysis, the affected security concept, impact, proof of concept, and remediation recommendations.

> **Disclaimer:** All testing documented in this repository is performed only in authorized lab environments for educational purposes.

---

## Findings

| # | Vulnerability                                                                  |   Status  |  
| - | ------------------------------------------------------------------------------ | --------- | 
| 1 | JWT None Algorithm Attack                                                      | Completed |   
| 2 | Mass Assignment                                                                | Planned   |   
| 3 | JWT Algorithm Confusion Attack                                                 | Completed |   
| 4 | Broken Object Level Authorization (BOLA)                                       | Planned   |   
| 5 | API Security Misconfiguration                                                  | Planned   |   

---

## Repository Structure

```text
API-SECURITY/
│
├── README.md
│
├── JWT-None-Algorithm-Attack/
│   ├── README.md
│   ├── report.pdf
│   └── screenshots/
│
├── Mass-Assignment/
│   ├── README.md
│   └── screenshots/
│
├── JWT-Algorithm-Confusion-Attack/
│   ├── README.md
│   └── screenshots/
│
├── BOLA/
│   ├── README.md
│   └── screenshots/
│
└── Security-Misconfiguration/
    ├── README.md
    └── screenshots/
```

---

## Methodology

My general approach for documenting each security finding is:

1. **Understand the vulnerability**
2. **Identify the affected API functionality**
3. **Perform testing in an authorized lab**
4. **Analyze the application's behavior**
5. **Document the security impact**
6. **Capture proof-of-concept evidence**
7. **Provide remediation recommendations**

---

## Learning Areas

This repository currently focuses on:

* API Authentication and Authorization
* JWT Security
* Broken Object Level Authorization (BOLA)
* Mass Assignment
* JWT Algorithm confusion attack
* API Security Misconfiguration
* API Token and Authentication Attacks

More findings will be added as I continue learning and practicing API security.

---

## Lab Environments

The findings in this repository are documented from authorized security labs and intentionally vulnerable applications, including:

* crAPI
* Other authorized API security labs

---

## Documentation Format

Each vulnerability folder may contain:

* `README.md` — Detailed explanation of the vulnerability
* `screenshots/` — Sanitized proof-of-concept screenshots
* `report.pdf` — Additional lab documentation, when available

Sensitive information, credentials, tokens, and personal data are removed or blurred before publication.

---

## Disclaimer

This repository is created strictly for **educational and authorized security testing purposes**.

Do not use the techniques documented here against systems, APIs, applications, or accounts without explicit permission from the owner.
