# Twisty Curves — ImaginaryCTF Writeup

**Challenge:** Twisty Curves  
**Category:** Rev  
**Difficulty:** Easy  
**Points:** 50 pts  
**Flag:** `ictf{surprisingly_simple_space_filling_curves_are_really_cool_though!}`

---

## Description

> I built this brand new sick and twisted flag printer... note: wrap the output in ictf{} format

**Attachment:** Source code + encoded output

---

## Background Knowledge (Read This First!)

### What is a Hilbert Curve?

A **Hilbert curve** is a type of space-filling curve — a fractal that visits every point in a 2D grid exactly once. It has useful properties: points that are close together on the curve tend to be close together in 2D space too.

### How is the Flag Encoded?

Each character of the flag is encoded as a **position along the 2D Hilbert curve**. To decode:
1. Take each encoded position number
2. Convert it from a 1D Hilbert curve index back to a 2D (x, y) coordinate
3. Map that coordinate back to the original character

Since the output is small, this can even be done by hand!

*(The full source is provided in the challenge attachments.)*

---

## Solution

### Step 1 — Download and read the source

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <challenge url> -O twisty_curves.py
└─$ cat twisty_curves.py
# → Encodes characters as Hilbert curve positions
```

### Step 2 — Download and run the solve script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <solve url> -O solve.py
└─$ python3 solve.py
# → surprisingly_simple_space_filling_curves_are_really_cool_though!
```

### Step 3 — Wrap in ictf format

```
ictf{surprisingly_simple_space_filling_curves_are_really_cool_though!}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Python 3 | Reverse Hilbert curve position to character | ⭐⭐ Medium |

---

## Key Takeaways

- Hilbert curves map 1D indices to 2D positions — easy to invert
- Space-filling curves are used in databases, image processing, and now CTF encoding!
- The encoding is small enough to solve by hand if you understand the curve structure
