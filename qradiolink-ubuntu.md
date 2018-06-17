# How to Compile QRadioLink for use with LimeSDR on Ubuntu 16.04

Since QRadioLink does not provide binaries for Ubuntu, building from source is currently the only option. After following this guide, you will have the entire SDR stack compiled from source and installed into one folder, which you can then easily move to other machines.

## Status

This document is in development, therefore you should expect omissions and errors. In semver terms, you can think of it as v0.0.2.

## Assumptions

- You are running Ubuntu 16.04. However, the instructions should be easily adaptable to other Linux distributions.
- You have a Lime board plugged in. However, this should be usable with all the boards that Soapy SDR supports.
- Basics such as git, gcc and cmake installed.

## Install the SDR Stack

First, follow these excellent instructions by Xavier Ruppen to compile the core SDR Stack.

[http://blog.reds.ch/?p=43](http://blog.reds.ch/?p=43)

This will get you to the point where you can start installing QRadioLink. Some important points:

- We recommend you use the naming conventions suggested in the above link so that you can copy and paste code snippets. Particularly, `$LIME_INSTALL` and `$LIME_SRC` will be used here.
- We won't be using Pothos or Gqrx, so feel free to skip those steps.
- If you are having trouble installing with apt, try aptitude. Often it will be able to install where apt will not.


## Install QRadioLink Dependencies

```sh
sudo apt install libconfig++-dev libgsm1-dev libprotobuf-dev libopus-dev libpulse-dev libasound2-dev libcodec2-dev libsqlite3-dev libjpeg-dev libprotoc-dev protobuf-compiler libqwt5-qt4-dev

sudo apt install libasound2-dev libasound2 speex libspeex-dev libspeex-dev libspeexdsp1 libspeexdsp-dev
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
LD_LIBRARY_PATH=$LIME_INSTALL/lib CPATH=$LIME_INSTALL/include make
```

Now you have `qradiolink` executable in `$LIME_SRC/qradiolink/build` directory. You will need to specify the LD_LIBRARY_PATH when launching; we'll make a script for that.

First, move the executable into `$LIME_INSTALL/bin/`.

```sh
mv qradiolink "$LIME_INSTALL/bin/"
```

Create the file qradiolink.sh, as below. Make sure to customize your LIME_INSTALL path above if yours is different.

```sh
#!/bin/sh
# qradiolink.sh

LIME_INSTALL=$HOME/sdr_install # change this path to your install folder
LD_LIBRARY_PATH=$LIME_INSTALL/lib $LIME_INSTALL/bin/qradiolink
```
You can copy this file anywhere. For example:

```sh
sudo cp qradiolink.sh /usr/local/bin/qradiolink
sudo a+x /usr/local/bin/qradiolink
```

Now you can launch and use the app. If you are limited on disk space, you can delete the `$LIME_SRC` directory (≈700 MB). You may want to keep it though, since now you can easily update each component to the latest changes.

You can also zip the entire `$LIME_INSTALL` folder and move it to another machine, which will need only a few basic packages installed with `apt`.

## Device Setup

The device needs to be configured in the "Setup" tab of QRadioLink. At minimum, device args and antenna names need to be changed from defaults.

For lime boards:
- the device args should be `driver=lime,soapy=0` or empty. The latter option will select the first SDR board the Soapy finds.
- the TX antenna can be either `BAND1` or `BAND2`.
- the RX antenna can be `LNAH` (for RX1_L) or `LNAH`, `LNAW`, `LB1` or `LB2`.

## Conclusion

This is it. If you find errors in this document, we will appreciate you filing an issue, and will try our best to help.
