# New Wave Rev — ImaginaryCTF Writeup

**Challenge:** New Wave Rev  
**Category:** Rev  
**Difficulty:** Hard  
**Points:** 110 pts  
**Flag:** `ictf{the_wave_equation_is_time_reversible}`

---

## Description

> Due to popular demand, here's a rev with absolutely no crypto in it! Look at the pretty waves this program makes, some say there's even a flag hidden in them...

**Attachment:** Program + BMP file

---

## Background Knowledge (Read This First!)

### What is the Wave Equation?

The **wave equation** is a partial differential equation that describes how waves propagate:

```
∂²u/∂t² = c² ∇²u
```

It models sound waves, light waves, water ripples, and more. A key property: **the wave equation is time-reversible** — you can run it backwards and recover the original state.

### What is the Challenge Doing?

The program:
1. Takes a 400×400 BMP file (which contains the hidden flag) as an initial state
2. Applies the wave equation forward in time — transforming it into "pretty waves"
3. Gives you the current wave state — not the original flag image

### How Do You Recover the Flag?

Press the **`-` key** in the program! The program already has a built-in reverse timestep feature. Running it backwards undoes all the wave propagation and recovers the original BMP with the flag.

---

## Solution

### Step 1 — Download and run the program

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <attachment url> -O wave_rev
└─$ chmod +x wave_rev
└─$ ./wave_rev output.bmp
# → A window opens showing the current wave state
```

### Step 2 — Press `-` to reverse time

```
# In the program window:
# Press '-' repeatedly (or hold it) to reverse the timestep
# → The waves collapse back into the original image
# → The flag becomes visible!
```

### Step 3 — Read the flag

```
ictf{the_wave_equation_is_time_reversible}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| The program itself | Press `-` to reverse the wave equation | ⭐ Easy (once you know!) |

---

## Key Takeaways

- The wave equation is time-reversible — this is a fundamental physics property
- Always read the keybindings when reversing interactive programs — `-` was the key here!
- "No crypto in it" — pure physics and signal processing instead
- EDIT: the original file had a bug making the flag unrecoverable — always use the latest link
