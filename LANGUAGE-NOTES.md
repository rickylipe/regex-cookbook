# Language-Specific Notes

Regex syntax and behavior varies between languages and engines.
This covers the most common differences you'll run into.

---

## JavaScript

```javascript
// Literal syntax
const pattern = /^[a-z]+$/i;

// Constructor syntax (useful for dynamic patterns)
const pattern = new RegExp('^[a-z]+$', 'i');

// Test for match
pattern.test('hello');  // true

// Extract first match
'hello world'.match(/\w+/);  // ['hello']

// Extract all matches
'hello world'.match(/\w+/g);  // ['hello', 'world']

// Replace
'hello world'.replace(/\w+/g, 'x');  // 'x x'

// Replace with function
'hello world'.replace(/\w+/g, m => m.toUpperCase());  // 'HELLO WORLD'
```

**Gotchas:**
- No lookbehind support in older engines (added in ES2018)
- `lastIndex` state on regex with `g` flag can cause unexpected behavior
  when reusing the same regex object
- Named capture groups added in ES2018: `(?<name>...)`

---

## PHP

```php
// preg functions use PCRE
preg_match('/^[a-z]+$/i', $str);         // returns 0 or 1
preg_match_all('/\w+/', $str, $matches); // fills $matches array
preg_replace('/\w+/', 'x', $str);       // returns modified string
preg_split('/\s+/', $str);               // returns array

// Named capture groups
preg_match('/(?P<year>\d{4})-(?P<month>\d{2})/', $str, $m);
echo $m['year'];
```

**Gotchas:**
- Delimiters are required (`/pattern/flags`)
- Any non-alphanumeric character works as delimiter (`#pattern#` is common
  when pattern contains forward slashes)
- `preg_match` returns 1 on match, 0 on no match, false on error

---

## Python

```python
import re

# Compile for reuse
pattern = re.compile(r'^[a-z]+$', re.IGNORECASE)

# Test
bool(re.match(r'^[a-z]+$', 'hello', re.I))  # True

# Find first match
re.search(r'\w+', 'hello world').group()  # 'hello'

# Find all matches
re.findall(r'\w+', 'hello world')  # ['hello', 'world']

# Replace
re.sub(r'\w+', 'x', 'hello world')  # 'x x'

# Named groups
m = re.search(r'(?P<year>\d{4})-(?P<month>\d{2})', s)
m.group('year')
```

**Gotchas:**
- Use raw strings (`r'...'`) to avoid double-escaping backslashes
- `re.match` only matches at the start of the string
- `re.search` matches anywhere in the string
- `re.fullmatch` requires the entire string to match

---

## MySQL

```sql
-- Basic match (case insensitive by default)
SELECT * FROM table WHERE column REGEXP '^[0-9]+$';

-- Case sensitive
SELECT * FROM table WHERE column REGEXP BINARY '^[A-Z]+$';

-- Used in CREATE FUNCTION
RETURN str REGEXP '^[0-9]+$';
```

**Gotchas:**
- MySQL uses POSIX extended regex, not PCRE
- No lookahead or lookbehind support
- No non-capturing groups `(?:...)`
- `\d` does not work - use `[0-9]`
- `\w` does not work - use `[a-zA-Z0-9_]`
- `\s` does not work in older versions - use `[[:space:]]`

---

## Differences to Watch For

| Feature | JS | PHP (PCRE) | Python | MySQL |
|---|---|---|---|---|
| Lookahead `(?=...)` | yes | yes | yes | no |
| Lookbehind `(?<=...)` | ES2018+ | yes | yes | no |
| Non-capturing group `(?:...)` | yes | yes | yes | no |
| Named groups | ES2018+ | yes | yes | no |
| `\d`, `\w`, `\s` | yes | yes | yes | no |
| Unicode properties | ES2018+ | with `/u` | yes | no |
