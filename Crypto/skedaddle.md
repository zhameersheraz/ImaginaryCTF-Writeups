# skedaddle — ImaginaryCTF Writeup

**Challenge:** skedaddle  
**Category:** Crypto  
**Difficulty:** Medium  
**Points:** 75 pts  
**Flag:** `ictf{13621417624426829092}`

---

## Description

> Just run this program and wait until it prints out the flag. It'll take a while though, so go on and skedaddle.

**Attachment:** C source file

---

## Background Knowledge (Read This First!)

### What is the program doing?

The program applies a series of operations involving XOR and multiplication to a 64-bit value. The key operation is:

```c
k ^= k >> 33
```

### Why does `k ^= k >> 33` cancel itself?

When you XOR a value with its upper 33 bits shifted right, the **upper 33 bits are preserved unchanged** and the lower 31 bits are affected. But critically, this operation is **self-inverting** in a specific sense — the upper 33 bits are never touched, which means we only need to brute force the lower 33 bits!

This reduces the search space from `2^64` to `2^33` — roughly 8.5 billion values, which is feasible in seconds on a modern CPU.

---

## Solution

### Step 1 — Download and understand the source

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <attachment url> -O skedaddle.c
└─$ cat skedaddle.c
# → Identify the XOR-shift operations and the brute force window
```

### Step 2 — Write the optimized brute force in C

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.c
```

```c
#include <stdio.h>
#include <stdint.h>

int main() {
    for (uint64_t i = 1; i < (1UL << 33); i++) {
        uint64_t j = (i ^ (i * 0xc4ceb9fe1a85ec53 * 0xff51afd7ed558ccd)) << 33 | i;
        if ((j ^ (j * 0xc4ceb9fe1a85ec53 * 0xff51afd7ed558ccd)) == j >> 33) {
            uint64_t k = j * 0xc4ceb9fe1a85ec53;
            printf("ictf{%lu}\n", k ^ (k >> 33));
        }
    }
    return 0;
}
```

### Step 3 — Compile and run

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ gcc -O2 -o solve solve.c
└─$ ./solve
# → ictf{13621417624426829092}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| C (gcc) | Fast 33-bit brute force | ⭐⭐ Medium |

---

## Key Takeaways

- `k ^= k >> 33` preserves the upper 33 bits — reducing brute force from 2^64 to 2^33
- Always look for operations that reduce the effective search space
- The challenge name "skedaddle" = go away and wait — it was designed to take forever naively!
