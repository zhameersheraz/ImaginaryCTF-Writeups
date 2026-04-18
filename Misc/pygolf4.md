# pygolf4 — ImaginaryCTF Writeup

**Challenge:** pygolf4  
**Category:** Misc  
**Difficulty:** Medium  
**Points:** 85 pts  
**Flag:** `ictf{pygolfing_without_going_to_the_library}`

---

## Description

> The more restrictions the better

**Connection:** `nc 155.248.210.243 42144`

---

## Background Knowledge (Read This First!)

### What Changed from pygolf3?

More restrictions are added — some of the previous tricks no longer work. But the NFKC normalization trick for `flag` still applies.

### What is the New Leak Method?

Instead of `{}[ﬂag]`, the new approach is:

```python
ﬂag(*1)
```

This tries to **call** `flag` as a function with `*1` as unpacked arguments. Since `1` is an integer and not iterable, Python throws a `TypeError` that mentions the flag variable — leaking its value through stderr.

The `*1` syntax is the key — it is a novel way to trigger an error that exposes the variable name/value in Python's error output.

---

## Solution

### Step 1 — Connect to the server

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nc 155.248.210.243 42144
# → Server expects Python code within stricter limits
```

### Step 2 — Send the payload

```python
ﬂag(*1)
```

### Step 3 — Read the flag from the error output

```
TypeError: ictf{pygolfing_without_going_to_the_library} argument after * must be an iterable, not int
```

The flag is embedded in the error message! ✅

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Unicode ligature (ﬂ) | Write `flag` in 3 chars | ⭐⭐ Medium |
| `(*1)` TypeError | Novel error leak for the flag value | ⭐⭐⭐ Hard |

---

## Key Takeaways

- `ﬂag(*1)` triggers a TypeError that includes the variable name — a creative stderr leak
- Each pygolf challenge adds restrictions — you need new tricks each time
- The flag "pygolfing without going to the library" = solving without importing any modules
