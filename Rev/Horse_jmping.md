# Horse jmping — ImaginaryCTF Writeup

**Challenge:** Horse jmping  
**Category:** Rev  
**Difficulty:** Medium  
**Points:** 75 pts  
**Flag:** `ictf{ACosmicBitFlipSimulatorLol}`

---

## Description

> ...umapyoi

**Attachment:** Binary with LCG implementation

---

## Background Knowledge (Read This First!)

### What is a Linear Congruential Generator (LCG)?

An **LCG** is a simple PRNG defined by:

```
X_(n+1) = (a * X_n + c) mod m
```

Where `a`, `c`, and `m` are constants. LCGs are fast but **cryptographically weak** — given a few outputs, you can fully recover the state and predict all future/past outputs.

### What is the LCG Period?

This LCG has a period of **2^48** — meaning after 2^48 steps it repeats. This sounds large but is actually manageable for algorithms like Baby-Step Giant-Step.

### What is Baby-Step Giant-Step (BSGS)?

**Baby-Step Giant-Step** is a meet-in-the-middle algorithm for solving discrete logarithms in O(√n) time and space. For a period of 2^48, BSGS runs in O(2^24) — very fast.

### Matrix Form of LCG

The LCG can be written as a matrix multiplication:

```
[X_(n+1)]   [a  c] [X_n]
[  1    ] = [0  1] [  1] mod m
```

This matrix form makes it easy to apply group theory and BSGS.

---

## Solution

### Step 1 — Extract the LCG parameters

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ strings horse_jmping | grep -E "[0-9]{10,}"
# → Find a, c, m values
```

Or disassemble:

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ objdump -d horse_jmping | grep -A 5 "mul"
```

### Step 2 — Solve the discrete log with BSGS

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.sage
```

```python
# SageMath — Baby-Step Giant-Step on LCG in matrix form
a = ...   # LCG multiplier
c = ...   # LCG increment
m = 2^48  # LCG modulus

# Matrix form
M = Matrix(Zmod(m), [[a, c], [0, 1]])

# Known output — use BSGS to find n such that M^n * initial = observed
# SageMath discrete_log handles this
n = discrete_log(observed_state_matrix, M)
print("Steps:", n)

# Recover the flag from the state
```

### Step 3 — Run it and decode

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ sage solve.sage
# → ictf{ACosmicBitFlipSimulatorLol}
```

*(Full source and solve provided in the challenge attachments.)*

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| SageMath | LCG discrete log via Baby-Step Giant-Step | ⭐⭐⭐ Hard |
| `objdump` / `strings` | Extract LCG parameters from the binary | ⭐⭐ Medium |

---

## Key Takeaways

- LCGs with period 2^48 are NOT secure — BSGS solves them in O(2^24)
- The **matrix form** of an LCG enables group theory attacks like discrete logarithm
- "jmping" = jumping → the LCG jumps forward in its sequence
