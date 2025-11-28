# FDM

Frequency-Division-Multiplexing---Modulation-and-Demodulation-using-Python:

Aim:

To generate an FDM signal by multiplexing multiple baseband message signals on different carrier frequencies, transmit (sum) them, optionally add channel noise, then recover each message by bandpass filtering and coherent demodulation in Python (Google Colab). Observe time & frequency domain signals and measure recovery quality.

Apparatus Required:

Google Colab (or any Python environment)

Python libraries: numpy, matplotlib, scipy (scipy.signal)

Theory:

FDM places different message signals in separate, non-overlapping frequency bands by modulating each message onto a distinct carrier frequency. The multiplexed signal is the sum of all modulated channels. At the receiver, bandpass filters (or tuned filters) isolate each channel; then each isolated carrier is demodulated (coherently multiplied by a synchronized carrier) and low-pass filtered to recover the original baseband.

Procedure:

1 — Imports and parameters

2 — Create message signals and carriers

3 — Modulate each message (standard AM DSB-SC) and form FDM signal

4 — Frequency domain (spectrum) of FDM signal

5 — (Optional) Add AWGN noise to FDM signal

6 — Receiver: isolate each channel with bandpass filter

7 — Demodulate each isolated channel (coherent) and low-pass filter to recover baseband
```
Program:

fs = 11000; 
t = 0:1/fs:0.01-1/fs; 


f = [200 300 400 500 600]; 

// Generate 5 signals with amplitude = 1
m1 = sin(2 * %pi * f(1) * t);  m1 = m1 ./ max(abs(m1));
m2 = sin(2 * %pi * f(2) * t);  m2 = m2 ./ max(abs(m2));
m3 = sin(2 * %pi * f(3) * t);  m3 = m3 ./ max(abs(m3));
m4 = sin(2 * %pi * f(4) * t);  m4 = m4 ./ max(abs(m4));
m5 = sin(2 * %pi * f(5) * t);  m5 = m5 ./ max(abs(m5));

// Plot message signals
scf(0);
subplot(5,1,1); plot(t, m1); title("Message Signal 1 (Amp = 1)");
subplot(5,1,2); plot(t, m2); title("Message Signal 2 (Amp = 1)");
subplot(5,1,3); plot(t, m3); title("Message Signal 3 (Amp = 1)");
subplot(5,1,4); plot(t, m4); title("Message Signal 4 (Amp = 1)");
subplot(5,1,5); plot(t, m5); title("Message Signal 5 (Amp = 1)");

// TDM modulation
tdm = zeros(1, 5 * length(t));
for i = 1:length(t)
    tdm(5*(i-1)+1) = m1(i);
    tdm(5*(i-1)+2) = m2(i);
    tdm(5*(i-1)+3) = m3(i);
    tdm(5*(i-1)+4) = m4(i);
    tdm(5*(i-1)+5) = m5(i);
end

// Time vector for TDM
tdm_t = 0:1/fs:(length(tdm)-1)/fs;

// Plot TDM
scf(1);
plot(tdm_t, tdm, 'm');
title("TDM Modulated Signal (Amplitude = 1)");
xlabel("Time"); ylabel("Amplitude"); xgrid();

// Demodulate
m1_d = tdm(1:5:$);
m2_d = tdm(2:5:$);
m3_d = tdm(3:5:$);
m4_d = tdm(4:5:$);
m5_d = tdm(5:5:$);
_

```
Output:

<img width="783" height="442" alt="image" src="https://github.com/user-attachments/assets/ccc9a5ad-2da0-4855-ae66-699ecd20b4ac" />
<img width="796" height="547" alt="image" src="https://github.com/user-attachments/assets/b6bc51d1-bee0-4a83-ac83-dc598120aadd" />
<img width="796" height="547" alt="image" src="https://github.com/user-attachments/assets/02377310-d3a4-49b5-9d78-adf69985ef01" />
<img width="965" height="1280" alt="image" src="https://github.com/user-attachments/assets/08a1e55c-dc02-4e41-9ff0-4ada81d08494" />



Result:

FDM experiment was successfully simulated and verified using Python. The message signals were multiplexed and perfectly recovered.
