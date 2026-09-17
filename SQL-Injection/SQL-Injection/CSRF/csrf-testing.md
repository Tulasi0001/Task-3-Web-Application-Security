# Cross-Site Request Forgery (CSRF) Testing

## Objective

Demonstrate a Cross-Site Request Forgery (CSRF) vulnerability in DVWA by testing whether a user's password can be changed through a request without requiring a valid CSRF token.

The testing also demonstrates token-based CSRF protection.

## Environment

- Application: Damn Vulnerable Web Application (DVWA)
- Testing Environment: Localhost
- Security Level: Low / Impossible
- Tool: Web Browser
- Authentication: DVWA user session



# 1. Legitimate Password Change

Before testing the CSRF vulnerability, a normal password-change operation was performed through the DVWA CSRF functionality.

The password was successfully changed using the application's legitimate form.

This established that the password-change functionality was working normally before testing the attack scenario.



# 2. CSRF Attack Scenario

## Description

Cross-Site Request Forgery occurs when an attacker causes a victim's authenticated browser to send an unwanted request to a web application.

If the application does not verify that the request originated from a legitimate form, an attacker may be able to perform actions using the victim's authenticated session.

## Vulnerable Implementation

At the Low security level, the password-change functionality does not require a CSRF token before processing the request.

The application reads the password parameters and performs the password update without validating a request-specific token.

Conceptually, the vulnerable request contains parameters such as:


password_new
password_conf
Change
without a CSRF token.

Forged Request

A request was manually constructed using the password-change parameters:

http://localhost/dvwa/vulnerabilities/csrf/?password_new=CSRFdemo123!&password_conf=CSRFdemo123!&Change=Change

The request was submitted while authenticated as the DVWA administrator.

Observed Result

The application returned:

Password Changed.

This demonstrated that the password-change operation could be triggered without supplying a CSRF token at the Low security level.

Note: This test demonstrates tokenless request acceptance in the controlled DVWA environment. It is not a separate third-party-origin webpage PoC.

3. Vulnerable Code

The Low security implementation does not perform a CSRF token validation before processing the password-change request.

The absence of request-token validation allows the password-change request to be processed based on the supplied parameters and the authenticated session.

4. Token-Based Protection
Description

A common defense against CSRF is to include a unique, unpredictable CSRF token with state-changing requests.

The server validates the submitted token against the token associated with the user's session before performing the requested operation.

Protected Implementation

At the Impossible security level, DVWA performs CSRF token validation.

The implementation contains a check equivalent to:

checkToken(
    $_REQUEST['user_token'],
    $_SESSION['session_token'],
    'index.php'
);

The request token is compared with the token stored in the user's session.

Only a request containing the expected token can proceed.

Protected Request Flow

The protected workflow is:

The server generates or maintains a session-specific CSRF token.
The legitimate form includes the token.
The browser submits the token with the request.
The server compares the submitted token with the session token.
The request is accepted only when the token is valid.
5. Verification of Protection

A tokenless password-change request was tested against the protected implementation.

The request did not contain a valid CSRF token.

Observed Result

The application rejected the request and displayed:

CSRF token is incorrect.

This demonstrated that the protected implementation prevents the tested tokenless password-change request.

6. Security Impact

A successful CSRF attack against a password-change function could allow an attacker to cause an authenticated user to change their password without their intended action.

Depending on the affected functionality, CSRF may also target:

Account settings
Email address changes
Profile modifications
Administrative actions
Other state-changing operations
7. Mitigation

Recommended CSRF defenses include:

Use unpredictable CSRF tokens for state-changing requests.
Validate the token on the server before processing the request.
Use appropriate SameSite cookie settings as an additional defense.
Require appropriate authentication or re-authentication for highly sensitive operations.
Do not rely solely on client-side validation.

Conclusion

The CSRF testing demonstrated that the DVWA Low security implementation accepts a password-change request without requiring a valid CSRF token.

The Impossible security implementation adds token validation before processing the request. A tokenless request was rejected with a CSRF token error, demonstrating the effectiveness of request-token validation against the tested attack scenario.
