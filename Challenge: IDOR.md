Challenge: IDOR

Module: AWSGoat Module 1

Vulnerability:
Insecure Direct Object Reference / Broken Object-Level Authorization

Affected Functionality:
Password modification

Attack:
The authenticated user's password-change request contained a
client-controlled object ID. The ID was modified to reference
another user's object.

Result:
The application accepted the request for an object that was not
owned by the authenticated user.

Root Cause:
The server failed to perform an authorization check confirming
that the requested object belonged to the authenticated user.

Impact:
An attacker may be able to modify another user's account data,
including credentials, potentially resulting in account takeover.

CWE:
CWE-639 — Authorization Bypass Through User-Controlled Key

<img width="1527" height="870" alt="Screenshot 2026-08-17 190843" src="https://github.com/user-attachments/assets/f8a12b13-837c-4561-a8be-6091f7c57aad" />
![Uploading Screenshot 2026-08-17 191115.png…]()



Remediation:
Perform server-side object-level authorization checks for every
request. Never rely solely on client-supplied object identifiers.
