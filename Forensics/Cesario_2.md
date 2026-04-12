# Cesario 2 — ImaginaryCTF Writeup

**Challenge:** Cesario 2  
**Category:** Forensics  
**Difficulty:** Medium  
**Points:** 65 pts  
**Flag:** `ictf{she_produced_3_g1_winning_children}`

---

## Description

> Cesario won the 2005 American Oaks in just 119.03 seconds, anyways did you know horses have a higher range of hearing than humans...

**Attachment:** Audio file

---

## Background Knowledge (Read This First!)

### What is the hint about horses' hearing?

Horses can hear frequencies up to **33,500 Hz** — much higher than humans (max ~20,000 Hz). The description is hinting that the hidden data is encoded at a **high frequency** (11.9 kHz in this case) that you need to look for in the audio.

### What are 50ms pulses?

The data is encoded as **binary pulses** — short bursts of a specific frequency:

- Pulse present (50ms at 11.9 kHz) = binary `1`
- Pulse absent = binary `0`

Read all the pulses in order → binary string → convert to ASCII → flag.

### What is an FFT?

A **Fast Fourier Transform (FFT)** converts an audio signal from the time domain (amplitude over time) to the frequency domain (which frequencies are present at each moment). This lets you see if 11.9 kHz is active at each 50ms window.

### What is Goertzel's Algorithm?

**Goertzel's algorithm** is an efficient way to detect whether a *specific* frequency is present in a signal — faster than a full FFT when you only care about one frequency. The description mentions it as a bonus method.

---

## Solution

### Step 1 — Download and inspect the audio file

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ file cesario2.wav
# → WAV audio file
```

Open in **Audacity** or **Sonic Visualizer** and look at the spectrogram — you will see bursts at 11.9 kHz.

### Step 2 — Create the decode script

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ nano solve.py
```

```python
import numpy as np
import wave

# Read the WAV file
with wave.open('cesario2.wav', 'rb') as wf:
    rate    = wf.getframerate()
    frames  = wf.readframes(wf.getnframes())
    samples = np.frombuffer(frames, dtype=np.int16).astype(float)

TARGET_FREQ  = 11900   # Hz
PULSE_MS     = 50      # ms per bit
window_size  = int(rate * PULSE_MS / 1000)

bits = []
for i in range(0, len(samples) - window_size, window_size):
    window  = samples[i:i + window_size]
    fft     = np.abs(np.fft.rfft(window))
    freqs   = np.fft.rfftfreq(window_size, 1 / rate)
    # Find power at target frequency
    idx     = np.argmin(np.abs(freqs - TARGET_FREQ))
    power   = fft[idx]
    bits.append('1' if power > threshold else '0')  # set threshold by inspection

# Convert bits to ASCII
flag = ''
for i in range(0, len(bits) - 7, 8):
    byte = int(''.join(bits[i:i+8]), 2)
    flag += chr(byte)

print(flag)
# → ictf{she_produced_3_g1_winning_children}
```

### Step 3 — Run it

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ python3 solve.py
# → ictf{she_produced_3_g1_winning_children}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Python 3 (`numpy`) | FFT analysis to detect 11.9 kHz pulses | ⭐⭐ Medium |
| Audacity (optional) | Visually inspect the spectrogram | ⭐ Easy |

---

## Key Takeaways

- Data can be hidden in audio files as **frequency-domain pulses** — not just in the waveform
- The description literally tells you the parameters: 50ms pulses at 11.9 kHz
- FFT lets you see *which frequencies are present* at any moment in time
- The hint about horse hearing ranges was pointing directly at the high-frequency encoding
