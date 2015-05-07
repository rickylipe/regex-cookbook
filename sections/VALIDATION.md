# Validation Patterns

Extended validation patterns beyond what's in the main README.

---

## Canadian Postal Code

```
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
| `11A 1A1` | no (starts with digit) |
| `D1A 1A1` | no (D not valid first character) |

---

## UK Postal Code

```
^[A-Z]{1,2}[0-9][0-9A-Z]?\s?[0-9][A-Z]{2}$
```

Matches UK postcodes. Use case insensitive flag.

| Input | Match |
|---|---|
| `SW1A 1AA` | yes |
| `M1 1AE` | yes |
| `B11 1BB` | yes |
| `sw1a 1aa` | yes (with `i` flag) |
| `1234` | no |
| `ABCD 1EF` | no (too many letters at start) |

---

## Australian Postcode

```
^(0[289][0-9]{2})|([1-9][0-9]{3})$
```

Matches Australian 4-digit postcodes.

| Input | Match |
|---|---|
| `2000` | yes (Sydney) |
| `3000` | yes (Melbourne) |
| `0800` | yes (Darwin) |
| `200` | no (too short) |
| `20000` | no (too long) |

---

## IBAN (International Bank Account Number)

```
^[A-Z]{2}[0-9]{2}[A-Z0-9]{4}[0-9]{7}([A-Z0-9]?){0,16}$
```

Matches IBAN format structure (not country-specific length validation).
Use case insensitive flag.

| Input | Match |
|---|---|
| `GB82WEST12345698765432` | yes |
| `DE89370400440532013000` | yes |
| `12345678` | no (no country code) |
| `GB82` | no (too short) |

---

## VIN (Vehicle Identification Number)

```
^[A-HJ-NPR-Z0-9]{17}$
```

Matches 17-character VINs. Excludes I, O, and Q which are not
used in VINs to avoid confusion with 1, 0, and 0.

| Input | Match |
|---|---|
| `1HGBH41JXMN109186` | yes |
| `JH4KA7650MC002480` | yes |
| `1HGBH41JXMN10918` | no (16 chars) |
| `1HGBH41JXMN10918O` | no (contains O) |

---

## MAC Address

```
^([0-9A-Fa-f]{2}[:\-]){5}[0-9A-Fa-f]{2}$
```

Matches MAC addresses with either `:` or `-` as separator.

| Input | Match |
|---|---|
| `00:1A:2B:3C:4D:5E` | yes |
| `00-1A-2B-3C-4D-5E` | yes |
| `00:1a:2b:3c:4d:5e` | yes |
| `001A2B3C4D5E` | no (no separator) |
| `00:1A:2B:3C:4D` | no (only 5 groups) |

---

## Semantic Version Number

Full semver 2.0.0 compliant:
```
^(0|[1-9]\d*)\.(0|[1-9]\d*)\.(0|[1-9]\d*)(?:-((?:0|[1-9]\d*|\d*[a-zA-Z-][0-9a-zA-Z-]*)(?:\.(?:0|[1-9]\d*|\d*[a-zA-Z-][0-9a-zA-Z-]*))*))?(?:\+([0-9a-zA-Z-]+(?:\.[0-9a-zA-Z-]+)*))?$
```

Simple MAJOR.MINOR.PATCH only:
```
^\d+\.\d+\.\d+$
```

| Input | Full semver | Simple |
|---|---|---|
| `1.0.0` | yes | yes |
| `2.1.0-alpha.1` | yes | no |
| `1.0.0+build.1` | yes | no |
| `1.0` | no | no |
| `v1.0.0` | no | no |

---

## ISBN-13

Simple format check:
```
^97[89][- ]?[0-9]{10}$
```

| Input | Match |
|---|---|
| `9780306406157` | yes |
| `978-0-306-40615-7` | no (hyphens between each part not supported in simple version) |
| `9780306406` | no (too short) |

---

## Latitude

```
^(\+|-)?(?:90(?:\.0{1,6})?|(?:[0-9]|[1-8][0-9])(?:\.[0-9]{1,6})?)$
```

| Input | Match |
|---|---|
| `0` | yes |
| `90` | yes |
| `-45.123456` | yes |
| `90.000001` | no (exceeds 90) |
| `91` | no |

## Longitude

```
^(\+|-)?(?:180(?:\.0{1,6})?|(?:[0-9]|[1-9][0-9]|1[0-7][0-9])(?:\.[0-9]{1,6})?)$
```

| Input | Match |
|---|---|
| `0` | yes |
| `180` | yes |
| `-122.419418` | yes |
| `180.000001` | no (exceeds 180) |
| `181` | no |
