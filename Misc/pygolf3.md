# pygolf3 — ImaginaryCTF Writeup

**Challenge:** pygolf3  
**Category:** Misc  
**Difficulty:** Medium  
**Points:** 75 pts  
**Flag:** `ictf{h4ve_y0u_h3ard_Of_D1cTi0n@ry??}`

---

## Description

> Inspired by previous challenges but with shorter input!

**Connection:** `nc 8.138.23.35 8000`

---

## Background Knowledge (Read This First!)

### What is Python Code Golf?

**Code golf** is writing the shortest possible code to solve a problem. Python golf challenges give you a strict character or byte limit — you must read the flag using as few characters as possible.

### What is NFKC Normalization?

**NFKC (Normalization Form Compatibility Composition)** is a Unicode normalization form. Python applies it to identifiers before parsing. This means certain Unicode ligatures and special characters get expanded into multiple ASCII characters.

For example, the **ﬂ ligature** (a single Unicode character, U+FB02) normalizes to `fl`. So:

```python
ﬂag  →  flag  (after NFKC normalization)
```

This lets you write `flag` in just **3 characters** (`ﬂ`, `a`, `g`) instead of 4!

### What is the Empty Dictionary Leak?

```python
{}[ﬂag]
```

This tries to access key `flag` in an empty dictionary `{}`. It throws a `KeyError` — and Python prints the key value in the error message on **stderr**. Since the flag variable is named `flag`, the error message leaks its value!

---

## Solution

### Step 1 — Connect to the server

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nc 8.138.23.35 8000
# → Server expects Python code within a character limit
```

### Step 2 — Send the payload

Type or paste this (the `ﬂ` is a single Unicode ligature character):

```python
{}[ﬂag]
```

### Step 3 — Read the flag from stderr

The server returns a KeyError that contains the flag value:

```
KeyError: 'ictf{h4ve_y0u_h3ard_Of_D1cTi0n@ry??}'
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Unicode ligature (ﬂ) | Write `flag` in 3 chars via NFKC normalization | ⭐⭐ Medium |
| Empty dict KeyError | Leak the flag value through stderr | ⭐⭐ Medium |

---

## Key Takeaways

- Python applies NFKC normalization to identifiers — Unicode ligatures expand to ASCII
- `ﬂ` = `fl` after NFKC, so `ﬂag` = `flag` in 3 characters
- Empty dict access (`{}[key]`) leaks the key value in the error message — a novel side channel
- Code golf teaches you creative ways to abuse Python's Unicode and error handling
