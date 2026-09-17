# SQL Injection — Mitigation Notes

## Primary Mitigation: Prepared Statements

Use prepared statements / parameterized queries for database operations involving user input. The SQL structure is separated from the supplied value, preventing the value from being interpreted as SQL syntax.

### Secure Approach

```php
$stmt = $db->prepare(
    "SELECT first_name, last_name FROM users WHERE user_id = ?"
);
