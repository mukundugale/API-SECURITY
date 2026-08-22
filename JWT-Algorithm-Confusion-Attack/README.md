# JWT Algorithm Confusion Attack in crAPI

## 📌 Overview

This repository documents my hands-on demonstration of a **JWT Algorithm Confusion Attack** in the **crAPI** lab environment.

Algorithm confusion can occur when a JWT implementation does not strictly enforce the expected signing algorithm during token verification. In this scenario, the JWT algorithm is changed from an asymmetric algorithm such as `RS256` to a symmetric algorithm such as `HS256`.

If the server improperly trusts the algorithm specified in the JWT header, it may use key material incorrectly during verification, potentially allowing an attacker to forge a valid token.

---

## 🎯 Objective

The objective of this lab was to understand how improper JWT algorithm validation can lead to token forgery and authentication or authorization bypass.

---

## 🧪 Lab Workflow

### 1. Register a User and Obtain a JWT

First, I registered a user in the crAPI application and logged in to obtain a JWT.

---

### 2. Store the JWT

The obtained JWT was stored as an environment variable for further analysis and testing.

---

### 3. Analyze the JWT

I analyzed the JWT using `jwt_tool`.

The original token used:

```text
alg: RS256
```

The JWT contained standard information including user-related claims and the signing algorithm.

---

### 4. Fetch the Public Key

I retrieved the application's public key information from the JWKS endpoint.

JWKS contains public key information that can be used by applications for JWT signature verification.

---

### 5. Convert JWKS to PEM Format

The RSA public key information from the JWKS was converted into PEM format for use during the lab.

---

### 6. Perform the Algorithm Confusion Test

Using `jwt_tool`, I modified the JWT algorithm:

```text
RS256 → HS256
```

This tests whether the server properly enforces the expected signing algorithm or incorrectly trusts the attacker-controlled `alg` value from the JWT header.

The token was then processed using the available public-key material in the algorithm confusion scenario.

---

### 7. Verify the Forged Token

The modified token was decoded and verified to confirm that the header contained:

```json
{
  "alg": "HS256"
}
```

The payload claims were also preserved during the test.

---

### 8. Test the Token Against the Application

Finally, the forged token was tested against the crAPI application to determine whether the server accepted it.

> This testing was performed in an authorized lab environment.

---

## ⚠️ Security Impact

If successfully exploitable, an Algorithm Confusion vulnerability may allow an attacker to:

* Forge JWTs
* Manipulate JWT claims
* Bypass authentication mechanisms
* Bypass authorization controls
* Potentially impersonate other users

---

## 🛡️ Remediation

To prevent JWT Algorithm Confusion attacks:

* Explicitly specify which algorithms the application accepts
* Reject tokens containing unexpected algorithm values
* Do not trust the `alg` value from an attacker-controlled JWT without validation
* Use algorithm-specific verification methods
* Ensure asymmetric public keys are never incorrectly used as symmetric HMAC secrets
* Use well-maintained JWT libraries with secure algorithm validation

---

## 🧠 Key Takeaway

> Never allow the JWT header to decide which verification algorithm the server should trust.

The server should enforce the expected algorithm and verify the token using the appropriate key type.

---

## 🛠️ Tools Used

* crAPI
* jwt_tool
* Python
* JWKS
* OpenSSL / PEM key format

---

## 📚 Related JWT Security Testing

* JWT `alg: none` attack
* JWT Algorithm Confusion attack
* Weak HMAC secret testing
* JWT claim manipulation
* `kid` header vulnerabilities

---

## ⚖️ Disclaimer

This project was created for **educational purposes and authorized security testing only**. All testing was performed in a controlled lab environment using crAPI.

**Do not use these techniques against systems without explicit permission.**

