# Silver Sonic — ImaginaryCTF Writeup

**Challenge:** Silver Sonic  
**Category:** Crypto  
**Difficulty:** Medium  
**Points:** 75 pts  
**Flag:** `ictf{https://www.youtube.com/watch?v=ED3uWRn4j5Q}`

---

## Description

> Stay gold genes get results, terms and conditions apply...

**Attachment:** Sequence data file

---

## Background Knowledge (Read This First!)

### What is a Linear Recurrence?

A **linear recurrence** is a sequence where each term is a linear combination of previous terms. The most famous example is Fibonacci:

```
F(n) = F(n-1) + F(n-2)
```

Many crypto challenges hide data in linear recurrences — you are given some terms and must figure out the pattern to predict the rest.

### What is Berlekamp-Massey?

**Berlekamp-Massey** is an algorithm that finds the **shortest linear recurrence** that generates a given sequence. Given enough terms of the sequence, it tells you exactly what the recurrence rule is.

Once you know the rule, you can generate ALL future (and past) terms — effectively cracking the sequence.

### What does "polynomials of that form are just linear transforms" mean?

The challenge's polynomials produce sequences that are equivalent to **linear transformations**. This means Berlekamp-Massey applies directly — the sequence is completely determined by its recurrence relation.

---

## Solution

### Step 1 — Download and read the file

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ cat silverSonic.txt
# → A sequence of numbers
```

### Step 2 — Apply Berlekamp-Massey to find the recurrence

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.sage
```

```python
# SageMath
seq = [...]  # paste the sequence values here

# Berlekamp-Massey finds the minimal linear recurrence
L = berlekamp_massey(seq)
print("Recurrence coefficients:", L)

# Use the recurrence to generate the rest of the sequence
full_seq = list(seq)
for i in range(len(seq), target_length):
    next_val = sum(-L[j] * full_seq[i-j-1] for j in range(len(L))) % modulus
    full_seq.append(next_val)

# Convert sequence to flag
flag = bytes(full_seq).decode()
print(flag)
```

### Step 3 — Run it

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ sage solve.sage
# → ictf{https://www.youtube.com/watch?v=ED3uWRn4j5Q}
```

*(A full solve script is also provided in the challenge attachments.)*

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| SageMath (`berlekamp_massey`) | Find the linear recurrence from the sequence | ⭐⭐⭐ Hard |
| Python 3 | Generate the rest of the sequence | ⭐⭐ Medium |

---

## Key Takeaways

- Polynomials over finite fields that form linear transforms generate **linearly recurrent sequences**
- **Berlekamp-Massey** is the standard algorithm to crack any linear recurrence given enough terms
- Once you have the recurrence, you can generate the entire sequence — past and future
- "Stay gold genes" is a reference to Gold Ship, Silver Sonic's sire — a famous Japanese racehorse
