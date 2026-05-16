# ASK AND FSK
# Aim
Write a simple Python program for the modulation and demodulation of ASK and FSK.
# Tools required
Google Colab
# Program
## ASK
```
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, lfilter

# Butterworth low-pass filter
def butter_lowpass_filter(data, cutoff, fs, order=5):
    nyquist = 0.5 * fs
    normal_cutoff = cutoff / nyquist
    b, a = butter(order, normal_cutoff, btype='low', analog=False)
    return lfilter(b, a, data)

# Parameters
fs = 1000              # Sampling frequency
f_carrier = 50         # Carrier frequency
bit_rate = 10          # Bit rate
T = 1                  # Total time

# Time axis
t = np.linspace(0, T, int(fs * T), endpoint=False)

# Binary data
bits = np.array([0, 0, 1, 1, 1, 0, 1, 0, 0, 0])

# Message signal
bit_duration = fs // bit_rate
message_signal = np.repeat(bits, bit_duration)

# Carrier signal
carrier = np.sin(2 * np.pi * f_carrier * t)

# ASK Modulation
ask_signal = message_signal * carrier

# ASK Demodulation
demodulated = ask_signal * carrier
filtered_signal = butter_lowpass_filter(demodulated, f_carrier, fs)

# Decode bits
decoded_bits = (filtered_signal[::bit_duration] > 0.25).astype(int)

# Plotting
plt.figure(figsize=(12, 8))

# Message Signal
plt.subplot(4, 1, 1)
plt.plot(t, message_signal, color='b')
plt.title("Message Signal")
plt.grid(True)

# Carrier Signal
plt.subplot(4, 1, 2)
plt.plot(t, carrier, color='g')
plt.title("Carrier Signal")
plt.grid(True)

# ASK Modulated Signal
plt.subplot(4, 1, 3)
plt.plot(t, ask_signal, color='r')
plt.title("ASK Modulated Signal")
plt.grid(True)

# Decoded Bits
plt.subplot(4, 1, 4)
plt.step(np.arange(len(decoded_bits)),
         decoded_bits,
         where='mid',
         color='r',
         marker='x')

plt.title("Decoded Bits")
plt.ylim(-0.1, 1.2)
plt.grid(True)

plt.tight_layout()
plt.show()
```






## FSK
```
import numpy as np
import matplotlib.pyplot as plt
from scipy.signal import butter, lfilter

# Low Pass Filter Function
def butter_lowpass_filter(data, cutoff, fs, order=5):
    nyquist = 0.5 * fs
    normal_cutoff = cutoff / nyquist
    b, a = butter(order, normal_cutoff, btype='low', analog=False)
    return lfilter(b, a, data)

# Parameters
fs = 1000          # Sampling frequency
f1 = 30            # Frequency for bit 0
f2 = 70            # Frequency for bit 1
bit_rate = 10      # Number of bits
T = 1              # Total time duration

# Time vector
t = np.linspace(0, T, int(fs * T), endpoint=False)

# Message bits
bits = np.array([1,1,1,1,1,0,1,1,0,0])

# Convert bits into signal waveform
bit_duration = fs // bit_rate
message_signal = np.repeat(bits, bit_duration)

# Carrier Signals
carrier_f1 = np.sin(2 * np.pi * f1 * t)
carrier_f2 = np.sin(2 * np.pi * f2 * t)

# FSK Modulation
fsk_signal = np.zeros_like(t)

for i, bit in enumerate(bits):
    
    start = i * bit_duration
    end = start + bit_duration

    freq = f2 if bit else f1

    fsk_signal[start:end] = np.sin(
        2 * np.pi * freq * t[start:end]
    )

# Reference signals
ref_f1 = np.sin(2 * np.pi * f1 * t)
ref_f2 = np.sin(2 * np.pi * f2 * t)

# Correlation and filtering
corr_f1 = butter_lowpass_filter(
    fsk_signal * ref_f1,
    f2,
    fs
)

corr_f2 = butter_lowpass_filter(
    fsk_signal * ref_f2,
    f2,
    fs
)

# Demodulation
decoded_bits = []

for i in range(bit_rate):

    start = i * bit_duration
    end = start + bit_duration

    energy_f1 = np.sum(corr_f1[start:end] ** 2)
    energy_f2 = np.sum(corr_f2[start:end] ** 2)

    decoded_bits.append(
        1 if energy_f2 > energy_f1 else 0
    )

decoded_bits = np.array(decoded_bits)

# Final Demodulated Signal
demodulated_signal = np.repeat(
    decoded_bits,
    bit_duration
)

# Plotting
plt.figure(figsize=(12,12))

# Message Signal
plt.subplot(5,1,1)
plt.plot(t, message_signal, color='b')
plt.title('Message Signal')
plt.grid(True)

# Carrier for bit 0
plt.subplot(5,1,2)
plt.plot(t, carrier_f1, color='g')
plt.title('Carrier Signal for bit = 0 (f1)')
plt.grid(True)

# Carrier for bit 1
plt.subplot(5,1,3)
plt.plot(t, carrier_f2, color='r')
plt.title('Carrier Signal for bit = 1 (f2)')
plt.grid(True)

# FSK Modulated Signal
plt.subplot(5,1,4)
plt.plot(t, fsk_signal, color='m')
plt.title('FSK Modulated Signal')
plt.grid(True)

# Demodulated Signal
plt.subplot(5,1,5)
plt.plot(t, demodulated_signal, color='k')
plt.title('Final Demodulated Signal')
plt.grid(True)

plt.tight_layout()
plt.show()
```


# Output Waveform
## ASK
<img width="1194" height="795" alt="image" src="https://github.com/user-attachments/assets/7c8d3993-36a4-40a5-8357-5981b41ffc9f" />








## FSK
<img width="1188" height="1190" alt="image" src="https://github.com/user-attachments/assets/3ed1366f-2030-4b08-b163-3c51949d0a56" />

# Results
THUS, THE ASK (Amplitude Shift Keying) AND FSK (Frequency Shift Keying) ARE PERFORMED
USING PYTHON

