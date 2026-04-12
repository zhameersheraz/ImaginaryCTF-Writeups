# Cesario — ImaginaryCTF Writeup

**Challenge:** Cesario  
**Category:** Crypto  
**Difficulty:** Easy  
**Points:** 50 pts  
**Flag:** `ictf{she_was_also_one_of_the_greatest_broodmares_in_jra_history}`

---

## Description

> Cesario is the only (I think?) horse to have ever won both the Japanese and American Oaks, hint challenge is inspired by questionably-distributed from the annual thx Minerva

**Attachment:** Encrypted file / grid

---

## Background Knowledge (Read This First!)

### What is a Caesar Cipher?

A **Caesar cipher** shifts each letter by a fixed number of positions in the alphabet. For example, with a shift of 3:

```
A → D,  B → E,  C → F, ...
```

It is one of the oldest and simplest encryption methods.

### What is special about "along the diagonal"?

The challenge hides the cipher along the **diagonal** of a grid of text. Only the diagonal characters are encrypted — the rest are filler or noise. This is the trick that makes it non-obvious at first glance.

### How do you find the diagonal?

If the grid is N×N, the diagonal is the characters at positions `(0,0), (1,1), (2,2), ...` — one character per row, stepping one column to the right each time.

### How do you know which shift to use?

Since only the diagonal has **special characters** (non-standard or unusual values), that diagonal is what you need to target. Try all 26 Caesar shifts on just those characters until you get readable English.

---

## Solution

### Step 1 — Download and inspect the file

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ cat cesario.txt
# → A grid of characters — look for special characters along the diagonal
```

### Step 2 — Extract the diagonal

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.py
```

```python
with open('cesario.txt') as f:
    lines = f.readlines()

# Extract diagonal characters
diagonal = ''
for i, line in enumerate(lines):
    line = line.rstrip('\n')
    if i < len(line):
        diagonal += line[i]

print("Diagonal:", diagonal)

# Try all 26 Caesar shifts
for shift in range(26):
    decrypted = ''
    for c in diagonal:
        if c.isalpha():
            decrypted += chr((ord(c.lower()) - ord('a') - shift) % 26 + ord('a'))
        else:
            decrypted += c
    print(f"Shift {shift}: {decrypted}")
```

### Step 3 — Run and identify the correct shift

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 solve.py
# → One shift will produce readable English — that is your flag
# → ictf{she_was_also_one_of_the_greatest_broodmares_in_jra_history}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Python 3 | Extract diagonal, brute force all 26 Caesar shifts | ⭐ Easy |

---

## Key Takeaways

- Always look at the **structure** of the ciphertext — not just the content. Diagonals, rows, columns can all hide data
- Special characters in a grid (non-standard values) are a big hint about where the cipher is applied
- Caesar cipher has only 26 possible keys — always brute force all of them
- The description references "diagonal" — hints in challenge descriptions are always useful
