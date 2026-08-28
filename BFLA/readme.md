# Broken Function Level Authorization (BFLA) – crAPI

## Overview

During security testing of the intentionally vulnerable **crAPI (Completely Ridiculous API)** application, I identified a **Broken Function Level Authorization (BFLA)** vulnerability.

BFLA occurs when an API does not properly verify whether an authenticated user is authorized to execute a specific function or privileged operation.

In this case, a low-privileged attacker was able to modify an API request and invoke a privileged function that should not have been accessible to the attacker.

---

## Vulnerability

**Vulnerability:** Broken Function Level Authorization (BFLA)

**Application:** crAPI

**OWASP API Security Top 10:** API5:2023 – Broken Function Level Authorization

**Severity:** High

**Attack Type:** Privilege/Function Authorization Bypass

---

## Attack Scenario

### 1. Authenticate as an attacker

First, I authenticated to the crAPI application using an attacker-controlled account and obtained a valid JWT authentication token.

### 2. Authenticate as a victim

I then accessed a victim account to identify a normal user functionality that could be tested for authorization weaknesses.

### 3. Upload a video

From the victim account, I uploaded a personal video.

The upload operation was successfully completed.

### 4. Capture the API request

Using **Burp Suite**, I intercepted the request generated when managing the uploaded video.

This allowed me to analyze the HTTP method, endpoint, parameters, and JWT authorization token.

### 5. Identify allowed HTTP methods

I sent an `OPTIONS` request to determine which HTTP methods were accepted by the endpoint.

The response indicated that additional HTTP methods were available.

### 6. Modify the request

I modified the request by:

- Changing the HTTP method from `OPTIONS` to `DELETE`
- Replacing the victim's JWT with the attacker's JWT
- Modifying the requested function from the normal user function to an administrative function

### 7. Execute the unauthorized function

The modified request was sent using the attacker's authorization token.

The server accepted the request and successfully deleted the victim's video.

This demonstrated that the API was validating authentication but failing to properly enforce **function-level authorization**.

---

## Impact

Successful exploitation of this vulnerability can allow an attacker to:

- Execute unauthorized privileged functions
- Delete critical business information
- Modify sensitive system data
- Perform privilege escalation
- Access functionality intended only for higher-privileged users
- Cause data loss
- Undermine application trust and compliance

---

## Remediation

The application should enforce authorization checks **server-side** for every sensitive function and endpoint.

Recommended remediation:

- Implement server-side role verification for privileged endpoints.
- Use **Role-Based Access Control (RBAC)** or **Attribute-Based Access Control (ABAC)**.
- Enforce authorization checks through middleware before executing sensitive functions.
- Apply the **Principle of Least Privilege**.
- Do not rely on URL paths or endpoint obscurity as an authorization mechanism.
- Verify that the authenticated user's role and permissions match the requested function.
- Ensure authorization is enforced regardless of the HTTP method used.

---

## References

- [OWASP API Security Top 10 – API5:2023 Broken Function Level Authorization](https://owasp.org/API-Security/editions/2023/en/0xa5-broken-function-level-authorization/)
- [OWASP crAPI – Challenge 7](https://owasp.org/www-project-crapi/)

---

## Tools Used

- **Burp Suite**
- **cURL**
- **JWT**
- **crAPI**
- **REST API / HTTP**

---

## Disclaimer

This vulnerability was tested against **crAPI**, an intentionally vulnerable application created for security training and educational purposes.

All testing was performed in a controlled lab environment.
