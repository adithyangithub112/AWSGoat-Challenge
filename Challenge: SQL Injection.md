Challenge: SQL Injection

Module: AWSGoat Module 1

Vulnerability: SQL Injection

Testing Method:
The application's user-search functionality was tested using
Burp Suite Repeater.

Baseline:
A normal search was submitted and the response was recorded.

Payload:
' or '1'='1

Result:
The manipulated input altered the application's database query
and caused information for multiple users to be returned.

Root Cause:
User-controlled input is incorporated into a SQL query without
adequate parameterization/prepared statements.

Impact:
An attacker may be able to bypass intended query restrictions
and retrieve unauthorized database records. Depending on the
application's database privileges and query structure, SQL
injection may potentially allow further database manipulation.

CWE:
CWE-89 — Improper Neutralization of Special Elements used in an
SQL Command (SQL Injection)

Remediation:
Use parameterized queries/prepared statements instead of
concatenating user-controlled input into SQL queries. Apply
appropriate server-side input validation and restrict database
account privileges according to least privilege.
<img width="1901" height="872" alt="Screenshot 2026-08-17 185539" src="https://github.com/user-attachments/assets/4b5ccecd-11cf-4d1a-9c2a-552dd21b554e" />
<img width="1821" height="902" alt="Screenshot 2026-08-17 185614" src="https://github.com/user-attachments/assets/b04de857-2663-41fc-ba51-d519eeeb39a0" />

