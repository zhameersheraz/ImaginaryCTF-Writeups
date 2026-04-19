# modjail3 — ImaginaryCTF Writeup

**Challenge:** modjail3  
**Category:** Crypto  
**Difficulty:** Hard  
**Points:** 110 pts  
**Flag:** `ictf{tH3_B1t$iz3_of_T#e_bi7S1Ze_0F_7hE_Pr1m3_15_d3cRE@s!n6_L1ne4RLy...}`

---

## Description

> Are you tired of this series yet?

**Connection:** `nc 155.248.210.243 42111`

---

## Background Knowledge (Read This First!)

### What is Unique About modjail3?

The prime used in the modular conditions has **decreasing bit size** — each round uses a smaller prime. The flag literally describes this: "the bit size of the bit size of the prime is decreasing linearly."

This means the conditions become progressively easier to satisfy individually, but the combined constraint grows more complex.

### What is the `'5'*193` Trick?

The solve script uses `'5' * 193` — a string of 193 fives. This ensures a `#` (hash tag) appears at the beginning of the hash, satisfying a specific format requirement of the server. It is an ad-hoc trick found during solving.

*(The full solve script is provided in the challenge attachments.)*

---

## Solution

### Step 1 — Connect and observe the conditions

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nc 155.248.210.243 42111
# → Read the conditions involving decreasing-size primes
```

### Step 2 — Download and run the solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <solve url> -O solve.py
└─$ python3 solve.py
# → ictf{tH3_B1t$iz3_of_T#e_bi7S1Ze_0F_7hE_Pr1m3_15_d3cRE@s!n6_L1ne4RLy...}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Python 3 + pwntools | Solve decreasing-prime modular conditions | ⭐⭐⭐ Hard |

---

## Key Takeaways

- The flag describes the mathematical structure: prime bit sizes decrease linearly
- Sometimes CTF solves involve ad-hoc tricks (`'5'*193`) found through experimentation
- modjail3 is the hardest in the series — the author gave up on neat code by this point!
