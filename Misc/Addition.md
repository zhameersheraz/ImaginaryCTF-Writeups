# Addition — ImaginaryCTF Writeup

**Challenge:** Addition  
**Category:** Misc  
**Difficulty:** Medium  
**Points:** 65 pts  
**Flag:** `ictf{5p1ky_d157r1bu710n5_c4n_l00k_fl47_fr0m_4_d1574nc3}`

---

## Description

> The long awaited sequel to wj@'s Multiplication, Addition!

**Attachment:** Challenge source + solve script

---

## Background Knowledge (Read This First!)

### What is Floating Point Arithmetic?

**Floating point** is how computers represent real numbers — using a sign, exponent, and mantissa (fraction). The key property: floating point numbers have **limited precision**.

### What is the Vulnerability?

This challenge uses floating point addition in a way that **masks out the lower 21 bits** of the float representation. When you unpack the IEEE 754 float bytes, you can read those bits directly — they encode the flag.

### What is IEEE 754?

**IEEE 754** is the standard for floating point numbers. A 64-bit double has:
- 1 sign bit
- 11 exponent bits
- 52 mantissa bits

By interpreting the raw bytes of the float as an integer, you expose all these bits — including the ones the challenge hides data in.

*(The full solve script is provided in the challenge attachments.)*

---

## Solution

### Step 1 — Download the challenge and solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <challenge url> -O addition.py
└─$ wget <solve url> -O solve.py
```

### Step 2 — Run the solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 solve.py
# → ictf{5p1ky_d157r1bu710n5_c4n_l00k_fl47_fr0m_4_d1574nc3}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Python 3 (`struct`) | Unpack float bytes and read hidden bits | ⭐⭐ Medium |

---

## Key Takeaways

- Floating point addition masks lower bits — those bits can hide data
- `struct.unpack` lets you treat floats as raw bytes to read the bit pattern
- "Spiky distributions can look flat from a distance" — the bias is subtle at a high level but obvious bit-by-bit
