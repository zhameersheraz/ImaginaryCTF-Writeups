# Algebra Dream — ImaginaryCTF Writeup

**Challenge:** Algebra Dream  
**Category:** Crypto  
**Difficulty:** Hard  
**Points:** 125 pts  
**Flag:** `ictf{is_1t_s0lv4ble_if_q_is_2+p+p^2_1n5t34d1b591d9e}`

---

## Description

> Want my flag? Factor it 😃

**Attachment:** Large composite number to factor

---

## Background Knowledge (Read This First!)

### What is Factoring with Cyclotomic Polynomials?

The paper **"Factoring with Cyclotomic Polynomials"** describes a method for factoring certain special integers using **cyclotomic polynomials** — polynomials whose roots are roots of unity.

For integers of the form `n = p * q` where `q = 2 + p + p²` (or similar algebraic relations), cyclotomic polynomial factoring is efficient because the algebraic relationship between `p` and `q` creates a special structure exploitable by this method.

### What is the Flag Saying?

`is_1t_s0lv4ble_if_q_is_2+p+p^2` — the challenge uses exactly this structure: `q = 2 + p + p²`. The cyclotomic polynomial factoring method is designed for this.

---

## Solution

### Step 1 — Download the challenge

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <attachment url> -O algebra_dream.txt
└─$ cat algebra_dream.txt
# → A very large number n = p * q where q = 2 + p + p²
```

### Step 2 — Apply cyclotomic polynomial factoring

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.sage
```

```python
# SageMath — Cyclotomic polynomial factoring
# Reference: "Factoring with Cyclotomic Polynomials" paper

n = ...  # paste the large number here

# q = 2 + p + p² means n = p * (2 + p + p²)
# Set up as polynomial: p³ + p² + 2p - n = 0
R.<p> = ZZ[]
f = p^3 + p^2 + 2*p - n
roots = f.roots()
if roots:
    p_val = roots[0][0]
    q_val = n // p_val
    print(f"p = {p_val}")
    print(f"q = {q_val}")
    # Decrypt RSA with recovered p and q
```

### Step 3 — Decrypt and get the flag

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ sage solve.sage
# → ictf{is_1t_s0lv4ble_if_q_is_2+p+p^2_1n5t34d1b591d9e}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| SageMath | Cyclotomic polynomial factoring | ⭐⭐⭐ Hard |

---

## Key Takeaways

- Algebraic relations between `p` and `q` (like `q = 2 + p + p²`) enable efficient factoring
- Cyclotomic polynomial factoring exploits the polynomial structure of the primes
- The flag encodes the exact algebraic relationship — always read it for hints!
