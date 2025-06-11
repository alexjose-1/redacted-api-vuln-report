# Redacted-webapp-vuln-report
“Redacted penetration test write-up of a web application”
🔍 Manual Penetration Test Report – [REDACTED TARGET]

📅 Date of Test: June 2025

🧑‍💻 Author: Alex Jose

🛡️ Disclosure Status: Redacted and anonymized for public sharing

This test was performed with permission as part of a cybersecurity internship. All sensitive identifiers have been removed for ethical disclosure.


🧭 Scope & Objective
The objective of this test was to perform a manual penetration test on a public-facing web application and identify potential security flaws through ethical means. This assessment focused on common OWASP Top 10 vulnerabilities using hands-on techniques.


🧰 Tools Used
Tool	Purpose
WHOIS/Dig	Reconnaissance (DNS/IP info)
Gobuster	Directory enumeration
Assetfinder	Subdomain discovery
Burp Suite	Manual interception & fuzzing
Firefox DevTools	Manual inspection


🛠️ Testing Steps & Key Findings
1. 🛰️ Reconnaissance
Basic WHOIS and DNS enumeration revealed standard hosting and domain setup.

Subdomain enumeration was performed using assetfinder.

The target used a third-party cloud host and exposed several endpoints for testing.


2. 🗂️ Directory & Resource Enumeration
Using gobuster, several public endpoints and directories were mapped.

No sensitive files (e.g., .env, .git) were discovered during this phase.


3. 🧪 Vulnerability Testing
💀 3.1 Reflected XSS (Unconfirmed)
Test Area: Contact Form

Parameter: first_name

Payload: <script>alert(1)</script>

Result:

Received a 302 redirect after submission.

No script executed immediately.

Analysis: No confirmed reflected XSS, but payload was accepted without sanitization.

Recommendation: Input validation and output encoding, even on redirected forms.


⚠️ 3.2 Critical XSS in Chat Interface (Confirmed)
Component: AI Chatbot Widget

Payload: <img src=q onerror=prompt(8)>

Result: Prompt dialog appeared — JavaScript executed in the browser.

Analysis: Unsanitized input was directly rendered in the DOM.

Risk Level: High

Recommendation:

Sanitize and encode all user input.

Use a CSP header.

Apply both client- and server-side input validation.


🔐 3.3 SQL Injection (Attempted)
Endpoint: Contact Form

Payload: ' OR '1'='1

Response: 302 redirect; no SQL errors or unusual behavior.

Analysis: Not conclusive. No evidence of SQLi, but blind testing needed.

Recommendation: Enforce prepared statements and parameterized queries.


📁 3.4 Path Traversal (Attempted)
Payload: ../../../../../../etc/passwd

Response: Redirect, no file content leaked.

Conclusion: No evidence of successful path traversal.

Recommendation: Sanitize file path inputs and avoid unsafe file access patterns.


🔐 3.5 Brute-Force Login Test (Django Admin)
Method: Burp Intruder, tested common credentials

Observation: All attempts returned 200 OK, but admin:admin returned a shorter response

Analysis: No lockout, no CAPTCHA; brute-force protection is weak

Recommendation:

Add CAPTCHA and rate-limiting

Implement account lockout policy

Enable MFA for admin users


🧩 Business Logic Testing
Observation: Contact form accepted any input and redirected without validation feedback.

Potential Issue: Misleading or broken workflow could be abused.

Recommendation: Add clear success/error messages and validate on both client/server side.


🧠 Summary of Findings
Vulnerability	Risk Level	Status
Reflected XSS	Medium	Suspected
Chatbot XSS	High	Confirmed
SQL Injection	Medium	Unconfirmed
Path Traversal	Low	Unconfirmed
Brute Force Login	Medium	Weak protection observed


🛡️ Recommendations
Sanitize and encode all user inputs.

Implement CAPTCHA, lockout, and 2FA for admin areas.

Use secure development practices (e.g., parameterized queries).

Deploy Content Security Policy (CSP) headers.

Test dynamic components like chatbots for injection.


✅ Ethical Disclosure
This test was conducted under permission as part of a cybersecurity internship. All sensitive identifiers (e.g., domain, IP, company name) have been redacted to maintain ethical and responsible disclosure.
