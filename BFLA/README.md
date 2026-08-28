# 🔐 Broken Function Level Authorization (BFLA) in crAPI

> **Lab:** crAPI  
> **Testing Environment:** Deliberately vulnerable API lab  
> **Authorization:** Educational and authorized security testing only

## 📌 Overview

During security testing of the intentionally vulnerable **crAPI** application, I identified a **Broken Function Level Authorization (BFLA)** vulnerability.

The application failed to properly verify whether an authenticated user was authorized to execute a privileged function.

By manipulating the HTTP method and function value and using an attacker's JWT, I was able to execute a function associated with another user's data and successfully delete the victim's video.

This demonstrates insufficient **server-side function-level authorization**. :contentReference[oaicite:0]{index=0}

---

## 🎯 Vulnerability Summary

| Field | Details |
|---|---|
| **Vulnerability Type** | Broken Function Level Authorization |
| **OWASP API Top 10** | API5:2023 – Broken Function Level Authorization |
| **Affected Function** | Video management |
| **Attack Prerequisite** | Normal authenticated user |
| **Authentication** | JWT Bearer Token |
| **Tool Used** | Burp Suite |
| **Impact** | Unauthorized execution of privileged functionality |

---

# 🔍 Attack Scenario

## 1️⃣ Authenticate as Attacker

I logged into the application using an attacker account and obtained a valid JWT authentication token.

---

## 2️⃣ Authenticate as Victim

A separate victim account was used to perform the legitimate video upload operation.

The victim's account contained a personal video.

---

## 3️⃣ Upload a Video

I uploaded a video through the victim account.

The application successfully processed the upload. :contentReference[oaicite:1]{index=1}

---

## 4️⃣ Capture the API Request

I intercepted the video-related API request using **Burp Suite**.

The request contained the authentication token and parameters related to the requested function.

---

## 5️⃣ Identify Allowed HTTP Methods

I inspected the API behavior to determine which HTTP methods were accepted by the endpoint.

The testing revealed that the endpoint responded to different HTTP methods. :contentReference[oaicite:2]{index=2}

---

## 6️⃣ Manipulate the Request

I modified the request:

```text
HTTP Method:
OPTIONS → DELETE

JWT:
Victim JWT → Attacker JWT

Function:
user → admin

---

## 🧠 Root Cause

The vulnerability appears to occur because the API does not properly enforce function-level authorization on the server side.

Authentication verifies the identity of the user, but the application failed to adequately verify whether that user was authorized to execute the requested function.

Sensitive functions must always be protected by server-side authorization checks.

---

##⚠️ Security Impact

A successful BFLA vulnerability could allow an attacker to:

Execute unauthorized functions.
Access administrative functionality.
Delete critical business information.
Modify sensitive system data.
Perform privilege escalation.
Potentially modify balances or user roles.

These impacts can result in data loss, unauthorized access, and significant business impact

---

##Remediation :
–	Implement server-side role verification on all admin endpoints
–	Use attribute-based access control (ABAC) or role-based access  control (RBAC) checks in middleware
–	Never rely on URL path obscurity as an authorization mechanism
–	Apply the principle of least privilege via middleware

---

##🛠️ Tools Used
Burp Suite – API interception and request manipulation
crAPI – Deliberately vulnerable API security lab
JWT – Authentication token tested during the assessment

---
##📚 References
OWASP API Top 10 – API5:2023 Broken Function Level Authorization
OWASP crAPI – Challenge 7


