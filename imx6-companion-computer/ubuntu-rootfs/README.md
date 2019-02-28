---
description: DART-MX6 Ubuntu ROOTFS
---

# Compiling Ubuntu Xenial for DART-MX6



{% hint style="info" %}
References:

[https://community.nxp.com/docs/DOC-330147](https://community.nxp.com/docs/DOC-330147)

[https://github.com/varigit/debian-var/blob/debian\_jessie\_mx6ul\_var01/make\_var\_mx6ul\_dart\_debian.sh](https://github.com/varigit/debian-var/blob/debian_jessie_mx6ul_var01/make_var_mx6ul_dart_debian.sh)

[http://variwiki.com/index.php?title=Debian\_Build\_Release&release=RELEASE\_STRETCH\_V2.0\_VAR-SOM-MX6](http://variwiki.com/index.php?title=Debian_Build_Release&release=RELEASE_STRETCH_V2.0_VAR-SOM-MX6)
{% endhint %}

### Deploy Source

Download archive containing the build script and support files for building Ubuntu ALIP for this board:

```text
$ cd ~/imx6_ws
$ git clone https://github.com/tcheehow/debian-var.git -b ubuntu_alip_mx6 var_som_mx6_ubuntu
```

Create environment \(Internet connection should be available\):

```text
$ cd ~/var_som_mx6_ubuntu
$ ./make_var_som_mx6_ubuntu.sh -c deploy
```

### 3.2 Build by parts

#### 3.2.1 Build bootloader

```text
$ cd ~/var_som_mx6_ubuntu
$ sudo ./make_var_som_mx6_ubuntu.sh -c bootloader
```

#### 3.2.2 Build kernel, dtb files and kernel modules

```text
$ cd ~/var_som_mx6_ubuntu
$ sudo ./make_var_som_mx6_ubuntu.sh -c kernel
$ sudo ./make_var_som_mx6_ubuntu.sh -c modules
```

#### 3.2.3 Build rootfs

Internet connection should be available

```text
$ cd ~/var_som_mx6_ubuntu
$ sudo ./make_var_som_mx6_ubuntu.sh -c rootfs
```

#### 3.2.4 Pack rootfs

To create the root file system archive \(rootfs.tar.gz\), run the following commands:

```text
$ cd ~/var_som_mx6_ubuntu
$ sudo ./make_var_som_mx6_ubuntu.sh -c rtar
```

## 4 Create boot SD card

1. Follow the above steps for make rootfs, kernel, bootloader;
2. Insert the SD card to card reader connected to a host system;
3. Run the following commands \(Caution! All data on the card will be destroyed\):

```text
$ cd ~/var_som_mx6_ubuntu
$ sudo ./make_var_som_mx6_ubuntu.sh -c sdcard -d /dev/sdX
```



























