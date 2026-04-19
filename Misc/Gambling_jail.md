# Gambling jail — ImaginaryCTF Writeup

**Challenge:** Gambling jail  
**Category:** Misc  
**Difficulty:** Medium  
**Points:** 49 pts  
**Flag:** `ictf{holy_smokes_i_won_the_jackpot}`

---

## Description

> we really will approve anything these days... Just print the flag bro

**Connection:** `nc 155.248.210.243 42391`

---

## Background Knowledge (Read This First!)

### What is the Challenge?

The server requires you to provide an input whose **MD5 hash starts with `flag#`**. This is a partial preimage challenge — you need to find any input whose MD5 hash begins with that specific prefix.

### Why is This Hard?

MD5 outputs are 128 bits — finding a hash starting with a specific 5-character string requires trying millions of inputs. However, tools designed to find MD5 hashes with many **leading zeros** can be repurposed, since leading zeros and arbitrary prefixes are both partial preimage problems.

### What Tool Was Used?

The md5zerofinder tool on GitHub was adapted. With 20 threads, the solution `bonk_28160000003HNEIMNHNFNEIIJDJ` was found in about **one hour**.

---

## Solution

### Step 1 — Clone and modify md5zerofinder

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ git clone https://github.com/EnesO226/md5zerofinder
└─$ cd md5zerofinder
# → Modify to search for hashes starting with "flag#" instead of leading zeros
```

### Step 2 — Run with multiple threads

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 finder.py --threads 20 --prefix "flag#"
# → Runs for ~1 hour
# → Found: bonk_28160000003HNEIMNHNFNEIIJDJ
```

### Step 3 — Send to the server

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nc 155.248.210.243 42391
> bonk_28160000003HNEIMNHNFNEIIJDJ
# → ictf{holy_smokes_i_won_the_jackpot}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| md5zerofinder (modified) | Find MD5 hash starting with "flag#" | ⭐⭐⭐ Hard |

---

## Key Takeaways

- Partial MD5 preimage search is feasible with GPU-accelerated tools
- Tools built for one purpose (leading zeros) can be repurposed for adjacent problems (custom prefixes)
- 20 threads × 1 hour is about 72 trillion hashes tried — brute force at scale
