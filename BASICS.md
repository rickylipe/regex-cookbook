# Regex Basics

A regex (regular expression) is a sequence of characters that defines a
search pattern. At its core it's a way of describing text - "find me
something that looks like this."

This is a practical reference, not a comprehensive spec. It covers the
building blocks you'll actually use day to day.

---

## How a Regex Works

A regex engine reads your pattern and your input string, then tries to
find a part of the string that matches the pattern.

```
Pattern:  \d{3}-\d{4}
Input:    "Call us at 555-1234 for more info"
Match:    "555-1234"
```

The engine found the part that fits. Everything before and after it is
ignored unless you use anchors (see below).

---

## Literal Characters

The simplest regex is just a plain string. It matches exactly those characters.

```
Pattern:  hello
Input:    "say hello to everyone"
Match:    "hello"
```

Most letters and numbers are literal. The following characters have special
meaning and need to be escaped with `\` if you want to match them literally:

```
. * + ? ^ $ { } [ ] | ( ) \
```

To match a literal dot: `\.`
To match a literal dollar sign: `\$`
To match a literal backslash: `\\`

---

## Anchors

Anchors don't match characters - they match *positions* in the string.

| Anchor | Meaning |
|---|---|
| `^` | Start of the string (or line with `m` flag) |
| `$` | End of the string (or line with `m` flag) |

```
Pattern:  ^hello
"hello world"   -> match  (hello is at the start)
"say hello"     -> no match (hello is not at the start)

Pattern:  world$
"hello world"   -> match  (world is at the end)
"world peace"   -> no match (world is not at the end)

Pattern:  ^hello$
"hello"         -> match  (entire string is hello)
"hello world"   -> no match
```

Using both `^` and `$` means "match the entire string" - nothing before
or after the pattern is allowed.

---

## Character Classes

A character class matches one character from a defined set. Written with
square brackets `[ ]`.

| Pattern | Matches |
|---|---|
| `[abc]` | one character: a, b, or c |
| `[a-z]` | one lowercase letter |
| `[A-Z]` | one uppercase letter |
| `[0-9]` | one digit |
| `[a-zA-Z]` | one letter (upper or lower) |
| `[a-zA-Z0-9]` | one letter or digit |
| `[^abc]` | one character that is NOT a, b, or c |
| `[^0-9]` | one character that is NOT a digit |

The `^` inside `[ ]` means "not". Outside of brackets, `^` means start of string.

---

## Shorthand Character Classes

Common character classes have shorthand versions:

| Shorthand | Equivalent | Matches |
|---|---|---|
| `\d` | `[0-9]` | any digit |
| `\D` | `[^0-9]` | any non-digit |
| `\w` | `[a-zA-Z0-9_]` | any word character (letter, digit, underscore) |
| `\W` | `[^a-zA-Z0-9_]` | any non-word character |
| `\s` | `[ \t\r\n\f]` | any whitespace (space, tab, newline, etc.) |
| `\S` | `[^ \t\r\n\f]` | any non-whitespace |

> These shorthands do **not** work in MySQL. Use `[0-9]`, `[a-zA-Z0-9_]`,
> and `[[:space:]]` instead. See [Language-Specific Notes](LANGUAGE-NOTES.md).

---

## The Dot

The `.` matches any single character except a newline.

```
Pattern:  c.t
"cat"   -> match
"cut"   -> match
"ct"    -> no match (dot requires exactly one character)
"coat"  -> no match (dot matches only one character)
```

To match a literal dot, escape it: `\.`

To make `.` match newlines too, use the "dotall" flag (`s` in most languages).

---

## Quantifiers

Quantifiers specify how many times the preceding element must match.

| Quantifier | Meaning |
|---|---|
| `*` | 0 or more times |
| `+` | 1 or more times |
| `?` | 0 or 1 time (makes it optional) |
| `{n}` | exactly n times |
| `{n,}` | n or more times |
| `{n,m}` | between n and m times |

```
Pattern:  \d+
"42"      -> match
"0"       -> match
"abc"     -> no match (needs at least one digit)

Pattern:  colou?r
"color"   -> match (u is optional)
"colour"  -> match
"colouur" -> no match (only 0 or 1 u allowed)

Pattern:  \d{3}-\d{4}
"555-1234"    -> match
"55-1234"     -> no match (need exactly 3 digits)
```

### Greedy vs Lazy

By default quantifiers are **greedy** - they match as much as possible.

```
Pattern:  <.+>
Input:    "<div>hello</div>"
Match:    "<div>hello</div>"  (matched everything from first < to last >)
```

Add `?` after a quantifier to make it **lazy** - match as little as possible:

```
Pattern:  <.+?>
Input:    "<div>hello</div>"
Matches:  "<div>", "</div>"  (stopped at each >)
```

---

## Groups

Parentheses `( )` group parts of a pattern together. They also *capture*
the matched text so you can reference it later.

```
Pattern:  (hello|hi) world
"hello world"  -> match
"hi world"     -> match
"hey world"    -> no match
```

### Capture Groups

Whatever matches inside `( )` is captured and can be extracted.

```
Pattern:  (\d{4})-(\d{2})-(\d{2})
Input:    "2023-12-31"
Group 1:  "2023"
Group 2:  "12"
Group 3:  "31"
```

### Non-Capturing Groups

`(?:...)` groups without capturing. Useful for applying a quantifier
to a group without extracting its content.

```
Pattern:  (?:ha)+
"ha"       -> match
"hahaha"   -> match
"hahahaha" -> match
```

### Named Capture Groups

Most modern engines support naming groups for clarity:

```
Pattern:  (?P<year>\d{4})-(?P<month>\d{2})-(?P<day>\d{2})
```

Syntax varies by language - `(?P<name>...)` in Python, `(?<name>...)` in
JavaScript and PHP.

---

## Alternation

The `|` operator means "or". Matches either the expression on the left
or the right.

```
Pattern:  cat|dog
"I have a cat"  -> match
"I have a dog"  -> match
"I have a fish" -> no match
```

Use groups to limit the scope of `|`:

```
Pattern:  I have a (cat|dog)
"I have a cat"     -> match
"I have a catfish" -> no match (requires end after cat/dog)
```

---

## Flags

Flags modify how the entire pattern behaves. Applied after the closing
delimiter (e.g. `/pattern/gi`).

| Flag | Letter | Effect |
|---|---|---|
| Case insensitive | `i` | `[a-z]` also matches `[A-Z]` |
| Global | `g` | Find all matches, not just the first |
| Multiline | `m` | `^` and `$` match start/end of each line, not just the whole string |
| Dotall | `s` | `.` matches newlines too |

---

## Lookahead and Lookbehind

These match a position based on what comes before or after it, without
including that surrounding text in the match.

| Syntax | Name | Meaning |
|---|---|---|
| `(?=...)` | Positive lookahead | Followed by... |
| `(?!...)` | Negative lookahead | NOT followed by... |
| `(?<=...)` | Positive lookbehind | Preceded by... |
| `(?<!...)` | Negative lookbehind | NOT preceded by... |

```
Pattern:  \d+(?= dollars)
Input:    "I have 100 dollars and 50 euros"
Match:    "100"  (the 50 is not followed by " dollars")

Pattern:  (?<=\$)\d+
Input:    "Price: $149"
Match:    "149"  (digits preceded by $, but $ not included in match)
```

> Lookahead and lookbehind are not supported in MySQL regex.
> Support for lookbehind in JavaScript was added in ES2018.

---

## Backreferences

`\1`, `\2`, etc. refer back to what was captured in group 1, group 2, etc.
Useful for finding repeated patterns.

```
Pattern:  (\w+)\s+\1
Input:    "the the problem is is real"
Matches:  "the the", "is is"
```

The `\1` matches whatever the first group matched - it's not "any word",
it's specifically the same word that was already captured.

---

## Quick Reference Card

| Symbol | Meaning |
|---|---|
| `.` | Any character except newline |
| `^` | Start of string |
| `$` | End of string |
| `*` | 0 or more |
| `+` | 1 or more |
| `?` | 0 or 1 (optional) |
| `{n}` | Exactly n times |
| `{n,m}` | Between n and m times |
| `[abc]` | One of: a, b, c |
| `[^abc]` | Not: a, b, c |
| `[a-z]` | Range: a through z |
| `\d` | Digit `[0-9]` |
| `\w` | Word character `[a-zA-Z0-9_]` |
| `\s` | Whitespace |
| `\D`, `\W`, `\S` | Negated versions of above |
| `(...)` | Capture group |
| `(?:...)` | Non-capturing group |
| `a\|b` | a or b |
| `\1` | Backreference to group 1 |
| `(?=...)` | Positive lookahead |
| `(?!...)` | Negative lookahead |
| `(?<=...)` | Positive lookbehind |
| `(?<!...)` | Negative lookbehind |
