# Neck Strain — ImaginaryCTF Writeup

**Challenge:** Neck Strain  
**Category:** Crypto  
**Difficulty:** Easy  
**Points:** 50 pts  
**Flag:** `ictf{do_you_like_ascii_art}`

---

## Description

> Sometimes you just need to look at things from another angle...

**Attachment:** ASCII art file

---

## Background Knowledge (Read This First!)

### What is Upside-Down ASCII Art?

The challenge encodes the flag as **reversed ASCII art** — the text is flipped upside-down so it is unreadable at normal orientation. To decode it, you either:
- Reverse the file (flip it back)
- Flip your monitor 180 degrees (the "intended" solve according to the author 😄)

---

## Solution

### Option A — Reverse the file (easy)

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ tac ascii_art.txt | rev
# tac reverses line order, rev reverses each line
# → ictf{do_you_like_ascii_art}
```

### Option B — Python

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 -c "
lines = open('ascii_art.txt').readlines()
for line in reversed(lines):
    print(line[::-1].rstrip())
"
# → ictf{do_you_like_ascii_art}
```

### Option C — Flip your monitor (intended solve)

Rotate your monitor 180° and read the ASCII art normally. 😃

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| `tac` + `rev` | Flip text upside-down to reveal flag | ⭐ Easy |

---

## Key Takeaways

- "Look at things from another angle" = literally flip the text 180°
- `tac` reverses line order, `rev` reverses characters — together they flip text upside-down
- Sometimes the simplest solution (flip your monitor) is the intended one!
