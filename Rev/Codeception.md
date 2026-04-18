# Codeception — ImaginaryCTF Writeup

**Challenge:** Codeception  
**Category:** Rev  
**Difficulty:** Medium  
**Points:** 75 pts  
**Flag:** `ictf{a_code_in_code_is_usually_bad_code}`

---

## Description

> we know you all love bad code, so we put some bad code inside your bad code 😄

**Attachment:** Binary using asmjit

---

## Background Knowledge (Read This First!)

### What is asmjit?

**AsmJIT** is a just-in-time compilation library that generates machine code at runtime. Programs using asmjit build and execute assembly instructions dynamically — making static analysis harder since the code doesn't exist until the program runs.

### What is a Runtime-Built Hash?

The program builds a **hash function at runtime** using asmjit — the hash function is assembled in memory when you run the binary. This means:
- You cannot see the hash function in a static disassembler
- You must either run it and intercept it dynamically, or understand how asmjit constructs it statically

### Why is it "easily invertible"?

Despite being built dynamically, the hash function itself has a simple mathematical structure — it is invertible (you can reverse it to find the preimage). Once you isolate the hash function (either by dumping it at runtime or analyzing the asmjit code), breaking it is straightforward.

*(The full source is provided in the challenge attachments.)*

---

## Solution

### Step 1 — Download the binary

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <challenge url> -O codeception
└─$ chmod +x codeception
└─$ file codeception
```

### Step 2 — Option A: Isolate at runtime with GDB

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ gdb ./codeception
(gdb) break main
(gdb) run
# → Step through until asmjit generates the hash function in memory
# → Dump the JIT-compiled bytes and analyze them
(gdb) x/20i <jit_address>
```

### Step 3 — Option B: Analyze the asmjit code statically

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ ghidra
# → Load the binary → find the asmjit API calls → reconstruct what hash is being built
```

### Step 4 — Invert the hash and decrypt

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 solve.py
# → ictf{a_code_in_code_is_usually_bad_code}
```

*(Full source provided in the challenge attachments.)*

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| GDB | Intercept JIT-compiled hash at runtime | ⭐⭐⭐ Hard |
| Ghidra (optional) | Static analysis of asmjit calls | ⭐⭐⭐ Hard |
| Python 3 | Invert the hash function | ⭐⭐ Medium |

---

## Key Takeaways

- JIT-compiled code (via asmjit) is invisible to static analysis — always check for runtime code generation
- Even dynamically built code can be intercepted with a debugger at the right moment
- "Code in code" = JIT generated code inside a compiled binary
