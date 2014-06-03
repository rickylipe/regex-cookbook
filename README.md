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

```regex
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

---

### US Phone Number

```regex
^(\+1[\s\-]?)?\(?\d{3}\)?[\s\-\.]?\d{3}[\s\-\.]\d{4}(\s*(x|ext|extension)\.?\s*\d{1,6})?$
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

---

### US ZIP Code

```regex
^\d{5}(-\d{4})?$
```

Matches 5-digit ZIP codes and ZIP+4 format.

| Input | Match |
|---|---|
| `63101` | yes |
| `63101-9234` | yes |
| `6310` | no |
| `63101-92` | no |

---

### URL

```regex
^(https?:\/\/)?(www\.)?[-a-zA-Z0-9@:%._\+~#=]{1,256}\.[a-zA-Z0-9()]{1,6}([-a-zA-Z0-9()@:%_\+.~#?&\/=]*)$
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

```regex
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

---

### Credit Card Number (basic)

```regex
^(?:4[0-9]{12}(?:[0-9]{3})?|5[1-5][0-9]{14}|3[47][0-9]{13}|6(?:011|5[0-9]{2})[0-9]{12})$
```

Matches common card number formats (no spaces or dashes):
- Visa: starts with 4, 13 or 16 digits
- Mastercard: starts with 51-55, 16 digits
- Amex: starts with 34 or 37, 15 digits
- Discover: starts with 6011 or 65, 16 digits

> This validates format only. Use the Luhn algorithm to validate
> the number itself before any actual processing.

---

### Strong Password

```regex
^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&\-_#])[A-Za-z\d@$!%*?&\-_#]{8,}$
```

Requires at least 8 characters with: one lowercase, one uppercase,
one digit, and one special character.

Adjust the special character set and minimum length to match your
requirements.

---

## Extraction

### Extract Email Addresses from Text

```regex
[a-zA-Z0-9._%+\-]+@[a-zA-Z0-9.\-]+\.[a-zA-Z]{2,}
```

Use with global flag (`g`) to find all email addresses in a block of text.

---

### Extract US Phone Numbers from Text

```regex
(\+1[\s\-]?)?\(?\d{3}\)?[\s\-\.]?\d{3}[\s\-\.]\d{4}
```

Finds phone numbers embedded in text. Use with global flag.

---

### Extract URLs from Text

```regex
https?:\/\/(www\.)?[-a-zA-Z0-9@:%._\+~#=]{1,256}\.[a-zA-Z0-9()]{1,6}([-a-zA-Z0-9()@:%_\+.~#?&\/=]*)
```

Finds HTTP/HTTPS URLs in a block of text. Requires `http://` or `https://`
prefix - won't match bare domain names.

---

### Extract HTML Tags

```regex
<([a-z][a-z0-9]*)[^>]*>(.*?)<\/>
```

Captures an HTML tag name in group 1 and its content in group 2.
Not reliable for nested tags of the same type.

> Don't parse HTML with regex for anything serious. Use a proper
> HTML parser. This is fine for simple extraction tasks on known,
> controlled input.

---

### Extract Numbers from a String

```regex
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

```regex
#([a-zA-Z0-9_]+)
```

Captures the hashtag text (without the `#`) in group 1.

---

### Extract Mentions (@username)

```regex
@([a-zA-Z0-9_]+)
```

Captures the username (without `@`) in group 1.

---

## Formatting & Cleanup

### Remove HTML Tags

```regex
<[^>]*>
```

Replace with empty string to strip all HTML tags from a string.
Leaves the text content intact.

---

### Remove Extra Whitespace

```regex
\s{2,}
```

Replace with a single space to collapse multiple consecutive
whitespace characters.

---

### Remove Leading and Trailing Whitespace

```regex
^\s+|\s+$
```

Replace with empty string. Most languages have a native `trim()` -
use that instead when available.

---

### Normalize Line Endings

```regex

|
```

Replace with `
` to normalize Windows (`
`) and old Mac (``)
line endings to Unix (`
`).

---

### Strip Non-Numeric Characters

```regex
[^0-9]
```

Replace with empty string to keep only digits. Useful for normalizing
phone numbers and ZIP codes before storing.

---

### Strip Non-Alphanumeric Characters

```regex
[^a-zA-Z0-9]
```

Replace with empty string or a separator to sanitize strings for
use as slugs, IDs, or filenames.

---

### URL Slug

```regex
[^a-z0-9\-]
```

Use case-insensitive flag and replace with empty string after
lowercasing and replacing spaces with hyphens. Two-step process:

1. Replace spaces with `-`
2. Apply this pattern to remove remaining non-slug characters

---

## URLs & Web

### Extract Domain from URL

```regex
^(?:https?:\/\/)?(?:www\.)?([^\/\?#]+)
```

Captures the domain (without `www`) in group 1.

| Input | Group 1 |
|---|---|
| `https://www.example.com/path` | `example.com` |
| `http://sub.domain.com` | `sub.domain.com` |
| `example.com` | `example.com` |

---

### Extract Query String Parameters

```regex
[?&]([^=&#]+)=([^&#]*)
```

Captures parameter name in group 1 and value in group 2.
Use with global flag to get all parameters.

---

### Match a Specific Domain

```regex
^https?:\/\/(www\.)?example\.com(\/.*)?$
```

Replace `example\.com` with your domain. Matches with or without
`www` and any path.

---

## Dates & Times

### US Date Format (MM/DD/YYYY)

```regex
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

```regex
^\d{4}-(0[1-9]|1[0-2])-(0[1-9]|[12]\d|3[01])$
```

| Input | Match |
|---|---|
| `2023-12-31` | yes |
| `2023-1-5` | no |
| `23-12-31` | no |

---

### 12-Hour Time (HH:MM AM/PM)

```regex
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

```regex
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

```regex
^-?\d+$
```

Matches positive and negative whole numbers.

---

### Decimal / Float

```regex
^-?\d+(\.\d+)?$
```

Matches integers and decimals. Requires digits on both sides of
the decimal point.

---

### Currency (US)

```regex
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

---

### Hexadecimal Color Code

```regex
^#([A-Fa-f0-9]{6}|[A-Fa-f0-9]{3})$
```

Matches both 3 and 6 character hex color codes with leading `#`.

| Input | Match |
|---|---|
| `#FF5733` | yes |
| `#f57` | yes |
| `#GG5733` | no |
| `FF5733` | no |

---

### Percentage

```regex
^(100|[0-9]{1,2})(\.[0-9]+)?%?$
```

Matches 0-100 with optional decimal and optional `%` sign.

---

## Miscellaneous

### US Social Security Number

```regex
^(?!000|666|9\d{2})\d{3}-(?!00)\d{2}-(?!0000)\d{4}$
```

Matches valid SSN format (XXX-XX-XXXX) while excluding known
invalid ranges (000, 666, 900-999 as area numbers).

> Never store SSNs in plaintext. This pattern is for format
> validation only.

---

### US State Abbreviation

```regex
^(AL|AK|AZ|AR|CA|CO|CT|DE|FL|GA|HI|ID|IL|IN|IA|KS|KY|LA|ME|MD|MA|MI|MN|MS|MO|MT|NE|NV|NH|NJ|NM|NY|NC|ND|OH|OK|OR|PA|RI|SC|SD|TN|TX|UT|VT|VA|WA|WV|WI|WY|DC|PR|GU|VI|AS|MP)$
```

Matches all 50 states plus DC and territories. Case sensitive -
use case insensitive flag (`i`) if needed.

---

### Username

```regex
^[a-zA-Z0-9_\-]{3,20}$
```

3-20 characters, letters, numbers, underscores, and hyphens only.
Adjust length limits to match your requirements.

---

### UUID (v4)

```regex
^[0-9a-f]{8}-[0-9a-f]{4}-4[0-9a-f]{3}-[89ab][0-9a-f]{3}-[0-9a-f]{12}$
```

Matches a properly formatted UUID v4. Use case insensitive flag for
uppercase UUIDs.

---

### Blank Lines

```regex
^\s*$
```

Use with multiline flag (`m`) to match empty or whitespace-only lines.
Useful for cleaning up text output.

---

### Repeated Words

```regex
(\w+)\s+
```

Matches accidentally repeated words like "the the" or "is is".
Case sensitive - use `i` flag for case insensitive matching.

---

### HTML Comment

```regex
<!--[\s\S]*?-->
```

Matches HTML comments including multiline ones. The `[\s\S]*?`
pattern matches any character including newlines.

---

## Contributing

If you have a pattern that's earned a permanent spot in your toolkit,
open a PR. Include a plain English explanation, at least three test
cases, and any relevant edge case notes.

## License

MIT
