BOPLA – Mass Assignment in crAPI

Overview

This project demonstrates a Broken Object Property Level Authorization (BOPLA) vulnerability in the intentionally vulnerable crAPI application.

The vulnerability occurs when an API fails to properly control access to individual object properties. In this case, the status property of an order can be controlled by an authenticated user.

By modifying the API request and adding the following property:

{
  "status": "returned"
}

the user can manipulate the order status and trigger a refund without properly returning the purchased item.

This demonstrates the Mass Assignment aspect of BOPLA, where a client can modify a property that should be controlled by server-side logic.

Disclaimer: This testing was performed only in an authorized, intentionally vulnerable lab environment for educational purposes.

Lab Environment

Application: crAPI

Vulnerability: Broken Object Property Level Authorization (BOPLA)

Vulnerability Type: Mass Assignment

Affected Endpoint: /workshop/api/shop/order/{order_id}

Affected Property: status

Testing Tool: Burp Suite

Environment: Authorized and intentionally vulnerable lab

Purpose: API security testing and educational research

Attack Flow

1. Create an Account

Register and log in to an account in the crAPI application.

2. Purchase a Product

Navigate to the Shop section and purchase a product.

After purchasing the product, access the Past Orders section and open the order details.

3. Capture the Request

Intercept the Order Details request using Burp Suite and send it to Repeater.

The original request uses:

GET /workshop/api/shop/order/{order_id}

4. Modify the HTTP Method

Change the HTTP method from:

GET → PUT

5. Add the status Property

Add the following JSON property to the request:

{
  "status": "returned"
}

Example:

PUT /workshop/api/shop/order/{order_id} HTTP/1.1
Authorization: Bearer <JWT>
Content-Type: application/json

{
  "status": "returned"
}

6. Send the Request

Send the modified request through Burp Suite Repeater.

The application accepts the client-controlled status property.

7. Observe the Result

The order status is changed to returned, allowing the refund process to be triggered without properly verifying that the purchased item was returned.

Attack Flow Diagram

Authenticated User
        |
        v
Purchase Product
        |
        v
Open Order Details
        |
        v
Capture API Request
        |
        v
Send Request to Burp Repeater
        |
        v
GET → PUT
        |
        v
Add "status": "returned"
        |
        v
Server Accepts Property
        |
        v
Order Status Manipulated
        |
        v
Refund Triggered

Impact

1. Financial Fraud

An authenticated user can potentially trigger a refund without actually returning the purchased item.

This can result in direct financial loss to the application or business.

2. Order State Manipulation

The attacker can manipulate the order lifecycle:

Pending → Returned → Refunded

This bypasses the intended return-verification process.

3. Business Logic Bypass

The application relies on the order status to determine whether a refund should be processed.

Because the client can control the status property, an attacker can bypass the intended business workflow.

4. Unauthorized Property Modification

The status property should be controlled by server-side business logic.

Allowing the client to modify this property demonstrates a Mass Assignment vulnerability.

Remediation

1. Server-Side State Control

Order status transitions should be controlled entirely by server-side business logic.

A secure workflow should be:

Return Requested
       ↓
Item Received
       ↓
Return Verified
       ↓
Order Status Updated
       ↓
Refund Processed

The server should never trust a client-supplied value such as:

{
  "status": "returned"
}

to perform a sensitive state transition.

2. Restrict Writable Properties

Use an allowlist of properties that the client is allowed to modify.

Sensitive properties such as:

status
user_id
refund_amount
payment_status

should not be directly controlled by the client.

3. Implement Request Schema Validation

The API should validate incoming JSON against a predefined schema.

Unexpected or unauthorized properties should be rejected.

For example, if status is a server-controlled property, the following request should not be accepted:

{
  "status": "returned"
}

4. Restrict HTTP Methods

Disable unnecessary HTTP methods.

If the endpoint is intended only to retrieve order information, methods such as PUT and DELETE should not be enabled.

Only required HTTP methods should be exposed.

5. Validate Business Logic

The refund process should verify that the required business conditions have been completed before issuing a refund.

A client-controlled status value should never be sufficient to trigger a refund.

6. Response Schema Validation

Use explicit response schemas to prevent internal or sensitive object properties from being unintentionally exposed.

References

OWASP API3:2023 – Broken Object Property Level Authorization

OWASP API Security Top 10

crAPI – Completely Ridiculous API

Burp Suite – Web Security Testing Platform

Disclaimer

This project was conducted only in an authorized and intentionally vulnerable crAPI lab environment for educational and security research purposes.

Do not use these techniques against systems, APIs, or accounts without explicit authorization.
