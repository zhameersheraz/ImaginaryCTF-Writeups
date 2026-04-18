# Cutting Edge Crypto — ImaginaryCTF Writeup

**Challenge:** Cutting Edge Crypto  
**Category:** Crypto  
**Difficulty:** Easy  
**Points:** 25 pts  
**Flag:** `ictf{This_is_not_the_flag}`

---

## Description

> abandon all hope ye who enter, state of the art lattice attacks be ahead... hint reading the recent literature is highly recommended JK april fools nerddddd

**Attachment:** `chal.py`

---

## Background Knowledge (Read This First!)

### What is an April Fools Challenge?

This is a **troll challenge** released on April 1st. The description makes it sound like a terrifying cutting-edge lattice attack on RSA — but it is completely fake. The actual flag is just a template string hardcoded inside `chal.py`.

There are no known efficient attacks on RSA given the partial information provided. The author was just messing with people!

---

## Solution

### Step 1 — Download and open the source file

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <attachment url> -O chal.py
└─$ cat chal.py
# → Look for the flag variable at the top or bottom of the file
```

### Step 2 — Read the hardcoded flag

The flag is literally written as a template string inside `chal.py`:

```
ictf{This_is_not_the_flag}
```

### Step 3 — Submit it

Yes, that is actually the flag. Happy April Fools! 🎉

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| `cat` | Read the source file | ⭐ Trivial |

---

## Key Takeaways

- Always check the source file for hardcoded strings before attempting any math
- April Fools challenges in CTFs are a tradition — expect trolls on April 1st
- The description was designed to cause maximum fear — do not panic, read the code first
