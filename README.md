<img width="1920" height="1080" alt="Screenshot (138)" src="https://github.com/user-attachments/assets/442bffbc-36c1-4840-a867-3d658162f68c" />
<img width="1920" height="1080" alt="Screenshot (137)" src="https://github.com/user-attachments/assets/4be7f822-14cb-401e-8491-a08ebd774309" />
<img width="1920" height="1080" alt="Screenshot (136)" src="https://github.com/user-attachments/assets/31b508e0-0c28-4e0d-b12c-8073c6aef9d5" />
<img width="1920" height="1080" alt="Screenshot (135)" src="https://github.com/user-attachments/assets/d22fee77-407a-417b-abac-756967b74f6c" />
# Lecture05-Fourier--Assignment
Background

A square wave with amplitude ±1 and frequency 1 Hz can be written as a Fourier series that has only odd harmonics:

$$x(t) = \frac{4}{\pi}\sum_{k=1,3,5,\dots}^{H} \frac{\sin(2\pi k t)}{k}$$

Each harmonic is weaker than the one before it (amplitude falls as 1/k), so the low harmonics give the basic shape and the high harmonics add the fine detail.

1. Prediction (before running MATLAB)
With H = 1 the result will be just a sine wave, since only the fundamental is used.
As more odd harmonics are added, the waveform should look more and more like a square wave: flatter tops and bottoms and steeper edges.
The approximation will never be perfect with a finite number of terms. I expect some ripple on the flat parts, and probably some overshoot near the jumps because a smooth sum of sines cannot jump instantly.
2–3. Results

Square wave comparison

H	Odd harmonics used	Number of terms	Peak value
1	1	1	1.273 (= 4/π)
3	1, 3	2	1.200
9	1, 3, 5, 7, 9	5	1.182
25	1, 3, …, 25	13	1.179
4. What changes as H increases
H = 1: a pure sine wave with peak 4/π ≈ 1.27. It has the right period (1 s) and crosses zero at the right times, but its shape is nothing like a square wave.
H = 3: the third harmonic flattens the top and bottom. Each half period has two humps with a dip between them, and the edges are steeper.
H = 9: the top and bottom are much flatter, with small fast ripples. The transitions at t = 0, ±0.5 and ±1 s are now quite steep.
H = 25: the waveform looks close to an ideal square wave. The edges are almost vertical and the ripple on the flat parts is small and fast.

In short, as H increases:

The edges get steeper, because the high harmonics carry the fast changes.
The ripple gets faster (more oscillations per half period) and its amplitude in the middle of the flat parts gets smaller.
The approximation converges to the square wave everywhere except right at the jumps.
5. What happens near the sharp transitions

Close to each jump there is an overshoot: the signal goes above +1 (and below −1) before settling down. This is the Gibbs phenomenon.

The overshoot does not go away as H increases. The peak goes from 1.200 (H = 3) to 1.182 (H = 9) to 1.179 (H = 25), approaching about 1.179. That is roughly 9 % of the jump size (the jump is 2, from −1 to +1, so the overshoot is about 0.18).
What does change is the width of the overshoot. It gets narrower and moves closer to the edge as H increases, so its energy shrinks even though its height stays the same.
The reason is that a sum of a finite number of smooth sine waves cannot follow a discontinuous jump exactly. Cutting off the series abruptly at H leaves this ringing at every discontinuity.

Practical relevance: the same effect appears when a signal is band-limited (filtered or sampled), for example ringing next to sharp edges in images and audio, or overshoot in a digital signal's rise. Smoother truncation, using window functions such as Hann or Hamming, is the usual way to reduce it.

Conclusion

My prediction was mostly correct. Adding more odd harmonics makes the waveform steadily more square. The one thing more harmonics do not fix is the overshoot at the jumps, which settles at about 9 % (the Gibbs phenomenon) no matter how large H gets.
EXERCISE 2
<img width="1920" height="1080" alt="Screenshot (139)" src="https://github.com/user-attachments/assets/79516fb2-96e5-4ecd-a515-c48c4332ff35" />
1. Before running MATLAB: expected frequencies and amplitudes

The signal is

x(t) = 0.8·sin(2π·15·t) + 0.4·sin(2π·40·t)

Component	Frequency	Amplitude
1	15 Hz	0.8
2	40 Hz	0.4

Sampling settings: fs = 200 Hz, T = 1 s, so N = fs·T = 200 samples.

2 and 3. Time-domain plot and single-sided amplitude spectrum
matlab
fs = 200;                 % sampling frequency (Hz)
T  = 1;                   % duration (s)
t  = 0:1/fs:T-1/fs;       % time vector (200 samples)

x = 0.8*sin(2*pi*15*t) + 0.4*sin(2*pi*40*t);

% ---- Task 2: time domain ----
figure
subplot(2,1,1)
plot(t, x)
grid on
xlabel('Time (s)')
ylabel('Amplitude')
title('Signal in the time domain')

% ---- Task 3: single-sided amplitude spectrum ----
N  = length(x);
Y  = fft(x);
P2 = abs(Y/N);            % two-sided spectrum
P1 = P2(1:N/2+1);         % keep 0 ... fs/2
P1(2:end-1) = 2*P1(2:end-1);   % double everything except DC and Nyquist
f  = fs*(0:N/2)/N;        % frequency axis

subplot(2,1,2)
plot(f, P1)
grid on
xlabel('Frequency (Hz)')
ylabel('Amplitude')
title('Single-sided amplitude spectrum')

% ---- Task 4: find the peaks ----
[pks, locs] = findpeaks(P1, 'MinPeakHeight', 0.1);
peakFreqs = f(locs)
peakAmps  = pks

% ---- Task 5: frequency-bin spacing ----
df = fs/N
4. The two peaks

Your figure shows two peaks:

	Expected	Measured from FFT
Peak 1	15 Hz, 0.8	15 Hz, 0.8
Peak 2	40 Hz, 0.4	40 Hz, 0.4

The measured frequencies and amplitudes match the values used to build the signal exactly. That happens because the record holds a whole number of cycles of each sine: 15 cycles and 40 cycles in 1 s. Each tone therefore falls exactly on an FFT bin, so there’s no spectral leakage. The spectrum only goes up to 100 Hz because that is the Nyquist frequency, fs/2.

5. Frequency-bin spacing

Δf = fs / N = 200 / 200 = 1 Hz

This is the same as 1/T, which is 1/1 s. Each FFT bin is 1 Hz wide, so the 15 Hz tone lands in bin 15 and the 40 Hz tone lands in bin 40.
