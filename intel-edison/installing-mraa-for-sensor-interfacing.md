# MRAA for Sensor Interfacing

## Install Dependencies

```text
# first, we get the another dependency
sudo apt-get install python3 libpcre3-dev

# get the latest SWIG, mraa requires version > 3.0.5 
git clone https://github.com/swig/swig.git

# make sure we have the necessary build dependencies
sudo apt-get install bison automake autoconf build-essential g++

cd \pathTo\swig
./autogen.sh
./configure
make
sudo make install
```

## Install MRAA

To install MRAA, follow the instructions from [SparkFun](https://learn.sparkfun.com/tutorials/installing-libmraa-on-ubilinux-for-edison).

```text
git clone https://github.com/intel-iot-devkit/mraa.git
git checkout tags/v1.8.0
mkdir mraa/build && cd $_
cmake .. -DBUILDSWIGNODE=OFF
make
sudo make install
cd
```

## Update Shared Library Cache

This is required so that C or C++ programs have access to the library. `nano /etc/ld.so.conf` and add the following to the bottom of the file.

```text
/usr/local/lib/i386-linux-gnu/
```

When done, execute `sudo ldconfig` and check to make sure that the cache was updated by running

`sudo ldconfig -p | grep mraa`

If done correctly, you should see the following

```text
edison@jubilinux:~$ sudo ldconfig -p | grep mraa
        libmraa.so.1 (libc6) => /usr/local/lib/libmraa.so.1
        libmraa.so.0 (libc6) => /usr/local/lib/i386-linux-gnu/libmraa.so.0
        libmraa.so (libc6) => /usr/local/lib/libmraa.so
        libmraa.so (libc6) => /usr/local/lib/i386-linux-gnu/libmraa.so
```

## Export Library for Python

To gain access to mraa with Python, append the following to the end of `~/.bashrc`.

`export PYTHONPATH=$PYTHONPATH:$(dirname $(find /usr/local -name mraa.py))`

## Updating Sudoers File

Open sudoers files, and run `sudo visudo` and add the following immediately after the first "Defaults" line:

```text
Defaults        env_keep += PYTHONPATH
```

## Setting I2C and Serial Permission

Finally, we install i2c-tools and correctly set up the udev permission

```text
sudo apt-get install i2c-tools
```

```text
sudo reboot
sudo usermod -aG i2c edison
```

```text
sudo usermod -aG dialout edison
```

Reboot the edison for udev rules to take effect.

