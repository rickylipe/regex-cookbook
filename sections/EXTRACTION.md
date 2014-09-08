# Extraction Patterns

Patterns for pulling specific data out of larger blocks of text.
All patterns intended for use with global flag unless noted.

---

## Extract All URLs

```regex
https?:\/\/(www\.)?[-a-zA-Z0-9@:%._\+~#=]{1,256}\.[a-zA-Z0-9()]{1,6}([-a-zA-Z0-9()@:%_\+.~#?&\/=]*)
```

Finds all HTTP/HTTPS URLs in text. Requires protocol prefix.

---

## Extract All Email Addresses

```regex
[a-zA-Z0-9._%+\-]+@[a-zA-Z0-9.\-]+\.[a-zA-Z]{2,}
```

---

## Extract All Phone Numbers (US)

```regex
(\+1[\s\-]?)?\(?\d{3}\)?[\s\-\.]?\d{3}[\s\-\.]\d{4}
```

---

## Extract Prices

```regex
\$\s?[0-9]+(\.[0-9]{2})?
```

Captures dollar amounts with optional cents.

| Input | Matches |
|---|---|
| `Total: $14.99` | `$14.99` |
| `From $5 to $50.00` | `$5`, `$50.00` |

---

## Extract Dates (Multiple Formats)

```regex
(0?[1-9]|1[0-2])[\/\-\.](0?[1-9]|[12][0-9]|3[01])[\/\-\.]([0-9]{2,4})
```

Matches MM/DD/YYYY, MM-DD-YYYY, MM.DD.YY etc.

---

## Extract Words in ALL CAPS

```regex
[A-Z]{2,}
```

Matches sequences of 2 or more uppercase letters. Useful for finding
acronyms or emphasis text.

---

## Extract Content Between Tags

```regex
(?<=<tag>)[\s\S]*?(?=<\/tag>)
```

Replace `tag` with the tag name you're looking for. Uses lookbehind
and lookahead to exclude the tags from the match.

Not all regex engines support lookbehind. Alternative with capture group:

```regex
<tag>([\s\S]*?)<\/tag>
```

---

## Extract First N Words

```regex
^(\S+\s+){0,N}\S+
```

Replace N with one less than the number of words you want.
For 5 words: `^(\S+\s+){0,4}\S+`

---

## Extract Lines Containing a Word

```regex
^.*word.*$
```

Use with multiline (`m`) and global (`g`) flags.
Replace `word` with your search term.

---

## Extract Content in Parentheses

```regex
\(([^)]+)\)
```

Captures text inside parentheses in group 1. Does not handle nested
parentheses.

---

## Extract Content in Quotes

Double quotes:
```regex
"([^"]*)"
```

Single quotes:
```regex
'([^']*)'
```

Either:
```regex
["'](.*?)["']
```

---

## Extract File Extension

```regex
\.([a-zA-Z0-9]+)$
```

Captures the extension (without dot) in group 1.

| Input | Group 1 |
|---|---|
| `document.pdf` | `pdf` |
| `archive.tar.gz` | `gz` |
| `README` | no match |

---

## Extract Filename from Path

```regex
[^\/]+(?=\.[^\/]+$)
```

Captures filename without extension. Works with both forward
and backslashes.

---

## Extract IP Addresses from Logs

```regex
(?:\d{1,3}\.){3}\d{1,3}
```

Finds IPv4 addresses in text. Does not validate octet ranges -
use the validation pattern for that.

---

## Extract JSON Values by Key

```regex
"key"\s*:\s*"([^"]*)"
```

Replace `key` with the field name. Captures the string value in group 1.
Only works for string values - not reliable for nested objects or arrays.

> Use a proper JSON parser for anything beyond simple one-off extraction.

---

## Extract CSS Color Values

Hex:
```regex
#([A-Fa-f0-9]{6}|[A-Fa-f0-9]{3})
```

RGB:
```regex
rgb\(\s*(\d{1,3})\s*,\s*(\d{1,3})\s*,\s*(\d{1,3})\s*\)
```

HSL:
```regex
hsl\(\s*(\d{1,3})\s*,\s*(\d{1,3})%\s*,\s*(\d{1,3})%\s*\)
```
