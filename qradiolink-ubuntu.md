# How to Compile QRadioLink for use with LimeSDR on Ubuntu 16.04

Since QRadioLink does not provide binaries for Ubuntu, building from source is currently the only option. After following this guide, you will have the entire SDR stack compiled from source and installed into one folder, which you can then easily move to other machines.

## Status

This document is in development, therefore you should expect omissions and errors. After their initial creation, the instructions here have been tested once on a fresh Ubuntu installation. Since then, we've added the SDR Angel section and udev rules installation instruction, neither of which have been tested on a fresh OS.

## Assumptions

- You are running a fresh installation of Ubuntu 16.04. However, the instructions should be easily adaptable to other Linux distributions.
- You have a Lime board plugged in. However, the instructions should be easily adaptible for all the boards that Soapy SDR supports.

## Install Some Prerequisites

These will fill in some dependencies for the upcoming steps.

```
sudo apt update
sudo apt install git libpython-dev 
sudo apt-get install libboost-all-dev # required by multiple components
sudo apt install python-mako python-six # reqiored by Volk

# required by gnuradio
sudo apt install qt4-default python-qt4 libqwt5-qt4-dev libqwt5-qt4 python-qwt liblog4cpp5-dev
sudo apt install fftw-dev python-fftw libfftw3-dev libfftw3-bin # fftw
sudo apt install python-lxml python-wxgtk3.0 python-wxgtk3.0-dev # wx gui
sudo apt install libgsl-dev libcppunit-dev
sudo apt install python-cheetah python-gtk2-dev 

```

## Install the SDR Stack

Please follow these excellent instructions by Xavier Ruppen to compile the core SDR Stack.

[http://blog.reds.ch/?p=43](http://blog.reds.ch/?p=43)

_Important_: Until this issue https://github.com/gnuradio/gnuradio/issues/1856 is resolved, the GNU Radio install step only works on the `351dfb8ec4b07dddbd921f994c2bfd89cc35eadf` revision. After cloning the gnuradio repository, you can get to it by `git checkout 351dfb8`.

This will get you to the point where you can start installing QRadioLink. Some important points:

- We recommend you use the naming conventions suggested in the above link so that you can copy and paste code snippets. Particularly, variables `$LIME_INSTALL` and `$LIME_SRC` will be used here.
- We won't be using Pothos or Gqrx, so feel free to skip the corresponding installation steps.
- If you are having trouble installing with apt, try aptitude. Often it will be able to install where apt will not.


## Install QRadioLink Dependencies

```sh
sudo apt install libconfig++-dev libgsm1-dev libprotobuf-dev libopus-dev libpulse-dev libasound2-dev libcodec2-dev libsqlite3-dev libjpeg-dev libprotoc-dev protobuf-compiler libqwt5-qt4-dev

sudo apt install libasound2-dev libasound2 speex libspeex-dev libspeexdsp1 libspeexdsp-dev
```
## Build QRadioLink

```sh
cd $LIME_SRC
git clone --depth 1 https://github.com/kantooon/qradiolink.git
cd qradiolink/
cd ext
protoc --cpp_out=. Mumble.proto
protoc --cpp_out=. QRadioLink.proto
cd ..
mkdir build
cd build
qmake-qt4 ..
LD_LIBRARY_PATH="$LIME_INSTALL/lib" CPATH="$LIME_INSTALL/include" SUBLIBS="-L$LIME_INSTALL/lib" make
```

Now you have the `qradiolink` executable in `$LIME_SRC/qradiolink/build` directory. You will need to specify the LD_LIBRARY_PATH when launching; we'll make a script for that.

First, move the executable into `$LIME_INSTALL/bin/`.

```sh
mv qradiolink "$LIME_INSTALL/bin/"
```

Create the file qradiolink.sh, as below. Make sure to customize your LIME_INSTALL path if yours is different.

```sh
#!/bin/sh
# qradiolink.sh

LIME_INSTALL="$HOME/sdr_install" # change this path to your install folder
LD_LIBRARY_PATH="$LIME_INSTALL/lib" "$LIME_INSTALL"/bin/qradiolink
```
You can copy this file anywhere. For example:

```sh
sudo cp qradiolink.sh /usr/local/bin/qradiolink
sudo chmod a+x /usr/local/bin/qradiolink
```
Now you can launch the app, but it still requires set up. 

## Device Setup

First, the udev rules have to be installed. You can do this by running `sudo $LIME_SRC/LimeSuite/udev-rules/install.sh`, if you haven't done so already. Alternatively, if security isn't a concern, you can run `qradiolink` with `sudo`.

The device has to be configured in the "Setup" tab of QRadioLink. At minimum, device args and antenna names need to be changed from defaults.

For lime boards:
- the device args should be `driver=lime,soapy=0` or empty. The latter option will select the first SDR board Soapy finds.
- the TX antenna can be either `BAND1` or `BAND2`.
- the RX antenna can be `LNAL` (for RX1_L) or `LNAH`, `LNAW`, `LB1` or `LB2`.

## Bonus: SDR Angel

You can now easily install [SDR Angel](https://github.com/f4exb/sdrangel) in a similar way.

```
cd $LIME_SRC
git clone --depth 1 https://github.com/f4exb/sdrangel.git
cd sdrangel
sudo apt install libqt5multimedia5-plugins qtmultimedia5-dev qttools5-dev qttools5-dev-tools libqt5opengl5-dev qtbase5-dev libusb-1.0 librtlsdr-dev libboost-all-dev libasound2-dev pulseaudio libnanomsg-dev libopencv-dev libsqlite3-dev libxml2-dev bison flex ffmpeg libavcodec-dev libavformat-dev
cmake -DCMAKE_INSTALL_PREFIX=$LIME_INSTALL -DCMAKE_PREFIX_PATH=$LIME_INSTALL ..
make
make install
```

Now you should have the `sdrangel` executable in `$LIME_INSTALL/bin`.

## Conclusion

If you are limited on disk space, you can delete the `$LIME_SRC` directory (≈700 MB). You may want to keep it though, since now you can easily update each component to the latest changes.

You can also zip the entire `$LIME_INSTALL` folder and move it to another machine, which will need only a few basic packages installed with `apt`.

This is it. If you find errors in this document, we will appreciate you filing an issue, and will try our best to help. 
