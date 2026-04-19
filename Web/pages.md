# pages — ImaginaryCTF Writeup

**Challenge:** pages  
**Category:** Web  
**Difficulty:** Hard  
**Points:** 125 pts  
**Flag:** `ictf{nice_very_cool_:)}`

---

## Description

> page page page page page page

**Site:** `http://155.248.210.243:42136/`

---

## Background Knowledge (Read This First!)

### What is the Challenge?

This is a complex web challenge. The full write-up by the author is available on GitHub Gist. The "page page page" description hints at pagination or page-based navigation.

*(The full writeup is available at: `https://gist.github.com/icesfont/487d2838af9ade8a94da5ec8e3072273`)*

---

## Solution

### Step 1 — Read the full writeup

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ firefox "https://gist.github.com/icesfont/487d2838af9ade8a94da5ec8e3072273"
```

### Step 2 — Follow the exploit steps

The detailed exploitation steps are in the author's gist. Implement and run against the server.

```
→ ictf{nice_very_cool_:)}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Browser DevTools | Explore the web app | ⭐⭐ Medium |
| Python requests | Exploit the vulnerability | ⭐⭐⭐ Hard |

---

## Key Takeaways

- Complex web challenges often have detailed author writeups — always check the attachments and linked gists
- "page page page" = the vulnerability involves page/pagination manipulation
