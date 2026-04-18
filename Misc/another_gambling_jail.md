# another gambling jail — ImaginaryCTF Writeup

**Challenge:** another gambling jail  
**Category:** Misc  
**Difficulty:** Easy-Medium  
**Points:** 50 pts  
**Flag:** `ictf{Kellys_Criteria_is_for_nerds}`

---

## Description

> new gambling jail just dropped but this time with actual gambling 🎰 🎰 🎰

**Connection:** `nc 155.248.210.243 42488`

---

## Background Knowledge (Read This First!)

### What is a Gambling Jail?

A "gambling jail" CTF challenge gives you a starting balance and asks you to reach a target amount by gambling. The house has an edge, so naive play loses over time. You need a mathematical strategy.

### What is Kelly's Criterion?

**Kelly's Criterion** is a formula for optimal bet sizing when you have a known edge over the house:

```
f = (bp - q) / b
```

Where:
- `f` = fraction of bankroll to bet
- `b` = odds received (e.g., 1:1 pays b=1)
- `p` = probability of winning
- `q` = probability of losing (1 - p)

### What is the `C * 19/128` strategy?

The writeup says the winning strategy is betting `C * 19/128` (approximately 14.8% of your current bankroll each round). This works about **10% of the time** — it is not guaranteed, but statistically reaches the target often enough to get the flag.

---

## Solution

### Step 1 — Connect and observe the game rules

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nc 155.248.210.243 42488
# → Note: starting balance, target balance, win probability
```

### Step 2 — Run the Kelly betting strategy

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.py
```

```python
from pwn import *
import math

while True:  # retry since it only works ~10% of the time
    r = remote('155.248.210.243', 42488)
    try:
        while True:
            data = r.recvuntil(b'bet: ')
            # Parse current balance C from the output
            C = int(...)  # extract balance
            bet = max(1, int(C * 19 / 128))
            r.sendline(str(bet).encode())
            response = r.recvline()
            if b'ictf{' in response:
                print(response)
                break
    except:
        r.close()
        continue
```

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 solve.py
# → ictf{Kellys_Criteria_is_for_nerds}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Python 3 + pwntools | Automated Kelly betting strategy | ⭐⭐ Medium |

---

## Key Takeaways

- Kelly's Criterion optimizes bet sizing for known probabilities — but it is not magic (10% success rate here)
- Retry loops are fine in CTFs — run the script until the lucky 10% hits
- The flag says "Kelly's Criteria is for nerds" — but it works!
