# Composite Rev — ImaginaryCTF Writeup

**Challenge:** Composite Rev  
**Category:** Rev  
**Difficulty:** Hard  
**Points:** 100 pts  
**Flag:** `ictf{The_R_in_RSA_Stands_For_Rev}`

---

## Description

> Writing this code was more of a pain than solving it will be...

**Attachment:** Binary with RSA implementation + source

---

## Background Knowledge (Read This First!)

### What is the Challenge?

The binary implements a **very badly written RSA** scheme. You need to:
1. Reverse engineer the communication protocol
2. Understand what math questions the server is asking
3. Answer them correctly to get the flag

### Why is it Called "Composite Rev"?

The RSA implementation uses **composite numbers** in places where primes should be — a fundamental mistake. But the "Rev" part means the primary challenge is reading and understanding the broken code, not attacking RSA mathematically.

*(The full source and solve are provided in the challenge attachments.)*

---

## Solution

### Step 1 — Download and reverse the binary

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <challenge url> -O composite_rev
└─$ chmod +x composite_rev
└─$ ghidra
# → Load binary → identify the RSA operations and communication format
```

### Step 2 — Download and run the solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <solve url> -O solve.py
└─$ python3 solve.py
# → ictf{The_R_in_RSA_Stands_For_Rev}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Ghidra | Reverse engineer the binary and RSA protocol | ⭐⭐⭐ Hard |
| Python 3 + pwntools | Answer the server's math questions | ⭐⭐ Medium |

---

## Key Takeaways

- "The R in RSA stands for Rev" — the challenge is about reversing the implementation, not breaking RSA
- Badly implemented RSA (using composites instead of primes) is still reversible if you understand the code
- Always read the source when provided — it tells you exactly what to implement
