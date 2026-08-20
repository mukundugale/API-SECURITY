# JWT None Algorithm Attack

## Overview

This repository documents a hands-on demonstration of the JWT None Algorithm vulnerability in the intentionally vulnerable crAPIlab environment.

The vulnerability occurs when an application incorrectly accepts a JWT with its algorithm set to `none`, potentially allowing attacker-controlled claims to be trusted without proper signature verification.

Disclaimer: This project was performed only in an authorized, intentionally vulnerable lab environment for educational purposes.

---

## What is JWT?

JWT (JSON Web Token) is commonly used for authentication, authorization, and secure information exchange.

A JWT consists of three parts:

text
HEADER.PAYLOAD.SIGNATURE


### Header

The header defines the token type and signing algorithm.

json
{
  "alg": "HS256",
  "typ": "JWT"
}


### Payload

The payload contains claims about the user, session, or other data.

### Signature

The signature is used to help ensure that the token has not been tampered with.

---

## Vulnerability: None Algorithm Attack

A JWT None Algorithm Attack occurs when a server incorrectly accepts a JWT whose algorithm is set to `none`.

For example, a normal JWT header may use:

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

An attacker may attempt to change it to:

```json
{
  "alg": "none",
  "typ": "JWT"
}
```

The token then has no cryptographic signature. If the server accepts this token without properly enforcing signature verification, it may trust attacker-controlled claims.

---

## Lab Environment

Application: crAPI
Vulnerability: JWT None Algorithm Attack
Environment: Authorized and intentionally vulnerable lab
Purpose: Educational security research and API security practice

---

## Attack Flow

The following steps were performed in the crAPI lab:

1. Created two test accounts representing an attacker and a victim.
2. Logged into the attacker account and obtained a JWT.
3. Decoded the JWT header and payload.
4. Verified that the original token worked correctly.
5. Modified the token to demonstrate the `alg: none` scenario.
6. Created a forged token with modified claims.
7. Tested the forged token against the vulnerable lab application.
8. Observed the application's behavior when processing the modified token.

> Sensitive information and tokens have been removed or blurred.


## Impact

If an application accepts unsigned JWTs or fails to properly enforce signature verification, an attacker may potentially:

* Impersonate another user
* Modify authorization-related claims
* Access unauthorized resources
* Escalate privileges, depending on how JWT claims are used by the application

---

## Remediation

To prevent JWT None Algorithm vulnerabilities:

* Explicitly allow only expected algorithms, such as `HS256` or `RS256`.
* Never accept `alg: none` for authenticated tokens.
* Always verify the JWT signature before trusting token claims.
* Use up-to-date JWT libraries and secure verification APIs.

---

## Key Learning

> A JWT must not be trusted simply because it contains valid-looking claims. The server must enforce an expected signing algorithm and successfully verify the signature before using the token for authentication or authorization.

---

## Disclaimer

This repository is intended solely for educational purposes and documents testing performed in an authorized, intentionally vulnerable crAPI lab environment.

Do not use these techniques against systems, APIs, or accounts without explicit authorization.
