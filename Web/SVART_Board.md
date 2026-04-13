# SVART Board — ImaginaryCTF Writeup

**Challenge:** SVART Board  
**Category:** Web  
**Difficulty:** Hard  
**Points:** 125 pts  
**Flag:** `ictf{HOPE_THIS_WASNT_TOO_H{}`

---

## Description

> AW{-WINNING WELL-GU{ED HIGHLY-REG{ED SVART BO{
> (this challenge is based on BILLY BOARD from Hack.lu CTF 2025, but does not depend on having solved BILLY BOARD beforehand)

**Hint:** the initial navigation would send strict cookies if it were to the challenge site, but you can't initially send the bot to the challenge site since you need to perform csrf first, session history is involved after csrf

**Site:** `http://143.47.245.151:30560/`  
**Bot:** `143.47.245.151:30561`

---

## Background Knowledge (Read This First!)

### What is CSRF?

**CSRF (Cross-Site Request Forgery)** is an attack where you trick a victim's browser into making an authenticated request to a target site without their knowledge. The victim (here, the bot) is already logged in — your malicious page abuses that session.

### What are SameSite Cookies?

**SameSite** is a cookie attribute that controls when cookies are sent with cross-site requests:

- `Strict` — cookies are NEVER sent on cross-site navigation
- `Lax` — cookies are sent on top-level GET navigations only
- `None` — cookies are always sent (requires Secure)

The challenge hint says cookies are **strict** — meaning you cannot directly send the bot to the challenge site and have it bring its cookies. You need to perform CSRF **first**, then use session history to trigger the challenge site navigation.

### What is Session History?

Browsers keep a **history** of pages visited. After a CSRF interaction, you can use `history.back()` or `history.go()` to navigate to a previously visited page — and in some cases, the browser will include cookies it would not include on a fresh navigation.

---

## Solution

### Step 1 — Understand the flow

The attack chain is:

```
1. Send bot to your malicious page
2. Your page performs CSRF on the challenge site (bot has a session there)
3. Use session history (history.back / history.go) to navigate bot to challenge site
4. The navigation via history bypasses SameSite=Strict cookie restriction
5. Bot sends cookies → your page exfiltrates the flag
```

### Step 2 — Set up your malicious page

Host a page that:
1. Performs the CSRF action (e.g., a form POST to the challenge)
2. Then navigates back using session history to trigger the flag endpoint with the bot's cookies

```javascript
// On your malicious page:
// Step 1 - CSRF the challenge site
fetch('http://143.47.245.151:30560/action', {
    method: 'POST',
    credentials: 'include',
    body: '...'
}).then(() => {
    // Step 2 - use history to navigate to flag endpoint
    history.back();
    // or open the challenge URL via history manipulation
});
```

### Step 3 — Submit to the bot and capture the flag

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ curl 'http://143.47.245.151:30561/submit' --data 'url=http://your-server/exploit.html'
# → Bot visits your page → flag is exfiltrated to your server
```

*(Full exploit writeup by the author: `https://gist.github.com/icesfont/a38cf323817a75d61e0612662c6d0476`)*

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| HTML/JavaScript | CSRF exploit page + session history navigation | ⭐⭐⭐ Hard |
| `curl` | Submit URL to the bot | ⭐ Easy |
| Your own server | Receive the exfiltrated flag | ⭐⭐ Medium |

---

## Key Takeaways

- **SameSite=Strict** cookies block cross-site cookie sending on fresh navigation — but session history is a bypass
- **CSRF + history navigation** is a powerful combo when strict cookie policies are in play
- Always read the challenge hint carefully — it literally describes the attack chain
- This is based on BILLY BOARD from Hack.lu CTF 2025 — look up past CTF challenges for similar techniques
