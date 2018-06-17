# How to Compile QRadioLink for use with LimeSDR on Ubuntu 16.04

Since QRadioLink does not provide binaries for Ubuntu, building from source is currently the only option. After following this guide, you will have the entire SDR stack compiled from source and installed into one folder, which you can then easily move to other machines.

## Status

This document is in development, therefore it has many errors. The instructions here are not well tested and following them may not actually get you the intended result.

## Assumptions

- You are running Ubuntu 16.04
- Basics such as git, gcc and cmake installed.
- You can use apt to install missing pieces.

## Install the SDR Stack

First, follow this excellent tutorial by Xavier Ruppen to compile the core SDR Stack.

[http://blog.reds.ch/?p=43]()

This will get you to the point where you can start installing QRadioLink. Some important points:

- We recommend you use the naming conventions suggested in the above link so that you can copy and paste code snippets. Particularly, `$LIME_INSTALL` and `$LIME_SOURCE` will be used here.
- We won't be using Pothos or Gqrx, so feel free to skip those steps.
- If you are having trouble installing with apt, try aptitude. Often it will be able to install where apt will not.


## Install QRadioLink Dependencies

```
sudo apt install libconfig++-dev libgsm1-dev libprotobuf-dev libopus-dev libpulse-dev libasound2-dev libcodec2-dev libsqlite3-dev libjpeg-dev libprotoc-dev protobuf-compiler libqwt5-qt4-dev

sudo apt install libasound2-dev libasound2 speex libspeex-dev libspeex-dev libspeexdsp1 libspeexdsp-dev
```
## Build QRadioLink

```
cd $LIME_SRC
git clone --depth 1 https://github.com/kantooon/qradiolink.git
cd qradiolink/
cd ..
cd ext
protoc --cpp_out=. Mumble.proto
protoc --cpp_out=. QRadioLink.proto
cd ..
mkdir build
cd build
qmake-qt4 ..
LD_LIBRARY_PATH=$LIME_INSTALL/lib CPATH=$LIME_INSTALL/include make

```

Now you have `qradiolink` executable in `$LIME_SRC/qradiolink/build` directory. You will need to launch it with sudo, specifying the LD_LIBRARY_PATH.

We can move the executable into `$LIME_INSTALL/bin`.

```
mv qradiolink "$LIME_INSTALL/bin/"
```

Finally and optinally we can make a script to launch it, so that we don't have to type in the env variables every time.

```
#!/bin/sh
# runqradio.sh

LIME_INSTALL=$HOME/sdr_install # change this path to your install folder
sudo LD_LIBRARY_PATH=$LIME_INSTALL/lib CPATH=$LIME_INSTALL/include $LIME_INSTALL/bin/qradiolink
```

Make sure to customize your LIME_INSTALL path above if yours is different.
