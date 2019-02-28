# Additional Modules

Additional modules can be added to the kernel by using `menuconfig`.

To modify the kernel configuration \(add/remove features and drivers\) please follow the steps below:

```text
1. $ cd ~imx6_ws/var_som_mx6_ubuntu/src/kernel
2. $ sudo make ARCH=arm mrproper
3. $ sudo make ARCH=arm imx_v7_var_defconfig
4. $ sudo make ARCH=arm menuconfig
5. Navigate the menu and select the desired kernel functionality
6. Exit the menu and answer "Yes" when asked "Do you wish to save your new configuration?"
7. $ sudo make ARCH=arm savedefconfig
8. $ sudo cp arch/arm/configs/imx_v7_var_defconfig arch/arm/configs/imx_v7_var_defconfig.orig
9. $ sudo cp defconfig arch/arm/configs/imx_v7_var_defconfig
10. Follow the instructions above to rebuild kernel and modules, repack rootfs images and recreate SD card
```

## Example: CP210x Driver

In `menuconfig`, look for Device Drivers -&gt; USB Support -&gt; USB Serial Converters -&gt; CP210x

