# Compile QRadioLink on Ubuntu
Compile qradiolink on Ubuntu from Source

## Build Boost 1.62

```
# https://stackoverflow.com/questions/12578499/how-to-install-boost-on-ubuntu
sudo apt install python-dev autotools-dev libicu-dev build-essential libbz2-dev
curl -JLO https://sourceforge.net/projects/boost/files/boost/1.62.0/boost_1_62_0.tar.gz
tar -xf boost_1_62_0.tar.gz
cd boost_1_62_0

./bootstrap.sh --prefix=/usr/
./b2

sudo ./b2 install 
```

## Build gr-osmosdr

```
git clone --depth 1 git://git.osmocom.org/gr-osmosdr
cd gr-osmosdr/
mkdir build
cd build/
cmake ..
make
sudo make install
sudo ldconfig
```

## Install other dependencies

```
sudo apt install libconfig++-dev libgsm1-dev libprotobuf-dev libopus-dev libpulse-dev libasound2-dev libcodec2-dev libsqlite3-dev libjpeg-dev libprotoc-dev protobuf-compiler libqwt5-qt4-dev


sudo apt install libasound2-dev libasound2 speex libspeex-dev libspeex-dev libspeexdsp1 libspeexdsp-dev

```

## Compile QRadioLink

```
git clone --depth 1 https://github.com/kantooon/qradiolink.git
cd qradiolink/
mkdir build
cd build
cd ..
cd ext
protoc --cpp_out=. Mumble.proto
protoc --cpp_out=. QRadioLink.proto
cd ../build/
qmake-qt4 ..
make

```
