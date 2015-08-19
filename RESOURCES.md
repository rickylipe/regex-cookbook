# Tools & Resources

## Testing & Building

### regex101.com

The best online regex tester. Paste your pattern and test string,
and it highlights matches in real time, explains each part of your
pattern in plain English, and shows capture groups.

Supports PCRE, JavaScript, Python, and Golang flavors. Patterns
can be saved and shared via URL.

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
