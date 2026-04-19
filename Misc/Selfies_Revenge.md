# Selfies Revenge — ImaginaryCTF Writeup

**Challenge:** Selfies Revenge  
**Category:** Misc  
**Difficulty:** Hard  
**Points:** 125 pts  
**Flag:** `ictf{evil_moai_statuette_fans}`

---

## Description

> The hackers are back at it again. This time they're demanding all my Kavakava, help me retrieve my flag...
> note: flag will be in all caps `ICTF{SOME_STRING}`, submit in all lowercase `ictf{some_string}`
> Hint: The paper this challenge is based on is by one D. baby et al.

**Attachment:** Challenge files + solve script

---

## Background Knowledge (Read This First!)

### Who is "D. baby et al"?

This is a hint to a specific academic paper on steganography or cryptography. The name is a playful reference — track down the paper to understand the technique used.

### What is the Challenge?

The challenge involves recovering a hidden flag from some carrier data. The procedure is "very similar but not identical" to the referenced paper — meaning you need to adapt the technique slightly.

*(The full solve script is provided in the challenge attachments.)*

---

## Solution

### Step 1 — Download the challenge and solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <challenge url>
└─$ wget <solve url> -O solve.py
```

### Step 2 — Run the solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 solve.py
# → ICTF{EVIL_MOAI_STATUETTE_FANS}
# → Submit as: ictf{evil_moai_statuette_fans}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Python 3 | Implement the adapted technique from the paper | ⭐⭐⭐ Hard |

---

## Key Takeaways

- CTF hints referencing papers are telling you exactly which technique to use — find and read the paper
- "Very similar but not identical" means you need to tweak the implementation slightly
- Always submit in the correct case format — all caps output → all lowercase submission here
