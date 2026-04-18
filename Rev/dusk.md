# dusk — ImaginaryCTF Writeup

**Challenge:** dusk  
**Category:** Rev  
**Difficulty:** Hard  
**Points:** 100 pts  
**Flag:** `ictf{fun_w17h_f4k3_350l4n65_a55b1dfe}`

---

## Description

> x86 sucks

**Attachment:** Binary with nested VM

---

## Background Knowledge (Read This First!)

### What is subleq?

**subleq** (subtract and branch if less than or equal) is a one-instruction computer. It is a Turing-complete esoteric architecture where every operation is:

```
subleq a, b, c:
  mem[b] -= mem[a]
  if mem[b] <= 0: goto c
```

This challenge uses a **modified variant** that branches on equality instead of ≤.

### What is a Nested VM?

The outer VM (subleq-like) runs a program that contains another **inner VM** with a different instruction set. You must:
1. Reverse the outer VM
2. Extract the inner VM's code
3. Reverse the inner VM's instruction set
4. Decode the flag from the inner VM's operations

### What are the Inner VM Instructions?

- `.` → decrements the current input character
- `-` → checks if current value minus `0x23` equals zero

The flag is recovered by splitting the inner VM code on `-` separators and counting the `.` operations in each segment:

```python
flag_char = chr(count_of_dots + 0x23)
```

---

## Solution

### Step 1 — Extract the inner VM code

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 -c "
with open('code.bin', 'rb') as f:
    code = f.read()
raw = code[0x8000//2:]
data = b''
for i in range(0, len(raw), 8):
    import struct
    data += bytes([struct.unpack('<Q', raw[i:i+8])[0]])
print(repr(data[:100]))
"
```

### Step 2 — Decode the flag from inner VM

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.py
```

```python
from pwn import *

with open('code.bin', 'rb') as f:
    code = f.read()

raw = code[0x8000//2:]
data = b''
for i in range(0, len(raw), 8):
    data += bytes([u64(raw[i:i+8])])

flag = ''
for part in data.split(b'-'):
    if len(part) == 0:
        continue
    flag += chr(len(part) + 0x23)

print(flag)
# → ictf{fun_w17h_f4k3_350l4n65_a55b1dfe}
```

### Step 3 — Run it

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 solve.py
# → ictf{fun_w17h_f4k3_350l4n65_a55b1dfe}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| pwntools | Parse binary and extract inner VM code | ⭐⭐⭐ Hard |
| Python 3 | Decode flag from inner VM instruction counts | ⭐⭐ Medium |

---

## Key Takeaways

- Nested VMs require you to reverse TWO instruction sets — always identify all layers first
- subleq is a well-known esoteric architecture — recognize it from the subtract-and-branch pattern
- Counting instruction occurrences (`len(part) + 0x23`) is a common unary encoding trick
- "x86 sucks" — the author chose to implement a one-instruction computer instead!
