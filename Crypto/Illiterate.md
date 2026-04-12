# Illiterate — ImaginaryCTF Writeup

**Challenge:** Illiterate  
**Category:** Crypto  
**Difficulty:** Medium  
**Points:** 75 pts  
**Flag:** `ictf{ai_told_me_16_rounds_was_solid}`

---

## Description

> No libraries won't stop me from writing insecure code -w-

**Connection:** `nc 155.248.210.243 42123`

---

## Background Knowledge (Read This First!)

### What is a Primality Test?

When a server needs large prime numbers (for RSA, ECC, etc.), it runs a **primality test** to check if a number is prime. A common one is the **Solovay–Strassen test** — it runs multiple rounds. More rounds = more confidence.

**The problem:** 16 rounds sounds like a lot, but if the witnesses (the random numbers used in each round) are **fixed** (not truly random), the test can be fooled.

### What are Carmichael Numbers?

A **Carmichael number** is a composite (non-prime) number that *passes* certain primality tests even though it is not actually prime. They are sometimes called "pseudoprimes."

The attack here: generate Carmichael numbers until you find one that passes all 16 fixed witness rounds of the Solovay–Strassen test. This tricks the server into thinking your fake number is a prime.

### What is Pohlig-Hellman?

**Pohlig-Hellman** is an algorithm that solves the discrete logarithm problem efficiently when the group order has small prime factors. Since our fake prime is a Carmichael number, its factors are small enough for this to work.

---

## Solution

### Step 1 — Connect to the server

```
┌──(zham㉿kali)-[~]
└─$ nc 155.248.210.243 42123
```

### Step 2 — Run the Carmichael number generator

Run a script to generate Carmichael numbers and test each one against the server's 16 fixed witnesses. This takes roughly **5 minutes of computing**.

The parameters that worked:

```
a   = 28662185558404391344601135531809588907013812
b   = 16171080326069448853813193139376819675521374
p   = 36328006888239989979835683756179050367864581
E   = 36328006888239989979846533687841998147747521
g_x = 5777125167305480814518525709561856971293800
g_y = 10000000000000000
```

### Step 3 — Build the elliptic curve and solve with Pohlig-Hellman

Use **SageMath** to construct the curve and recover the secret:

```python
p  = 36328006888239989979835683756179050367864581
a  = 28662185558404391344601135531809588907013812
b  = 16171080326069448853813193139376819675521374
E  = EllipticCurve(GF(p), [a, b])
G  = E(g_x, g_y)

# SageMath has built-in Pohlig-Hellman inside discrete_log
secret = discrete_log(target_point, G, operation='+')
print(secret)
```

### Step 4 — Submit the answer

Send the recovered secret → server prints the flag:

```
ictf{ai_told_me_16_rounds_was_solid}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Python 3 | Generate Carmichael numbers, test fixed witnesses | ⭐⭐⭐ Hard |
| SageMath | Build elliptic curve and run Pohlig-Hellman | ⭐⭐⭐ Hard |
| `nc` | Connect to the challenge server | ⭐ Easy |

---

## Key Takeaways

- **Fixed witnesses** in primality tests are a critical vulnerability — always use random witnesses
- **16 rounds is not enough** if the attacker controls the input
- Carmichael numbers are the classic weapon against weak primality tests
- If the curve order has small factors, Pohlig-Hellman will break your ECDLP security
