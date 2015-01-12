# Validation Pattern Test Cases

Test cases for patterns in the main README and VALIDATION.md.
Use these to verify patterns work correctly in your environment.

---

## Email

Pattern: `^[a-zA-Z0-9._%+\-]+@[a-zA-Z0-9.\-]+\.[a-zA-Z]{2,}$`

**Should match:**
- `user@example.com`
- `user.name@example.com`
- `user+tag@example.com`
- `user@sub.domain.com`
- `user@domain.co.uk`
- `123@domain.com`
- `user_name@domain.com`

**Should not match:**
- `@domain.com`
- `user@`
- `user@domain`
- `user @domain.com`
- `user@@domain.com`
- (empty string)

---

## US Phone

Pattern: `^(\+1[\s\-]?)?\(?\d{3}\)?[\s\-\.]?\d{3}[\s\-\.]\d{4}(\s*(x|ext|extension)\.?\s*\d{1,6})?$`

**Should match:**
- `314-555-1234`
- `(314) 555-1234`
- `314.555.1234`
- `3145551234`
- `+1 314-555-1234`
- `+1-314-555-1234`
- `314-555-1234 x99`
- `314-555-1234 ext 100`
- `314-555-1234 extension 1234`

**Should not match:**
- `555-1234` (7 digits without area code)
- `314-555-12345` (too many digits)
- `314-5555-1234` (wrong grouping)
- `(314 555-1234` (mismatched parens)
- `abc-def-ghij`

---

## ZIP Code

Pattern: `^\d{5}(-\d{4})?$`

**Should match:**
- `63101`
- `00501`
- `99999`
- `63101-9234`
- `00501-1234`

**Should not match:**
- `6310` (too short)
- `631011` (too long without dash)
- `63101-123` (ZIP+4 too short)
- `63101-12345` (ZIP+4 too long)
- `6310A` (non-numeric)

---

## IPv4

Pattern: `^((25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)$`

**Should match:**
- `0.0.0.0`
- `192.168.1.1`
- `255.255.255.255`
- `10.0.0.1`
- `172.16.0.1`

**Should not match:**
- `256.0.0.1` (octet out of range)
- `192.168.1` (missing octet)
- `192.168.1.1.1` (too many octets)
- `192.168.01.1` (leading zero - debatable, some engines flag this)
- `192.168.1.` (trailing dot)

---

## ISO 8601 Date

Pattern: `^\d{4}-(0[1-9]|1[0-2])-(0[1-9]|[12]\d|3[01])$`

**Should match:**
- `2023-01-01`
- `2023-12-31`
- `2000-02-29` (note: passes regex, but Feb 29 only valid in leap years)
- `1999-06-15`

**Should not match:**
- `23-01-01` (2-digit year)
- `2023-13-01` (invalid month)
- `2023-01-32` (invalid day)
- `2023/01/01` (wrong separator)
- `2023-1-1` (no zero padding)
