# MagicHash — ImaginaryCTF Writeup

**Challenge:** MagicHash  
**Category:** Crypto  
**Difficulty:** Hard  
**Points:** 100 pts  
**Flag:** `ictf{easy_peasy_hash_collision_from_linearity}`

---

## Description

> Do you think combining weak hash functions can make it stronger?

**Connection:** `nc ictf.maple3142.net 1337`

---

## Background Knowledge (Read This First!)

### What is MD5?

**MD5** is a broken cryptographic hash function. It is possible to find **collisions** — two different inputs that produce the same MD5 hash. Tools like `fastcoll` can generate MD5 collisions quickly.

### What is fastcoll?

**fastcoll** is a tool that generates MD5 collisions. A single call gives you two different 128-byte inputs with the same MD5 hash. By **chaining** multiple fastcoll outputs together, you can create **2^N collisions** using only N fastcoll calls.

For 2^33 collisions, you need 33 fastcoll calls.

### What is CRC?

**CRC (Cyclic Redundancy Check)** is a checksum algorithm. It is **linear** over GF(2) — meaning it satisfies:

```
CRC(A XOR B) = CRC(A) XOR CRC(B)
```

This linearity means you can use linear algebra to construct inputs with a target CRC value.

### What is the Combined Attack?

1. Generate a **2^33-collision** of MD5 by chaining 33 fastcoll outputs
2. Use CRC's linearity to set up a linear system over GF(2)
3. Solve the system to find which combination of the 2^33 inputs also collides on CRC
4. Submit that input to the server → both hashes match → get the flag

*(The full solve script is provided in the challenge attachments.)*

---

## Solution

### Step 1 — Install fastcoll

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ sudo apt-get install fastcoll
```

### Step 2 — Download and run the solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <solve url> -O solve.py
└─$ python3 solve.py
# → ictf{easy_peasy_hash_collision_from_linearity}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| fastcoll | Generate 2^33 MD5 collisions | ⭐⭐⭐ Hard |
| SageMath | Solve linear system over GF(2) for CRC collision | ⭐⭐⭐ Hard |
| Python 3 | Orchestrate the full attack | ⭐⭐ Medium |

---

## Key Takeaways

- MD5 is broken — fastcoll creates collisions in seconds
- Chaining N fastcoll outputs gives 2^N collisions — exponential growth!
- CRC's linearity over GF(2) makes it solvable via Gaussian elimination
- Combining weak hash functions does NOT make them stronger
