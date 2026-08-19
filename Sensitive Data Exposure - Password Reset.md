## Vulnerability Assessment Report: Insecure Password Reset & Sensitive Data Exposure

### 1. Executive Summary

During the security assessment of the **AWSGoat** application, a critical vulnerability was identified in the **Password Reset / Account Management** mechanism. The application fails to properly validate whether the requesting user is authorized to update a specific account's credentials. An attacker can exploit this flaw by manipulating user identifiers in HTTP requests, resulting in unauthorized account takeover (ATO) and sensitive data exposure.

### 2. Vulnerability Details

| **Parameter** | **Details** |
| --- | --- |
| **Vulnerability Type** | Insecure Direct Object Reference (IDOR) / Broken Object Level Authorization (BOLA) |
| **OWASP Top 10** | A01:2021 – Broken Access Control |
| **Severity** | **Critical** (CVSS: 9.8) |
| **Affected Endpoint** | `/api/reset-password` (or related user update endpoint) |
| **Impact** | Full account takeover (including administrative accounts), unauthorized data access |

### 3. Technical Findings & Root Cause

The application processes password reset/update requests by relying on client-supplied parameters (such as `id` or `user_id`) in the request payload rather than validating the user's authenticated session or enforcing an out-of-band, single-use token mechanism.

- **Root Cause:** Absence of server-side authorization checks linking the authenticated session to the targeted user ID, combined with predictable sequential identifiers.

#### Attack Scenario

1. An authenticated attacker intercepts their own valid password reset request.
2. In the request body, the attacker alters the user identifier:JSON
    
    ```
    {
      "id": 1,
      "new_password": "AttackerControlledPassword123!"
    }
    ```
    
3. The server updates the password for `id: 1` (Administrator) without verification.
4. The attacker logs in as the administrator and accesses protected sensitive assets.

### 4. Impact Analysis

- **Confidentiality:** **High** — Attackers gain full read access to private data and cloud resources exposed to compromised accounts.
- **Integrity:** **High** — Attackers can modify application settings, user records, and backend assets.
- **Availability:** **High** — Legitimate users are locked out of their accounts once their credentials are changed.

### 5. Recommended Remediation

- **Implement Token-Based Verification:** Enforce standard password reset flows using cryptographically secure, time-limited, single-use reset tokens sent directly to the registered email address.
- **Enforce Strict Session Binding:** Validate on the server side that the requesting session identity strictly matches the identity being modified.
- **Use Non-Sequential Identifiers:** Replace sequential integer IDs with cryptographically random UUIDs/GUIDs to prevent straightforward enumeration attacks.
