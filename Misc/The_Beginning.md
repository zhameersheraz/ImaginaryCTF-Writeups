# The Beginning — ImaginaryCTF Writeup

**Challenge:** The Beginning  
**Category:** Misc  
**Difficulty:** Easy  
**Points:** 50 pts  
**Flag:** `ictf{1594666272}`

---

## Description

> What was the time, in Unix timestamp, that the ImaginaryCTF discord server was created?
> Ex: `ictf{1766164753}`
> Note: You do not have to be in the discord server in order to solve this challenge.

**Hint:** *(none)*

**Attachment:** None

---

## Background Knowledge (Read This First!)

### What is a Unix Timestamp?

A **Unix timestamp** is the number of seconds that have passed since January 1, 1970 (UTC). It is how computers internally represent points in time. For example, `1594666272` = July 13, 2020.

### What is a Discord Snowflake ID?

Every Discord server, user, and message has a unique **Snowflake ID** — a large integer that encodes the creation time inside it. You can decode a Snowflake ID to extract the exact timestamp of when that object was created.

### How do I find the Discord server ID?

The ImaginaryCTF Discord server is publicly listed. Its server ID is embedded in the invite URL:

```
https://discord.com/servers/imaginaryctf-732308165265326080
                                            ↑
                                   This number is the Snowflake ID
```

The Snowflake ID is: `732308165265326080`

---

## Solution

### Step 1 — Find the server Snowflake ID

You do NOT need to join the server. Search for ImaginaryCTF on:

```
https://discord.com/servers
```

Or go directly to:

```
https://discord.com/servers/imaginaryctf-732308165265326080
```

The numerical part of the URL — `732308165265326080` — is the **Discord Snowflake ID**.

### Alternative — Developer Mode (if you are in the server)

```
Discord Settings → Advanced → Enable Developer Mode
Right-click the server icon → Copy Server ID
```

### Step 2 — Decode the Snowflake to get the Unix timestamp

Go to any Discord Snowflake decoder, for example:

```
https://discord.com/developers/docs/reference#snowflakes
```

Or use this Python one-liner:

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 -c "print((732308165265326080 >> 22) // 1000 + 1420070400)"
# → 1594666272
```

### Step 3 — Submit the flag

```
ictf{1594666272}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| discord.com/servers | Find the server URL and Snowflake ID | ⭐ Easy |
| Python 3 | Decode the Snowflake to Unix timestamp | ⭐ Easy |

---

## Key Takeaways

- Every Discord server/user/message has a **Snowflake ID** that encodes its creation time
- You can decode any Snowflake with: `(snowflake >> 22) // 1000 + 1420070400`
- You do NOT need to be in a Discord server to find its ID — the public server list URL contains it
- OSINT challenges often involve finding hidden metadata in public links or IDs
