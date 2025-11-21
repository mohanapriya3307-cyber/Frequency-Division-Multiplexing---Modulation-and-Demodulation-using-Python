# Frequency-Division-Multiplexing---Modulation-and-Demodulation-using-Python

__Aim__:

To generate an FDM signal by multiplexing multiple baseband message signals on different carrier frequencies, transmit (sum) them, optionally add channel noise, then recover each message by bandpass filtering and coherent demodulation in Python (Google Colab). Observe time & frequency domain signals and measure recovery quality.


__Apparatus Required__:

Google Colab (or any Python environment)

Python libraries: numpy, matplotlib, scipy (scipy.signal)


__Theory__:

FDM places different message signals in separate, non-overlapping frequency bands by modulating each message onto a distinct carrier frequency. The multiplexed signal is the sum of all modulated channels. At the receiver, bandpass filters (or tuned filters) isolate each channel; then each isolated carrier is demodulated (coherently multiplied by a synchronized carrier) and low-pass filtered to recover the original baseband.

__Procedure__:

1 — Imports and parameters

2 — Create message signals and carriers

3 — Modulate each message (standard AM DSB-SC) and form FDM signal

4 — Frequency domain (spectrum) of FDM signal

5 — (Optional) Add AWGN noise to FDM signal

6 — Receiver: isolate each channel with bandpass filter

7 — Demodulate each isolated channel (coherent) and low-pass filter to recover baseband

__PROGRAM__:
```
import numpy as np
import matplotlib.pyplot as plt

# Time variables
Fs = 5000            # Sampling frequency
t = np.arange(0, 0.04, 1/Fs)

# Message signals
Am1, Fm1 = 1, 50     # Message 1
Am2, Fm2 = 1, 100    # Message 2

m1 = Am1 * np.sin(2*np.pi*Fm1*t)
m2 = Am2 * np.sin(2*np.pi*Fm2*t)

# Carrier signals for FDM
Fc1 = 500
Fc2 = 1200

c1 = np.cos(2*np.pi*Fc1*t)
c2 = np.cos(2*np.pi*Fc2*t)

# Modulation (DSB-SC)
s1 = m1 * c1
s2 = m2 * c2

# Multiplexed signal (FDM combined)
fdm_signal = s1 + s2

# Demodulation
d1 = fdm_signal * c1
d2 = fdm_signal * c2

# Low-pass filter (simple moving average)
def low_pass_filter(sig, N=200):
    return np.convolve(sig, np.ones(N)/N, mode='same')

dm1 = low_pass_filter(d1)
dm2 = low_pass_filter(d2)

# Plot all signals
plt.figure(figsize=(12, 18))

plt.subplot(6,1,1)
plt.plot(t, m1)
plt.title("Message Signal m1 (50 Hz)")

plt.subplot(6,1,2)
plt.plot(t, m2)
plt.title("Message Signal m2 (100 Hz)")

plt.subplot(6,1,3)
plt.plot(t, fdm_signal)
plt.title("Multiplexed FDM Signal")

plt.subplot(6,1,4)
plt.plot(t, dm1)
plt.title("Demodulated Signal 1 (Recovered m1)")

plt.subplot(6,1,5)
plt.plot(t, dm2)
plt.title("Demodulated Signal 2 (Recovered m2)")

plt.tight_layout()
plt.show()
```

__Output__:

<img width="1552" height="769" alt="image" src="https://github.com/user-attachments/assets/c31bebc7-dc3e-4acf-a882-9bc74a188053" />


<img width="1483" height="750" alt="image" src="https://github.com/user-attachments/assets/b093da12-e374-4943-b46f-353268582808" />


<img width="1482" height="384" alt="image" src="https://github.com/user-attachments/assets/c520b60f-ab03-4eeb-98b8-255246281725" />


__Result__:
FDM experiment was successfully simulated and verified using Python. The message signals were multiplexed and perfectly recovered.
