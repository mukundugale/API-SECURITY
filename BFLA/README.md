🔐 Broken Function Level Authorization (BFLA) in crAPI
Lab: crAPI

Testing Environment: Deliberately vulnerable API lab

Authorization: Educational and authorized security testing only
📌 Overview
During security testing of the intentionally vulnerable crAPI application, I identified a Broken Function Level Authorization (BFLA) vulnerability.
The API failed to properly verify whether an authenticated user was authorized to execute a specific function. By manipulating the HTTP method, function value, and JWT, I was able to execute a restricted function using an attacker account and delete the victim's video.

🎯 Vulnerability Summary
Field	Details
Vulnerability Type	Broken Function Level Authorization
OWASP API Top 10	API5:2023 – Broken Function Level Authorization
Affected Function	Video management
Attack Prerequisite	Normal authenticated user
Authentication	JWT Bearer Token
Tool Used	Burp Suite
Impact	Unauthorized execution of privileged functionality


🔍 Attack Scenario
1️⃣ Authenticate as Attacker
I logged into the application using an attacker account and obtained a valid JWT authentication token.
2️⃣ Authenticate as Victim
A separate victim account was used to perform the legitimate video upload operation.
3️⃣ Upload a Video
I uploaded a video through the victim account.
4️⃣ Capture the API Request
I intercepted the video-related API request using Burp Suite.
5️⃣ Identify Allowed HTTP Methods
I inspected the API behavior to determine which HTTP methods were accepted by the endpoint.
6️⃣ Manipulate the Request
I modified the request:
HTTP Method:
OPTIONS → DELETE

JWT:
Victim JWT → Attacker JWT

Function:
user → admin

The modified request was then sent to the server.

💥 Proof of Impact
The server accepted the request despite the attacker using a normal user JWT.
The video belonging to the victim was successfully deleted using the attacker's authorization token.
Attacker Account
       ↓
Valid JWT
       ↓
Modify HTTP Method
       ↓
Change Function: user → admin
       ↓
Send Request
       ↓
Unauthorized Function Execution
       ↓
Victim Video Deleted

🚨 Final Result
A normal authenticated attacker was able to execute a restricted function and delete another user's video.

🧠 Root Cause
The vulnerability appears to occur because the API does not properly enforce function-level authorization on the server side.
Authentication verifies the identity of the user, but the application failed to adequately verify whether that user was authorized to execute the requested function.
Sensitive functions must always be protected by server-side authorization checks.

⚠️ Security Impact
A successful BFLA vulnerability could allow an attacker to:
·	Execute unauthorized functions.
·	Access administrative functionality.
·	Delete critical business information.
·	Modify sensitive system data.
·	Perform privilege escalation.
·	Potentially modify balances or user roles.
These impacts can result in data loss, unauthorized access, and significant business impact.

🛡️ Remediation
·	Implement server-side role verification on all admin endpoints.
·	Use Attribute-Based Access Control (ABAC) or Role-Based Access Control (RBAC) checks in middleware.
·	Never rely on URL path obscurity as an authorization mechanism.
·	Apply the principle of least privilege via middleware.

🛠️ Tools Used
·	Burp Suite – API interception and request manipulation
·	crAPI – Deliberately vulnerable API security lab
·	JWT – Authentication token tested during the assessment

📚 References
·	OWASP API Top 10 – API5:2023 Broken Function Level Authorization
·	OWASP crAPI – Challenge 7

⚖️ Disclaimer
This finding was tested only in the intentionally vulnerable crAPI lab environment for educational and authorized security research.
No real-world application or unauthorized system was targeted.
