# Challenge: Reflected XSS

Module: AWSGoat Module 1

Vulnerability: Reflected Cross-Site Scripting (XSS)

Affected Functionality: Search

Payload:
<img src='a' onerror=alert('xss')>

Result:
The supplied HTML was reflected by the application and the
JavaScript event handler executed in the browser.

Impact:
An attacker may be able to execute attacker-controlled
JavaScript in the security context of a victim's browser.

Root Cause:
User-controlled input is reflected into HTML without
appropriate context-sensitive output encoding/sanitization.

CWE:
CWE-79 — Improper Neutralization of Input During Web Page
Generation (Cross-Site Scripting)

Remediation:
Apply context-aware output encoding to untrusted data before
rendering it in HTML. Use appropriate input validation and
avoid inserting untrusted input into HTML using unsafe DOM
operations.

<img width="1902" height="977" alt="Screenshot 2026-08-17 182836" src="https://github.com/user-attachments/assets/3f513dce-de3b-41e9-9189-2718ff07fc4b" />
<img width="1885" height="877" alt="Screenshot 2026-08-17 183801" src="https://github.com/user-attachments/assets/f5f6147b-1632-4f56-bb21-2b51a52e54f7" />
<img width="1914" height="749" alt="Screenshot 2026-08-17 183830" src="https://github.com/user-attachments/assets/876ded55-6281-4c42-b11b-e76a976dcd4e" />



