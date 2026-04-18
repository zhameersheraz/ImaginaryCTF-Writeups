# Vault — ImaginaryCTF Writeup

**Challenge:** Vault  
**Category:** Rev  
**Difficulty:** Medium  
**Points:** 75 pts  
**Flag:** `ictf{w4Sm_W4sm_wAsM_R3v3rS1ng}`

---

## Description

> Can you unlock the vault and steal my secret??

**Site:** `http://217.154.76.42:8080/`

---

## Background Knowledge (Read This First!)

### What is WebAssembly (WASM)?

**WebAssembly (WASM)** is a binary instruction format that runs in the browser at near-native speed. Many web apps use WASM for performance-critical code — like PIN validation here.

### What is Z3?

**Z3** is a powerful **SMT solver** (Satisfiability Modulo Theories) from Microsoft Research. You give it constraints, and it finds values that satisfy all of them. It is widely used in CTF challenges to solve complex systems of equations.

### What is the Attack?

1. Download the WASM binary from the site
2. Reverse it to find the PIN validation constraints
3. Feed those constraints into Z3
4. Z3 solves for the correct PIN
5. Enter the PIN on the site to get the flag

---

## Solution

### Step 1 — Download the WASM binary

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ firefox "http://217.154.76.42:8080/"
# → Open DevTools → Sources → find the .wasm file → download it
```

### Step 2 — Decompile the WASM

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wasm2wat vault.wasm -o vault.wat
└─$ cat vault.wat
# → Find the PIN validation function and extract constraints
```

### Step 3 — Write a Z3 script to solve the constraints

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.py
```

```python
from z3 import *

# Create Z3 variables for each digit of the PIN
digits = [BitVec(f'd{i}', 32) for i in range(PIN_LENGTH)]
s = Solver()

# Add constraints extracted from the WASM
# (fill these from the decompiled .wat file)
s.add(digits[0] + digits[1] == ...)
s.add(digits[2] * digits[3] == ...)
# ...

if s.check() == sat:
    m = s.model()
    pin = ''.join(str(m[d]) for d in digits)
    print("PIN:", pin)
```

### Step 4 — Enter the PIN and get the flag

```
PIN: <solved by Z3>
→ ictf{w4Sm_W4sm_wAsM_R3v3rS1ng}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| `wasm2wat` | Decompile WASM binary to readable WAT format | ⭐⭐ Medium |
| Z3 (Python) | Solve PIN constraints automatically | ⭐⭐⭐ Hard |
| Browser DevTools | Download the WASM binary | ⭐ Easy |

---

## Key Takeaways

- WASM binaries can be decompiled with `wasm2wat` — they are not obfuscated
- Z3 can automatically solve complex constraint systems in seconds
- Any PIN validation in WASM is reversible — never trust client-side validation
