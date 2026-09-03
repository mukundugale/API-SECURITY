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
| 2 | Business Logic Vulnerability                                                   | Completed |   
| 3 | JWT Algorithm Confusion Attack                                                 | Completed |   
| 4 | Broken Object Level Authorization (BOLA)                                       | Completed |   
| 5 | Broken Function Level Authorization (BFLA)                                     | Completed |
| 6 | Broken Object Property Level Authorization (BOPLA)                             | Completed | 

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
│   
│
├── Business-Logic-Vulnerability/
│   ├── README.md
│   └── Report.pdf
│
├── JWT-Algorithm-Confusion-Attack/
│   ├── README.md
|   └── Report.pdf
│   
│
├── BOLA/
│   ├── README.md
│   └── Report.pdf
│
└── BFLA/
│    ├── README.md
│    └── Report.pdf
│
└── BOPLA/
    ├── README.md
    └── Report.pdf
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
* Business Logic Vulnerability
* JWT Algorithm confusion attack
* Broken Function Level Authorizatioon
* API Token and Authentication Attacks
* Broken Object Property Level Authorization (BOPLA)

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
* `report.pdf` — Additional lab documentation, when available

Sensitive information, credentials, tokens, and personal data are removed or blurred before publication.

---

## Disclaimer

This repository is created strictly for **educational and authorized security testing purposes**.

Do not use the techniques documented here against systems, APIs, applications, or accounts without explicit permission from the owner.
