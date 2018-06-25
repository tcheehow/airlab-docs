# Intel Edison Setup

## Installing Jubilinux

### Before Starting

Before starting with the installation it's a good idea to boot the Edison straight out of the box to make sure it's working. This way we can make sure we have a functional board before proceeding and we won't be mistakenly blaming setup issues if something is wrong here.

Connect one USB cable to the cosole port and then start your temrminal app \(see next section for more information on this\). Once you are connected plug in the second USB cable for power and after 15 seconds you should see the system booting. If you want to login the user name is root \(no password\).

### Flash Debian \(Jubilinux\)

Download jubilinux from [http://www.jubilinux.org/](http://www.jubilinux.org/). The latest version tested is jubilinux-v0.2.0.zip based on Debian Jessie.

If Windows is used, dfu-util is required. Download the latest version from this page: [http://dfu-util.sourceforge.net/releases/](http://dfu-util.sourceforge.net/releases/). Add dfu-utils to environment variables.

Run PowerShell as administrator, execute the following commands

```
cd \pathToJubilinux
.\flashall.sh
```

When asked to plug and reobot the board, 1\) first power up the edison through the console port, 2\) then plug the OTG cable for flashing. You MUST NOT remove power before it’s done or it could be bricked. If you don't have a console connection make sure you wait 2 minutes at the end of the installation as it instructs. During this time it is completing the installion which shoudln't be interrupted. If you don't get any update on your console after this message is displayed restart your console terminal connection.

Connect to the console with 115000 8N1, for example:

`screen /dev/USB0 115200 8N1`

and login as edison \(password: edison\)

## Post Debian Install

After flashing jubilinux, the filesystem storage should be similar to this

```
Filesystem      1K-blocks   Used Available Use% Mounted on
/dev/root         1441648 247412   1099212  19% /
devtmpfs           491720      0    491720   0% /dev
tmpfs              492016      0    492016   0% /dev/shm
tmpfs              492016   6760    485256   2% /run
tmpfs                5120      0      5120   0% /run/lock
tmpfs              492016      0    492016   0% /sys/fs/cgroup
tmpfs              492016      0    492016   0% /tmp
/dev/mmcblk0p10   1337936 645976    675576  49% /home
/dev/mmcblk0p7      32686   4906     27780  16% /boot
tmpfs               98404      0     98404   0% /run/user/1002
```

### Free Up Root Partition

Before proceeding to install ROS, we would need to free up more space on the home/root partition. To do so, run the following commands as root:

```
//TODO:
```

## One-Time Setup

### Network Configuration

Run `wpa_passphrase your-ssid your-wifi-password` to generate a secure pks. Then, `cd /etc/network`  and edit the `cd /etc/network/interfaces`

* edit the wpa-ssid to the network ssid
* edit the wpa-pks to the encrypted psk generated earlier
* uncomment `auto wlan0`
* append `post-up iwconfig wlan0 power off `to disable power management that leads to occassional drop out.
* save, then run `ifup wlan0`as root

The final copy should look similar to

```
# interfaces(5) file used by ifup(8) and ifdown(8)
auto lo
iface lo inet loopback

auto usb0
iface usb0 inet static
    address 192.168.2.15
    netmask 255.255.255.0

auto wlan0
iface wlan0 inet dhcp
    # For WPA
    wpa-ssid FreeWifi1234
    wpa-psk 75ffd9ac3a51c3a61cdb32613fe9b979ae6005d3051e71ad29667bd9d121b6dc
```

### Debian Update

Modify the sources list at `/etc/apt/sources.list `to become

```
deb http://ftp.sg.debian.org/debian jessie main contrib non-free
#deb-src http://http.debian.net/debian jessie main contrib non-free

deb http://ftp.sg.debian.org/debian jessie-updates main contrib non-free
#deb-src http://http.debian.net/debian jessie-updates main contrib non-free

deb http://security.debian.org/ jessie/updates main contrib non-free
#deb-src http://security.debian.org/ jessie/updates main contrib non-free

#deb http://ubilinux.org/edison wheezy main

deb http://ftp.sg.debian.org/debian jessie-backports main
```

```
sudo apt-get -y update
sudo apt-get -y upgrade
```

### Setup /etc/rc.local

Logged in as `root`, edit `/etc/rc.local` and comment or uncomment lines as appropriate for your configuration. `/etc/rc.local` is used for the configuration of:

* USB OTG port device vs host mode
* Enabling bluetooth
* Enabling bluetooth rfcomm login console
* Enabling auto-acceptance of bluetooth pairing requests
* Activating wifi without `systemd`

### Locales

```
dpkg-reconfigure locales # Select only en_US.UTF8 and select None as the default on the confirmation page that follows.
update-locale
```

Update the `/etc/default/locale` file and ensure `LANG=en_US.UTF-8` and it uncommented out. Add `LC_ALL=C`. Then reboot.

Note that if you receive warning messages about missing or wrong languages this is likely to be due to the locale being forwarded when using SSH. Either ignore them or complete this step via the serial console by commenting out the SendEnv LANG LC\_\* line in the local /etc/ssh/ssh\_config file on your machine \(not the Edison\).

### Timezone

Edit the config file at `sudo nano /etc/ntp.conf` and change all instance of  `server x.debian.pool.ntp.org` to `server x.sg.pool.ntp.org.`

When done, `sudo dpkg-reconfigure tzdata`

## ROS/MAVROS Installation

Finally, we are ready to install ROS and the necesary packages. As ROS packages for the Edison/Ubilinux don't exist we will have to build it from source. This process will take about 1.5 hours but most of it is just waiting for it to build.

A script \(`ros-setups/intel-edison/install_ros.sh)`\) has been writen to automate the building and installation of ROS. Current testing has been copy-pasting line by line to the console. 

If all went well you should have a ROS installation. Hook your Edison up to the Pixhawk and run a test. See this page for instructions: [https://pixhawk.org/peripherals/onboard\_computers/intel\_edison](https://pixhawk.org/peripherals/onboard_computers/intel_edison)



