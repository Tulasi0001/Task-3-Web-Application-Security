# Cross-Site Scripting (XSS) Testing

## Objective

Demonstrate common Cross-Site Scripting (XSS) vulnerabilities in DVWA, including:

- Stored XSS
- Reflected XSS
- Content Security Policy (CSP) mitigation

The testing was performed in a controlled local DVWA environment.

## Environment

- Application: Damn Vulnerable Web Application (DVWA)
- Testing Environment: Localhost
- Security Level: Low
- Tool: Web Browser
- Server: Apache
- Mitigation: Content Security Policy (CSP)

---

# 1. Stored XSS

## Description

Stored XSS occurs when malicious client-side script is submitted to an application, stored by the application, and later rendered to users without appropriate sanitization or output encoding.

## Attack Scenario

The DVWA Guestbook functionality was tested using a JavaScript payload in the message field.

Payload:

```html
<script>alert('Stored XSS')</script>
The payload was accepted by the application at the Low security level.

Observed Result

The browser executed the injected JavaScript and displayed an alert containing:

Stored XSS

The payload was also stored in the guestbook and executed again when the affected page was loaded.

Security Impact

Stored XSS can allow attacker-controlled JavaScript to execute in the browsers of users viewing the affected content.

Potential impacts include:

Unauthorized JavaScript execution
Manipulation of webpage content
Unauthorized actions within a user's session
Exposure of information accessible to client-side scripts
Mitigation

Recommended defenses include:

Validate input according to the expected data type.
Apply context-appropriate output encoding.
Sanitize HTML where HTML input is intentionally allowed.
Implement a restrictive Content Security Policy (CSP).
2. Reflected XSS
Description

Reflected XSS occurs when user-controlled input is immediately returned in the application's response without appropriate validation or output encoding.

Unlike Stored XSS, the malicious input is not permanently stored by the application.

Attack Scenario

The DVWA XSS functionality was tested with the following payload:

<script>alert('Reflected XSS')</script>

The payload was submitted through the application's input field.

Observed Result

The injected JavaScript was reflected into the application's response and executed by the browser.

An alert containing:

Reflected XSS

was displayed.

Security Impact

Reflected XSS can be used to execute attacker-controlled JavaScript when a victim interacts with a crafted request or link.

Potential impacts include:

Execution of unauthorized JavaScript
Modification of webpage content
Unauthorized actions in the user's browser session
Exposure of information accessible to client-side scripts
Mitigation

Recommended defenses include:

Validate user input.
Encode output before inserting it into HTML.
Avoid directly inserting untrusted input into executable contexts.
Implement an appropriate Content Security Policy.
3. Content Security Policy (CSP) Mitigation
Description

Content Security Policy is a browser security mechanism that restricts the sources from which scripts and other resources can be loaded.

For this testing environment, a CSP was configured through Apache.

Apache Configuration

The following security configuration was added:

<IfModule mod_headers.c>
    Header always set Content-Security-Policy "default-src 'self'; script-src 'self'; object-src 'none'; base-uri 'self'"
</IfModule>

The Apache configuration was then tested and reloaded.

Verification

The HTTP response headers were checked to confirm that the CSP header was being returned by the Apache server.

The configured policy included:

default-src 'self'
script-src 'self'
object-src 'none'
base-uri 'self'
Observed Result

The previously demonstrated inline JavaScript payload was no longer executed after the CSP was enabled.

This demonstrated that CSP provided an additional browser-side defense against the tested inline script execution.

Security Considerations

CSP should be treated as an additional security layer rather than a replacement for secure input handling and output encoding.

Conclusion

The XSS testing demonstrated both Stored and Reflected XSS vulnerabilities in the DVWA Low security environment.

Stored XSS demonstrated persistent malicious input, while Reflected XSS demonstrated immediate execution of attacker-controlled input.

A Content Security Policy was subsequently configured in Apache. After enabling the policy, the tested inline JavaScript was blocked, demonstrating CSP as an additional defense layer.
