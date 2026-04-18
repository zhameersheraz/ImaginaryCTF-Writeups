# Math Jail 2 — ImaginaryCTF Writeup

**Challenge:** Math Jail 2  
**Category:** Misc  
**Difficulty:** Medium  
**Points:** 75 pts  
**Flag:** `ictf{what?_it_was_gambling_all_along?}`

---

## Description

> Why is the person who doesn't know math making a crypto challenge...

**Connection:** `nc 155.248.210.243 42179`

---

## Background Knowledge (Read This First!)

### What is Math Jail?

Math Jail challenges give you a math oracle — you send expressions, the server evaluates them. The goal is usually to satisfy some condition (like producing a prime number) 100 times in a row.

### What is the Legitimate Approach?

The server asks you to produce a prime number 100 times. Normally you would compute primes — but the challenge blocks the `*` operator, making proper primality computation harder.

### What is the Gambling Approach?

Only about **5% of numbers between 100M and 1B are prime**. So if you send `n - n` (which equals 0 — a non-prime), what happens?

Actually the smart gamble is: only ~5% of random numbers in that range are prime. The chance that NONE of 100 random numbers from that range are prime is:

```
(1 - 0.05)^100 ≈ 0.006 ≈ 1/169
```

So about **1 in 169 attempts succeeds** just by sending a composite-guaranteed expression repeatedly!

---

## Solution

### Step 1 — Create the gambling script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.py
```

```python
from pwn import *

while True:
    try:
        r = remote('155.248.210.243', 42179)
        for _ in range(100):
            r.recvuntil(b'> ')
            r.sendline(b'n-n')  # always sends 0, never prime
        output = r.recvall(timeout=3)
        if b'ictf{' in output:
            print(output)
            break
        r.close()
    except:
        pass
```

### Step 2 — Run and wait for the lucky run

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 solve.py
# → Runs ~169 times on average before hitting the 1/169 chance
# → ictf{what?_it_was_gambling_all_along?}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Python 3 + pwntools | Automated retry gambling script | ⭐⭐ Medium |

---

## Key Takeaways

- Not every CTF challenge has a clean mathematical solution — sometimes gambling wins!
- 1 in 169 odds with an automated retry loop takes seconds to hit
- The flag literally reveals the intended unintended solution: "it was gambling all along"
- Prime density around 100M-1B is ~5% — always worth knowing for math jail challenges
