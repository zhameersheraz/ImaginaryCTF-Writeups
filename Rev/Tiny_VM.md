# Tiny VM — ImaginaryCTF Writeup

**Challenge:** Tiny VM  
**Category:** Rev  
**Difficulty:** Hard  
**Points:** 100 pts  
**Flag:** `ictf{4_vm_4nd_Rc4_for_fuN}`

---

## Description

> Hey bro, I misplaced my flag somewhere in a stack of instructions. Think you can dig it out for me?

**Attachment:** Binary executable

---

## Background Knowledge (Read This First!)

### What is a Stack-Based VM?

A **stack-based virtual machine (VM)** is an interpreter that executes a custom instruction set using a stack for temporary storage. Instead of registers, operations push and pop values on the stack. The challenge hides the flag inside a custom VM's instruction stream.

### What is RC4?

**RC4** is a stream cipher that generates a keystream. It is symmetric — the same operation encrypts and decrypts:

```
ciphertext = plaintext XOR keystream
plaintext  = ciphertext XOR keystream
```

This means to decrypt, you just run RC4 again with the same key on the ciphertext.

### What are the Two Approaches?

1. **Reverse the VM** — analyze the instruction set, understand the RC4 implementation, extract the keystream and decrypt manually
2. **Debugger shortcut** — attach a debugger, feed the ciphertext as input, set a breakpoint at the comparison step — the decrypted flag is already in memory there!

---

## Solution (Debugger Method — Easier)

### Step 1 — Run the binary and observe

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ chmod +x tiny_vm
└─$ ./tiny_vm
# → Asks for input, checks against flag
```

### Step 2 — Attach GDB and find the comparison

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ gdb tiny_vm
(gdb) run
# → Input anything
(gdb) info functions
# → Find the comparison function
(gdb) break *<comparison_address>
(gdb) run
# → Feed ciphertext bytes as input
(gdb) x/s $rdi
# → Decrypted flag is in memory!
# → ictf{4_vm_4nd_Rc4_for_fuN}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| GDB | Debug the VM and intercept the decrypted flag at comparison | ⭐⭐⭐ Hard |
| Binary analysis (optional) | Reverse the full VM instruction set | ⭐⭐⭐ Hard |

---

## Key Takeaways

- RC4 encryption and decryption are identical — feeding ciphertext through the same cipher gives plaintext
- Breakpoints at comparison instructions reveal decrypted values in memory — a classic debugger trick
- Custom VMs in CTFs usually implement simple ciphers — always look for RC4, XOR, Caesar patterns
