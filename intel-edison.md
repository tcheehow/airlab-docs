# Intel Edison Setup

## Before Starting

Before starting with the installation it's a good idea to boot the Edison straight out of the box to make sure it's working. This way we can make sure we have a functional board before proceeding and we won't be mistakenly blaming setup issues if something is wrong here.

Connect one USB cable to the cosole port and then start your temrminal app \(see next section for more information on this\). Once you are connected plug in the second USB cable for power and after 15 seconds you should see the system booting. If you want to login the user name is root \(no password\).

## Flash Debian \(Jubilinux\)

Download jubilinux from [http://www.jubilinux.org/](http://www.jubilinux.org/). The latest version tested is jubilinux-v0.2.0.zip based on Debian Jessie.

If Windows is used, dfu-util is required. Download the latest version from this page: [http://dfu-util.sourceforge.net/releases/](http://dfu-util.sourceforge.net/releases/)

Make sure you have the console USB cable in place and use it so you know when the installation has finished. You MUST NOT remove power before it’s done or it could be bricked. If you don't have a console connection make sure you wait 2 minutes at the end of the installation as it instructs. During this time it is completing the installion which shoudln't be interrupted. If you don't get any update on your console after this message is displayed restart your console terminal connection.

Connect to the console with 115000 8N1, for example:

`screen /dev/USB0 115200 8N1`

and login as edison \(password: edison\)



