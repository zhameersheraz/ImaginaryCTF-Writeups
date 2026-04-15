# Tanino Gimlet — ImaginaryCTF Writeup

**Challenge:** Tanino Gimlet  
**Category:** Crypto  
**Difficulty:** Easy  
**Points:** 50 pts  
**Flag:** `ictf{thishorseabsolutelyhatesfencesforsomereason}`

---

## Description

> Vodka's dad has an eccentric hobby, do you know what it is?

**Attachment:** Ciphertext directly in the challenge:

```
tbafs{hashtsoaofieoyeerentssllscsr}chruefnoeiotem
```

---

## Background Knowledge (Read This First!)

### What is a Rail Fence Cipher?

A **Rail Fence cipher** is a transposition cipher that writes plaintext in a zigzag pattern across multiple "rails" (rows), then reads off each rail in order.

Example with 3 rails:

```
Rail 1: t . . . b . . . a . . . f . . . s
Rail 2: . a . s . h . s . t . o . a . o .
Rail 3: . . n . . . i . . . r . . . e . .
```

Reading row by row gives the ciphertext. To decrypt, you reverse the process.

### Why is it called "Rail Fence"?

The zigzag pattern visually resembles a wooden **rail fence** — hence the name. The description saying "this horse absolutely hates fences" is the hint!

---

## Solution

### Step 1 — Identify the cipher

The ciphertext `tbafs{hashtsoaofieoyeerentssllscsr}chruefnoeiotem` looks scrambled but has the `{` and `}` characters in it — it is a transposition, not a substitution.

The description mentions "fence" → **Rail Fence cipher**.

### Step 2 — Decode using dcode.fr or CyberChef

**Option A — dcode.fr:**

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ firefox "https://www.dcode.fr/rail-fence-cipher"
# → Paste ciphertext → Auto-solve → get flag
```

**Option B — CyberChef:**

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ firefox "https://gchq.github.io/CyberChef"
# → Add "Rail Fence Cipher Decode" operation → try rails 2, 3, 4...
```

### Step 3 — Read the flag

```
ictf{thishorseabsolutelyhatesfencesforsomereason}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| dcode.fr or CyberChef | Auto-decode Rail Fence cipher | ⭐ Easy |

---

## Key Takeaways

- The description hint "fences" directly tells you the cipher type
- Rail Fence is a transposition cipher — letter frequency analysis won't help, you need to find the rail count
- dcode.fr and CyberChef can auto-solve Rail Fence with just a few clicks
