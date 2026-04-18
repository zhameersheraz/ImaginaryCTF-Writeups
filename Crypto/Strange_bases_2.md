# Strange bases 2 — ImaginaryCTF Writeup

**Challenge:** Strange bases 2  
**Category:** Crypto  
**Difficulty:** Medium  
**Points:** 65 pts  
**Flag:** `ictf{ju57_74k3_7h3_r007_br0}`

---

## Description

> practical? useful? I don't even know what those words mean...

**Attachment:** Challenge source + solve script

---

## Background Knowledge (Read This First!)

### What is the "root" being referenced?

The flag says `ju57_74k3_7h3_r007_br0` — "just take the root bro." This hints at a mathematical root operation being the key step in the solution.

### What is the Attack?

The challenge involves a non-standard base encoding where the solution requires finding a mathematical root (square root, cube root, or nth root) of a large number modulo some value.

*(The full solve script is provided in the challenge attachments.)*

---

## Solution

### Step 1 — Download the challenge and solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <challenge url> -O strange_bases_2.py
└─$ wget <solve url> -O solve.py
```

### Step 2 — Run the solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ sage solve.py
# → ictf{ju57_74k3_7h3_r007_br0}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| SageMath | Mathematical root computation | ⭐⭐⭐ Hard |

---

## Key Takeaways

- The flag itself is the hint: "just take the root bro"
- Non-standard base encodings often hide a mathematical operation as the core step
- SageMath's built-in root finding over rings and fields handles these cleanly
