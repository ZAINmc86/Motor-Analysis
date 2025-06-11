% --- Load Data ---
healthy_Hz5  = readtable('5HZ_Healthy_Motor.csv');
healthy_Hz10 = readtable('Healthy_Motor_10Hz.csv');
healthy_Hz20 = readtable('20HZ_NEW_Healthy.csv');

% --- Time Conversion ---
Time_5Hz  = healthy_Hz5.Time_ms_  / 1000;
Time_10Hz = healthy_Hz10.Time_ms_ / 1000;
Time_20Hz = healthy_Hz20.Time_ms_ / 1000;

% --- Truncate to 120s ---
idx5  = Time_5Hz  <= 120;
idx10 = Time_10Hz <= 120;
idx20 = Time_20Hz <= 120;

Time_5Hz  = Time_5Hz(idx5);
Time_10Hz = Time_10Hz(idx10);
Time_20Hz = Time_20Hz(idx20);

healthy_Hz5  = healthy_Hz5(idx5, :);
healthy_Hz10 = healthy_Hz10(idx10, :);
healthy_Hz20 = healthy_Hz20(idx20, :);

% ----------------------- AX ------------------------
Ax_5Hz  = healthy_Hz5.Ax_g_;
Ax_10Hz = healthy_Hz10.Ax_g_;
Ax_20Hz = healthy_Hz20.Ax_g_;

[b5, a5] = butter(4, 0.25/(5/2));
[b10, a10] = butter(4, 0.5/(10/2));
[b20, a20] = butter(4, 1.0/(20/2));

Ax_filt_5Hz  = filtfilt(b5, a5, Ax_5Hz);
Ax_filt_10Hz = filtfilt(b10, a10, Ax_10Hz);
Ax_filt_20Hz = filtfilt(b20, a20, Ax_20Hz);

n5 = length(Ax_5Hz); f5 = (0:n5-1)*(5/n5);
n10 = length(Ax_10Hz); f10 = (0:n10-1)*(10/n10);
n20 = length(Ax_20Hz); f20 = (0:n20-1)*(20/n20);

Ax_fft_5Hz = abs(fft(Ax_filt_5Hz));
Ax_fft_10Hz = abs(fft(Ax_filt_10Hz));
Ax_fft_20Hz = abs(fft(Ax_filt_20Hz));

figure;
subplot(3,1,1);
plot3(Time_5Hz, 5*ones(size(Time_5Hz)), Ax_5Hz, 'r'); hold on;
plot3(Time_10Hz, 10*ones(size(Time_10Hz)), Ax_10Hz, 'g');
plot3(Time_20Hz, 20*ones(size(Time_20Hz)), Ax_20Hz, 'b');
xlabel('Time (s)'); ylabel('Sampling Rate (Hz)'); zlabel('Ax Raw ');
title('3D Raw Acceleration Signals (Ax)'); grid on; view(45,25); legend('5Hz','10Hz','20Hz');

subplot(3,1,2);
plot3(Time_5Hz, 5*ones(size(Time_5Hz)), Ax_filt_5Hz, 'r'); hold on;
plot3(Time_10Hz, 10*ones(size(Time_10Hz)), Ax_filt_10Hz, 'g');
plot3(Time_20Hz, 20*ones(size(Time_20Hz)), Ax_filt_20Hz, 'b');
xlabel('Time (s)'); ylabel('Sampling Rate (Hz)'); zlabel('Ax Filtered');
title('3D Filtered Acceleration Signals (Ax)'); grid on; view(45,25); legend('5Hz','10Hz','20Hz');

subplot(3,1,3);
plot(f5, Ax_fft_5Hz, 'r'); hold on;
plot(f10, Ax_fft_10Hz, 'g');
plot(f20, Ax_fft_20Hz, 'b');
xlim([0 10]); xlabel('Frequency (Hz)'); ylabel('|FFT|');
title('FFT Spectrum (Ax)'); grid on; legend('5Hz','10Hz','20Hz');

% ----------------------- AY ------------------------
Ay_5Hz  = healthy_Hz5.Ay_g_;
Ay_10Hz = healthy_Hz10.Ay_g_;
Ay_20Hz = healthy_Hz20.Ay_g_;

Ay_filt_5Hz  = filtfilt(b5, a5, Ay_5Hz);
Ay_filt_10Hz = filtfilt(b10, a10, Ay_10Hz);
Ay_filt_20Hz = filtfilt(b20, a20, Ay_20Hz);

Ay_fft_5Hz = abs(fft(Ay_filt_5Hz));
Ay_fft_10Hz = abs(fft(Ay_filt_10Hz));
Ay_fft_20Hz = abs(fft(Ay_filt_20Hz));

figure;
subplot(3,1,1);
plot3(Time_5Hz, 5*ones(size(Time_5Hz)), Ay_5Hz, 'r'); hold on;
plot3(Time_10Hz, 10*ones(size(Time_10Hz)), Ay_10Hz, 'g');
plot3(Time_20Hz, 20*ones(size(Time_20Hz)), Ay_20Hz, 'b');
xlabel('Time (s)'); ylabel('Sampling Rate (Hz)'); zlabel('Ay Raw');
title('3D Raw Acceleration Signals (Ay)'); grid on; view(45,25); legend('5Hz','10Hz','20Hz');

subplot(3,1,2);
plot3(Time_5Hz, 5*ones(size(Time_5Hz)), Ay_filt_5Hz, 'r'); hold on;
plot3(Time_10Hz, 10*ones(size(Time_10Hz)), Ay_filt_10Hz, 'g');
plot3(Time_20Hz, 20*ones(size(Time_20Hz)), Ay_filt_20Hz, 'b');
xlabel('Time (s)'); ylabel('Sampling Rate (Hz)'); zlabel('Ay Filtered');
title('3D Filtered Acceleration Signals (Ay)'); grid on; view(45,25); legend('5Hz','10Hz','20Hz');

subplot(3,1,3);
plot(f5, Ay_fft_5Hz, 'r'); hold on;
plot(f10, Ay_fft_10Hz, 'g');
plot(f20, Ay_fft_20Hz, 'b');
xlim([0 10]); xlabel('Frequency (Hz)'); ylabel('|FFT|');
title('FFT Spectrum (Ay)'); grid on; legend('5Hz','10Hz','20Hz');

% ----------------------- AZ ------------------------
Az_5Hz  = healthy_Hz5.Az_g_;
Az_10Hz = healthy_Hz10.Az_g_;
Az_20Hz = healthy_Hz20.Az_g_;

Az_filt_5Hz  = filtfilt(b5, a5, Az_5Hz);
Az_filt_10Hz = filtfilt(b10, a10, Az_10Hz);
Az_filt_20Hz = filtfilt(b20, a20, Az_20Hz);

Az_fft_5Hz = abs(fft(Az_filt_5Hz));
Az_fft_10Hz = abs(fft(Az_filt_10Hz));
Az_fft_20Hz = abs(fft(Az_filt_20Hz));

figure;
subplot(3,1,1);
plot3(Time_5Hz, 5*ones(size(Time_5Hz)), Az_5Hz, 'r'); hold on;
plot3(Time_10Hz, 10*ones(size(Time_10Hz)), Az_10Hz, 'g');
plot3(Time_20Hz, 20*ones(size(Time_20Hz)), Az_20Hz, 'b');
xlabel('Time (s)'); ylabel('Sampling Rate (Hz)'); zlabel('Az Raw');
title('3D Raw Acceleration Signals (Az)'); grid on; view(45,25); legend('5Hz','10Hz','20Hz');

subplot(3,1,2);
plot3(Time_5Hz, 5*ones(size(Time_5Hz)), Az_filt_5Hz, 'r'); hold on;
plot3(Time_10Hz, 10*ones(size(Time_10Hz)), Az_filt_10Hz, 'g');
plot3(Time_20Hz, 20*ones(size(Time_20Hz)), Az_filt_20Hz, 'b');
xlabel('Time (s)'); ylabel('Sampling Rate (Hz)'); zlabel('Az Filtered');
title('3D Filtered Acceleration Signals (Az)'); grid on; view(45,25); legend('5Hz','10Hz','20Hz');

subplot(3,1,3);
plot(f5, Az_fft_5Hz, 'r'); hold on;
plot(f10, Az_fft_10Hz, 'g');
plot(f20, Az_fft_20Hz, 'b');
xlim([0 10]); xlabel('Frequency (Hz)'); ylabel('|FFT|');
title('FFT Spectrum (Az)'); grid on; legend('5Hz','10Hz','20Hz');