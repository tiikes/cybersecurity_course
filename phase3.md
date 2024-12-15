Security check of the booking system

## Manual testing

All following system specs were met when system was tested manually:
- User registration works for both reserver and administrator roles.
- Access control allows registered users to perform role-specific actions
- Resources and reservations can be viewed without logging in, and the reserver’s identity is appropriately hidden.
- Users below 15 years of age cannot make reservations.

## Security check using ZAP:

First, I did automated scan, spider scan and second automated scan. This resulted to only one informational alert about authentication request identified. No immediate action is required. 

Second, I opened ZAP again and performed manual explore and three active scans. This led to 1 medium alert, 1 low alert and 6 informational alerts:


### Summary of Alerts

| Risk Level | Number of Alerts |
| --- | --- |
| High | 0 |
| Medium | 1 |
| Low | 1 |
| Informational | 6 |


### Alerts

| Name | Risk Level | Number of Instances |
| --- | --- | --- |
| Format String Error | Medium | 1 |
| Cross Site Scripting Weakness (Persistent in JSON Response) | Low | 2 |
| Authentication Request Identified | Informational | 1 |
| Information Disclosure - Suspicious Comments | Informational | 1 |
| Loosely Scoped Cookie | Informational | 2 |
| Modern Web Application | Informational | 1 |
| Session Management Response Identified | Informational | 2 |
| User Agent Fuzzer | Informational | 108 |

## Finding and follow-up

The 5 most important point that should be fixed. All alerts were flagged during ZAP active scan.

1. Format String Error
The system may allow unvalidated user inputs in string formatting functions, risking code execution or application crashes.

Fixing: Ensure all user inputs are validated and properly escaped before being used in string formatting.

2. Cross-Site Scripting (XSS) Weakness
User inputs are not sanitized, allowing malicious scripts to persist in JSON responses, potentially compromising user sessions.

Fixing: Implement robust input validation and escaping mechanisms to prevent the injection of scripts into responses.

3. Loosely Scoped Cookie
Cookies are not securely scoped, making them accessible to unintended domains or paths, increasing the risk of session hijacking.

Fixing: Set secure flags (HttpOnly, Secure, SameSite) on cookies and scope them to specific domains and paths.

4. Session Management Response Identified
Session responses may lack robust security measures, potentially allowing unauthorized session reuse or hijacking.

Fixing: Implement secure session management practices, including unique session tokens, proper expiration, and invalidation after logout.

5. Information Disclosure - Suspicious Comments
Comments in the code contain potentially sensitive information, revealing system details to attackers.

Fixing: Remove all sensitive or unnecessary comments from the code before deploying the system to production.
