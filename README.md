EXERCISE 1
<img width="1920" height="1080" alt="Screenshot (138)" src="https://github.com/user-attachments/assets/442bffbc-36c1-4840-a867-3d658162f68c" />
<img width="1920" height="1080" alt="Screenshot (137)" src="https://github.com/user-attachments/assets/4be7f822-14cb-401e-8491-a08ebd774309" />
<img width="1920" height="1080" alt="Screenshot (136)" src="https://github.com/user-attachments/assets/31b508e0-0c28-4e0d-b12c-8073c6aef9d5" />
<img width="1920" height="1080" alt="Screenshot (135)" src="https://github.com/user-attachments/assets/d22fee77-407a-417b-abac-756967b74f6c" />

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

6. EXERCISE 3
7. <img width="1920" height="1080" alt="Screenshot (145)" src="https://github.com/user-attachments/assets/0897b652-f2a3-41e8-b41a-c31af784557d" />
<img width="1920" height="1080" alt="Screenshot (144)" src="https://github.com/user-attachments/assets/a41a1cb5-25b9-40cb-bbaa-5663b1290f00" />
<img width="1920" height="1080" alt="Screenshot (143)" src="https://github.com/user-attachments/assets/76cfc2b1-91eb-42a1-becd-6dea9fdd3342" />
<img width="1920" height="1080" alt="Screenshot (142)" src="https://github.com/user-attachments/assets/475931a1-a174-456c-9b45-54809918acbc" />
<img width="1920" height="1080" alt="Screenshot (141)" src="https://github.com/user-attachments/assets/8e2762c8-126c-4390-ab2f-105cd3a3d098" />
<img width="1920" height="1080" alt="Screenshot (140)" src="https://github.com/user-attachments/assets/57ae44c9-a0bd-4f3a-a99e-537da28cb5c4" />


Δf = fs / N = 200 / 200 = 1 Hz

This is the same as 1/T, which is 1/1 s. Each FFT bin is 1 Hz wide, so the 15 Hz tone lands in bin 15 and the 40 Hz tone lands in bin 40.
%% Exercise 3 - Non-integer number of cycles, longer recording, zero-padding
% Saves three spectra + README.md into the folder "Exercise 3".
clear; close all; clc

d  = 4;                    % <-- the last digit of your student number (0-9)
fs = 200;                  % sampling frequency (Hz)
f1 = 15;                   % fits exactly 15 cycles in 1 s
f2 = 13.2 + 0.17*d;        % does NOT fit an integer number of cycles

outDir = 'Exercise 3';
if ~exist(outDir, 'dir'), mkdir(outDir); end

sig = @(t) 0.8*sin(2*pi*f1*t) + 0.4*sin(2*pi*f2*t);

fprintf('d = %d,  f2 = %.2f Hz,  cycles of f2 in 1 s = %.2f\n', d, f2, f2*1);

%% Task 2: one-second recording
[f_1s, P_1s, df_1s] = singleSided(sig, fs, 1, 1);

%% Task 3: four-second recording (same fs, same frequencies)
[f_4s, P_4s, df_4s] = singleSided(sig, fs, 4, 1);

%% Task 5: one-second recording, zero-padded to 4*N
[f_zp, P_zp, df_zp] = singleSided(sig, fs, 1, 4);

%% Plots (saved as PNG)
plotSpectrum(f_1s, P_1s, f1, f2, ...
    sprintf('1 s recording (N = %d), \\Deltaf = %.2f Hz', fs*1, df_1s), ...
    fullfile(outDir, 'spectrum_1s.png'));
plotSpectrum(f_4s, P_4s, f1, f2, ...
    sprintf('4 s recording (N = %d), \\Deltaf = %.2f Hz', fs*4, df_4s), ...
    fullfile(outDir, 'spectrum_4s.png'));
plotSpectrum(f_zp, P_zp, f1, f2, ...
    sprintf('1 s recording zero-padded to %d points, \\Deltaf = %.2f Hz', 4*fs, df_zp), ...
    fullfile(outDir, 'spectrum_1s_zeropadded.png'));

% Task 4: comparison of all three in one figure
fig = figure('Name', 'Comparison');
stem(f_1s, P_1s, 'filled', 'LineWidth', 1.2, 'MarkerSize', 5); hold on
plot(f_4s, P_4s, '-o', 'MarkerSize', 3);
plot(f_zp, P_zp, '--', 'LineWidth', 1.2);
xline(f2, ':k', sprintf('f2 = %.2f Hz', f2));
xline(f1, ':k', '15 Hz');
hold off, grid on, xlim([10 20])
xlabel('Frequency (Hz)'), ylabel('Amplitude')
legend('1 s (\Deltaf = 1 Hz)', '4 s (\Deltaf = 0.25 Hz)', ...
       '1 s zero-padded (\Deltaf = 0.25 Hz)', 'Location', 'northwest')
title(sprintf('Comparison, d = %d, f2 = %.2f Hz', d, f2))
exportgraphics(fig, fullfile(outDir, 'spectrum_comparison.png'), 'Resolution', 150);

%% Numbers for the README
[a1, b1] = nearestBin(f_1s, P_1s, f2);  A15_1s = P_1s(abs(f_1s - f1) < 1e-9);
[a4, b4] = nearestBin(f_4s, P_4s, f2);  A15_4s = P_4s(abs(f_4s - f1) < 1e-9);
[az, bz] = nearestBin(f_zp, P_zp, f2);  A15_zp = P_zp(abs(f_zp - f1) < 1e-9);

fprintf('\n                 df (Hz)  bin nearest f2      amp    amp @ 15 Hz\n');
fprintf('1 s              %5.2f    %6.2f Hz       %5.3f   %5.3f\n', df_1s, a1, b1, A15_1s);
fprintf('4 s              %5.2f    %6.2f Hz       %5.3f   %5.3f\n', df_4s, a4, b4, A15_4s);
fprintf('1 s zero-padded  %5.2f    %6.2f Hz       %5.3f   %5.3f\n', df_zp, az, bz, A15_zp);

%% README.md
close15 = abs(f1 - f2) < 1.5;   % f2 within ~1 bin of 15 Hz in the 1 s spectrum

L = {};
L{end+1} = '# Exercise 3 - Spectral leakage, record length and zero-padding';
L{end+1} = '';
L{end+1} = sprintf('- **d** = %d', d);
L{end+1} = sprintf('- **f2** = 13.2 + 0.17*d = **%.2f Hz**', f2);
L{end+1} = sprintf('- fs = %d Hz, signal x(t) = 0.8 sin(2*pi*15*t) + 0.4 sin(2*pi*f2*t)', fs);
L{end+1} = '';
L{end+1} = '## Files';
L{end+1} = '- `spectrum_1s.png` - one-second recording';
L{end+1} = '- `spectrum_4s.png` - four-second recording';
L{end+1} = '- `spectrum_1s_zeropadded.png` - one-second recording, fft(x, 4*length(x))';
L{end+1} = '- `spectrum_comparison.png` - all three on the same axes (10-20 Hz)';
L{end+1} = '';
L{end+1} = '## 1. Prediction (before running MATLAB)';
L{end+1} = sprintf(['With T = 1 s the FFT bins are spaced fs/N = 1/T = 1 Hz, so they sit at whole ' ...
    'hertz values. 15 Hz completes exactly 15 cycles and lands on a bin, so it should appear as one ' ...
    'clean line of height 0.8. f2 = %.2f Hz completes %.2f cycles, which is not an integer, so it ' ...
    'falls between the %d Hz and %d Hz bins. I expect spectral leakage: no single bin reaches 0.4, ' ...
    'the largest part of the peak appears at the nearest bin (about %d Hz) with reduced height, and ' ...
    'small non-zero values spread into many neighbouring bins.'], ...
    f2, f2, floor(f2), ceil(f2), round(f2));
if close15
    L{end+1} = '';
    L{end+1} = sprintf(['Because f2 is only %.2f Hz below 15 Hz, part of the leaked energy of f2 ' ...
        'also falls into the 15 Hz bin, so I expect the 15 Hz line to differ from 0.8 too.'], f1 - f2);
end
L{end+1} = '';
L{end+1} = '## 2-3. Results';
L{end+1} = '';
L{end+1} = '| Spectrum | Bin spacing | Bin nearest f2 | Amplitude there | Amplitude at 15 Hz |';
L{end+1} = '|---|---|---|---|---|';
L{end+1} = sprintf('| 1 s | %.2f Hz | %.2f Hz | %.3f | %.3f |', df_1s, a1, b1, A15_1s);
L{end+1} = sprintf('| 4 s | %.2f Hz | %.2f Hz | %.3f | %.3f |', df_4s, a4, b4, A15_4s);
L{end+1} = sprintf('| 1 s zero-padded | %.2f Hz | %.2f Hz | %.3f | %.3f |', df_zp, az, bz, A15_zp);
L{end+1} = '';
L{end+1} = '(True amplitudes: 0.8 at 15 Hz and 0.4 at f2.)';
L{end+1} = '';
L{end+1} = '## 4. Comparison: 1 s vs 4 s';
L{end+1} = sprintf(['The bin spacing is fs/N = 1/T. With 1 s it is %.2f Hz; with 4 s it is %.2f Hz, ' ...
    'four times finer. In the 1 s spectrum f2 is smeared over the bins around it and its highest bin ' ...
    'is only %.3f instead of 0.4. In the 4 s spectrum there are bins much closer to f2 (nearest %.2f Hz), ' ...
    'so the peak is narrower in hertz, sits closer to the true frequency and is taller (%.3f). ' ...
    'The two tones are clearly separate. f2 still does not fit a whole number of cycles in 4 s ' ...
    '(%.2f cycles), so some leakage remains, but it is confined closer to the peak. ' ...
    'A longer recording gives real extra frequency resolution because it contains more signal.'], ...
    df_1s, df_4s, b1, a4, b4, 4*f2);
if close15
    L{end+1} = '';
    L{end+1} = sprintf(['In the 1 s spectrum the 15 Hz line measures %.3f instead of 0.8, because ' ...
        'leakage from f2 lands in the same bin. With 4 s the two peaks are about %d bins apart and the ' ...
        '15 Hz value returns to %.3f.'], A15_1s, round((f1 - f2)/df_4s), A15_4s);
end
L{end+1} = '';
L{end+1} = '## 5. Zero-padding';
L{end+1} = sprintf(['fft(x, 4*length(x)) appends zeros so the FFT has 4*N points. The plotted bins are ' ...
    'now %.2f Hz apart, so the curve looks smoother and shows the shape of the main lobe and side lobes ' ...
    'between the original 1 Hz points. The values at the original 1 Hz bins are unchanged. However, ' ...
    'the zeros contain no signal. The recording still lasts 1 s, so the peaks keep a main-lobe width ' ...
    'of about 2/T = 2 Hz. Zero-padding only interpolates the same spectrum on a finer grid: it does not ' ...
    'add information or improve the ability to separate close frequencies. Only the longer (4 s) ' ...
    'recording does that.'], df_zp);
if close15
    L{end+1} = '';
    L{end+1} = sprintf(['This is visible here: in the zero-padded plot the f2 component and the 15 Hz ' ...
        'component still merge into one wide lobe, while the 4 s spectrum separates them.']);
end

fid = fopen(fullfile(outDir, 'README.md'), 'w', 'n', 'UTF-8');
fprintf(fid, '%s\n', L{:});
fclose(fid);
fprintf('\nSaved figures and README.md in "%s"\n', outDir);

%% ---------------- Local functions ----------------
function [f, P1, df] = singleSided(sig, fs, T, padFactor)
    t    = 0:1/fs:T-1/fs;
    x    = sig(t);
    N    = length(x);
    Nfft = padFactor*N;
    Y    = fft(x, Nfft);
    P2   = abs(Y/N);               % divide by N (signal length), not Nfft
    P1   = P2(1:Nfft/2+1);
    P1(2:end-1) = 2*P1(2:end-1);
    f    = fs*(0:Nfft/2)/Nfft;
    df   = fs/Nfft;
end

function plotSpectrum(f, P, f1, f2, ttl, fname)
    fig = figure;
    stem(f, P, 'filled', 'MarkerSize', 4), grid on
    xline(f2, ':r', sprintf('f2 = %.2f Hz', f2));
    xline(f1, ':k', '15 Hz');
    xlim([0 30])
    xlabel('Frequency (Hz)'), ylabel('Amplitude')
    title({'Single-sided amplitude spectrum', ttl})
    exportgraphics(fig, fname, 'Resolution', 150);
end

function [fb, Pb] = nearestBin(f, P, f0)
    [~, k] = min(abs(f - f0));
    fb = f(k);  Pb = P(k);
end

EXERCISE 4
<img width="1920" height="1080" alt="Screenshot (148)" src="https://github.com/user-attachments/assets/0704727b-ab9b-4f5e-adb0-33af1bacf1ac" />
<img width="1920" height="1080" alt="Screenshot (147)" src="https://github.com/user-attachments/assets/1401cc5b-ae04-4b9a-a182-8b81e5332e31" />
<img width="1920" height="1080" alt="Screenshot (146)" src="https://github.com/user-attachments/assets/f0438cd4-866e-4b12-abd9-f8ca8eeeef70" />
%% DTFT vs DFT of x = [1 1 1 1]
clear; close all; clc

x = [1 1 1 1];
n = 0:length(x)-1;

%% Task 1: DTFT at 1000 frequencies between -pi and pi
w = linspace(-pi, pi, 1000);
X = x * exp(-1j * (n.' * w));        % X(w) = sum x[n] e^{-jwn}

figure
plot(w, abs(X), 'LineWidth', 1.5), grid on
xlabel('Angular frequency (rad/sample)')
ylabel('Magnitude')
title('DTFT of x = [1 1 1 1]')
xlim([-pi pi])
xticks(-pi:pi/2:pi)
xticklabels({'-\pi', '-\pi/2', '0', '\pi/2', '\pi'})

%% Task 2: DFT with 4 and 64 points
X4  = fft(x, 4);
X64 = fft(x, 64);                    % x is zero-padded to 64 samples

disp('fft(x,4) ='), disp(X4)

% Frequencies of the DFT bins, shifted to the range -pi ... pi
w4  = (-2:1)   * 2*pi/4;             % -pi, -pi/2, 0, pi/2
w64 = (-32:31) * 2*pi/64;

%% Task 3: magnitude of each DFT
figure
subplot(2,1,1)
stem(0:3, abs(X4), 'filled'), grid on
xlabel('k'), ylabel('|X[k]|'), title('|fft(x,4)|')

subplot(2,1,2)
stem(0:63, abs(X64), 'filled', 'MarkerSize', 3), grid on
xlabel('k'), ylabel('|X[k]|'), title('|fft(x,64)|')

%% Task 4: compare the DTFT curve with the DFT values
figure
plot(w, abs(X), 'k', 'LineWidth', 1.5), hold on
stem(w64, abs(fftshift(X64)), 'b', 'MarkerSize', 3)
stem(w4,  abs(fftshift(X4)),  'r', 'filled', 'LineWidth', 1.5, 'MarkerSize', 7)
hold off, grid on
xlim([-pi pi])
xticks(-pi:pi/2:pi)
xticklabels({'-\pi', '-\pi/2', '0', '\pi/2', '\pi'})
xlabel('Angular frequency (rad/sample)'), ylabel('Magnitude')
legend('DTFT', 'fft(x,64)', 'fft(x,4)')
title('DFT values are samples of the DTFT')

%% Task 6: where the DTFT magnitude is zero
zeroFreqs = w(abs(X) < 0.02);
fprintf('DTFT magnitude ~0 near w = %s rad/sample\n', mat2str(zeroFreqs, 3))
% Expected: w = -pi, -pi/2, pi/2, pi

