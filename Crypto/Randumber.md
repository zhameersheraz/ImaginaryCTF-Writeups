# Randumber — ImaginaryCTF Writeup

**Challenge:** Randumber  
**Category:** Crypto  
**Difficulty:** Hard  
**Points:** 100 pts  
**Flag:** `ictf{a_doubly_smooth_number_in_this_economy?!?}`

---

## Description

> Making a prng is easy, the hard part is hiding the backdoor...

**Attachment:** PRNG output + encryption source

---

## Background Knowledge (Read This First!)

### What is a PRNG?

A **Pseudo-Random Number Generator (PRNG)** is an algorithm that produces a sequence of numbers that appear random. If the PRNG is weak, an attacker can predict future (or past) outputs.

### What is a Smooth Number?

A **smooth number** is an integer whose prime factors are all small. For example, `2^10 * 3^5 * 7^2` is a smooth number because all its prime factors (2, 3, 7) are small.

Smooth numbers are dangerous in cryptography because:
- They are easy to factor
- Discrete logarithms are easy to compute using **Pohlig-Hellman**

### What is the Attack?

The PRNG modulus has two weaknesses:
1. It has many **tiny factors** plus one **big factor** → easy to factor completely
2. The biggest factor is itself a **smooth number** → Pohlig-Hellman works on it

Using two known plaintext-ciphertext pairs, you can recover the PRNG state by solving the RSA problem and the discrete logarithm.

---

## Solution

### Step 1 — Download and inspect the output

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ cat output.txt
# → PRNG modulus, encrypted pairs, ciphertext
```

### Step 2 — Factor the modulus

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.sage
```

```python
# SageMath — factor the weak modulus
n = ...  # paste modulus
print(factor(n))
# → Many small factors + 1 big smooth factor
```

### Step 3 — Compute discrete log with Pohlig-Hellman

```python
# Because the order is smooth, Pohlig-Hellman is fast
state = discrete_log(known_output, generator, ord=modulus_order)
print("Recovered PRNG state:", state)
```

### Step 4 — Decrypt the flag

```python
from Crypto.Util.number import long_to_bytes
# Use recovered state to reconstruct keystream and decrypt
flag = decrypt(ciphertext, state)
print(long_to_bytes(flag))
# → ictf{a_doubly_smooth_number_in_this_economy?!?}
```

*(Full solve script provided in the challenge attachments.)*

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| SageMath | Factor the modulus + Pohlig-Hellman discrete log | ⭐⭐⭐ Hard |
| Python 3 | Decrypt the flag using recovered PRNG state | ⭐ Easy |

---

## Key Takeaways

- A **doubly smooth** modulus (smooth + has a big smooth factor) is catastrophically weak
- **Pohlig-Hellman** makes discrete logs easy when the group order is smooth
- PRNG security depends entirely on the hardness of its underlying mathematical problem
- The "backdoor" in the title: the challenge author intentionally chose a smooth modulus
