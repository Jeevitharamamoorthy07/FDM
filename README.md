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

Fs = 52300; t = 0:1/Fs:0.02;
m1 = sin(2*%pi100t); m2 = sin(2*%pi200t); m3 = sin(2*%pi300t); m4 = sin(2*%pi400t); m5 = sin(2*%pi500t); m6 = sin(2*%pi600t);

c1 = 2000; c2 = 4000; c3 = 8000; c4 = 10000; c5 = 12000; c6 = 15000;

carrier1 = cos(2*%pic1t); carrier2 = cos(2*%pic2t); carrier3 = cos(2*%pic3t); carrier4 = cos(2*%pic4t); carrier5 = cos(2*%pic5t); carrier6 = cos(2*%pic6t);

s1 = m1 .* carrier1; s2 = m2 .* carrier2; s3 = m3 .* carrier3; s4 = m4 .* carrier4; s5 = m5 .* carrier5; s6 = m6 .* carrier6;

s_total = s1 + s2 + s3 + s4 + s5 + s6;

 Demultiplex: multiply by carriers to shift each band back to baseband r1 = s_total .* carrier1; r2 = s_total .* carrier2; r3 = s_total .* carrier3; r4 = s_total .* carrier4; r5 = s_total .* carrier5; r6 = s_total .* carrier6;
 Simple FFT-based ideal low-pass filter (avoids butter/toolbox issues) function y = ideal_lowpass_fft(x, Fs, fc) N = length(x); X = fft(x); f = Fs*(0:N-1)/N; mask = (f <= fc) | (f >= Fs-fc); Y = X .* mask; y = real(ifft(Y)); endfunction

fc = 1000; dm1 = ideal_lowpass_fft(r1, Fs, fc); dm2 = ideal_lowpass_fft(r2, Fs, fc); dm3 = ideal_lowpass_fft(r3, Fs, fc); dm4 = ideal_lowpass_fft(r4, Fs, fc); dm5 = ideal_lowpass_fft(r5, Fs, fc); dm6 = ideal_lowpass_fft(r6, Fs, fc);
figure(1); subplot(3,2,1); plot(t,m1); title("Message Signal 1"); subplot(3,2,2); plot(t,m2); title("Message Signal 2"); subplot(3,2,3); plot(t,m3); title("Message Signal 3"); subplot(3,2,4); plot(t,m4); title("Message Signal 4"); subplot(3,2,5); plot(t,m5); title("Message Signal 5"); subplot(3,2,6); plot(t,m6); title("Message Signal 6");

figure(2); plot(t, s_total); title("Multiplexed FDM Signal");

figure(3); subplot(3,2,1); plot(t,dm1); title("Recovered Signal 1"); subplot(3,2,2); plot(t,dm2); title("Recovered Signal 2"); subplot(3,2,3); plot(t,dm3); title("Recovered Signal 3"); subplot(3,2,4); plot(t,dm4); title("Recovered Signal 4"); subplot(3,2,5); plot(t,dm5); title("Recovered Signal 5"); subplot(3,2,6); plot(t,dm6); title("Recovered Signal 6");

```
Output:

<img width="759" height="634" alt="image" src="https://github.com/user-attachments/assets/1e7426e1-06d0-476c-afc2-53de87a3b1b6" />
<img width="989" height="390" alt="image" src="https://github.com/user-attachments/assets/605d06c6-d074-43e1-a517-0a06d198c52e" />

Result:

FDM experiment was successfully simulated and verified using Python. The message signals were multiplexed and perfectly recovered.
