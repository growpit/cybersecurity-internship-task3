# Task 3 – Web Application Security Testing Report

## 1. Objective

The objective of this task was to identify and demonstrate common web application security vulnerabilities in a controlled DVWA lab environment and document suitable mitigation techniques.

## 2. Lab Environment

- Target: Damn Vulnerable Web Application (DVWA)
- Environment: Kali Linux
- Target URL: http://127.0.0.1/DVWA/
- Testing Type: Controlled local laboratory
- Tools: DVWA, Browser, Burp Suite

## 3. SQL Injection

### Vulnerability

The SQL Injection vulnerability was tested through the DVWA SQL Injection module.

### Test Payload

`1' OR '1'='1`

The payload caused the application to return multiple user records instead of only the requested user.

Further testing demonstrated UNION-based SQL injection and exposed information from the `users` table, including usernames and password hashes.

### Impact

An attacker could potentially bypass intended database queries and access unauthorized information.

### Mitigation

- Use prepared statements / parameterized queries.
- Never concatenate user input directly into SQL queries.
- Validate and sanitize input.
- Apply least-privilege permissions to database accounts.

## 4. Cross-Site Scripting (XSS)

### 4.1 Reflected XSS

Payload tested:

`<script>alert(1)</script>`

The JavaScript executed in the browser, demonstrating reflected XSS.

### 4.2 Stored XSS

The same payload was submitted through the stored XSS functionality. The script executed when the stored content was displayed.

### 4.3 DOM XSS

A JavaScript payload supplied through the URL parameter was executed by the client-side application, demonstrating DOM-based XSS.

### Impact

XSS can allow malicious JavaScript to execute in a victim's browser and may lead to session theft, phishing, or unauthorized actions.

### Mitigation

- Apply context-aware output encoding.
- Validate and sanitize user input.
- Use secure frameworks and templating mechanisms.
- Implement an appropriate Content Security Policy (CSP).
- Avoid unsafe DOM APIs such as `innerHTML` when handling untrusted data.

## 5. Cross-Site Request Forgery (CSRF)

The DVWA CSRF module was tested using its password-change functionality.

The application accepted a password-change request containing the required parameters, demonstrating the risk of performing sensitive actions without sufficient request protection.

### Impact

An attacker could potentially cause an authenticated victim's browser to perform an unwanted state-changing action.

### Mitigation

- Use unpredictable CSRF tokens.
- Validate the token on every state-changing request.
- Use appropriate SameSite cookie settings.
- Verify the request origin where appropriate.
- Require re-authentication for highly sensitive actions.

## 6. File Inclusion

The DVWA File Inclusion functionality was tested.

A local PHP file was successfully accessed through the application's file inclusion functionality.

Remote File Inclusion was not demonstrated because the lab configuration had `allow_url_include` disabled.

### Mitigation

- Do not allow user-controlled file paths.
- Use an allowlist of permitted files.
- Normalize and validate paths.
- Disable unnecessary remote file inclusion functionality.

## 7. Security Recommendations

The application should implement:

1. Prepared statements for database queries.
2. Strong server-side input validation.
3. Context-aware output encoding.
4. CSRF tokens for state-changing requests.
5. Secure session and cookie configuration.
6. Content Security Policy headers.
7. Secure HTTP response headers.
8. Least-privilege database permissions.
9. Regular vulnerability assessments and security testing.

## 8. Conclusion

The testing demonstrated several common web application vulnerabilities in a controlled DVWA environment, including SQL Injection, Reflected XSS, Stored XSS, DOM XSS, CSRF and Local File Inclusion.

The exercises also demonstrated why secure coding practices such as parameterized queries, input validation, output encoding, CSRF tokens and security headers are important for protecting web applications.
