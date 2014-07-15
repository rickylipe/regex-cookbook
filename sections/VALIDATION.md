# Validation Patterns

Extended validation patterns beyond what's in the main README.

---

## Canadian Postal Code

```regex
^[ABCEGHJ-NPRSTVXY]\d[ABCEGHJ-NPRSTV-Z]\s?\d[ABCEGHJ-NPRSTV-Z]\d$
```

Matches Canadian postal codes in A1A 1A1 or A1A1A1 format.
First character excludes D, F, I, O, Q, U (not used in Canadian postal codes).
Use case insensitive flag.

| Input | Match |
|---|---|
| `K1A 0A9` | yes |
| `V6B 4A2` | yes |
| `k1a0a9` | yes (with `i` flag) |
| `11A 1A1` | no |

---

## UK Postal Code

```regex
^[A-Z]{1,2}[0-9][0-9A-Z]?\s?[0-9][A-Z]{2}$
```

Matches UK postcodes. Use case insensitive flag.

| Input | Match |
|---|---|
| `SW1A 1AA` | yes |
| `M1 1AE` | yes |
| `B11 1BB` | yes |
| `1234` | no |

---

## Australian Postcode

```regex
^(0[289][0-9]{2})|([1-9][0-9]{3})$
```

Matches Australian 4-digit postcodes.

---

## IBAN (International Bank Account Number)

```regex
^[A-Z]{2}[0-9]{2}[A-Z0-9]{4}[0-9]{7}([A-Z0-9]?){0,16}$
```

Matches IBAN format structure (not country-specific length validation).
Use case insensitive flag.

---

## VIN (Vehicle Identification Number)

```regex
^[A-HJ-NPR-Z0-9]{17}$
```

Matches 17-character VINs. Excludes I, O, and Q which are not
used in VINs to avoid confusion with 1, 0, and 0.

---

## MAC Address

```regex
^([0-9A-Fa-f]{2}[:\-]){5}[0-9A-Fa-f]{2}$
```

Matches MAC addresses with either `:` or `-` as separator.

| Input | Match |
|---|---|
| `00:1A:2B:3C:4D:5E` | yes |
| `00-1A-2B-3C-4D-5E` | yes |
| `001A2B3C4D5E` | no |

---

## Semantic Version Number

```regex
^(0|[1-9]\d*)\.(0|[1-9]\d*)\.(0|[1-9]\d*)(?:-((?:0|[1-9]\d*|\d*[a-zA-Z-][0-9a-zA-Z-]*)(?:\.(?:0|[1-9]\d*|\d*[a-zA-Z-][0-9a-zA-Z-]*))*))?(?:\+([0-9a-zA-Z-]+(?:\.[0-9a-zA-Z-]+)*))?$
```

Full semver 2.0.0 compliant pattern. Matches versions like `1.0.0`,
`2.1.0-alpha.1`, `1.0.0+build.1`.

For simple MAJOR.MINOR.PATCH without pre-release or build metadata:

```regex
^\d+\.\d+\.\d+$
```

---

## ISBN-13

```regex
^(?:ISBN(?:-13)?:?\s)?(?=[0-9]{13}$|(?=(?:[0-9]+[-\s]){4})[-\s0-9]{17}$)97[89][-\s]?[0-9]{1,5}[-\s]?[0-9]+[-\s]?[0-9]+[-\s]?[0-9]$
```

Matches ISBN-13 with or without hyphens and optional "ISBN" prefix.

For a simpler check that just validates the digit count:

```regex
^97[89][- ]?[0-9]{10}$
```

---

## Latitude / Longitude

Latitude (-90 to 90):
```regex
^(\+|-)?(?:90(?:(?:\.0{1,6})?)|(?:[0-9]|[1-8][0-9])(?:(?:\.[0-9]{1,6})?))$
```

Longitude (-180 to 180):
```regex
^(\+|-)?(?:180(?:(?:\.0{1,6})?)|(?:[0-9]|[1-9][0-9]|1[0-7][0-9])(?:(?:\.[0-9]{1,6})?))$
```
