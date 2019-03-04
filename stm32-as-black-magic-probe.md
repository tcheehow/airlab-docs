---
description: The instructions were tested on an STM32F103C8T6
---

# STM32 as Black Magic Probe

{% hint style="info" %}
**References**

\[1\] [Converting an STM32F103 board to a Black Magic Probe](https://medium.com/@paramaggarwal/converting-an-stm32f103-board-to-a-black-magic-probe-c013cf2cc38c)

\[2\] [Building a Blackmagic Debug Probe](https://primalcortex.wordpress.com/2017/06/13/building-a-black-magic-debug-probe/)
{% endhint %}

## STM32 Loader

```text
mkdir ~/blackmagic_ws
cd ~/blackmagic_ws
wget https://raw.githubusercontent.com/jsnyder/stm32loader/master/stm32loader.py
chmod 774 stm32loader.py
sudo apt install python-pip
pip install pyserial
```

## Downloading and Compiling the Firmware

Clone the blackmagic repository and compile. Since we are compiling to ST32F103 board we will assume it is a ST-Link clone.

```text
git clone https://github.com/blacksphere/blackmagic
cd blackmagic/
make # to initialise and retrieve the submodules
```

Now we target the firmware to the STM32 board, compiling the firmware as it was for a ST-Link clone

```text
cd src
make clean && make PROBE_HOST=stlink
```

## Flashing the DFU Firmware

From the previous step, we should have compiled at least the blackmagic\_dfu.bin and blackmagic.bin file in the src directory.

First, the blackmagic\_dfu.bin needs to be flashed. This can be done through the serial port on PA9 and PA10. Make sure the BOOT0 is set to 1 and BOOT1 remains set at 0 to use the hardware serial port for loading. Then, run the following command \(you might need to press the reset button to get it working\)

```text
cd ~/blackmagic_ws
sudo ./stm32loader.py -p /dev/ttyUSB0 -V -e -w -v ./blackmagic/src/blackmagic_dfu.bin
```

## Flashing the BlackMagic Firmware

Finally, with the DFU firmware loaded, set the BOOT0 back to 0 and unplug the serial cable. We can now use the usb port to load the blackmagic firmware. 

```text
sudo dfu-util -d 1d50:6017 -s 0x08002000:leave -D blackmagic.bin
```

## Verifying the STM32

We can do a quick test that the STM32 is working by running `dmesg | grep ttyACM*` and the following should show up.

`[21223.612443] cdc_acm 1-9.1.2:1.0: ttyACM0: USB ACM device [21223.613549] cdc_acm 1-9.1.2:1.2: ttyACM1: USB ACM device`





















