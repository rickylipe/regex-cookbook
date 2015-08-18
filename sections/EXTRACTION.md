# Extraction Patterns

Patterns for pulling specific data out of larger blocks of text.

**About the global flag:** Most extraction patterns are meant to find
*all* occurrences in a string, not just the first one. By default, most
regex engines stop after the first match. The global flag (`g` in
JavaScript, or using `findall`/`match_all` in other languages) tells
the engine to keep going and return every match.

```javascript
// Without global flag - only first match
'call 314-555-1234 or 800-555-9876'.match(/\d{3}-\d{3}-\d{4}/)
// ['314-555-1234']

// With global flag - all matches
'call 314-555-1234 or 800-555-9876'.match(/\d{3}-\d{3}-\d{4}/g)
// ['314-555-1234', '800-555-9876']
```

Patterns below are noted when a different behavior is expected.

---

## Extract All URLs

```
https?:\/\/(www\.)?[-a-zA-Z0-9@:%._\+~#=]{1,256}\.[a-zA-Z0-9()]{1,6}([-a-zA-Z0-9()@:%_\+.~#?&\/=]*)
```

Finds all HTTP/HTTPS URLs in text. Requires protocol prefix.

| Input | Matches |
|---|---|
| `See https://example.com and http://test.org/page` | `https://example.com`, `http://test.org/page` |
| `Visit example.com` | no match (no protocol) |
> [Live example on regex101](https://regex101.com/r/xrmNC7/1)

| `Download at https://files.example.com/doc.pdf?v=2` | `https://files.example.com/doc.pdf?v=2` |

---

## Extract All Email Addresses

```
[a-zA-Z0-9._%+\-]+@[a-zA-Z0-9.\-]+\.[a-zA-Z]{2,}
```

| Input | Matches |
|---|---|
| `Contact info@example.com or support@help.org` | `info@example.com`, `support@help.org` |
| `user.name+tag@sub.domain.co.uk` | `user.name+tag@sub.domain.co.uk` |
> [Live example on regex101](https://regex101.com/r/jIuIAZ/1)

| `not an email` | no match |

---

## Extract All Phone Numbers (US)

```
(\+1[\s\-]?)?\(?\d{3}\)?[\s\-\.]?\d{3}[\s\-\.]\d{4}
```

| Input | Matches |
|---|---|
| `Call 314-555-1234 or (800) 555-9876` | `314-555-1234`, `(800) 555-9876` |
| `Phone: +1 314.555.1234` | `+1 314.555.1234` |
> [Live example on regex101](https://regex101.com/r/QOez5n/1)


---

## Extract Prices

```
\$\s?[0-9]+(\.[0-9]{2})?
```

Captures dollar amounts with optional cents.

| Input | Matches |
|---|---|
| `Total: $14.99` | `$14.99` |
| `From $5 to $50.00` | `$5`, `$50.00` |
| `Save $10 today` | `$10` |
| `Price: 14.99` | no match (no $) |
> [Live example on regex101](https://regex101.com/r/nXMZpH/1)


---

## Extract Dates (Multiple Formats)

```
(0?[1-9]|1[0-2])[\/\-\.](0?[1-9]|[12][0-9]|3[01])[\/\-\.]([0-9]{2,4})
```

Matches MM/DD/YYYY, MM-DD-YYYY, MM.DD.YY etc.

| Input | Matches |
|---|---|
| `Expires 12/31/2025` | `12/31/2025` |
| `Meeting on 1-5-2024 and 2-14-2024` | `1-5-2024`, `2-14-2024` |
| `Date: 06.15.23` | `06.15.23` |
> [Live example on regex101](https://regex101.com/r/FnRwHb/1)


---

## Extract Words in ALL CAPS

```
[A-Z]{2,}
```

Matches sequences of 2 or more uppercase letters. Useful for finding
acronyms or emphasis text. Add word boundary alternative `(?<![A-Z])` at
start and `(?![A-Z])` at end if you need isolated words only.

| Input | Matches |
|---|---|
| `The CIA and FBI are US agencies` | `CIA`, `FBI`, `US` |
| `ERROR: connection FAILED` | `ERROR`, `FAILED` |
> [Live example on regex101](https://regex101.com/r/G3Q0da/1)

| `Hello World` | no match (only one cap per word) |

---

## Extract Content Between Tags

```
<tag>([\s\S]*?)<\/tag>
```

Replace `tag` with the tag name. Captures content in group 1.

| Input | Group 1 |
|---|---|
| `<title>My Page</title>` | `My Page` |
| `<p>Hello world</p>` | `Hello world` |
| `<div>line1\nline2</div>` | `line1\nline2` |
> [Live example on regex101](https://regex101.com/r/ywil3M/1)


---

## Extract First N Words

```
^(\S+\s+){0,4}\S+
```

This example captures the first 5 words. Change `{0,4}` to `{0,N-1}`
for a different count.

| Input | Match |
|---|---|
| `one two three four five six seven` | `one two three four five` |
| `only three words` | `only three words` |

---

## Extract Content in Parentheses

```
\(([^)]+)\)
```

Captures text inside parentheses in group 1. Does not handle nested
parentheses.

| Input | Group 1 |
|---|---|
| `Result (see appendix)` | `see appendix` |
| `Price (USD): 14.99` | `USD` |
| `No parens here` | no match |
> [Live example on regex101](https://regex101.com/r/l9JJHV/1)


---

## Extract Content in Quotes

Double quotes:
```
"([^"]*)"
```

Single quotes:
```
'([^']*)'
```

| Input | Group 1 |
|---|---|
| `He said "hello world"` | `hello world` |
| `class='active'` | `active` |
> [Live example on regex101](https://regex101.com/r/1MvinV/1)


---

## Extract File Extension

```
\.([a-zA-Z0-9]+)$
```

Captures the extension (without dot) in group 1.

| Input | Group 1 |
|---|---|
| `document.pdf` | `pdf` |
| `archive.tar.gz` | `gz` (last extension only) |
| `image.JPEG` | `JPEG` |
| `README` | no match |
> [Live example on regex101](https://regex101.com/r/DPKlxx/1)


---

## Extract Filename from Path

```
[^\\/]+(?=\.[^\\/]+$)
```

Captures filename without extension. Works with both forward
and backslashes.

| Input | Match |
|---|---|
| `/var/www/html/index.html` | `index` |
| `C:\Users\ricky\document.pdf` | `document` |
| `archive.tar.gz` | `archive.tar` |

---

## Extract IP Addresses from Logs

```
(?<!\d)(\d{1,3}\.){3}\d{1,3}(?!\d)
```

Finds IPv4 addresses in text without matching partial numbers.

| Input | Matches |
|---|---|
| `Request from 192.168.1.1 at 10:30` | `192.168.1.1` |
| `Failed: 10.0.0.1 and 172.16.0.55` | `10.0.0.1`, `172.16.0.55` |
| `Not an IP: 999.999.999.999` | matches (use validation pattern to check range) |
> [Live example on regex101](https://regex101.com/r/G3Q0da/1)


---

## Extract JSON Values by Key

```
"key"\s*:\s*"([^"]*)"
```

Replace `key` with the field name. Captures the string value in group 1.
Only works for string values - not reliable for nested objects or arrays.

| Input | Group 1 |
|---|---|
| `{"name": "Ricky", "city": "St. Louis"}` | `Ricky` (for key `name`) |

> Use a proper JSON parser for anything beyond simple one-off extraction.

---

## Extract CSS Color Values

Hex colors (with `#`):
```
#([A-Fa-f0-9]{6}|[A-Fa-f0-9]{3})
```

| Input | Matches |
|---|---|
| `color: #FF5733; background: #fff` | `#FF5733`, `#fff` |
| `color: rgb(255, 0, 0)` | no match |
> [Live example on regex101](https://regex101.com/r/NlaPt5/1)


RGB values:
```
rgb\(\s*(\d{1,3})\s*,\s*(\d{1,3})\s*,\s*(\d{1,3})\s*\)
```

| Input | Matches |
|---|---|
| `color: rgb(255, 87, 51)` | `rgb(255, 87, 51)` - groups: `255`, `87`, `51` |
| `rgb(0,0,0)` | `rgb(0,0,0)` - groups: `0`, `0`, `0` |
| `color: #FF5733` | no match |

HSL values:
```
hsl\(\s*(\d{1,3})\s*,\s*(\d{1,3})%\s*,\s*(\d{1,3})%\s*\)
```

| Input | Matches |
|---|---|
| `color: hsl(9, 100%, 60%)` | `hsl(9, 100%, 60%)` - groups: `9`, `100`, `60` |
| `hsl(0, 0%, 0%)` | `hsl(0, 0%, 0%)` |
| `color: rgb(255, 0, 0)` | no match |

---

## Extract Hashtags

```
#([a-zA-Z0-9_]+)
```

| Input | Group 1 |
|---|---|
| `Loving #regex and #coding` | `regex`, `coding` |
| `Price: $14.99` | no match |

---

## Extract Mentions

```
@([a-zA-Z0-9_]+)
```

| Input | Group 1 |
|---|---|
| `Thanks @rickylipe for the tip` | `rickylipe` |
| `Email user@example.com` | `example` (partial - avoid this with word boundaries) |
