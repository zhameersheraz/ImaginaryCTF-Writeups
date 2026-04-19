# modjail2 — ImaginaryCTF Writeup

**Challenge:** modjail2  
**Category:** Crypto  
**Difficulty:** Hard  
**Points:** 100 pts  
**Flag:** `ictf{a_c@R3fU1lY_Cr4fT3d_PayL04d!}`

---

## Description

> More conditions = difficulty upgrade

**Connection:** `nc 155.248.210.243 42115`

---

## Background Knowledge (Read This First!)

### What Changed from modjail?

More modular conditions are imposed simultaneously. You now need to satisfy **multiple** congruences at once:

```
x ≡ r1 (mod m1)
x ≡ r2 (mod m2)
x ≡ r3 (mod m3)
```

### What is the Chinese Remainder Theorem (CRT)?

**CRT** states that if the moduli `m1, m2, m3, ...` are pairwise coprime, there exists a unique `x` (modulo `m1 * m2 * m3 * ...`) satisfying all congruences simultaneously. CRT gives you an efficient algorithm to compute it.

*(The full solve script is provided in the challenge attachments.)*

---

## Solution

### Step 1 — Connect and observe the conditions

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nc 155.248.210.243 42115
# → Read the multiple modular conditions
```

### Step 2 — Download and run the solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <solve url> -O solve.py
└─$ python3 solve.py
# → ictf{a_c@R3fU1lY_Cr4fT3d_PayL04d!}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Python 3 + CRT | Solve multiple simultaneous congruences | ⭐⭐⭐ Hard |

---

## Key Takeaways

- Multiple modular conditions → Chinese Remainder Theorem
- CRT works when moduli are pairwise coprime — check this first
- "Carefully crafted payload" = the CRT solution must be computed precisely
