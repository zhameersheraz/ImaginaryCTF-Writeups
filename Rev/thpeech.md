# thpeech — ImaginaryCTF Writeup

**Challenge:** thpeech  
**Category:** Rev  
**Difficulty:** Medium  
**Points:** 65 pts  
**Flag:** `ictf{lisp_wildity_9a2630268b069154}`

---

## Description

> impediment

**Attachment:** Lisp source code

---

## Background Knowledge (Read This First!)

### What is Lisp?

**Lisp** is one of the oldest programming languages, famous for its unique parenthesis-heavy syntax and its use in AI research. It is rarely seen in CTF challenges — making this unusual.

### What is Deterministic Randomness in Lisp?

In many Lisp implementations, the random number generator is **deterministic** — it produces the same sequence every single time you run the program. Unlike Python or C where the RNG is usually seeded by time, Lisp often starts with a fixed seed.

This means if the program uses "random" numbers, you can predict them by just running the same Lisp code yourself.

### What does "wild out the flag" mean?

Once you understand that the Lisp randomness is deterministic, you run the program yourself (or simulate it) and the flag falls out naturally — you just need to figure out what the program is doing.

*(The full source is provided in the challenge attachments.)*

---

## Solution

### Step 1 — Download and read the Lisp source

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <challenge url> -O thpeech.lisp
└─$ cat thpeech.lisp
# → Lisp program using random numbers
```

### Step 2 — Install a Lisp interpreter and run it

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ sudo apt-get install sbcl
└─$ sbcl --script thpeech.lisp
# → Same output every time due to deterministic RNG
# → Flag is revealed: ictf{lisp_wildity_9a2630268b069154}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| SBCL (Steel Bank Common Lisp) | Run the Lisp program | ⭐⭐ Medium |

---

## Key Takeaways

- Lisp's random number generator is deterministic — same seed, same output every time
- "Impediment" in the description refers to a speech impediment — "thpeech" = "speech" with a lisp!
- When a language's RNG is deterministic, running the program directly gives the flag
- First time programming in Lisp? Don't worry about indentation — just run it!
