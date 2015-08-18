# regex-cookbook

A curated reference of regular expressions for common real-world problems.

Organized by category. Each pattern includes a plain English explanation,
example matches, and notes on edge cases or limitations.

These are patterns I've reached for repeatedly across different projects
and languages. Not exhaustive - just the ones that actually get used.

## Categories

- [Validation](#validation)
- [Extraction](#extraction)
- [Formatting & Cleanup](#formatting--cleanup)
- [URLs & Web](#urls--web)
- [Dates & Times](#dates--times)
- [Numbers](#numbers)
- [Miscellaneous](#miscellaneous)

## Syntax Notes

All patterns are written in standard regex syntax compatible with most
languages and engines (PCRE, JavaScript, Python, PHP, etc.).

Flags are noted where relevant:
- `i` - case insensitive
- `g` - global (find all matches)
- `m` - multiline

---

## Validation

### Email Address

```
^[a-zA-Z0-9._%+\-]+@[a-zA-Z0-9.\-]+\.[a-zA-Z]{2,}$
```

Matches a valid email address format. This is intentionally permissive -
RFC 5322 compliant email validation is absurdly complex and unnecessary
for most use cases. This covers 99% of real addresses.

| Input | Match |
|---|---|
| `user@example.com` | yes |
| `user.name+tag@sub.domain.co.uk` | yes |
| `user@domain` | no |
| `@domain.com` | no |
| `user @domain.com` | no |

> Note: Always send a confirmation email rather than relying solely on
> regex validation. A regex can't tell you if the address actually exists.
> [Live example on regex101](https://regex101.com/r/y1fAXf/1)


---

### US Phone Number

```
^(\+1[\s\-]?)?\(?\d{3}\)?[\s\-\.]?\d{3}[\s\-\.]?\d{4}(\s*(x|ext|extension)\.?\s*\d{1,6})?$
```

Matches US phone numbers in most common formats, with optional country
code and extension.

| Input | Match |
|---|---|
| `314-555-1234` | yes |
| `(314) 555-1234` | yes |
| `314.555.1234` | yes |
| `+1 314 555 1234` | yes |
| `314-555-1234 x99` | yes |
| `5551234` | no |
| `314-555-12345` | no |
> [Live example on regex101](https://regex101.com/r/QOez5n/1)


---

### US ZIP Code

```
^\d{5}(-\d{4}|\d{4})?$
```

Matches 5-digit ZIP codes and ZIP+4 format.

| Input | Match |
|---|---|
| `63101` | yes |
| `63101-9234` | yes |
| `6310` | no |
| `63101-92` | no |
> [Live example on regex101](https://regex101.com/r/VvxeS1/1)


---

### URL

```
^(https?:\/\/)?(www\.)?[-a-zA-Z0-9@:%._\+~#=]{1,256}\.[a-zA-Z0-9()]{1,6}([-a-zA-Z0-9()@:%_\+.~#?&\/=]*)$
```

Matches HTTP and HTTPS URLs, with or without `www`. Handles paths,
query strings, and fragments.

| Input | Match |
|---|---|
| `https://example.com` | yes |
| `http://www.example.com/path?q=1#anchor` | yes |
| `example.com` | yes |
| `ftp://example.com` | no |
| `not a url` | no |

> For strict URL validation, use your language's built-in URL parser
> instead. This pattern is useful for quick checks, not security validation.

---

### IP Address (IPv4)

```
^((25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)$
```

Matches valid IPv4 addresses only (0-255 per octet).

| Input | Match |
|---|---|
| `192.168.1.1` | yes |
| `255.255.255.0` | yes |
| `0.0.0.0` | yes |
| `256.1.1.1` | no |
| `192.168.1` | no |
> [Live example on regex101](https://regex101.com/r/ZEmHyk/1)


---

### Credit Card Number (basic)

```
^(?:4[0-9]{12}(?:[0-9]{3})?|5[1-5][0-9]{14}|3[47][0-9]{13}|6(?:011|5[0-9]{2})[0-9]{12})$
```

Matches common card number formats (no spaces or dashes):
- Visa: starts with 4, 13 or 16 digits
- Mastercard: starts with 51-55, 16 digits
- Amex: starts with 34 or 37, 15 digits
- Discover: starts with 6011 or 65, 16 digits

| Input | Match |
|---|---|
| `4111111111111111` | yes (Visa) |
| `5500005555555559` | yes (Mastercard) |
| `371449635398431` | yes (Amex) |
| `1234567890123456` | no |

> This validates format only. Use the Luhn algorithm to validate
> the number itself before any actual processing.
> [Live example on regex101](https://regex101.com/r/WoQiaB/1)


---

### Strong Password

```
^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&\-_#])[A-Za-z\d@$!%*?&\-_#]{8,}$
```

Requires at least 8 characters with: one lowercase, one uppercase,
one digit, and one special character.

| Input | Match |
|---|---|
| `Password1!` | yes |
| `Tr0ub4dor&3` | yes |
| `password1!` | no (no uppercase) |
| `PASSWORD1!` | no (no lowercase) |
| `Password!` | no (no digit) |
| `Pass1!` | no (too short) |
> [Live example on regex101](https://regex101.com/r/mbchNt/1)


Adjust the special character set and minimum length to match your
requirements.

---

## Extraction

### Extract Email Addresses from Text

```
[a-zA-Z0-9._%+\-]+@[a-zA-Z0-9.\-]+\.[a-zA-Z]{2,}
```

Use with global flag (`g`) to find all email addresses in a block of text.

| Input | Matches |
|---|---|
| `Contact us at info@example.com or support@example.com` | `info@example.com`, `support@example.com` |
| `Send to user.name+tag@domain.co.uk` | `user.name+tag@domain.co.uk` |
> [Live example on regex101](https://regex101.com/r/jIuIAZ/1)


---

### Extract US Phone Numbers from Text

```
(\+1[\s\-]?)?\(?\d{3}\)?[\s\-\.]?\d{3}[\s\-\.]\d{4}
```

Finds phone numbers embedded in text. Use with global flag.

| Input | Matches |
|---|---|
| `Call 314-555-1234 or (800) 555-9876` | `314-555-1234`, `(800) 555-9876` |

---

### Extract URLs from Text

```
https?:\/\/(www\.)?[-a-zA-Z0-9@:%._\+~#=]{1,256}\.[a-zA-Z0-9()]{1,6}([-a-zA-Z0-9()@:%_\+.~#?&\/=]*)
```

Finds HTTP/HTTPS URLs in a block of text. Requires `http://` or `https://`
prefix - won't match bare domain names.

| Input | Matches |
|---|---|
| `Visit https://example.com or http://www.test.org/path` | `https://example.com`, `http://www.test.org/path` |
| `Go to example.com` | no match (no protocol) |
> [Live example on regex101](https://regex101.com/r/xrmNC7/1)


---

### Extract HTML Tags

```
<([a-z][a-z0-9]*)\b[^>]*>(.*?)<\/\1>
```

Captures an HTML tag name in group 1 and its content in group 2.
Not reliable for nested tags of the same type.

| Input | Group 1 | Group 2 |
|---|---|---|
| `<p>Hello world</p>` | `p` | `Hello world` |
| `<a href="/about">About</a>` | `a` | `About` |

> Don't parse HTML with regex for anything serious. Use a proper
> HTML parser. This is fine for simple extraction tasks on known,
> controlled input.

---

### Extract Numbers from a String

```
-?\d+(\.\d+)?
```

Finds integers and decimals, including negatives.

| Input | Matches |
|---|---|
| `Price: $14.99` | `14.99` |
| `Temp: -22.5 degrees` | `-22.5` |
| `3 items at 2.5 each` | `3`, `2.5` |

---

### Extract Hashtags

```
#([a-zA-Z0-9_]+)
```

Captures the hashtag text (without the `#`) in group 1.

| Input | Group 1 |
|---|---|
| `Loving #regex and #coding today` | `regex`, `coding` |
| `Price is $14.99` | no match ($ not #) |
> [Live example on regex101](https://regex101.com/r/e9j9Wa/1)


---

### Extract Mentions (@username)

```
@([a-zA-Z0-9_]+)
```

Captures the username (without `@`) in group 1.

| Input | Group 1 |
|---|---|
| `Thanks @rickylipe for the help` | `rickylipe` |
| `Email user@example.com` | `example` (partial - use email pattern instead for emails) |
> [Live example on regex101](https://regex101.com/r/9AVSJs/1)


---

## Formatting & Cleanup

### Remove HTML Tags

```
<[^>]*>
```

Replace with empty string to strip all HTML tags from a string.
Leaves the text content intact.

| Input | Output after replace |
|---|---|
| `<p>Hello <strong>world</strong></p>` | `Hello world` |
| `<a href="/page">Click here</a>` | `Click here` |
> [Live example on regex101](https://regex101.com/r/NlaPt5/1)


---

### Remove Extra Whitespace

```
\s{2,}
```

Replace with a single space to collapse multiple consecutive
whitespace characters.

| Input | Output after replace |
|---|---|
| `Hello   world` | `Hello world` |
| `too    many    spaces` | `too many spaces` |

---

### Remove Leading and Trailing Whitespace

```
^\s+|\s+$
```

Replace with empty string. Most languages have a native `trim()` -
use that instead when available.

---


---

### Strip Non-Numeric Characters

```
[^0-9]
```

Replace with empty string to keep only digits. Useful for normalizing
phone numbers and ZIP codes before storing.

| Input | Output after replace |
|---|---|
| `(314) 555-1234` | `3145551234` |
| `$1,299.99` | `129999` |

---

### Strip Non-Alphanumeric Characters

```
[^a-zA-Z0-9]
```

Replace with empty string or a separator to sanitize strings for
use as slugs, IDs, or filenames.

---

### URL Slug

```
[^a-z0-9\-]
```

Use case-insensitive flag and replace with empty string after
lowercasing and replacing spaces with hyphens. Two-step process:

1. Replace spaces with `-`
2. Apply this pattern to remove remaining non-slug characters

| Input | Output after both steps |
|---|---|
| `Hello World!` | `hello-world` |
| `My Favorite Résumé` | `my-favorite-rsum` (accent stripped) |
> [Live example on regex101](https://regex101.com/r/Oulq6K/1)


---

## URLs & Web

### Extract Domain from URL

```
^(?:https?:\/\/)?(?:www\.)?([^\/\?#]+)
```

Captures the domain (without `www`) in group 1.

| Input | Group 1 |
|---|---|
| `https://www.example.com/path` | `example.com` |
| `http://sub.domain.com` | `sub.domain.com` |
| `example.com` | `example.com` |
> [Live example on regex101](https://regex101.com/r/fBdIS8/1)


---


---

### Match a Specific Domain

```
^https?:\/\/(www\.)?example\.com(\/.*)?$
```

Replace `example\.com` with your domain. Matches with or without
`www` and any path.

| Input | Match |
|---|---|
| `https://example.com` | yes |
| `https://www.example.com/about` | yes |
| `https://other.com` | no |

---

## Dates & Times

### US Date Format (MM/DD/YYYY)

```
^(0?[1-9]|1[0-2])\/(0?[1-9]|[12][0-9]|3[01])\/([12][0-9]{3})$
```

| Input | Match |
|---|---|
| `12/31/2023` | yes |
| `1/5/2023` | yes |
| `13/01/2023` | no |
| `12/32/2023` | no |

> This validates format and range but not calendar validity
> (e.g., Feb 30 would pass). Use a date library for full validation.

---

### ISO 8601 Date (YYYY-MM-DD)

```
^\d{4}([/\-.])(0[1-9]|1[0-2])\1(0[1-9]|[12]\d|3[01])$
```

| Input | Match |
|---|---|
| `2023-12-31` | yes |
| `2023-1-5` | no |
| `23-12-31` | no |
> [Live example on regex101](https://regex101.com/r/FnRwHb/1)


---

### 12-Hour Time (HH:MM AM/PM)

```
^(0?[1-9]|1[0-2]):[0-5][0-9]\s?(AM|PM|am|pm)$
```

| Input | Match |
|---|---|
| `12:30 PM` | yes |
| `1:05am` | yes |
| `13:00 PM` | no |
| `12:60 PM` | no |

---

### 24-Hour Time (HH:MM)

```
^([01]?[0-9]|2[0-3]):[0-5][0-9]$
```

| Input | Match |
|---|---|
| `23:59` | yes |
| `00:00` | yes |
| `9:05` | yes |
| `24:00` | no |

---

## Numbers

### Integer

```
^-?\d+$
```

Matches positive and negative whole numbers.

| Input | Match |
|---|---|
| `42` | yes |
| `-7` | yes |
| `3.14` | no |
| `1e10` | no |

---

### Decimal / Float

```
^-?\d+(\.\d+)?$
```

Matches integers and decimals. Requires digits on both sides of
the decimal point.

| Input | Match |
|---|---|
| `3.14` | yes |
| `-0.5` | yes |
| `.5` | no (no leading digit) |
| `3.` | no (no trailing digit) |

---

### Currency (US)

```
^\$?([0-9]{1,3},([0-9]{3},)*[0-9]{3}|[0-9]+)(\.[0-9]{2})?$
```

Matches US dollar amounts with optional `$`, optional thousands
separators, and optional cents.

| Input | Match |
|---|---|
| `$1,234.56` | yes |
| `1234.56` | yes |
| `$0.99` | yes |
| `1,23.45` | no |
> [Live example on regex101](https://regex101.com/r/KnKwQD/1)


---

### Hexadecimal Color Code

**Validation (requires `#`):**
```
^#([A-Fa-f0-9]{6}|[A-Fa-f0-9]{3})$
```

**Normalization (adds `#` if missing):**

Use a two-step approach in your code:

```javascript
// JavaScript example
function normalizeHexColor(input) {
    const stripped = input.replace(/^#/, '');
    if (/^([A-Fa-f0-9]{6}|[A-Fa-f0-9]{3})$/.test(stripped)) {
        return '#' + stripped.toUpperCase();
    }
    return null; // invalid
}

normalizeHexColor('#FF5733');  // '#FF5733'
normalizeHexColor('FF5733');   // '#FF5733'
normalizeHexColor('#f57');     // '#F57'
normalizeHexColor('xyz');      // null
```

| Input | Normalized Output |
|---|---|
| `#FF5733` | `#FF5733` |
| `FF5733` | `#FF5733` |
| `#f57` | `#F57` |
| `f57` | `#F57` |
| `#GG5733` | `null` (invalid) |
> [Live example on regex101](https://regex101.com/r/I8SJXE/1)


---

### Percentage

```
^(100|[0-9]{1,2})(\.[0-9]+)?%?$
```

Matches 0-100 with optional decimal and optional `%` sign.

| Input | Match |
|---|---|
| `75` | yes |
| `99.9%` | yes |
| `100` | yes |
| `101` | no |
> [Live example on regex101](https://regex101.com/r/RDXcjw/1)


---

## Miscellaneous

### US Social Security Number

```
^(?!000|666|9[0-9]{2})\d{3}-(?!00)\d{2}-(?!0000)\d{4}$
```

Matches valid SSN format (XXX-XX-XXXX) while excluding known
invalid ranges (000, 666, 900-999 as area numbers).

| Input | Match |
|---|---|
| `123-45-6789` | yes |
| `000-45-6789` | no (invalid area) |
| `123-00-6789` | no (invalid group) |
| `123456789` | no (missing dashes) |

> Never store SSNs in plaintext. This pattern is for format
> validation only.

---

### US State Abbreviation

```
^(AL|AK|AZ|AR|CA|CO|CT|DE|FL|GA|HI|ID|IL|IN|IA|KS|KY|LA|ME|MD|MA|MI|MN|MS|MO|MT|NE|NV|NH|NJ|NM|NY|NC|ND|OH|OK|OR|PA|RI|SC|SD|TN|TX|UT|VT|VA|WA|WV|WI|WY|DC|PR|GU|VI|AS|MP)$
```

Matches all 50 states plus DC and US territories.

The pattern is uppercase. To make it case insensitive, add the `i` flag
for your language:

```javascript
// JavaScript
/^(AL|AK|AZ|...)$/i.test('mo')  // true
```
```php
// PHP
preg_match('/^(AL|AK|AZ|...)$/i', 'mo')  // 1
```
```python
# Python
re.match(r'^(AL|AK|AZ|...)$', 'mo', re.IGNORECASE)  // match
```
```sql
-- MySQL (case insensitive by default)
SELECT 'mo' REGEXP '^(AL|AK|AZ|...)$';  -- 1
```

| Input | Match (case insensitive) |
|---|---|
| `MO` | yes |
| `mo` | yes |
| `Mo` | yes |
| `ZZ` | no |

---

### Username

```
^[a-zA-Z0-9_\-]{3,20}$
```

3-20 characters, letters, numbers, underscores, and hyphens only.

| Input | Match |
|---|---|
| `rickylipe` | yes |
| `user_name-1` | yes |
| `ab` | no (too short) |
| `user name` | no (space not allowed) |
> [Live example on regex101](https://regex101.com/r/RQEcVI/1)


---

### UUID (v4)

```
^[0-9a-f]{8}-[0-9a-f]{4}-4[0-9a-f]{3}-[89ab][0-9a-f]{3}-[0-9a-f]{12}$
```

Matches a properly formatted UUID v4. Use case insensitive flag for
uppercase UUIDs.

| Input | Match |
|---|---|
| `550e8400-e29b-41d4-a716-446655440000` | yes |
| `550e8400-e29b-31d4-a716-446655440000` | no (v3, not v4) |
| `not-a-uuid` | no |
> [Live example on regex101](https://regex101.com/r/9hbYOZ/1)


---

### Blank Lines

```
^\s*$
```

Use with multiline flag (`m`) to match empty or whitespace-only lines.
Useful for cleaning up text output.

| Input (multiline) | Matches |
|---|---|
| Line 1\n\nLine 3 | the empty line between them |
| Line 1\n   \nLine 3 | the whitespace-only line |

---

### Repeated Words

```
(\w+)\s+\1
```

Matches accidentally repeated words like "the the" or "is is".
Group 1 captures the repeated word. Use case insensitive flag (`i`)
to catch "The the" as well.

> Note: `\1` is a backreference - it matches whatever was captured
> in group 1. So `(\w+)\s+\1` means "a word, then whitespace,
> then the exact same word again."

| Input | Match |
|---|---|
| `This is is wrong` | `is is` |
| `the the problem` | `the the` |
| `This is fine` | no match |
> [Live example on regex101](https://regex101.com/r/javhWK/1)


---

### HTML Comment

```
<!--[\s\S]*?-->
```

Matches HTML comments including multiline ones. The `[\s\S]*?`
pattern matches any character including newlines, non-greedy.

| Input | Match |
|---|---|
| `<!-- single line comment -->` | yes |
| `<!--\nmultiline\ncomment\n-->` | yes |
| `/* not html */` | no |
> [Live example on regex101](https://regex101.com/r/k6j2c6/1)


---

## Extended References

The main patterns above cover the most common use cases. For more:

- [Extended Validation Patterns](sections/VALIDATION.md) - postal codes, IBAN, VIN, MAC address, semver, ISBN, coordinates
- [Text Extraction Patterns](sections/EXTRACTION.md) - pulling data out of larger text blocks
- [Regex Basics](BASICS.md) - what all the symbols mean, how patterns are constructed
- [Language-Specific Notes](LANGUAGE-NOTES.md) - syntax differences between JS, PHP, Python, and MySQL
- [Validation Test Cases](tests/validation-tests.md) - test inputs for verifying patterns in your environment
- [Tools & Resources](RESOURCES.md) - recommended tools, testers, and references

## License

MIT
