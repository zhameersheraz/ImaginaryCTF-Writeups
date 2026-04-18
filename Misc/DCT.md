# DCT — ImaginaryCTF Writeup

**Challenge:** DCT  
**Category:** Misc  
**Difficulty:** Hard  
**Points:** 90 pts  
**Flag:** `ictf{c0mPress3d_s3nsing_1s_s0_c00o0OL}`

---

## Description

> Oops, I can't see anything once the matrix is applied...

**Attachment:** Challenge source + output

---

## Background Knowledge (Read This First!)

### What is DCT?

**DCT (Discrete Cosine Transform)** is a mathematical transformation used in image and audio compression (JPEG, MP3). It converts spatial data into frequency components.

### What is the Vulnerability?

The challenge applies DCT with **random sampling** — it only keeps a random subset of the DCT coefficients. This destroys some information, but not all.

### What is Compressed Sensing?

**Compressed Sensing** is a signal processing technique that recovers a sparse signal from far fewer measurements than traditional sampling theory requires. The key idea:

- If the signal is **sparse** in some domain (like DCT), you can reconstruct it from far fewer samples
- A QR code is highly sparse in the DCT domain — most DCT coefficients are near zero
- Using an L1 minimization algorithm (like LASSO or basis pursuit), you reconstruct the original QR code from the random DCT samples

Once recovered, scan the QR code to get the flag!

---

## Solution

### Step 1 — Download the challenge

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ wget <attachment url> -O dct_challenge.py
└─$ python3 dct_challenge.py
# → Outputs random DCT samples of a QR code
```

### Step 2 — Apply compressed sensing to recover the QR code

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ pip install scikit-learn --break-system-packages
└─$ nano solve.py
```

```python
import numpy as np
from sklearn.linear_model import Lasso
from scipy.fft import dctn, idctn

# Load the random DCT samples and their positions
samples   = np.load('samples.npy')
positions = np.load('positions.npy')

# Build the measurement matrix (DCT basis vectors at sampled positions)
N = 128  # QR code size
A = ...  # measurement matrix

# L1 minimization to recover sparse DCT coefficients
lasso = Lasso(alpha=0.001, max_iter=10000)
lasso.fit(A, samples)
recovered_dct = lasso.coef_

# Inverse DCT to get the QR code image
qr_image = idctn(recovered_dct.reshape(N, N))

# Save and scan
import PIL.Image
img = PIL.Image.fromarray((qr_image > 0.5).astype('uint8') * 255)
img.save('recovered_qr.png')
# → Scan with a QR reader → ictf{c0mPress3d_s3nsing_1s_s0_c00o0OL}
```

### Step 3 — Scan the recovered QR code

```
┌──(zham㉿kali)-[/media/sf_downloads]
└─$ zbarimg recovered_qr.png
# → ictf{c0mPress3d_s3nsing_1s_s0_c00o0OL}
```

---

## Tools Used

| Tool | Purpose | Level |
|------|---------|-------|
| Python 3 + scikit-learn | L1 minimization (LASSO) for compressed sensing | ⭐⭐⭐ Hard |
| scipy DCT/IDCT | Forward and inverse DCT transforms | ⭐⭐ Medium |
| zbarimg | Scan the recovered QR code | ⭐ Easy |

---

## Key Takeaways

- Compressed Sensing recovers sparse signals from far fewer measurements than expected
- QR codes are sparse in the DCT domain — perfect for compressed sensing recovery
- L1 minimization (LASSO) is the standard algorithm for compressed sensing reconstruction
- DCT-based challenges often involve JPEG-like compression — always think in frequency domain
