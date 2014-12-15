Matlab
======

Projects

song=input('Input song file:','s') 
N=input('input N:');%input filter length
[Y,Fs,Bits]=wavread(song);
%soundsc(Y,Fs);
smaNum=(1/N)*[ones(1,N)];%the SMA filter numerator coefficients
smaDen=1;%the SMA filter denominator coefficients
ximp=[1 zeros(1,N-1)];%input impulse signal
yimp=filter(smaNum,smaDen,ximp);%inpulse response
figure;
subplot(5,2,1);
stem(1:N,yimp,'b','.');
axis([0 9 0 2/N]);%set the ranges for x,y axes
title('Moving Average Filter:IMPULSE RESPONSE');  
xlabel('Samples');  
ylabel('Amplitude');
grid on;

Yleft=Y(:,1);%left channel of the sound clip
Yright=Y(:,2);%right channel of the sound clip
T=1/Fs;%sample time
Length=length(Y);
timeaxis=0:T:(Length-1)*T;

[h,w]=freqz(smaNum,smaDen,Length,Fs);%frequency response of SMA filter
subplot(5,2,2);
semilogx(w,20*log10(abs(h)),'r');%Unit is dB 
title('Moving Average Filter:Frequency Response'); 
xlabel('Frequency[Hz]');
ylabel('Magintude[dB]');
axis([0 20000 -100 0]);
grid on;

subplot(5,2,3);
plot(timeaxis,Yleft,'c');%time domain of original left channel
title('MusicExcerpt: Original Left Channel'); 
xlabel('Time[s]');
ylabel('Amplitude');
axis([0,30,-1,1]);
grid on;

subplot(5,2,4);
plot(timeaxis,Yright,'c');%time domain of original right channel
title('MusicExcerpt: Original Right Channel');
xlabel('Time[s]');
ylabel('Amplitude');
axis([0,30,-1,1]);
grid on;

Yleftf=fft(Yleft,Length);
f=Fs*(1:Length)/Length;
subplot(5,2,5);
semilogx(f,20*log10(abs(Yleftf)),'c');%frequency spectrum of original left channel
title('MusicExcerpt: Original Left Channel Spectrum');
xlabel('Frequency[Hz]');
ylabel('Magintude[dB]');
axis([0 20000 -100 100]);
grid on;

Yrightf=fft(Yright,Length);
subplot(5,2,6);
semilogx(f,20*log10(abs(Yrightf)),'c');%frequency spectrum of original right channel
title('MusicExcerpt: Original Right Channel Spectrum');
xlabel('Frequency[Hz]');
ylabel('Magintude[dB]');
axis([0 20000 -100 100]);
grid on;

Pleft=filter(smaNum,smaDen,Yleft);
%soundsc(Pleft,Fs);
subplot(5,2,7);
plot(timeaxis,Pleft,'g');%time domain of processed left channel
title('MusicExcerpt: Processed Left Channel');  
xlabel('Time[s]');
ylabel('Amplitude');
axis([0,30,-1,1]);
grid on;

Pright=filter(smaNum,smaDen,Yright);
%soundsc(Pright,Fs);
subplot(5,2,8);
plot(timeaxis,Pright,'g');%time domain of processed right channel
title('MusicExcerpt: Processed Right Channel'); 
xlabel('Time[s]');
ylabel('Amplitude');
axis([0,30,-1,1]);
grid on;

Pleftf=fft(Pleft,Length);
subplot(5,2,9);
semilogx(f,20*log10(abs(Pleftf)),'g');%frequency spectrum of processed left channel
title('MusicExcerpt: Processed Left Channel Spectrum');
xlabel('Frequency[Hz]');
ylabel('Magintude[dB]');
axis([0 20000 -100 100]);
grid on;

Prightf=fft(Pright,Length);
subplot(5,2,10);
semilogx(f,20*log10(abs(Prightf)),'g');%frequency spectrum of processed right channel
title('MusicExcerpt: Processed Right Channel Spectrum');
xlabel('Frequency[Hz]');
ylabel('Magintude[dB]');
axis([0 20000 -100 100]);
grid on;

figure;
zplane(smaNum,smaDen)
title('Pole-zero plot')
grid on;
