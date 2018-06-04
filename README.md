# HamComSDR
An intuitive light-weight software suite for Linux, and Android, in C++
    -geared toward hams that want to play around with SDR
    -a ham radio SDR Swiss Army knife

Goals:

    (Focusing primarily on RX/TX of data over PSK for UHF/VHF)

RX voice and data on bands 160m to 23cm, and all intermediate bands <br>
TX voice and data on bands 160m to 23cm, and all intermediate bands <br>

Opperating the following modes and modulations: <br> 

CW <br>
  -MCW <br>
  -FSCW <br>
FM<br>
AM<br>
PM<br>
qAM<br>
DSB-SC<br>
ISB<br>
LSB<br>
SSB<br>
USB<br>
DMR<br>
ASK<br>
FSK<br>
  -RTTY<br>
  -G-TOR<br>
  -HF Packet<br>
MSK<br>
MFSK<br>
  -MFSK16<br>
  -FSK441<br>
  -JT6M<br>
  -JT65<br>
  -JT65HF<br>
  -FT8<br>
  -Olivia<br>
  -Contestia<br>
  -DominoEX<br>
PSK<br>
  -PSK31<br>
  -PSK63<br>
  -Clover<br>
qPSK<br>
  -qPSK31<br>
  -qPSK 63<br>
MT63<br>
THROB<br>
PACTOR<br>
AMTOR<br>
SITOR<br>
ATV<br>
SSTV<br>
VSB<br>
OOK<br>
OSCAR<br>
QRP<br>
WSPR<br>
Hellschreiber<br>
Spread Spectrum<br>
  -FHSS<br>
  -DSSS<br>
  -THSS<br>
  -CSS<br>
<br>
AND MANY, MANY MORE!!!<br>
<br>
Strategy:
<br>
Program will develope in phases:<br>
<br>
Phase 0: port C++ Library PSKCoreDLL or a derivative to Lime SDR<br>
Phase 1: Command line functionality<br>
Phase 2: Integrate more modes/modulations
Phase 3:N-Curses interface<br>
Phase 4:GUI<br>
Phase 5:Android App<br>
<br>
<br>
Phase:0
<br>
[![TXStructure.jpg](https://s15.postimg.cc/mvicxizdn/TXStructure.jpg)](https://postimg.cc/image/i9m8p6duf/)
<br>
This is the structure of PSKCoreDLL TX Section. We will attempt to intercept the signal at I Q points (blue) and stream in the Lime SDR I and Q inputs.
<br>
Perhaps we need to include the occilators, in that case we will intersect at red points<br>
<br>
This is the structure of PSKCoreDLL RX Section. We will attempt to input the signal at I Q points(red) from the Lime SDR I and Q outputs
<br>
[![RX_structure_EDIT.jpg](https://s15.postimg.cc/du3tjw6l7/RX_structure_EDIT.jpg)](https://postimg.cc/image/ye8nidmc7/)
<br>
We will route the I an Q Signals in and out of the Lime Suite driver<br>
<br>

[![dependencies_EDIT.jpg](https://s15.postimg.cc/d74wuump7/dependencies_EDIT.jpg)](https://postimg.cc/image/nh7bu3ckn/)


