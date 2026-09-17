# SQL Injection — Attack Scenario

## Objective

Test whether user-controlled input in the DVWA SQL Injection module can alter SQL query logic and expose unintended database records.

## Test Method

1. DVWA security level was set to Low.
2. A normal numeric input was submitted to establish expected behavior.
3. Crafted SQL input was supplied to test whether the application was vulnerable to SQL Injection.
4. A UNION-based query was used in the controlled DVWA database to demonstrate unintended data retrieval.

## Observed Results

The normal input returned the expected user record. A boolean-based injection returned multiple records, demonstrating that the supplied input could alter query logic.

A UNION-based test returned usernames and stored password hashes from the DVWA `users` table.

> The values obtained were password **hashes**, not plaintext passwords.

## Vulnerable Implementation

The vulnerable implementation directly incorporated the request parameter into the SQL statement. This allowed specially crafted input to change the intended query structure.

## Impact

A successful SQL Injection vulnerability can allow unauthorized database queries and may expose application data depending on database privileges and application design.

## Verification of Protection

After applying the prepared-statement approach in the protected DVWA configuration, the same class of malicious input no longer produced the unintended multi-record result.

## Evidence

See the `screenshots/` directory for the SQL Injection evidence captured during testing.
