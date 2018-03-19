# ODROID-XU4 Setup for MAVROS

## Bill of Materials

* ODROID-XU4
* 64GB eMMc Modules for XU4
* WiFi Module

## First-Time Ubuntu Setup

### Download and Flash Ubuntu

Download the Ubuntu 16.04LTS minimal image from the [hardkernel's official repo](https://odroid.in/ubuntu_16.04lts/).

Follow the instructions from the [user manual](https://magazine.odroid.com/wp-content/uploads/odroid-xu4-user-manual.pdf) to flash the eMMc.

### Network Setup

A network connection is required to complete the first-time setup after flashing the distro. As `root`, do the following

In `nano /etc/network/interfaces`, add the following lines  
`auto wlan0`

`allow-hotplug wlan0`

`iface wlan0 inet dhcp`

`wireless-power off`

`wpa-ssid "FreeWifi1234"`

`wpa-psk "748748748"`

### User Account Setup

The minimal distribution only have `root` setup. To add our own user account, `odroid`, use the following commands

`adduser odroid`

`adduser odroid sudo`

## MAVROS Setup

## Install ROS

First, we need ROS to be installed on ODROID. Following the instructions on the [ROS Kinetic Wiki](http://wiki.ros.org/action/show/kinetic/Installation/Ubuntu), and follow the instructions to install ROS-Base \(Bare Bones\) using the `sudo apt-get install ros-kinetic-ros-base` command.

### Install MAVROS

Next, we need the MAVROS package to interface with autopilots using MAVLink protocol to ROS. Simply follow the instructions documented on [PX4 Dev Wiki](https://dev.px4.io/en/ros/mavros_installation.html) or [MAVROS GitHub](https://github.com/mavlink/mavros/tree/master/mavros#binary-installation-deb) for the binary installation of the `mavros` and `mavros-extra` package using `sudo apt-get install ros-kinetic-mavros ros-kinetic-mavros-extras`

### Install OpenCV and more

TODO

### Install Cartographer ROS

Follow the instructions on [Cartographer ROS Wiki](https://google-cartographer-ros.readthedocs.io/en/latest/) to build and install from source.

Cartographer requires the latest version of ninja, refer to the following [link](https://www.claudiokuenzler.com/blog/756/install-newer-ninja-build-tools-ubuntu-14.04-trusty#.Wq927icRVhE) for help if a ninja build error occurs.

If an Eigen3 build related error occur, run the following command `sudo cp /usr/share/cmake-2.8/Modules/FindEigen3.cmake /usr/share/cmake-3.2/Modules/`

## References

1. [https://mzahana.gitbooks.io/indoor-nav-at-risclab/content/](https://mzahana.gitbooks.io/indoor-nav-at-risclab/content/)



