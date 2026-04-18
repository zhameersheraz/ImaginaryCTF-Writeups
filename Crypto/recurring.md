# recurring — ImaginaryCTF Writeup

**Challenge:** recurring  
**Category:** Crypto  
**Difficulty:** Medium-Hard  
**Points:** 85 pts  
**Flag:** `ictf{wh4t_ev3n_i5_@_r34l_w0r1d_4ppl1c4ti0n_9OoYVHHxYhQG6teVZXHC}`

---

## Description

> do the math

**Attachment:** Challenge source + solve script

---

## Background Knowledge (Read This First!)

### What is a Recurrence?

A **recurrence relation** defines each term of a sequence using previous terms. The classic example is Fibonacci: `F(n) = F(n-1) + F(n-2)`.

### What is the Intended Solution?

The challenge name and writeup both say "it's called recurring because the intended solution draws on recurrence ideas." The solution involves identifying and exploiting a recurrence structure in the cryptographic scheme.

This typically means:
- The key or output follows a linear (or polynomial) recurrence
- Tools like **Berlekamp-Massey** find the recurrence from observed outputs
- Once you know the recurrence, you can predict all past/future values

*(The full solve script is provided in the challenge attachments.)*

---

## Solution

### Step 1 — Download the challenge and solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <challenge url> -O recurring.py
└─$ wget <solve url> -O solve.py
```

### Step 2 — Run the solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ sage solve.py
# → ictf{wh4t_ev3n_i5_@_r34l_w0r1d_4ppl1c4ti0n_9OoYVHHxYhQG6teVZXHC}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| SageMath | Recurrence analysis + Berlekamp-Massey | ⭐⭐⭐ Hard |

---

## Key Takeaways

- Recurrence relations in PRNGs or key schedules are exploitable via Berlekamp-Massey
- The challenge name is always a hint — "recurring" = recurrence relation
- Once you identify the recurrence, predicting outputs is straightforward
