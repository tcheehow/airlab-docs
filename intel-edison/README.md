# Intel Edison Setup

## Installing Jubilinux <a id="installing-jubilinux"></a>

### Before Starting

Before starting with the installation it's a good idea to boot the Edison straight out of the box to make sure it's working. This way we can make sure we have a functional board before proceeding and we won't be mistakenly blaming setup issues if something is wrong here.

Connect one USB cable to the cosole port and then start your temrminal app \(see next section for more information on this\). Once you are connected plug in the second USB cable for power and after 15 seconds you should see the system booting. If you want to login the user name is root \(no password\).

### Flash Debian \(Jubilinux\)

Download jubilinux from [http://www.jubilinux.org/](http://www.jubilinux.org/). The latest version tested is jubilinux-v0.2.0.zip based on Debian Jessie.

If Windows is used, dfu-util is required. Download the latest version from this page: [http://dfu-util.sourceforge.net/releases/](http://dfu-util.sourceforge.net/releases/). Add dfu-utils to environment variables.

Run PowerShell as administrator, execute the following commands

```text
cd \pathToJubilinux
.\flashall.sh
```

When asked to plug and reobot the board, 1\) first power up the edison through the console port, 2\) then plug the OTG cable for flashing. You MUST NOT remove power before it’s done or it could be bricked. If you don't have a console connection make sure you wait 2 minutes at the end of the installation as it instructs. During this time it is completing the installion which shoudln't be interrupted. If you don't get any update on your console after this message is displayed restart your console terminal connection.

Connect to the console with 115000 8N1, for example:

`screen /dev/USB0 115200 8N1`

and login as edison \(password: edison\)

## Post Debian Install

After flashing jubilinux, the filesystem storage should be similar to this

```text
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

### Free Up Partition Space

Before proceeding to install ROS, we would need to free up more space on the home partition. To do so, run the following commands as root:

```text
//make sure you log in as root, do not do this as edison user
mv /usr/share/ /opt/.usr/
ln -sf /opt/.usr/ /usr/share

mv /usr/include /opt/.usr/
ln -sf /opt/.usr/ /usr/include
```

If the process if done correct, perform `ls -l /usr/`and you should see similar results

```text
root@jubilinux:~# ls -l /usr/
total 44
drwxr-xr-x  2 root root  20480 Apr  6  2017 bin
drwxr-xr-x  2 root root   4096 Jul  7  2014 games
drwxr-xr-x 36 root root   4096 Apr  5  2017 include
drwxr-xr-x 40 root root   4096 Apr  6  2017 lib
drwxrwsr-x 11 root staff  4096 Feb 17  2015 local
drwxr-xr-x  2 root root   4096 Apr  6  2017 sbin
lrwxrwxrwx  1 root root     13 Jan  1 00:37 share -> /opt/.usr/
drwxr-xr-x  2 root root   4096 Jul  7  2014 src
```

Then, run `df -h` again to see the results of freeing up the partition

```text
root@jubilinux:~# df -h
Filesystem       Size  Used Avail Use% Mounted on
/dev/root        1.4G  499M  817M  38% /
devtmpfs         481M     0  481M   0% /dev
tmpfs            481M     0  481M   0% /dev/shm
tmpfs            481M  6.7M  474M   2% /run
tmpfs            5.0M     0  5.0M   0% /run/lock
tmpfs            481M     0  481M   0% /sys/fs/cgroup
tmpfs            481M     0  481M   0% /tmp
/dev/mmcblk0p10  1.3G  375M  917M  29% /home
/dev/mmcblk0p7    32M  4.8M   28M  16% /boot
tmpfs             97M     0   97M   0% /run/user/0
```

Also, we would clear space in the root partition, by removing unnecessary docs, locales and man.

Add the following to the file `/etc/dpkg/dpkg.cfg` to prevent installing docs, locales and man pages. You can also delete the contents of the folders `sudo rm -rf /usr/share/locale/*`, `sudo rm -rf /usr/share/man/*` and `sudo rm -rf /usr/share/doc/*`.

```text
# /etc/dpkg/dpkg.conf.d/01_nodoc

# Delete locales
path-exclude=/usr/share/locale/*

# Delete man pages
path-exclude=/usr/share/man/*

# Delete docs
path-exclude=/usr/share/doc/*
path-include=/usr/share/doc/*/copyright
```

## One-Time Setup

### Network Configuration

Run `wpa_passphrase your-ssid your-wifi-password` to generate a secure pks. Then, edit the network config by running `nano /etc/network/interfaces`

* edit the wpa-ssid to the network ssid
* edit the wpa-pks to the encrypted psk generated earlier
* uncomment `auto wlan0`
* append `post-up iwconfig wlan0 power off`to disable power management that leads to occassional drop out.
* save, then run `ifup wlan0`as root

The final copy should look similar to

```text
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
    post-up iwconfig wlan0 power off
```

### Debian Update

Modify the sources list at `/etc/apt/sources.list`to become

```text
deb http://ftp.sg.debian.org/debian jessie main contrib non-free
#deb-src http://http.debian.net/debian jessie main contrib non-free

deb http://ftp.sg.debian.org/debian jessie-updates main contrib non-free
#deb-src http://http.debian.net/debian jessie-updates main contrib non-free

deb http://security.debian.org/ jessie/updates main contrib non-free
#deb-src http://security.debian.org/ jessie/updates main contrib non-free

#deb http://ubilinux.org/edison wheezy main

deb http://ftp.sg.debian.org/debian jessie-backports main
```

```text
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

```text
dpkg-reconfigure locales # Select only en_US.UTF8 and select None as the default on the confirmation page that follows.
update-locale
```

Update the `/etc/default/locale` file and ensure `LANG=en_US.UTF-8` and it is uncommented out. Add `LC_ALL=C`. Then reboot.

Note that if you receive warning messages about missing or wrong languages this is likely to be due to the locale being forwarded when using SSH. Either ignore them or complete this step via the serial console by commenting out the SendEnv LANG LC\_\* line in the local /etc/ssh/ssh\_config file on your machine \(not the Edison\).

### Timezone

Edit the config file at `sudo nano /etc/ntp.conf` and change all instance of `server x.debian.pool.ntp.org` to `server x.sg.pool.ntp.org.`

When done, `sudo dpkg-reconfigure tzdata`

## ROS/MAVROS Installation

Finally, we are ready to install ROS and the necesary packages. As ROS packages for the Edison/Ubilinux don't exist we will have to build it from source. This process will take about 1.5 hours but most of it is just waiting for it to build.

A script \(`ros-setups/intel-edison/install_ros.sh`\) has been writen to automate the building and installation of ROS. Current testing has been copy-pasting line by line to the console. ~~Generally, the instructions are referenced from ~~\[Edison ROS Wiki\]\(~~[~~http://wiki.ros.org/wiki/edison~~](http://wiki.ros.org/wiki/edison)~~\) and \[ROSberryPi Wiki\]\(~~[~~http://wiki.ros.org/ROSberryPi/Installing~~](http://wiki.ros.org/ROSberryPi/Installing)~~~~~~ ROS Indigo on Raspberry Pi\)~~.~~

The ROS Kinetic instructions from [pennaerial ](https://raw.githubusercontent.com/pennaerial/ros-setups/master/intel-edison/install_kinetic_mavros.sh)works fine. For archival, the working line-by-line script is below:

```text
# !/bin/bash
# The following installation is based on: 
# 1. http://wiki.ros.org/wiki/edison 
# 2. http://wiki.ros.org/ROSberryPi/Installing%20ROS%20Indigo%20on%20Raspberry%20Pi
# 3. https://raw.githubusercontent.com/pennaerial/ros-setups/master/intel-edison/install_kinetic_mavros.sh

# 1. Update sources.list

sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu jessie main" > /etc/apt/sources.list.d/ros-latest.list'

# 2. Get ROS keys
wget https://raw.githubusercontent.com/ros/rosdistro/master/ros.key -O - | sudo apt-key add -

# 3. Update the OS
sudo apt-get -y update
sudo apt-get -y upgrade

# 4. Install required OS packages
sudo apt-get -y install python-rosdep python-rosinstall-generator python-wstool python-rosinstall build-essential

# 5. ROSDEP
sudo rosdep init
rosdep update

# 6. Create catkin workspace

mkdir ~/ros_catkin_ws
cd ~/ros_catkin_ws

# 7. Generate rosinstall
rosinstall_generator ros_comm mavros --rosdistro kinetic --deps --wet-only --exclude roslisp --tar > kinetic-ros_comm-wet.rosinstall

sudo wstool init src -j1 kinetic-ros_comm-wet.rosinstall
while [ $? != 0 ]; do
  echo "*** wstool - download failures, retrying ***"
  sudo wstool update -t src -j1
done

# 8. rosdep install - Errors at the end may or may not be normal ***"
cd ~/ros_catkin_ws
#  Python errors after the following command are normal.
rosdep install --from-paths src --ignore-src --rosdistro kinetic -y -q -r --os=debian:jessie

# 9. Building ROS 
sudo ./src/catkin/bin/catkin_make_isolated --install -DCMAKE_BUILD_TYPE=Release --install-space /home/ros/kinetic -j1

sudo ln -sf /home/ros /opt/

# 10. Updating .bashrc
echo "source /home/ros/kinetic/setup.bash" >> ~/.bashrc
source ~/.bashrc

cd ~/ros_catkin_ws
```

If all went well you should have a ROS installation. Hook your Edison up to the Pixhawk and run a test. See this page for instructions: [https://pixhawk.org/peripherals/onboard\_computers/intel\_edison](https://pixhawk.org/peripherals/onboard_computers/intel_edison)

### Post ROS Installation

After installing ROS, the storage condition on the Edison should be similar to this.

```text
edison@jubilinux:~/ros_catkin_ws$ df -h
Filesystem       Size  Used Avail Use% Mounted on
/dev/root        1.4G 1022M  294M  78% /
devtmpfs         481M     0  481M   0% /dev
tmpfs            481M     0  481M   0% /dev/shm
tmpfs            481M  6.7M  474M   2% /run
tmpfs            5.0M     0  5.0M   0% /run/lock
tmpfs            481M     0  481M   0% /sys/fs/cgroup
tmpfs            481M  6.1M  475M   2% /tmp
/dev/mmcblk0p10  1.3G  1.2G  155M  89% /home
/dev/mmcblk0p7    32M  4.8M   28M  16% /boot
tmpfs             97M     0   97M   0% /run/user/1002
```

