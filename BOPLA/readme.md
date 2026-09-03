# BOPLA – Mass Assignment in crAPI

## Overview

This repository documents a hands-on demonstration of the **Broken Object Property Level Authorization (BOPLA)** vulnerability in the intentionally vulnerable **crAPI** lab environment.

BOPLA occurs when an API fails to properly authorize access to individual properties of an object rather than the object itself.

In 2023, OWASP combined two previously separate vulnerabilities under BOPLA:

1. **Excessive Data Exposure** – occurs when an API exposes sensitive object properties that should not be accessible to the user.
2. **Mass Assignment** – occurs when a user can modify, add, or delete object properties that should be controlled by the server.

This project demonstrates the **Mass Assignment** aspect of BOPLA. The vulnerable API allows an authenticated user to modify the `status` property of an order and trigger a refund without properly returning the purchased item.

> **Disclaimer:** This project was performed only in an authorized, intentionally vulnerable crAPI lab environment for educational purposes.

---

## Vulnerability: Mass Assignment

The affected order endpoint accepts client-supplied properties and allows the `status` property to be modified.

The `status` property should be controlled by server-side business logic because changing the order status can affect the refund process.

During testing, the following property was added to the request:

```json
{
  "status": "returned"
}
```

If the server accepts this client-controlled property, an authenticated user can manipulate the order state and trigger a refund.

---

## Lab Environment

* **Application:** crAPI
* **Vulnerability:** BOPLA – Mass Assignment
* **Affected Endpoint:** `/workshop/api/shop/order/{order_id}`
* **Vulnerable Property:** `status`
* **Testing Tool:** Burp Suite
* **Environment:** Authorized and intentionally vulnerable lab
* **Purpose:** API security testing and educational research

---

## Attack Flow

The following steps were performed in the crAPI lab:

1. Registered an account and logged into the application.
2. Purchased a product from the Shop section.
3. Initiated the product return process.
4. Opened **Order Details** and captured the API request.
5. Sent the captured request to **Burp Suite Repeater**.
6. Changed the HTTP method from `GET` to `PUT`.
7. Added the following JSON property:

```json
{
  "status": "returned"
}
```

8. Sent the modified request to the server.
9. Observed that the order status was changed to `returned`.
10. Successfully triggered the refund without properly returning the purchased item.

### Request

```http
PUT /workshop/api/shop/order/{order_id} HTTP/1.1
Authorization: Bearer <JWT>
Content-Type: application/json

{
  "status": "returned"
}
```

### Attack Flow Diagram

```text
Authenticated User
        |
        v
Purchase Product
        |
        v
Initiate Return
        |
        v
Capture Order Request
        |
        v
Send to Burp Repeater
        |
        v
GET → PUT
        |
        v
Add "status": "returned"
        |
        v
Server Accepts Client-Controlled Property
        |
        v
Order Status Changed
        |
        v
Refund Triggered
```

---

## Impact

If an application allows users to modify server-controlled object properties, an attacker may potentially:

* **Trigger unauthorized refunds**, causing financial loss.
* **Manipulate order states** such as `pending → returned`.
* **Bypass business logic** that should verify the physical return of an item.
* **Modify sensitive object properties** that should only be controlled by server-side logic.

### Business Impact

The primary impact demonstrated in this lab is **financial fraud**. An authenticated user can potentially receive a refund while retaining the purchased item.

The vulnerability also compromises the integrity of the order lifecycle because the client is able to influence a sensitive server-side state.

---

## Remediation

To prevent BOPLA Mass Assignment vulnerabilities:

* **Implement server-side state control** – sensitive properties such as order status must be determined by trusted server-side business logic.
* **Use allowlists for writable properties** – only explicitly permitted properties should be accepted from client input.
* **Reject unexpected properties** – validate incoming JSON against a strict request schema.
* **Do not bind arbitrary JSON directly to internal objects** – use dedicated request/DTO models containing only client-writable fields.
* **Restrict HTTP methods** – disable unnecessary methods such as `PUT` or `DELETE when they are not required by the endpoint.
* **Validate business conditions before refunds** – verify that the item was actually received and the return was approved before changing the order state or issuing a refund.
* **Apply property-level authorization** – verify that the authenticated user is authorized to modify each individual property.
* **Use response schema validation** – prevent sensitive or internal object properties from being unintentionally exposed.

A secure return workflow should look like:

```text
Return Requested
       |
       v
Item Received
       |
       v
Return Verified
       |
       v
Order Status Updated
       |
       v
Refund Processed
```

The client should not be able to skip these steps simply by supplying:

```json
{
  "status": "returned"
}
```

---

## Key Learning

> **Client input should never be trusted for sensitive object properties that control authorization, financial transactions, or business state.**

Even if a user is authorized to access an object, that does not mean they should be authorized to modify every property of that object.

---

## References

* **OWASP API3:2023 – Broken Object Property Level Authorization**
* **OWASP API Security Top 10**
* **crAPI – Completely Ridiculous API**
* **Burp Suite – Web Security Testing Platform**

---

## Disclaimer

This repository is intended solely for educational purposes and documents testing performed in an authorized, intentionally vulnerable crAPI lab environment.

Do not use these techniques against systems, APIs, or accounts without explicit authorization.
