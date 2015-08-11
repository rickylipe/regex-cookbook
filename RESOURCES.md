# Tools & Resources

## Testing & Building

### regex101.com

The best online regex tester. Paste your pattern and test string,
and it highlights matches in real time, explains each part of your
pattern in plain English, and shows capture groups.

Supports PCRE, JavaScript, Python, and Golang flavors. Patterns
can be saved and shared via URL.

**Live examples from this cookbook** - click any link to open the
pattern in regex101 with test data pre-loaded:

#### Validation

| Pattern | Live Example |
|---|---|
| Email address | [regex101.com/r/y1fAXf/1](https://regex101.com/r/y1fAXf/1) |
| US phone number | [regex101.com/r/QOez5n/1](https://regex101.com/r/QOez5n/1) |
| US ZIP code | [regex101.com/r/VvxeS1/1](https://regex101.com/r/VvxeS1/1) |
| IPv4 address | [regex101.com/r/ZEmHyk/1](https://regex101.com/r/ZEmHyk/1) |
| ISO 8601 date | [regex101.com/r/FnRwHb/1](https://regex101.com/r/FnRwHb/1) |
| Hex color code | [regex101.com/r/I8SJXE/1](https://regex101.com/r/I8SJXE/1) |
| Strong password | [regex101.com/r/mbchNt/1](https://regex101.com/r/mbchNt/1) |
| UUID v4 | [regex101.com/r/9hbYOZ/1](https://regex101.com/r/9hbYOZ/1) |
| Credit card number | [regex101.com/r/WoQiaB/1](https://regex101.com/r/WoQiaB/1) |
| MAC address | [regex101.com/r/AuYoPJ/1](https://regex101.com/r/AuYoPJ/1) |
| VIN | [regex101.com/r/wb9kTI/1](https://regex101.com/r/wb9kTI/1) |
| Semantic version | [regex101.com/r/7CgxQG/1](https://regex101.com/r/7CgxQG/1) |
| Canadian postal code | [regex101.com/r/2TulEn/1](https://regex101.com/r/2TulEn/1) |
| UK postal code | [regex101.com/r/ur2msx/1](https://regex101.com/r/ur2msx/1) |
| Username | [regex101.com/r/RQEcVI/1](https://regex101.com/r/RQEcVI/1) |
| Percentage | [regex101.com/r/RDXcjw/1](https://regex101.com/r/RDXcjw/1) |
| Currency (US) | [regex101.com/r/KnKwQD/1](https://regex101.com/r/KnKwQD/1) |

#### Extraction

| Pattern | Live Example |
|---|---|
| Extract URLs from text | [regex101.com/r/xrmNC7/1](https://regex101.com/r/xrmNC7/1) |
| Extract emails from text | [regex101.com/r/jIuIAZ/1](https://regex101.com/r/jIuIAZ/1) |
| Extract prices from text | [regex101.com/r/nXMZpH/1](https://regex101.com/r/nXMZpH/1) |
| Extract hashtags | [regex101.com/r/e9j9Wa/1](https://regex101.com/r/e9j9Wa/1) |
| Extract mentions | [regex101.com/r/9AVSJs/1](https://regex101.com/r/9AVSJs/1) |
| Extract IP addresses from logs | [regex101.com/r/G3Q0da/1](https://regex101.com/r/G3Q0da/1) |
| Extract content in quotes | [regex101.com/r/1MvinV/1](https://regex101.com/r/1MvinV/1) |
| Extract file extensions | [regex101.com/r/DPKlxx/1](https://regex101.com/r/DPKlxx/1) |
| Extract content in parentheses | [regex101.com/r/l9JJHV/1](https://regex101.com/r/l9JJHV/1) |
| Extract domain from URL | [regex101.com/r/fBdIS8/1](https://regex101.com/r/fBdIS8/1) |
| Extract HTML tags | [regex101.com/r/ywil3M/1](https://regex101.com/r/ywil3M/1) |
| Extract HTML comments | [regex101.com/r/k6j2c6/1](https://regex101.com/r/k6j2c6/1) |

#### Formatting & Cleanup

| Pattern | Live Example |
|---|---|
| Remove HTML tags | [regex101.com/r/NlaPt5/1](https://regex101.com/r/NlaPt5/1) |
| URL slug cleanup | [regex101.com/r/Oulq6K/1](https://regex101.com/r/Oulq6K/1) |
| Repeated words | [regex101.com/r/javhWK/1](https://regex101.com/r/javhWK/1) |
| Blank lines | [regex101.com/r/s96Hr7/1](https://regex101.com/r/s96Hr7/1) |

---

### regexr.com

Another solid online tester with a clean interface and a community
library of shared patterns. Good for quick testing and learning.

---

### regexcrossword.com

Regex crossword puzzles. Genuinely fun way to get better at reading
patterns. Starts simple and gets difficult fast.

---

## References

### MDN Web Docs - Regular Expressions (JavaScript)
https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_expressions

The most thorough JavaScript-specific regex reference available. Covers
syntax, flags, methods, and Unicode support.

---

### PHP Manual - PCRE Syntax
https://www.php.net/manual/en/reference.pcre.pattern.syntax.php

Full PCRE syntax reference for PHP. Dense but complete.

---

### Python `re` module documentation
https://docs.python.org/3/library/re.html

Python's regex documentation. Covers the `re` module API and pattern
syntax. The "How to use Regex" HOWTO guide linked from this page is
particularly good for beginners.

---

### Regular-Expressions.info
https://www.regular-expressions.info

The most comprehensive regex reference on the web. Covers every feature
across every major engine with clear explanations and examples. If you
need to know whether a feature is supported in a specific engine, this
is the place to check.

---

## Books

**Mastering Regular Expressions** by Jeffrey Friedl (O'Reilly)

The definitive book on regex. Goes deep on how engines work, performance
optimization, and cross-engine compatibility. More than most people need,
but if you want to really understand what's happening under the hood it's
worth the read.

---

## Tips

**Test with real data.** Patterns that look right against two examples
often break on edge cases. Use regex101 and paste a sample of your actual
data before committing to a pattern.

**Don't over-engineer validation.** A regex that rejects 0.1% of valid
inputs is often worse than one that accepts 0.1% of invalid ones. For
email and phone specifically, keep it simple and validate by actually
contacting the address or number.

**Use verbose mode for complex patterns.** Many engines support an
extended flag (`x`) which allows whitespace and comments inside the
pattern for readability. Use it for anything complex that needs to
be maintained long term.
