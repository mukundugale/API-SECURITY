# Broken Object Level Authorization (BOLA) in crAPI

## Overview

This repository documents a **Broken Object Level Authorization (BOLA)** vulnerability identified while testing the deliberately vulnerable **crAPI** application in a controlled lab environment.

The vulnerability exists in the mechanic report endpoint:

```text
/workshop/api/mechanic/mechanic_report?report_id=11
```

The application accepts a user-controlled `report_id`. By modifying this object identifier, it was possible to access mechanic report data associated with another user.

> **OWASP API Security Top 10:** API1 – Broken Object Level Authorization

---

## Vulnerability Description

Object-level authorization is an access control mechanism that ensures users can access only the objects they are authorized to access.

In this case, the application trusted the user-supplied `report_id` without properly verifying whether the authenticated user owned or was authorized to access the requested mechanic report.

### Root Cause

The server failed to properly compare the requested object identifier with the authenticated user's identity or permissions.

As a result, changing the `report_id` could return data belonging to another user.

---

## Affected Endpoint

```http
GET /workshop/api/mechanic/mechanic_report?report_id=11
```

### Vulnerable Parameter

```text
report_id
```

---

## Attack Scenario

The following steps were performed in the controlled crAPI lab:

1. Created a normal user account.
2. Added a vehicle to the account.
3. Submitted a service request through the **Contact Mechanic** functionality.
4. Opened the **Vehicle Service History**.
5. Selected **View Service Report** and captured the API request.
6. Modified the `report_id` parameter.
7. Sent the modified request.
8. The application returned another user's vehicle/mechanic report data.
9. Additional testing with different numeric `report_id` values confirmed that multiple reports could potentially be accessed.

### Example

Original request:

```text
/workshop/api/mechanic/mechanic_report?report_id=<authorized_report_id>
```

Modified request:

```text
/workshop/api/mechanic/mechanic_report?report_id=11
```

The modified request returned data that did not belong to the authenticated user, confirming the BOLA vulnerability.

---

## Proof of Concept

The vulnerability was tested by intercepting the request and changing the object identifier:

```text
report_id=<different_report_id>
```

The server responded with another user's mechanic report instead of denying access.

Multiple numeric IDs were also tested to demonstrate the potential for object enumeration when identifiers are predictable.

---

## Impact

A successful attacker could potentially:

* Access other users' mechanic reports by changing the `report_id`.
* View sensitive information associated with unauthorized reports.
* Cause a data confidentiality and privacy breach.
* Enumerate valid report IDs and collect report data at scale if IDs are predictable.
* Use exposed information for further attacks, depending on the data contained in the reports.

---

## Remediation

The following security controls should be implemented:

* Perform **server-side object-level authorization** for every request.
* Verify that the authenticated user is authorized to access the requested `report_id`.
* Enforce ownership checks or appropriate **role-based access control (RBAC)** before returning the report.
* Do not rely on client-side checks or hidden/predictable identifiers as security controls.
* Implement automated authorization testing to ensure one user cannot access another user's objects.

---

## Key Takeaway

> **Authentication does not equal authorization.**

An authenticated user should not automatically be allowed to access every object in an application. APIs must validate authorization for **each requested object**, especially when object identifiers are supplied by the client.

---

## Tools Used

* crAPI
* Burp Suite
* Burp Suite Intruder

---

## Classification

| Category          | Details                                  |
| ----------------- | ---------------------------------------- |
| Vulnerability     | Broken Object Level Authorization (BOLA) |
| OWASP Category    | OWASP API Security Top 10 – API1         |
| Affected Resource | Mechanic Service Reports                 |
| Attack Vector     | Manipulation of `report_id`              |
| Primary Impact    | Unauthorized Access to Other Users' Data |

---

## Disclaimer

This research was performed **only in a controlled, intentionally vulnerable crAPI lab environment for educational and security research purposes**. No real-world systems or unauthorized targets were tested.

---

## Screenshots

Screenshots and the complete technical walkthrough are available in the repository report:

📄 **BOLA.pdf**

---

## Author

**Mukund Ugale**

Aspiring Penetration Tester | API Security | Web & Application Security
